# Basic CRUD

## CRUDs

CRUD is an acronym that stands for Create, Read, Update, and Delete. It is a set of basic operations commonly used in computer programming and database management systems to manipulate data.

Here's a breakdown of what each operation represents:

1. Create: It refers to the operation of creating new data records or objects in a system. For example, in a database context, it involves adding new entries or rows to a table.

2. Read: This operation involves retrieving or reading existing data from a system. It allows you to access and view data without modifying it. For instance, in a database, reading involves fetching data from a specific table or querying the database for specific information.

3. Update: It refers to modifying or updating existing data in a system. This operation allows you to make changes to the data that already exists. In a database, updating involves altering the values of one or more fields in a table or record.

4. Delete: This operation involves removing or deleting data from a system. It allows you to eliminate unwanted or obsolete data. In a database, deleting involves removing entries or rows from a table.

These four operations collectively form the basic building blocks for data manipulation in various software applications, web services, and databases. They provide a standardized approach for managing data and are widely used in the development of applications that interact with databases or perform data management tasks.

## READ

### GET

To implement a GET operation in Spring Boot that receives an ID and returns an entity through an API, you can follow these steps:

1. Define a REST endpoint in your controller class for the GET operation. Typically, this would be done using the `@GetMapping` annotation and specifying the path, such as `/entities/{id}`.

   ```java
   @GetMapping("/entities/{id}")
   public ResponseEntity<Entity> getEntityById(@PathVariable Long id) {
       // Implementation logic will be added in the next steps
   }
   ```

2. Implement the logic in the controller method to retrieve the entity by its ID. This can be done by injecting a service or repository component and invoking a method to fetch the entity based on the provided ID.

   ```java
   @Autowired
   private EntityService entityService;

   @GetMapping("/entities/{id}")
   public ResponseEntity<Entity> getEntityById(@PathVariable Long id) {
       Entity entity = entityService.getEntityById(id);
       if (entity != null) {
           return ResponseEntity.ok(entity);
       } else {
           return ResponseEntity.notFound().build();
       }
   }
   ```

3. Create a service or repository class to handle the data access and business logic for retrieving the entity by ID. This class can be responsible for interacting with the database or any other data source.

   ```java
   @Service
   public class EntityService {
       @Autowired
       private EntityRepository entityRepository;

       public Entity getEntityById(Long id) {
           return entityRepository.findById(id).orElse(null);
       }
   }
   ```

4. Create a repository interface that extends `JpaRepository` or any other Spring Data repository interface. This will provide the necessary methods to interact with the underlying data source.

   ```java
   public interface EntityRepository extends JpaRepository<Entity, Long> {
   }
   ```

5. Ensure that your entity class is properly defined with annotations such as `@Entity` and `@Id`. This will map it to the corresponding database table and specify the primary key.

   ```java
   @Entity
   public class Entity {
       @Id
       private Long id;
       // Other properties and methods
   }
   ```

With these steps, you can implement a GET operation in Spring Boot that receives an ID through the API endpoint. The entity associated with the provided ID is fetched using a service or repository class, and the response is returned as a `ResponseEntity` with the appropriate status code (`200 OK` if found or `404 Not Found` if not found).

### Pagination

Paginating database query results in an API (Application Programming Interface) is essential for several reasons:

1. Performance Optimization: When dealing with large datasets, retrieving and transmitting all the results in a single API response can be inefficient and slow. By paginating the query results, you can limit the amount of data retrieved and transmitted at once, improving response times and reducing the load on the server and network.

2. Resource Efficiency: Paginating query results allows you to fetch and process data in smaller, manageable chunks. This approach reduces the amount of memory and processing power required by the server, resulting in more efficient resource utilization.

3. Scalability: Paginating results enables your API to scale more effectively. By limiting the number of results per page, you can accommodate a larger number of concurrent users or API consumers without overwhelming your server resources. It ensures that your API remains responsive even during peak usage periods.

4. User Experience: Paginated results enhance the user experience by providing a more efficient and responsive interface. Users can navigate through different pages of results, allowing them to explore and find the specific information they need.

5. Bandwidth Optimization: Transmitting a smaller set of paginated results reduces the amount of data transferred over the network. This optimization is particularly crucial when users are accessing your API on mobile devices with limited bandwidth or in scenarios where data costs are a concern.

6. Compatibility with Client Applications: Many client applications, such as web and mobile applications, are designed to handle paginated results effectively. By providing paginated responses, you align with standard client-side patterns and make it easier for developers to integrate and consume your API.

In summary, paginating database query results in an API improves performance, resource efficiency, scalability, user experience, bandwidth usage, and compatibility with client applications. It is a best practice for handling large datasets and ensuring the optimal functioning of your API.

#### Pageable

In Spring Boot, `Pageable` is an interface provided by Spring Data that facilitates pagination of query results. It allows you to specify the page size, page number, and sorting options for retrieving data from a data source, such as a database, in a paginated manner.

Here are the key aspects of `Pageable`:

1. Page Size: The page size determines the number of items or records to be fetched per page. It is defined using the `size` parameter in the `PageRequest` class.

2. Page Number: The page number specifies the current page to retrieve. It is defined using the `page` parameter in the `PageRequest` class. Page numbering starts from zero.

3. Sorting: `Pageable` supports sorting options to order the results. You can specify the sorting properties and their directions (ascending or descending) using `Sort` object, which can be passed to the `PageRequest` constructor.

   ```java
   // Example of sorting by a specific property in descending order
   Sort sort = Sort.by(Sort.Direction.DESC, "propertyName");
   PageRequest pageable = PageRequest.of(page, size, sort);
   ```

4. Creating `Pageable` Object: To create a `Pageable` object, you typically use the `PageRequest` class, which is an implementation of the `Pageable` interface. You can instantiate it by providing the page number, page size, and optional sorting options.

   ```java
   // Example of creating a Pageable object
   PageRequest pageable = PageRequest.of(page, size);
   ```

5. Using `Page` Object: When executing a query method in Spring Data repositories that returns a collection of results, you can wrap the result set in a `Page` object. The `Page` object contains the actual results along with metadata about the total number of pages, total elements, current page number, and more.

   ```java
   // Example of executing a query method with pagination and receiving a Page object
   Page<MyEntity> page = myRepository.findAll(pageable);
   List<MyEntity> results = page.getContent();
   int totalPages = page.getTotalPages();
   long totalElements = page.getTotalElements();
   // ...
   ```

Using `Pageable` in combination with Spring Data repositories enables you to easily implement pagination in your data access layer. It simplifies the retrieval of paginated data, supports sorting options, and provides valuable metadata about the result set.


#### getPage Basic Example

To serve JSON paginated results to frontend requests using Spring Boot, you can follow these steps:

1. Set up a Spring Boot project: Start by creating a new Spring Boot project or using an existing one. You can set up the project using Spring Initializr or your preferred IDE.

2. Define your data model: Create a model class that represents the data you want to paginate. Annotate the class with appropriate annotations such as `@Entity` if you're using JPA or `@Document` if you're using MongoDB.

3. Create a repository: Set up a repository interface that extends `JpaRepository` or any other Spring Data repository interface. This interface will provide the necessary methods to interact with the database.

4. Implement pagination logic: In your service layer, write a method that performs pagination on the repository. You can use the `Pageable` interface from Spring Data to specify the page size, page number, and sorting options. The method should return a `Page` object containing the paginated data.

   ```java
   import org.springframework.data.domain.Page;
   import org.springframework.data.domain.Pageable;
   // ...

   public Page<MyModel> getPaginatedData(Pageable pageable) {
       return repository.findAll(pageable);
   }
   ```

5. Create a controller: Set up a REST controller that handles the incoming requests from the frontend. Inject the service layer dependency into the controller and define an API endpoint to serve the paginated results.

   ```java
   import org.springframework.data.domain.Page;
   import org.springframework.data.domain.PageRequest;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   // ...

   @RestController
   public class MyController {

       private final MyService myService;

       public MyController(MyService myService) {
           this.myService = myService;
       }

       @GetMapping("/data")
       public Page<MyModel> getPaginatedData(
               @RequestParam(defaultValue = "0") int page,
               @RequestParam(defaultValue = "10") int size
       ) {
           PageRequest pageable = PageRequest.of(page, size);
           return myService.getPaginatedData(pageable);
       }
   }
   ```

6. Configure JSON serialization: Ensure that your Spring Boot project is set up to serialize objects into JSON. Spring Boot uses Jackson as the default JSON serializer, so you generally don't need any additional configuration. However, make sure that the necessary dependencies for Jackson are included in your project's build configuration.

7. Run the application: Start your Spring Boot application, and it will be ready to serve JSON paginated results to frontend requests. Access the API endpoint (`/data` in the example) from your frontend application to retrieve the paginated JSON data.

By following these steps, you can use Spring Boot to serve JSON paginated results to frontend requests. Remember to adjust the code according to your specific requirements and data model.


### getPage Example

Here's another example of how to use `Pageable` to serve JSON responses:

1. Set up the API endpoint: Define a REST endpoint in your controller to handle the API request. The endpoint should include the parameters for page number, page size, and sorting order.

   ```java
   @GetMapping("/data")
   public ResponseEntity<Page<MyEntity>> getData(
           @RequestParam(defaultValue = "0") int page,
           @RequestParam(defaultValue = "10") int size,
           @RequestParam(defaultValue = "asc") String order
   ) {
       Sort.Direction direction = Sort.Direction.fromString(order);
       Pageable pageable = PageRequest.of(page, size, Sort.by(direction, "propertyName"));

       Page<MyEntity> dataPage = dataService.getData(pageable);

       return ResponseEntity.ok().body(dataPage);
   }
   ```

2. Create the `Pageable` object: Inside your controller method, create a `Pageable` object using `PageRequest.of()` and passing in the page number, page size, and sorting options. The sorting options are specified using `Sort.by()` and provide the direction (ascending or descending) and the property to sort by.

3. Retrieve the paginated data: Invoke a service or repository method passing the `Pageable` object as a parameter to retrieve the paginated data. The service/repository method should handle the pagination and sorting using the `Pageable` object.

   ```java
   public Page<MyEntity> getData(Pageable pageable) {
       return myRepository.findAll(pageable);
   }
   ```

4. Return the JSON response: Wrap the paginated data in a `ResponseEntity` and return it from the API endpoint. The `ResponseEntity` allows you to customize the HTTP response status and headers if needed.

   ```java
   return ResponseEntity.ok().body(dataPage);
   ```

5. Invoke the API: Make a GET request to the defined API endpoint, including the desired page number, page size, and sorting order as query parameters. The API will respond with a JSON object containing the paginated data based on the provided parameters.

By following these steps, you can serve JSON responses from your API requests with support for pagination, specifying the page number, page size, and sorting order. This enables clients to retrieve and navigate through paginated data efficiently.


#### getPage Example

To modify the previous example to allow ordering by any field and include a filter in the request, you can make the following adjustments:

1. Update the API endpoint to accept additional parameters for ordering and filtering:

   ```java
   @GetMapping("/data")
   public ResponseEntity<Page<MyEntity>> getData(
           @RequestParam(defaultValue = "0") int page,
           @RequestParam(defaultValue = "10") int size,
           @RequestParam(defaultValue = "asc") String order,
           @RequestParam(required = false) String filterField,
           @RequestParam(required = false) String filterValue
   ) {
       Sort.Direction direction = Sort.Direction.fromString(order);
       Pageable pageable;
       if (filterField != null && filterValue != null) {
           // Apply filtering if filterField and filterValue are provided
           pageable = PageRequest.of(page, size, Sort.by(direction, "propertyName"))
                   .with(PageRequestHelper.createFilter(filterField, filterValue));
       } else {
           pageable = PageRequest.of(page, size, Sort.by(direction, "propertyName"));
       }

       Page<MyEntity> dataPage = dataService.getData(pageable);

       return ResponseEntity.ok().body(dataPage);
   }
   ```

2. Create a `Pageable` object with filtering capability:

   ```java
   public class PageRequestHelper {
       public static Pageable createFilter(String filterField, String filterValue) {
           return new PageableFilter(filterField, filterValue);
       }

       private static class PageableFilter extends PageRequest {
           private final String filterField;
           private final String filterValue;

           public PageableFilter(String filterField, String filterValue) {
               super(0, Integer.MAX_VALUE); // Set a large page size to fetch all data for filtering
               this.filterField = filterField;
               this.filterValue = filterValue;
           }

           @Override
           public <T> T getFilter() {
               // Return a custom filter object or use simple key-value pairs as needed
               Map<String, String> filter = new HashMap<>();
               filter.put(filterField, filterValue);
               return (T) filter;
           }
       }
   }
   ```

3. Implement filtering in your repository or service method based on the provided filter:

   ```java
   public Page<MyEntity> getData(Pageable pageable) {
       // Retrieve the filter object from the pageable and perform filtering
       Map<String, String> filter = pageable.getFilter();
       // Implement filtering logic based on the filter object

       return myRepository.findAll(pageable);
   }
   ```

With these modifications, you can now include an optional filter field and value in your API request. The filtering logic can be implemented in your service or repository method based on the provided filter. Additionally, you can specify the ordering field and order direction in the request to retrieve paginated JSON responses with customized sorting and filtering.

### Create

To implement a create operation in Spring Boot you can follow these steps:

1. Define a REST endpoint in your controller class for the create operation. Typically, this would be done using the `@PostMapping` annotation and specifying the path.

   ```java
   @PostMapping("/entities")
   public ResponseEntity<Entity> createEntity(@RequestBody Entity entity) {
       // Implementation logic will be added in the next steps
   }
   ```

2. Implement the logic in the controller method to create the entity. This can be done by injecting a service or repository component and invoking a method to persist the entity in the database.

   ```java
   @Autowired
   private EntityService entityService;

   @PostMapping("/entities")
   public ResponseEntity<Entity> createEntity(@RequestBody Entity entity) {
       Entity createdEntity = entityService.createEntity(entity);
       return ResponseEntity.status(HttpStatus.CREATED).body(createdEntity);
   }
   ```

3. Create a service or repository class to handle the data access and business logic for creating the entity. This class can be responsible for interacting with the database or any other data source.

   ```java
   @Service
   public class EntityService {
       @Autowired
       private EntityRepository entityRepository;

       public Entity createEntity(Entity entity) {
           return entityRepository.save(entity);
       }
   }
   ```

4. Create a repository interface that extends `JpaRepository` or any other Spring Data repository interface. This will provide the necessary methods to interact with the underlying data source.

   ```java
   public interface EntityRepository extends JpaRepository<Entity, Long> {
   }
   ```

5. Ensure that your entity class is properly defined with annotations such as `@Entity` and `@Id`. This will map it to the corresponding database table and specify the primary key.

   ```java
   @Entity
   public class Entity {
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       // Other properties and methods
   }
   ```

6. Configure the serialization and deserialization of JSON in your Spring Boot application. By default, Spring Boot uses Jackson as the JSON serializer and deserializer, so you generally don't need any additional configuration.

With these steps, you can implement a create operation in Spring Boot that receives an entity JSON from the frontend. The entity is persisted in the database using a service or repository class, and the response is returned as a `ResponseEntity` with the appropriate status code (`201 Created`) and the created entity in the response body.

### Update

Here's an example of a Spring Boot application that handle the update operation:

1. Set up your Spring Boot project: Create a new Spring Boot project or use an existing one. Make sure you have the necessary dependencies for Spring Data JPA and a database driver in your project's build file (e.g., `pom.xml` for Maven or `build.gradle` for Gradle).

2. Define your entity class: Create a Java class that represents the entity you want to store in the database. Annotate it with `@Entity` to mark it as a persistent entity, and use appropriate annotations (`@Id`, `@Column`, etc.) to define the fields and their mappings to the database columns.

3. Create a repository interface: Define a repository interface that extends `JpaRepository` or another suitable repository interface provided by Spring Data JPA. This interface will provide basic CRUD operations for your entity.

4. Create a service class: Create a service class that will handle the update operation. This class should be annotated with `@Service` to mark it as a service component in Spring.

5. Autowire the repository in the service: In your service class, inject the repository instance by using the `@Autowired` annotation. This allows you to interact with the database through the repository methods.

6. Implement the update operation in the service: Create a method in the service class that handles the update operation. This method should receive the entity JSON from the frontend as a parameter.

7. Deserialize the JSON: Use a JSON deserialization library (e.g., Jackson) to convert the received JSON into an instance of your entity class.

8. Fetch the existing entity: Retrieve the existing entity from the database using the identifier or any other unique identifier that you can extract from the JSON. You can use the repository methods for this purpose.

9. Update the entity: Update the fields of the existing entity with the values from the received JSON entity.

10. Save the updated entity: Use the repository interface to save the updated entity to the database.

11. Create a controller: Create a controller class that handles the HTTP requests. Inject the service instance into the controller using the `@Autowired` annotation.

12. Implement the API endpoint: Define an API endpoint in the controller that receives the entity JSON from the frontend and calls the service method to update the database.

Here's an example implementation:

```java
@Entity
public class Entity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String description;

    // Getters and setters, constructors
}

@Repository
public interface EntityRepository extends JpaRepository<Entity, Long> {
}

@Service
public class EntityService {
    private final EntityRepository entityRepository;

    public EntityService(EntityRepository entityRepository) {
        this.entityRepository = entityRepository;
    }

    public void updateEntity(EntityJson entityJson) {
        Entity existingEntity = entityRepository.findById(entityJson.getId())
            .orElseThrow(() -> new EntityNotFoundException("Entity not found"));

        existingEntity.setName(entityJson.getName());
        existingEntity.setDescription(entityJson.getDescription());
        // Update other fields as needed

        entityRepository.save(existingEntity);
    }
}

@RestController
@RequestMapping("/api")
public class EntityController {
    private final EntityService entityService;

    public EntityController(EntityService entityService) {
        this.entityService = entityService;
    }

    @PostMapping("/updateEntity")
    public ResponseEntity<String> updateEntity(@RequestBody EntityJson entityJson) {
        entityService.updateEntity(entityJson);
        return ResponseEntity.ok("Entity updated successfully");
    }
}
```

In this example, the application consists of an

 entity class (`Entity`), a repository interface (`EntityRepository`), a service class (`EntityService`), and a controller class (`EntityController`). The controller exposes an API endpoint (`/api/updateEntity`) that receives the entity JSON from the frontend and calls the service method (`updateEntity`) to update the entity in the database.

Make sure to include the necessary configuration for your database connection in the `application.properties` file or any other relevant configuration file in your Spring Boot project.

Remember to handle exceptions, perform validation, and add appropriate error handling as per your application requirements.
