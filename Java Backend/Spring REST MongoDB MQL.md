# Develop a Java Spring Boot microservice that uses MongoDB to store and retrieve data, and expose a RESTful API to query the data using the MongoDB Query Language (MQL). The service should use the MongoDB Java Driver to interact with the database, and implement basic error handling and validation.

First, we need to add the required dependencies to our **`pom.xml`** file:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>

    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongo-java-driver</artifactId>
        <version>3.12.10</version>
    </dependency>
</dependencies>
```

Next, we can create a **`Book`** model class to represent our data:

```java
public class Book {
    @Id
    private String id;

    private String title;

    private String author;

    private int year;

    // getters and setters
}
```

We use the **`@Id`** annotation to specify the field that will be used as the document ID in MongoDB.

Next, we can create a **`BookRepository`** interface that extends the **`MongoRepository`** interface provided by Spring Data MongoDB:

```java
public interface BookRepository extends MongoRepository<Book, String> {
    List<Book> findByAuthor(String author);
    List<Book> findByYearGreaterThan(int year);
}
```

This interface provides several CRUD methods for us to use, as well as two additional methods to query the data using the **`author`** and **`year`** fields.

We can now create a **`BookController`** class that will handle incoming requests and interact with the repository:

```java
@RestController
@RequestMapping("/books")
public class BookController {
    @Autowired
    private BookRepository bookRepository;

    @GetMapping("/")
    public List<Book> getAllBooks() {
        return bookRepository.findAll();
    }

    @GetMapping("/{id}")
    public Book getBookById(@PathVariable("id") String id) {
        return bookRepository.findById(id).orElse(null);
    }

    @PostMapping("/")
    public Book addBook(@RequestBody Book book) {
        return bookRepository.save(book);
    }

    @PutMapping("/{id}")
    public Book updateBook(@PathVariable("id") String id, @RequestBody Book book) {
        Book existingBook = bookRepository.findById(id).orElse(null);
        if (existingBook != null) {
            existingBook.setTitle(book.getTitle());
            existingBook.setAuthor(book.getAuthor());
            existingBook.setYear(book.getYear());
            return bookRepository.save(existingBook);
        } else {
            return null;
        }
    }

    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable("id") String id) {
        bookRepository.deleteById(id);
    }

    @GetMapping("/search/author")
    public List<Book> getBooksByAuthor(@RequestParam("author") String author) {
        return bookRepository.findByAuthor(author);
    }

    @GetMapping("/search/year")
    public List<Book> getBooksByYear(@RequestParam("year") int year) {
        return bookRepository.findByYearGreaterThan(year);
    }
}
```

This controller provides endpoints for each of the CRUD operations, as well as two additional endpoints for querying the data by author and year.

```java
@Configuration
public class MongoConfig {

    @Value("${spring.data.mongodb.host}")
    private String mongoHost;

    @Value("${spring.data.mongodb.port}")
    private int mongoPort;

    @Value("${spring.data.mongodb.database}")
    private String mongoDB;

    @Bean
    public MongoTemplate mongoTemplate() throws Exception {
        MongoClient client = new MongoClient(mongoHost, mongoPort);
        MongoTemplate mongoTemplate = new MongoTemplate(client, mongoDB);
        return mongoTemplate;
    }
}
```

This class defines a **`MongoTemplate`** bean that will be used to interact with the MongoDB database. The **`@Value`** annotations are used to inject the MongoDB host, port, and database name from the **`application.properties`** file.

With the **`MongoConfig`** class in place, we can now create a **`Person`** class that will be used to store and retrieve data from the MongoDB database:

```java
@Document(collection = "persons")
public class Person {

    @Id
    private String id;

    @NotBlank(message = "Name is mandatory")
    private String name;

    private int age;

    public Person() {}

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getters and setters
}
```

This class is annotated with **`@Document`** to indicate that it represents a MongoDB document. The **`@Id`** annotation is used to mark the **`id`** field as the document's primary key. The **`@NotBlank`** annotation is used to validate that the **`name`** field is not blank.

Next, we can create a **`PersonRepository`** interface that extends **`MongoRepository<Person, String>`**:

```java
public interface PersonRepository extends MongoRepository<Person, String> {

    List<Person> findByName(String name);

    List<Person> findByAgeGreaterThan(int age);

}
```

This interface extends **`MongoRepository<Person, String>`** and defines two additional methods for querying data by name and age. The **`MongoRepository`** interface provides several other methods for common CRUD operations.

Finally, we can create a **`PersonController`** class that defines REST endpoints for creating, reading, updating, and deleting **`Person`** records:

```java
@RestController
@RequestMapping("/api/persons")
public class PersonController {

    @Autowired
    private PersonService personService;

    @PostMapping
    public ResponseEntity<Person> createPerson(@Valid @RequestBody Person person) {
        Person savedPerson = personService.savePerson(person);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedPerson);
    }

    @GetMapping("/{id}")
    public ResponseEntity<Person> getPersonById(@PathVariable("id") String id) {
        Person person = personService.getPersonById(id);
        if (person == null) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        return ResponseEntity.ok(person);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Person> updatePerson(@PathVariable("id") String id, @Valid @RequestBody Person person) {
        Person updatedPerson = personService.updatePerson(id, person);
        if (updatedPerson == null) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        return ResponseEntity.ok(updatedPerson);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deletePerson(@PathVariable("id") String id) {
        boolean deleted = personService.deletePerson(id);
        if (!deleted) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        return ResponseEntity.noContent().build();
    }

    @GetMapping
    public List<Person> getPersonsByAgeAndCity(@RequestParam(name = "age") int age, @RequestParam(name = "city") String city) {
        return personService.getPersonsByAgeAndCity(age, city);
    }
}
```

This controller exposes RESTful endpoints to create, read, update, and delete **`Person`** objects. It also includes an additional endpoint to query for persons based on age and city.

With this implementation, you now have a Java Spring Boot microservice that uses MongoDB to store and retrieve data, and exposes a RESTful API to query the data using the MongoDB Query Language. You can now deploy this service to your preferred cloud provider, and start interacting with it via HTTP requests.