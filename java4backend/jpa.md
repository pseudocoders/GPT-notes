# JPA

JPA stands for Java Persistence API. It is a Java specification that provides a programming interface for managing relational data in Java applications. JPA is part of the Java EE (Enterprise Edition) and Jakarta EE (formerly Java EE) specifications and is commonly used in Java-based enterprise applications.

JPA provides a set of APIs and annotations that allow developers to map Java objects to relational database tables and perform CRUD (Create, Read, Update, Delete) operations on those objects. It acts as a bridge between the object-oriented world of Java and the relational world of databases.

The main features and concepts of JPA include:

1. Object-Relational Mapping (ORM): JPA allows you to define mappings between Java classes and database tables, specifying how fields and relationships in Java objects correspond to columns and tables in the database.

2. Entity Classes: In JPA, entity classes represent the objects that you want to persist in the database. These classes are annotated with JPA annotations to define their mappings, relationships, and other metadata.

3. Persistence Unit: A persistence unit is a configuration file that specifies the persistence settings for the application. It includes information such as the database connection details, entity classes, and transaction settings.

4. EntityManager: The EntityManager is the main interface provided by JPA for performing database operations. It allows you to create, read, update, and delete entities, as well as perform queries and manage transactions.

5. Query Language: JPA provides a powerful query language called JPQL (Java Persistence Query Language), which is similar to SQL but operates on entity objects rather than database tables. JPQL allows you to express complex queries and retrieve data from the database based on various criteria.

JPA implementations, such as Hibernate, EclipseLink, and OpenJPA, provide the underlying functionality to implement the JPA specification. These implementations handle the translation of JPA operations into database-specific queries and interact with the database to persist and retrieve data.

Overall, JPA simplifies the process of working with relational databases in Java applications by providing a standardized and object-oriented approach to data persistence. It promotes code reusability, portability, and abstraction from specific database technologies, making it easier to develop and maintain database-driven applications.

## JPA in a Spring Boot project

To use JPA in a Spring Boot application, you need to follow these steps:

Step 1: Set up a Spring Boot project
Start by creating a new Spring Boot project using your preferred IDE or the Spring Initializr website (https://start.spring.io/). Make sure to include the necessary dependencies for JPA and your desired database (e.g., H2, MySQL, PostgreSQL, etc.).

Step 2: Define Entity Classes
Create the entity classes that represent your data model. An entity class is a POJO (Plain Old Java Object) annotated with JPA annotations to define its mappings and relationships. For example, consider a "User" entity class:

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;

    // Getters and setters

}
```

Step 3: Create a Repository Interface
Next, define a repository interface that extends the `JpaRepository` interface or one of its sub-interfaces. This interface will provide CRUD operations and other querying capabilities for your entity class. For example, a `UserRepository` interface:

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    // Additional query methods can be declared here

}
```

Step 4: Configure Database Properties
In your application.properties or application.yml file, configure the database connection properties such as the URL, username, password, and dialect specific to your database provider.

Step 5: Use JPA in your Service or Controller
Now, you can use JPA in your service classes or controllers by autowiring the repository interfaces and utilizing their methods. For example:

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    // Other methods

}
```

Step 6: Run and Test the Application
Finally, run your Spring Boot application, and you can test the JPA functionality using API endpoints or by writing test cases.

Spring Boot will automatically configure the JPA implementation (e.g., Hibernate) based on the dependencies and database properties you provided. It will handle the entity-to-table mapping, database connections, and transaction management.

Remember to handle exceptions, manage transactions, and handle any additional configurations specific to your application's requirements.

That's it! You have now integrated JPA into your Spring Boot application and can leverage its features for database operations.


## JPA repository methods

The `JpaRepository` interface provides a set of predefined methods that offer common CRUD (Create, Read, Update, Delete) operations and querying capabilities for JPA entities. This interface extends the `PagingAndSortingRepository` and `CrudRepository` interfaces, providing additional functionality. Here are some of the commonly used methods of `JpaRepository`:

1. `save(entity)`: Saves the given entity to the database. It can be used for both saving new entities and updating existing ones.

2. `saveAll(entities)`: Saves all the entities in the given iterable to the database.

3. `findById(id)`: Retrieves an entity by its primary key.

4. `existsById(id)`: Checks if an entity with the given primary key exists in the database.

5. `findAll()`: Retrieves all entities from the corresponding table.

6. `findAllById(ids)`: Retrieves entities with the given primary keys.

7. `count()`: Returns the total number of entities in the table.

8. `delete(entity)`: Deletes the given entity from the database.

9. `deleteById(id)`: Deletes the entity with the given primary key.

10. `deleteAll()`: Deletes all entities from the corresponding table.

11. `findAll(Pageable pageable)`: Retrieves entities in a paginated manner, based on the provided `Pageable` object. This method returns a `Page` object containing the entities and pagination information.

12. `findAll(Sort sort)`: Retrieves entities and sorts them based on the provided `Sort` object.

13. `flush()`: Flushes pending changes to the database.

14. `refresh(entity)`: Refreshes the state of the given entity from the database.

15. `findAll(Specification<T> spec)`: Retrieves entities that match the given `Specification`. Specifications are used to build complex queries using criteria-based predicates.

16. `findAll(Example<T> example)`: Retrieves entities that match the given example entity. Example queries use the properties of the example entity to build a query that matches similar entities in the database.

17. `findAll(Example<T> example, Sort sort)`: Retrieves entities that match the given example entity and sorts them based on the provided `Sort` object.

18. `findAll(Example<T> example, Pageable pageable)`: Retrieves entities that match the given example entity in a paginated manner, based on the provided `Pageable` object.

19. `findAll(Sort sort)`: Retrieves entities and sorts them based on the provided `Sort` object.

20. `findAllById(Iterable<ID> ids)`: Retrieves entities with the given primary keys in an iterable.

21. `findOne(Specification<T> spec)`: Retrieves a single entity that matches the given `Specification`.

22. `deleteAllInBatch()`: Deletes all entities in a batch. This method is more efficient for bulk deletions than individually deleting each entity.

23. `deleteInBatch(entities)`: Deletes the given entities in a batch. Similar to `deleteAllInBatch()`, this method is useful for bulk deletions.

24. `getOne(id)`: Returns a reference to the entity with the given primary key without actually fetching it from the database. This is useful for lazy loading or when you only need the entity's identifier.

These methods, provide a wide range of functionality for interacting with JPA entities in a Spring Boot application. By utilizing these methods, you can perform CRUD operations, execute queries, apply sorting and pagination, and manage entity lifecycle effectively.

Additionally, you can also define custom query methods using method naming conventions or by annotating methods with `@Query` to execute more complex queries using JPQL or native SQL.

By extending `JpaRepository` in your repository interface, you inherit these methods, and Spring Data JPA generates the necessary implementation code at runtime based on the entity type and its associated metadata.

It's important to note that you can also define additional methods in your repository interface to cover more specific querying needs or to execute custom database operations.

## Examples

Sure! Here are examples for each of the mentioned `JpaRepository` methods:

1. `save(entity)`:
```java
User user = new User("John Doe", "john@example.com");
userRepository.save(user);
```
This example creates a new `User` object and saves it to the database using the `save()` method. If the `User` object already exists in the database, it will be updated with the provided values.

2. `saveAll(entities)`:
```java
List<User> users = Arrays.asList(
    new User("John Doe", "john@example.com"),
    new User("Jane Smith", "jane@example.com")
);
userRepository.saveAll(users);
```
Here, a list of `User` objects is created, and `saveAll()` is used to save all the users to the database.

3. `findById(id)`:
```java
Optional<User> optionalUser = userRepository.findById(1L);
if (optionalUser.isPresent()) {
    User user = optionalUser.get();
    System.out.println(user.getName());
} else {
    System.out.println("User not found");
}
```
This example retrieves a `User` entity with the primary key `1L` from the database using `findById()`. It uses `Optional` to handle the case where the user might not exist.

4. `existsById(id)`:
```java
boolean exists = userRepository.existsById(1L);
if (exists) {
    System.out.println("User exists");
} else {
    System.out.println("User does not exist");
}
```
Here, `existsById()` is used to check if a `User` entity with the primary key `1L` exists in the database.

5. `findAll()`:
```java
List<User> users = userRepository.findAll();
for (User user : users) {
    System.out.println(user.getName());
}
```
This example retrieves all `User` entities from the corresponding table using `findAll()`. It then iterates over the list of users and prints their names.

6. `findAllById(ids)`:
```java
List<Long> userIds = Arrays.asList(1L, 2L, 3L);
List<User> users = userRepository.findAllById(userIds);
for (User user : users) {
    System.out.println(user.getName());
}
```
In this example, the `findAllById()` method is used to retrieve `User` entities with the primary keys 1L, 2L, and 3L from the database. The resulting list of users is then iterated over and their names are printed.

7. `count()`:
```java
long userCount = userRepository.count();
System.out.println("Total users: " + userCount);
```
Here, the `count()` method is used to get the total number of `User` entities present in the table. The count is then printed to the console.

8. `delete(entity)`:
```java
User user = userRepository.findById(1L).orElseThrow();
userRepository.delete(user);
```
In this example, the `delete()` method is used to delete a specific `User` entity from the database. The entity is retrieved by its primary key using `findById()` and then deleted using `delete()`.

9. `deleteById(id)`:
```java
userRepository.deleteById(1L);
```
This example shows how to delete a `User` entity with a specific primary key using the `deleteById()` method.

10. `deleteAll()`:
```java
userRepository.deleteAll();
```
In this example, the `deleteAll()` method is used to delete all the `User` entities from the corresponding table.

11. `findAll(Pageable pageable)`:
```java
Pageable pageable = PageRequest.of(0, 10); // Retrieves the first 10 entities
Page<User> userPage = userRepository.findAll(pageable);
List<User> users = userPage.getContent();
for (User user : users) {
    System.out.println(user.getName());
}
System.out.println("Total Pages: " + userPage.getTotalPages());
```
In this example, the `findAll()` method is used with a `Pageable` object to retrieve a specific number of `User` entities in a paginated manner. The first 10 users are retrieved using `PageRequest.of(0, 10)`, and their names are printed. Additionally, the total number of pages is printed.

12. `findAll(Sort sort)`:
```java
Sort sort = Sort.by(Sort.Direction.ASC, "name"); // Sorts users by name in ascending order
List<User> users = userRepository.findAll(sort);
for (User user : users) {
    System.out.println(user.getName());
}
```
In this example, the `findAll()` method is used with a `Sort` object to retrieve `User` entities from the database and sort them by the "name" property in ascending order.

13. `flush()`:
```java
User user = userRepository.findById(1L).orElseThrow();
user.setEmail("newemail@example.com");
userRepository.flush();
```
Here, the `flush()` method is used to synchronize pending changes made to a `User` entity with the database. In this example, the email of a user with a specific ID is updated, and `flush()` is called to immediately persist the change.

14. `refresh(entity)`:
```java
User user = userRepository.findById(1L).orElseThrow();
user.setEmail("newemail@example.com");
userRepository.refresh(user);
System.out.println("Refreshed email: " + user.getEmail());
```
In this example, the `refresh()` method is used to refresh the state of a `User` entity from the database. After updating the email, `refresh()` is called to reload the entity's state from the database, ensuring that the email matches the persisted value.

15. `findAll(Specification<T> spec)`:
```java
Specification<User> spec = (root, query, criteriaBuilder) ->
    criteriaBuilder.equal(root.get("name"), "John Doe");
List<User> users = userRepository.findAll(spec);
for (User user : users) {
    System.out.println(user.getName());
}
```
Here, a `Specification` is used to build a complex query that retrieves `User` entities with a specific name. The `findAll()` method is invoked with the specification, and the matching users are printed.

16. `findAll(Example<T> example)`:
```java
User exampleUser = new User();
exampleUser.setName("John");
Example<User> example = Example.of(exampleUser);
List<User> users = userRepository.findAll(example);
for (User user : users) {
    System.out.println(user.getName());
}
```
In this example, an example `User` object is created with a specific name. The `findAll()` method is then used with the example object to retrieve all `User` entities that match the provided example.

17. `findAll(Example<T> example, Sort sort)`:
```java
User exampleUser = new User();
exampleUser.setName("John");
Example<User> example = Example.of(exampleUser);
Sort sort = Sort.by(Sort.Direction.DESC, "name");
List<User> users = userRepository.findAll(example, sort);
for (User user : users) {
    System.out.println(user.getName());
}
```
In this example, an example `User` object is created with a specific name. The `findAll()` method is used with the example object and a `Sort` object to retrieve `User` entities that match the example and sort them by name in descending order.

18. `findAll(Example<T> example, Pageable pageable)`:
```java
User exampleUser = new User();
exampleUser.setName("John");
Example<User> example = Example.of(exampleUser);
Pageable pageable = PageRequest.of(0, 10);
Page<User> userPage = userRepository.findAll(example, pageable);
List<User> users = userPage.getContent();
for (User user : users) {
    System.out.println(user.getName());
}
System.out.println("Total Pages: " + userPage.getTotalPages());
```
Here, an example `User` object is created with a specific name. The `findAll()` method is used with the example object and a `Pageable` object to retrieve a specific number of `User` entities that match the example in a paginated manner. The first 10 users are retrieved, their names are printed, and the total number of pages is displayed.

19. `findAll(Sort sort)`:
```java
Sort sort = Sort.by(Sort.Direction.ASC, "name");
List<User> users = userRepository.findAll(sort);
for (User user : users) {
    System.out.println(user.getName());
}
```
This example retrieves all `User` entities from the database and sorts them by name in ascending order using the `findAll()` method with a `Sort` object.

20. `findAllById(Iterable<ID> ids)`:
```java
List<Long> userIds = Arrays.asList(1L, 2L, 3L);
List<User> users = userRepository.findAllById(userIds);
for (User user : users) {
    System.out.println(user.getName());
}
```
In this example, the `findAllById()` method is used to retrieve `User` entities with the primary keys 1L, 2L, and 3L from the database. The resulting list of users is then iterated over, and their names are printed.

21. `findOne(Specification<T> spec)`:
```java
Specification<User> spec = (root, query, criteriaBuilder) ->
    criteriaBuilder.equal(root.get("name"), "John Doe");
User user = userRepository.findOne(spec).orElse(null);
if (user != null) {
    System.out.println(user.getName());
} else {
    System.out.println("User not found");
}
```
Here, a `Specification` is used to build a complex query that retrieves a `User` entity with a specific name. The `findOne()` method is invoked with the specification, and the matching user is printed if found.

22. `deleteAllInBatch()`:
```java
userRepository.deleteAllInBatch();
```
In this example, the `deleteAllInBatch()` method is used to delete all entities in a batch. This method is more efficient for bulk deletions than individually deleting each entity.

23. `deleteInBatch(entities)`:
```java
List<User> users = userRepository.findAll();
userRepository.deleteInBatch(users);
```
Here, the `deleteInBatch()` method is used to delete multiple `User` entities in a batch

24. `getOne(id)`:
```java
User user = userRepository.getOne(1L);
System.out.println(user.getName());
```
In this example, the `getOne()` method is used to obtain a reference to the `User` entity with the primary key `1L` without actually fetching it from the database. It returns a proxy object representing the entity, which can be used for lazy loading or when only the entity's identifier is required. In this case, the name of the user is printed.

Please note that starting from Spring Data JPA 2.3, `getOne(id)` has been deprecated. It is recommended to use `findById(id)` instead, which returns an `Optional` object containing the entity.

## Custom query methods using method naming conventions in JpaRepository

In Spring Data JPA, you can define custom query methods in your `JpaRepository` interface by following the method naming conventions. The framework will automatically generate the appropriate query based on the method name. This approach allows you to create queries without writing explicit SQL or JPQL statements. Here's how it works:

1. Define the Method Signature:
   - Start the method name with one of the supported prefixes: `findBy`, `getBy`, `queryBy`, `readBy`, or `countBy`.
   - Follow the prefix with the property name on which you want to apply the filtering or sorting.
   - Optionally, add additional expressions to refine the query using keywords such as `And`, `Or`, `Between`, `LessThan`, `GreaterThan`, `OrderBy`, etc.
   - The method can have additional parameters that represent the values to be used for filtering.

2. Spring Data JPA Query Generation:
   - Spring Data JPA analyzes the method name and converts it into a query.
   - It derives the entity type from the `JpaRepository` interface's type parameter and maps the method name to properties of the entity.
   - By default, it performs case-insensitive matching for property names.

Here are a few examples to illustrate the usage of custom query methods using method naming conventions:

1. Finding users by their name:
```java
List<User> findByName(String name);
```

2. Finding users by age less than a specified value:
```java
List<User> findByAgeLessThan(int age);
```

3. Finding users whose name starts with a specific string and have an email address:
```java
List<User> findByNameStartingWithAndEmailIsNotNull(String prefix);
```

4. Counting users by their status:
```java
long countByStatus(UserStatus status);
```

5. Finding users with a specific name and age:
```java
List<User> findByNameAndAge(String name, int age);
```

These are just a few examples to give you an idea of how to use custom query methods using method naming conventions. You can create more complex queries by combining multiple property names and expressions in the method name. However, if you need more control over the query generation or want to write complex queries, you can also use `@Query` annotations with JPQL or native SQL queries.

### Prefixes

Here is a list of the supported prefixes for custom query methods using method naming conventions in `JpaRepository`:

1. `findBy`: Used to retrieve entities based on specific criteria.
2. `getBy`: Similar to `findBy`, used to retrieve entities based on specific criteria.
3. `queryBy`: Similar to `findBy`, used to retrieve entities based on specific criteria.
4. `readBy`: Similar to `findBy`, used to retrieve entities based on specific criteria.
5. `countBy`: Used to count entities based on specific criteria.
6. `deleteBy`: Used to delete entities based on specific criteria.
7. `removeBy`: Similar to `deleteBy`, used to delete entities based on specific criteria.

These prefixes are the commonly used ones in Spring Data JPA for defining custom query methods. You can choose the appropriate prefix based on the operation you want to perform, such as retrieving entities, counting entities, or deleting entities. Combine these prefixes with property names and additional expressions to create meaningful method names that map to the desired query functionality.

### Keywords

Here is a list of keywords that can be used to refine the query when using prefixes for custom query methods using method naming conventions in `JpaRepository`:

1. `And`: Used to combine multiple criteria with the logical AND operator.
2. `Or`: Used to combine multiple criteria with the logical OR operator.
3. `Between`: Used to specify a range for a property.
4. `LessThan`: Used to specify a less than comparison for a property.
5. `LessThanEqual`: Used to specify a less than or equal to comparison for a property.
6. `GreaterThan`: Used to specify a greater than comparison for a property.
7. `GreaterThanEqual`: Used to specify a greater than or equal to comparison for a property.
8. `IsNull`: Used to check if a property is null.
9. `IsNotNull`: Used to check if a property is not null.
10. `Like`: Used to perform a case-insensitive pattern matching on a property.
11. `NotLike`: Used to perform a case-insensitive negated pattern matching on a property.
12. `StartingWith`: Used to perform a case-insensitive prefix matching on a property.
13. `EndingWith`: Used to perform a case-insensitive suffix matching on a property.
14. `Containing`: Used to perform a case-insensitive substring matching on a property.
15. `OrderBy`: Used to specify the sorting order for the result.

These keywords can be used in combination with the supported prefixes and property names to create more specific and refined custom query methods. They allow you to add additional criteria and control the behavior of the query.

##  Examples of annotating JpaRepository methods with `@Query` to execute queries using JPQL or native SQL

Certainly! Here's a comprehensive example demonstrating the usage of `@Query` annotation to execute more complex queries using JPQL in a `JpaRepository`:

Assume we have an entity class called `Product` representing products in an e-commerce system with the following attributes: `id`, `name`, `price`, and `category`.

```java
@Entity
@Table(name = "products")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    
    private double price;
    
    private String category;
    
    // Constructors, getters, and setters
}
```

#### JPQL

And here's an example `JpaRepository` interface called `ProductRepository` with custom query methods using `@Query` annotation:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Long> {

    @Query("SELECT p FROM Product p WHERE p.price > :minPrice AND p.category = :category")
    List<Product> findExpensiveProductsInCategory(double minPrice, String category);

    @Query("SELECT p.category, COUNT(p) FROM Product p GROUP BY p.category")
    List<Object[]> countProductsByCategory();

    @Query("SELECT p.name FROM Product p WHERE p.name LIKE %:keyword%")
    List<String> searchProductByName(String keyword);
}
```

Let's break down the example:

1. `findExpensiveProductsInCategory`: This method demonstrates a parameterized JPQL query using `@Query` annotation. It selects all products with a price greater than `minPrice` in a specific category. The `:minPrice` and `:category` are named parameters that are mapped to the method parameters.

2. `countProductsByCategory`: This method showcases a group by query using `@Query` annotation. It selects the product category and the count of products in each category. The result is a list of `Object[]` arrays where each array contains the category and count.

3. `searchProductByName`: This method demonstrates a parameterized JPQL query with a `LIKE` clause using `@Query` annotation. It selects the names of products that match the provided keyword. The `%:keyword%` is a named parameter that performs a case-insensitive substring match.

By using `@Query` annotation, you can write complex JPQL queries and take advantage of the full expressive power of the JPQL language.

#### Native SQL

And here's an example `JpaRepository` interface called `ProductRepository` with custom query methods using `@Query` annotation to execute native SQL queries:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Long> {

    @Query(value = "SELECT * FROM products WHERE price > ?1 AND category = ?2", nativeQuery = true)
    List<Product> findExpensiveProductsInCategory(double minPrice, String category);

    @Query(value = "SELECT category, COUNT(*) FROM products GROUP BY category", nativeQuery = true)
    List<Object[]> countProductsByCategory();

    @Query(value = "SELECT name FROM products WHERE name LIKE %?1%", nativeQuery = true)
    List<String> searchProductByName(String keyword);
}
```

Let's break down the example:

1. `findExpensiveProductsInCategory`: This method demonstrates a parameterized native SQL query using `@Query` annotation. It selects all products with a price greater than `minPrice` in a specific category. The `?1` and `?2` are positional parameters that are mapped to the method parameters.

2. `countProductsByCategory`: This method showcases a group by query using `@Query` annotation. It selects the product category and the count of products in each category using native SQL. The result is a list of `Object[]` arrays where each array contains the category and count.

3. `searchProductByName`: This method demonstrates a parameterized native SQL query with a `LIKE` clause using `@Query` annotation. It selects the names of products that match the provided keyword using native SQL. The `%?1%` is a positional parameter that performs a case-insensitive substring match.

By using `@Query` annotation with `nativeQuery = true`, you can write complex native SQL queries and execute them directly. However, be cautious when using native SQL queries as they are specific to the database implementation and might not be portable across different database systems.

#### Native SQL with pagination

And here's an example `JpaRepository` interface called `ProductRepository` with a custom query method using `@Query` annotation to execute a native SQL query with pagination:

```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Long> {

    @Query(value = "SELECT * FROM products WHERE category = ?1",
            countQuery = "SELECT COUNT(*) FROM products WHERE category = ?1",
            nativeQuery = true)
    Page<Product> findByCategoryWithPagination(String category, Pageable pageable);
}
```

Let's break down the example:

1. `findByCategoryWithPagination`: This method demonstrates a native SQL query with pagination using `@Query` annotation. It selects all products in a specific category and returns the result as a `Page<Product>`. The native SQL query includes the `SELECT` statement to fetch the desired rows from the table. The `countQuery` is used to retrieve the total count of matching records for pagination. The `?1` is a positional parameter that is mapped to the `category` parameter of the method. The `Pageable` parameter is used to specify the pagination details such as page size, page number, and sorting.

By using `@Query` annotation with `nativeQuery = true` and providing the appropriate `countQuery` for pagination, you can execute complex native SQL queries and return the results with pagination using Spring Data JPA.

Please note that when executing native SQL queries with pagination, you need to provide a separate `countQuery` to retrieve the total count of matching records for proper pagination support.


