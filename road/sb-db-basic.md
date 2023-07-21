# Basic database operations

## Database

Let's create a simple schema with two tables: `products` and `product_types`.

1. Product Types Table:

The `product_types` table will store different types of products. It will have two columns: `id` (primary key) and `name`.

| id | name      |
|----|-----------|
| 1  | Electronics |
| 2  | Clothing   |
| 3  | Books      |
| ...| ...        |

2. Products Table:

The `products` table will store individual products and their information. It will have the following columns: `id` (primary key), `name`, `price`, `description`, and `type_id` (foreign key referencing `product_types`).

| id | name      | price  | description                      | type_id |
|----|-----------|--------|----------------------------------|---------|
| 1  | Laptop    | 1000   | High-performance laptop          | 1       |
| 2  | T-shirt   | 20     | Cotton T-shirt with logo print   | 2       |
| 3  | Book      | 15     | Bestseller novel                 | 3       |
| 4  | Smartphone| 800    | Latest smartphone model          | 1       |
| ...| ...       | ...    | ...                              | ...     |

In this example, the `type_id` column in the `products` table references the `id` column in the `product_types` table, establishing a relationship between the products and their corresponding types.

With this schema, you can store various product types in the `product_types` table (e.g., Electronics, Clothing, Books), and each product in the `products` table will be associated with a specific product type.

Keep in mind that this is just a basic example, and in a real-world scenario, you might have additional columns and more complex relationships depending on your application's requirements. Additionally, you'll need to set up the appropriate data access layer (such as Spring Data JPA) to interact with this database from your Spring Boot application.

### SQL

Here's the SQL code to create the tables for the products and product types as described in the previous example:

1. Create the `product_types` table:

```sql
CREATE TABLE product_types (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

INSERT INTO product_types (id, name) VALUES
    (1, 'Electronics'),
    (2, 'Clothing'),
    (3, 'Books');
-- Add more product types as needed
```

2. Create the `products` table:

```sql
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    description TEXT,
    type_id INT NOT NULL,
    FOREIGN KEY (type_id) REFERENCES product_types(id)
);

INSERT INTO products (id, name, price, description, type_id) VALUES
    (1, 'Laptop', 1000.00, 'High-performance laptop', 1),
    (2, 'T-shirt', 20.00, 'Cotton T-shirt with logo print', 2),
    (3, 'Book', 15.00, 'Bestseller novel', 3),
    (4, 'Smartphone', 800.00, 'Latest smartphone model', 1);
-- Add more products as needed
```

In this SQL code, we first create the `product_types` table with the appropriate columns for the product types. We then insert some initial data into the `product_types` table.

Next, we create the `products` table with the necessary columns to store product information, including the foreign key `type_id` that references the `id` column of the `product_types` table. We also insert some sample products into the `products` table.

Please note that the specific syntax of the SQL code may vary depending on the database management system (e.g., MySQL, PostgreSQL, SQL Server) you are using. The code provided above is compatible with most relational database systems, but you may need to adjust it slightly for some specific databases.

### Entities

Here are the entity classes for the Spring Boot project to map the tables described earlier:

1. ProductType Entity:

```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "product_types")
public class ProductType {

    @Id
    private int id;
    private String name;

    // Getters and Setters, Constructors, and other methods as needed
    
    // No-args constructor
    public ProductType() {}

    // Constructor with parameters
    public ProductType(int id, String name) {
        this.id = id;
        this.name = name;
    }

    // Getters and Setters
    // ...
}
```

2. Product Entity:

```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;
import java.math.BigDecimal;

@Entity
@Table(name = "products")
public class Product {

    @Id
    private int id;
    private String name;
    private BigDecimal price;
    private String description;

    @ManyToOne
    @JoinColumn(name = "type_id")
    private ProductType productType;

    // Getters and Setters, Constructors, and other methods as needed

    // No-args constructor
    public Product() {}

    // Constructor with parameters
    public Product(int id, String name, BigDecimal price, String description, ProductType productType) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.description = description;
        this.productType = productType;
    }

    // Getters and Setters
    // ...
}
```

In these entity classes, we use JPA annotations to map the classes to the corresponding database tables. The `@Entity` annotation marks the classes as JPA entities, and the `@Table` annotation specifies the name of the table that the entity represents.

In the `Product` entity, we use the `@ManyToOne` annotation to represent the relationship between `Product` and `ProductType`. This indicates that many `Product` entities can be associated with one `ProductType` entity. The `@JoinColumn` annotation is used to specify the column in the `products` table that corresponds to the foreign key reference to the `product_types` table.

Remember that you'll also need to set up the database configuration and Spring Data JPA repositories to make these entities interact with the database in your Spring Boot project.

## Reading an Entity from id

Below is an example of how to read one entity from its ID using Spring Boot and Spring Data JPA.

1. Create the entity classes as mentioned earlier (Product and ProductType) and set up their respective repositories.

2. Assuming you have your Spring Boot project set up with the necessary dependencies and the database configured, let's create a service class to read a product by its ID:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.Optional;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public Product getProductById(int id) {
        // Use the findById method from the ProductRepository to retrieve the product by ID
        Optional<Product> optionalProduct = productRepository.findById(id);
        return optionalProduct.orElse(null);
    }
}
```

In this example, we inject the `ProductRepository` into the `ProductService` using constructor injection. The `findById` method of the `ProductRepository` is used to retrieve the product by its ID.

3. Now, you can expose this functionality through a REST controller:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/products/{id}")
    public ResponseEntity<Product> getProductById(@PathVariable int id) {
        Product product = productService.getProductById(id);

        if (product != null) {
            return ResponseEntity.ok(product);
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}
```

In this example, we use the `ProductService` to retrieve the product by its ID when a GET request is made to the endpoint `/products/{id}`. If the product is found, we return it with an HTTP status of 200 (OK). If the product is not found, we return an HTTP status of 404 (Not Found).

Remember to handle exceptions appropriately in a real-world application to handle cases when the ID is invalid or there are other errors during database access.

## Reading an Entity from name

To read one product from its name using Spring Boot and Spring Data JPA, you can create a custom method in the repository that queries the database based on the product's name. Here's an example:

1. First, create the entity classes for `Product` and `ProductType`, and set up their respective repositories, similar to the previous examples.

2. Create a custom method in the `ProductRepository` to read the product by its name.

ProductRepository.java:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductRepository extends JpaRepository<Product, Integer> {
    Product findByName(String name);
}
```

In this example, we define a custom method `findByName` in the `ProductRepository`. The method takes a `String` parameter, representing the name of the product, and returns the corresponding `Product` entity.

3. Now, you can use this custom method in your service class:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public Product getProductByName(String name) {
        return productRepository.findByName(name);
    }
}
```

4. Finally, expose this functionality through a REST controller:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/products/{name}")
    public ResponseEntity<Product> getProductByName(@PathVariable String name) {
        Product product = productService.getProductByName(name);

        if (product != null) {
            return ResponseEntity.ok(product);
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}
```

In this example, when a GET request is made to the endpoint `/products/{name}`, the `ProductController` will call the `getProductByName` method from the `ProductService`. The service, in turn, will utilize the custom `findByName` method from the `ProductRepository` to retrieve the product with the given name.

As always, remember to handle exceptions appropriately to handle cases when the product name is not found or when there are other errors during database access.

## Reading all Entities

To get all product entities from the database using Spring Boot and Spring Data JPA, you can utilize the `findAll()` method provided by the repository. Here's an example:

1. Create the entity classes for `Product` and `ProductType`, and set up their respective repositories, similar to the previous examples.

2. Create a service class to handle business logic for retrieving all products:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }
}
```

In this example, we inject the `ProductRepository` into the `ProductService` using constructor injection. The `findAll()` method of the `ProductRepository` is used to retrieve all product entities from the database.

3. Expose this functionality through a REST controller:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/products")
    public ResponseEntity<List<Product>> getAllProducts() {
        List<Product> products = productService.getAllProducts();

        if (!products.isEmpty()) {
            return ResponseEntity.ok(products);
        } else {
            return ResponseEntity.noContent().build();
        }
    }
}
```

In this example, when a GET request is made to the endpoint `/products`, the `ProductController` will call the `getAllProducts` method from the `ProductService`. The service, in turn, will utilize the `findAll()` method from the `ProductRepository` to retrieve all product entities from the database.

If products are found, they will be returned with an HTTP status of 200 (OK). If there are no products in the database, an HTTP status of 204 (No Content) will be returned.

Make sure to handle exceptions appropriately for real-world scenarios to ensure proper error handling and graceful behavior when interacting with the database.

## Reading all Entities

To get all product entities from the database using Spring Boot and Spring Data JPA, you can utilize the `findAll()` method provided by the repository. Here's an example:

1. Create the entity classes for `Product` and `ProductType`, and set up their respective repositories, similar to the previous examples.

2. Create a service class to handle business logic for retrieving all products:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }
}
```

In this example, we inject the `ProductRepository` into the `ProductService` using constructor injection. The `findAll()` method of the `ProductRepository` is used to retrieve all product entities from the database.

3. Expose this functionality through a REST controller:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/products")
    public ResponseEntity<List<Product>> getAllProducts() {
        List<Product> products = productService.getAllProducts();

        if (!products.isEmpty()) {
            return ResponseEntity.ok(products);
        } else {
            return ResponseEntity.noContent().build();
        }
    }
}
```

In this example, when a GET request is made to the endpoint `/products`, the `ProductController` will call the `getAllProducts` method from the `ProductService`. The service, in turn, will utilize the `findAll()` method from the `ProductRepository` to retrieve all product entities from the database.

If products are found, they will be returned with an HTTP status of 200 (OK). If there are no products in the database, an HTTP status of 204 (No Content) will be returned.

Make sure to handle exceptions appropriately for real-world scenarios to ensure proper error handling and graceful behavior when interacting with the database.

## Reading by filtering 1

To read some entities of the `Product` table by filtering on a string contained in any part of the product name without using `@Query` in the repository, you can leverage Spring Data JPA's query creation mechanism. Here's an example using the `findBy` method:

1. Add the method in the `ProductRepository`:

ProductRepository.java:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface ProductRepository extends JpaRepository<Product, Integer> {
    List<Product> findByNameContainingIgnoreCase(String keyword);
}
```

In this example, we define a method `findByNameContainingIgnoreCase` in the `ProductRepository`. This method uses Spring Data JPA's query creation mechanism to create a query that finds products where the `name` field contains the specified `keyword`. The `Containing` keyword specifies that the `name` should contain the provided keyword, and `IgnoreCase` ensures a case-insensitive search.

2. Create a service class to handle business logic for retrieving products by keyword:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getProductsByKeyword(String keyword) {
        return productRepository.findByNameContainingIgnoreCase(keyword);
    }
}
```

In this example, we inject the `ProductRepository` into the `ProductService` using constructor injection. The `getProductsByKeyword` method in the service calls the `findByNameContainingIgnoreCase` method in the repository to get the products that match the provided keyword.

3. Expose this functionality through a REST controller:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/products/{keyword}")
    public ResponseEntity<List<Product>> getProductsByKeyword(@PathVariable String keyword) {
        List<Product> products = productService.getProductsByKeyword(keyword);

        if (!products.isEmpty()) {
            return ResponseEntity.ok(products);
        } else {
            return ResponseEntity.noContent().build();
        }
    }
}
```

In this example, when a GET request is made to the endpoint `/products/{keyword}`, the `ProductController` will call the `getProductsByKeyword` method from the `ProductService`. The service, in turn, will utilize the custom `findByNameContainingIgnoreCase` method from the `ProductRepository` to retrieve products that match the provided keyword.

If products are found, they will be returned with an HTTP status of 200 (OK). If there are no products that match the keyword, an HTTP status of 204 (No Content) will be returned.

Using the query creation mechanism allows you to avoid writing JPQL queries manually and lets Spring Data JPA handle query generation based on the method name. Remember to handle exceptions and sanitize user inputs properly in a real-world scenario to ensure proper error handling and security.

## Reading by filtering 2

To read the 10 most expensive entities of the `Product` table, you can leverage Spring Data JPA's query creation mechanism in combination with the `Pageable` interface to get the desired result. Here's an example:

1. Add the method in the `ProductRepository`:

ProductRepository.java:

```java
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface ProductRepository extends JpaRepository<Product, Integer> {
    List<Product> findAllByOrderByPriceDesc(Pageable pageable);
}
```

In this example, we define a method `findAllByOrderByPriceDesc` in the `ProductRepository`. This method uses Spring Data JPA's query creation mechanism along with the `Pageable` interface to create a query that finds products ordered by price in descending order. The `Pageable` parameter allows us to specify the number of results we want (in this case, the top 10 most expensive products).

2. Create a service class to handle business logic for retrieving the most expensive products:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getTop10MostExpensiveProducts() {
        Pageable pageable = PageRequest.of(0, 10); // Fetch the first 10 results
        return productRepository.findAllByOrderByPriceDesc(pageable);
    }
}
```

In this example, we inject the `ProductRepository` into the `ProductService` using constructor injection. The `getTop10MostExpensiveProducts` method in the service creates a `Pageable` object with the desired number of results (in this case, the first 10 results), and then calls the `findAllByOrderByPriceDesc` method in the repository with the `Pageable` parameter.

3. Expose this functionality through a REST controller:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/products/most-expensive")
    public ResponseEntity<List<Product>> getTop10MostExpensiveProducts() {
        List<Product> products = productService.getTop10MostExpensiveProducts();

        if (!products.isEmpty()) {
            return ResponseEntity.ok(products);
        } else {
            return ResponseEntity.noContent().build();
        }
    }
}
```

Now, when you make a GET request to the endpoint `/products/most-expensive`, the `ProductController` will call the `getTop10MostExpensiveProducts` method from the `ProductService`. The service, in turn, will utilize the custom `findAllByOrderByPriceDesc` method from the `ProductRepository` to retrieve only the 10 most expensive products from the database.

If the products are found, they will be returned with an HTTP status of 200 (OK). If there are no products in the database, an HTTP status of 204 (No Content) will be returned.

Using the `Pageable` interface allows you to limit the number of results without writing custom JPQL queries manually. Remember to handle exceptions and sanitize user inputs properly in a real-world scenario to ensure proper error handling and security.

## Creating and saving entites

To create and save a new random product in the example database, you can generate random product data using Java's random number generator or any other method you prefer. Here's an example of how to create and save a new random product:

Assuming you have already set up the `Product` entity, `ProductRepository`, `ProductService`, and `ProductController` as shown in the previous examples:

1. Create a method to generate a random product in a utility class (optional):

RandomProductGenerator.java:

```java
import java.math.BigDecimal;
import java.util.Random;

public class RandomProductGenerator {
    private static final String[] PRODUCT_NAMES = { "Laptop", "T-shirt", "Book", "Smartphone", "Watch", "Shoes" };
    private static final BigDecimal MAX_PRICE = new BigDecimal("1000");
    private static final int MAX_DESCRIPTION_LENGTH = 100;

    public static Product generateRandomProduct() {
        Random random = new Random();
        String name = PRODUCT_NAMES[random.nextInt(PRODUCT_NAMES.length)];
        BigDecimal price = new BigDecimal(random.nextDouble() * MAX_PRICE.doubleValue()).setScale(2, BigDecimal.ROUND_HALF_UP);
        String description = generateRandomDescription(random);
        return new Product(name, price, description);
    }

    private static String generateRandomDescription(Random random) {
        int length = random.nextInt(MAX_DESCRIPTION_LENGTH) + 1;
        StringBuilder descriptionBuilder = new StringBuilder();
        for (int i = 0; i < length; i++) {
            char randomChar = (char) (random.nextInt(26) + 'a');
            descriptionBuilder.append(randomChar);
        }
        return descriptionBuilder.toString();
    }
}
```

2. Use the random product generator in the controller to save a new random product:

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProductController {
    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @PostMapping("/products/random")
    public ResponseEntity<Product> saveRandomProduct() {
        Product randomProduct = RandomProductGenerator.generateRandomProduct();
        Product savedProduct = productService.saveProduct(randomProduct);
        return ResponseEntity.ok(savedProduct);
    }
}
```

In this example, when you make a POST request to the endpoint `/products/random`, the `ProductController` will call the `generateRandomProduct` method from the `RandomProductGenerator` utility class to create a new random product. Then, the `saveProduct` method from the `ProductService` will be called to save the new product to the database.

The newly created random product will be returned in the response with an HTTP status of 200 (OK).

Remember to handle exceptions and validation appropriately in a real-world scenario to ensure proper error handling and data integrity. Additionally, ensure that your database connection and transaction management are properly configured to enable saving the data to the database.

## Updating

If you want to update all products in the example database by adding 1% to their price, you can achieve this by using the `@Modifying` annotation along with the `@Query` annotation in the repository.

Here's an example of how to do it:

1. Add a method in the `ProductRepository` to update all products' prices:

ProductRepository.java:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

    @Modifying
    @Query("UPDATE Product p SET p.price = p.price * 1.01")
    void updateAllProductsPriceByOnePercent();
}
```

In this example, we define a custom update query using the `@Query` annotation. The query updates the price of all products in the database by multiplying their current price by 1.01 (1% increase). The `@Modifying` annotation is used to indicate that this is an update query.

2. Create a service class (optional):

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class ProductService {

    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Transactional
    public void updateAllProductsPriceByOnePercent() {
        productRepository.updateAllProductsPriceByOnePercent();
    }
}
```

In this example, we create a `ProductService` class with a method `updateAllProductsPriceByOnePercent` that calls the custom update method from the `ProductRepository`. We annotate this method with `@Transactional` to ensure that the update operation is executed within a transaction.

3. Expose this functionality through a REST controller (optional):

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProductController {

    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @PostMapping("/products/update-price")
    public ResponseEntity<String> updateAllProductsPrice() {
        productService.updateAllProductsPriceByOnePercent();
        return ResponseEntity.ok("Price updated for all products.");
    }
}
```

In this example, when you make a POST request to the endpoint `/products/update-price`, the `ProductController` will call the `updateAllProductsPriceByOnePercent` method from the `ProductService`, which, in turn, will update the price of all products in the database using the custom update query in the `ProductRepository`.

Remember to handle exceptions and perform proper error handling in a real-world scenario. Additionally, make sure to configure your application properties and database connection details correctly to enable the update operation.

## Delete

To delete all the 10 cheapest products from the example database, you can create a custom query method in the `ProductRepository` to fetch the 10 cheapest products, and then use the `deleteAll()` method to remove them. Here's an example:

1. Add the custom query method in the `ProductRepository`:

ProductRepository.java:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    @Query("SELECT p FROM Product p ORDER BY p.price ASC")
    List<Product> findTop10CheapestProducts();
}
```

In this example, we define a custom query method `findTop10CheapestProducts()` that fetches the 10 cheapest products from the database, ordered by price in ascending order.

2. Create a service class to handle business logic for deleting the 10 cheapest products:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public void deleteTop10CheapestProducts() {
        List<Product> cheapestProducts = productRepository.findTop10CheapestProducts();
        productRepository.deleteAll(cheapestProducts);
    }
}
```

In this example, we inject the `ProductRepository` into the `ProductService` using constructor injection. The `deleteTop10CheapestProducts` method in the service first fetches the 10 cheapest products using the custom `findTop10CheapestProducts` method from the `ProductRepository`. Then, it uses the `deleteAll` method of the `ProductRepository` to delete all the products in the fetched list.

3. Expose this functionality through a REST controller (optional):

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProductController {

    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @DeleteMapping("/products/delete-cheapest")
    public ResponseEntity<String> deleteTop10CheapestProducts() {
        productService.deleteTop10CheapestProducts();
        return ResponseEntity.ok("Deleted the 10 cheapest products.");
    }
}
```

In this example, when you make a DELETE request to the endpoint `/products/delete-cheapest`, the `ProductController` will call the `deleteTop10CheapestProducts` method from the `ProductService`. The service will then fetch the 10 cheapest products from the database and delete them using the `deleteAll` method.

Remember to handle exceptions and perform proper error handling in a real-world scenario. Additionally, make sure to configure your application properties and database connection details correctly to enable the delete operation.

## Processing

To iterate through all products and perform the mentioned operations (adding 1% to products of type 1, deleting products of type 2, and setting the price of products of type 3 to 30), you can use the `ProductRepository` to fetch the products based on their types, apply the changes, and then save the updated entities. Here's an example:

1. Modify the `ProductRepository` to include methods for fetching products by their types:

ProductRepository.java:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    List<Product> findByType(String type);
}
```

2. Create a service class to handle the business logic:

ProductService.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.math.BigDecimal;
import java.util.List;

@Service
public class ProductService {

    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public void updateProducts() {
        // Fetch all products of type 1
        List<Product> productsOfType1 = productRepository.findByType("Type 1");
        for (Product product : productsOfType1) {
            BigDecimal onePercent = product.getPrice().multiply(BigDecimal.valueOf(0.01));
            product.setPrice(product.getPrice().add(onePercent));
        }

        // Delete all products of type 2
        List<Product> productsOfType2 = productRepository.findByType("Type 2");
        productRepository.deleteAll(productsOfType2);

        // Update the price of all products of type 3 to 30
        List<Product> productsOfType3 = productRepository.findByType("Type 3");
        for (Product product : productsOfType3) {
            product.setPrice(BigDecimal.valueOf(30));
        }

        // Save the updated products
        productRepository.saveAll(productsOfType1);
        productRepository.saveAll(productsOfType3);
    }
}
```

In this example, we create a `ProductService` class with a method `updateProducts` that performs the operations on the products based on their types. It fetches all products of type 1, adds 1% to their price, deletes all products of type 2, sets the price of all products of type 3 to 30, and then saves the updated entities.

3. Expose this functionality through a REST controller (optional):

ProductController.java:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProductController {

    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @PutMapping("/products/update")
    public ResponseEntity<String> updateProducts() {
        productService.updateProducts();
        return ResponseEntity.ok("Products updated.");
    }
}
```

In this example, when you make a PUT request to the endpoint `/products/update`, the `ProductController` will call the `updateProducts` method from the `ProductService`. The service will then iterate through all products, apply the operations based on their types, and save the updated products.

Remember to handle exceptions and perform proper error handling in a real-world scenario. Additionally, make sure to configure your application properties and database connection details correctly to enable the update and delete operations.

