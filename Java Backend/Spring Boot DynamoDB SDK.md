# Design and implement a Java Spring Boot microservice that uses Amazon DynamoDB to store and retrieve data. The service should expose a RESTful API to create, read, update, and delete records, and use the DynamoDB SDK to interact with the database.

Step 1: Create a DynamoDB Table
The first step is to create a DynamoDB table to store the data. You can create a table using the AWS Console or programmatically using the DynamoDB SDK.

Here's an example of how to create a DynamoDB table using the AWS SDK for Java:

```java
import com.amazonaws.services.dynamodbv2.AmazonDynamoDB;
import com.amazonaws.services.dynamodbv2.AmazonDynamoDBClientBuilder;
import com.amazonaws.services.dynamodbv2.model.*;

public class DynamoDBTableCreator {

    public static void createTable(String tableName) {
        AmazonDynamoDB client = AmazonDynamoDBClientBuilder.standard().build();

        CreateTableRequest request = new CreateTableRequest()
                .withAttributeDefinitions(new AttributeDefinition("id", ScalarAttributeType.S))
                .withKeySchema(new KeySchemaElement("id", KeyType.HASH))
                .withBillingMode(BillingMode.PAY_PER_REQUEST)
                .withTableName(tableName);

        client.createTable(request);
    }
}
```

Step 2: Define the Data Model
The next step is to define the data model for the DynamoDB table. You can use the DynamoDB SDK to map the Java object to the DynamoDB table.

Here's an example of how to define a data model using the DynamoDB SDK:

```java
import com.amazonaws.services.dynamodbv2.datamodeling.*;

@DynamoDBTable(tableName = "example-table")
public class ExampleItem {

    private String id;
    private String name;

    @DynamoDBHashKey(attributeName = "id")
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }

    @DynamoDBAttribute(attributeName = "name")
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

}
```

Step 3: Implement CRUD Operations
The next step is to implement CRUD operations for the DynamoDB table. You can use the DynamoDB SDK to interact with the DynamoDB table.

Here's an example of how to implement CRUD operations using the DynamoDB SDK:

```java
import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.amazonaws.services.dynamodbv2.AmazonDynamoDB;
import com.amazonaws.services.dynamodbv2.datamodeling.DynamoDBMapper;
import com.amazonaws.services.dynamodbv2.datamodeling.DynamoDBScanExpression;

@Repository
public class DynamoDBRepository {

    @Autowired
    private AmazonDynamoDB amazonDynamoDB;

    public <T> void save(T item) {
        DynamoDBMapper mapper = new DynamoDBMapper(amazonDynamoDB);
        mapper.save(item);
    }

    public <T> Optional<T> findById(String id, Class<T> clazz) {
        DynamoDBMapper mapper = new DynamoDBMapper(amazonDynamoDB);
        return Optional.ofNullable(mapper.load(clazz, id));
    }

    public <T> List<T> findAll(Class<T> clazz) {
        DynamoDBMapper mapper = new DynamoDBMapper(amazonDynamoDB);
        DynamoDBScanExpression scanExpression = new DynamoDBScanExpression();
        return mapper.scan(clazz, scanExpression).stream().collect(Collectors.toList());
    }

    public <T> void delete(String id, Class<T> clazz) {
        DynamoDBMapper mapper = new DynamoDBMapper(amazonDynamoDB);
        mapper.delete(mapper.load(clazz, id));
    }
}
```

And here's an example controller that uses the **`DynamoDBRepository`** to handle CRUD operations:

```java
import java.util.List;
import java.util.Optional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private DynamoDBRepository repository;

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        repository.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable("id") String id) {
        Optional<User> optionalUser = repository.findById(id, User.class);
        if (optionalUser.isPresent()) {
            return ResponseEntity.ok(optionalUser.get());
        } else {
            return ResponseEntity.notFound().build();
        }
    }

    @GetMapping
    public List<User> getAllUsers() {
        return repository.findAll(User.class);
    }

    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable("id") String id, @RequestBody User user) {
        Optional<User> optionalUser = repository.findById(id, User.class);
        if (optionalUser.isPresent()) {
            user.setId(id);
            repository.save(user);
            return ResponseEntity.ok(user);
        } else {
            return ResponseEntity.notFound().build();
        }
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable("id") String id) {
        Optional<User> optionalUser = repository.findById(id, User.class);
        if (optionalUser.isPresent()) {
            repository.delete(id, User.class);
            return ResponseEntity.noContent().build();
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}
```

This example controller exposes a RESTful API for CRUD operations on a **`User`** entity, and uses the **`DynamoDBRepository`** to interact with the database. Note that the **`@RequestBody`** and **`@PathVariable`** annotations are used to map HTTP request data to Java objects, and the **`ResponseEntity`** class is used to provide control over the HTTP response status and body.