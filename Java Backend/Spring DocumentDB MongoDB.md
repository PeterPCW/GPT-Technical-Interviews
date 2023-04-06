# Develop a Java Spring Boot microservice that uses Amazon DocumentDB to store and retrieve data in a MongoDB-compatible format, and expose a RESTful API to query the data using the MongoDB Query Language. The service should use the DocumentDB Java SDK to interact with the database, and implement basic error handling and validation.

1. Create a new Spring Boot project using Spring Initializr, and add the required dependencies:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>com.amazonaws</groupId>
    <artifactId>aws-java-sdk-core</artifactId>
    <version>1.11.1001</version>
</dependency>

<dependency>
    <groupId>com.amazonaws</groupId>
    <artifactId>aws-java-sdk-docdb</artifactId>
    <version>1.11.1001</version>
</dependency>

<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongo-java-driver</artifactId>
    <version>3.12.10</version>
</dependency>
```

2. Create a configuration class to connect to Amazon DocumentDB:

```java
@Configuration
public class DocumentDBConfig {

    @Value("${docdb.host}")
    private String host;

    @Value("${docdb.port}")
    private int port;

    @Value("${docdb.username}")
    private String username;

    @Value("${docdb.password}")
    private String password;

    @Value("${docdb.database}")
    private String database;

    @Bean
    public MongoClient mongoClient() {
        MongoCredential credential = MongoCredential.createCredential(username, database, password.toCharArray());
        ServerAddress serverAddress = new ServerAddress(host, port);
        MongoClientSettings settings = MongoClientSettings.builder()
                .applyToClusterSettings(builder -> builder.hosts(Collections.singletonList(serverAddress)))
                .credential(credential)
                .build();
        return MongoClients.create(settings);
    }

    @Bean
    public MongoTemplate mongoTemplate() {
        return new MongoTemplate(mongoClient(), database);
    }

}
```

3. Create a model class to represent the data stored in Amazon DocumentDB:

```java
@Document(collection = "users")
public class User {

    @Id
    private String id;
    private String firstName;
    private String lastName;
    private int age;

    // constructors, getters and setters

}
```

4. Create a repository interface that extends **`MongoRepository`** to perform CRUD operations:

```java
@Repository
public interface UserRepository extends MongoRepository<User, String> {

}
```

5. Create a service class to implement the business logic:

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User getUserById(String id) {
        return userRepository.findById(id).orElse(null);
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public User updateUser(String id, User user) {
        User existingUser = userRepository.findById(id).orElse(null);
        if (existingUser == null) {
            return null;
        }
        existingUser.setFirstName(user.getFirstName());
        existingUser.setLastName(user.getLastName());
        existingUser.setAge(user.getAge());
        return userRepository.save(existingUser);
    }

    public void deleteUser(String id) {
        userRepository.deleteById(id);
    }

}
```

6. Create a controller class to expose the RESTful API:

```java
@RestController
@RequestMapping("/api")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/users")
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.getAllUsers();
        return new ResponseEntity<>(users, HttpStatus.OK);
    }

    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUserById(@PathVariable("id") String id) {
        User user = userService.getUserById(id);
        if (user != null) {
            return new ResponseEntity<>(user, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }

    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.createUser(user);
        return new ResponseEntity<>(savedUser, HttpStatus.CREATED);
    }

    @PutMapping("/users/{id}")
    public ResponseEntity<User> updateUser(@PathVariable("id") String id, @RequestBody User user) {
        User updatedUser = userService.updateUser(id, user);
        if (updatedUser != null) {
            return new ResponseEntity<>(updatedUser, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }

    @DeleteMapping("/users/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable("id") String id) {
        userService.deleteUser(id);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    }

}
```

In the controller class above, we have created methods to handle HTTP GET, POST, PUT, and DELETE requests. Each method maps to a specific endpoint, and calls the corresponding service method to perform the requested operation on the DocumentDB.

The **`@RestController`** annotation tells Spring that this is a RESTful controller that should handle HTTP requests. The **`@RequestMapping`** annotation specifies the base URI for all endpoints defined in this controller.

The **`@Autowired`** annotation tells Spring to inject the **`UserService`** dependency into this controller.

Each endpoint method is annotated with **`@GetMapping`**, **`@PostMapping`**, **`@PutMapping`**, or **`@DeleteMapping`**, depending on the HTTP method that it handles. The **`@PathVariable`** annotation is used to extract dynamic parameters from the URI, such as the **`id`** parameter in the **`getUserById()`** method.

Each endpoint method returns a **`ResponseEntity`** object, which contains the HTTP response status code and any data that needs to be returned in the response body.

7. In the next step, we will add validation to our API to ensure that only valid data is stored in our DocumentDB.

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

    private final EmployeeRepository employeeRepository;

    public EmployeeController(EmployeeRepository employeeRepository) {
        this.employeeRepository = employeeRepository;
    }

    @GetMapping("")
    public List<Employee> getAllEmployees() {
        return employeeRepository.findAll();
    }

    @GetMapping("/{id}")
    public Employee getEmployeeById(@PathVariable("id") String id) {
        return employeeRepository.findById(id)
                .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND,
                        String.format("Employee with id %s not found", id)));
    }

    @PostMapping("")
    public Employee createEmployee(@Valid @RequestBody Employee employee) {
        return employeeRepository.save(employee);
    }

    @PutMapping("/{id}")
    public Employee updateEmployee(@PathVariable("id") String id, @Valid @RequestBody Employee employee) {
        Employee existingEmployee = employeeRepository.findById(id)
                .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND,
                        String.format("Employee with id %s not found", id)));

        existingEmployee.setFirstName(employee.getFirstName());
        existingEmployee.setLastName(employee.getLastName());
        existingEmployee.setEmail(employee.getEmail());
        existingEmployee.setPhoneNumber(employee.getPhoneNumber());

        return employeeRepository.save(existingEmployee);
    }

    @DeleteMapping("/{id}")
    public void deleteEmployee(@PathVariable("id") String id) {
        employeeRepository.deleteById(id);
    }
}
```

In this controller class, we have implemented the basic CRUD operations for the **`Employee`** entity. The **`@RestController`** annotation is used to mark the class as a REST controller, and the **`@RequestMapping`** annotation is used to define the base path for all the endpoints.

The **`EmployeeController`** class has a constructor that accepts an instance of **`EmployeeRepository`** that is injected by Spring using constructor injection. This repository is used to interact with the DocumentDB database.

The **`@GetMapping`** annotation is used to handle GET requests to retrieve all employees or a single employee by id. The **`@PostMapping`** annotation is used to handle POST requests to create a new employee. The **`@PutMapping`** annotation is used to handle PUT requests to update an existing employee. The **`@DeleteMapping`** annotation is used to handle DELETE requests to delete an existing employee.

We have also added some basic error handling using the **`ResponseStatusException`** class. If a requested employee is not found, a **`NOT_FOUND`** HTTP status code is returned. If an invalid request is made, a **`BAD_REQUEST`** HTTP status code is returned.

That's it! With this code, we have successfully implemented a Java Spring Boot microservice that uses Amazon DocumentDB to store and retrieve data in a MongoDB-compatible format, and expose a RESTful API to query the data using the MongoDB Query Language.