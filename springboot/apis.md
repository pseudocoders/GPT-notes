# Building RESTful APIs with Spring Boot

A RESTful API (Representational State Transfer) is an architectural style for designing networked applications, specifically web services. It provides a set of constraints and principles for building scalable, stateless, and interoperable web APIs.

Key characteristics of a RESTful API include:

1. Stateless: The server does not store any client state between requests. Each request from a client must contain all the necessary information for the server to process it.

2. Client-Server Architecture: The client and server are separate entities that communicate over the network. The server provides resources and services, while the client consumes and interacts with those resources.

3. Uniform Interface: A uniform set of well-defined, standard methods is used to interact with resources. These methods include GET (retrieve resource), POST (create resource), PUT (update resource), DELETE (delete resource), and more.

4. Resource-Based: Resources are the fundamental units of the API, represented by URIs (Uniform Resource Identifiers). Clients interact with resources by sending requests to their corresponding URIs.

5. Representation-Oriented: Resources are represented in different formats such as JSON, XML, or HTML. Clients can request specific representations of resources based on their needs.

6. Hypermedia as the Engine of Application State (HATEOAS): The API responses contain hyperlinks that provide navigation and discoverability of related resources. Clients can follow these links to access related resources and perform further actions.

By adhering to these principles, RESTful APIs promote scalability, simplicity, and loose coupling between the client and server. They are widely used for building web services, providing a standardized and interoperable way for different systems to communicate and exchange data over the internet.

## Creating REST controllers

To create REST controllers in Spring Boot, you can follow these steps:

1. Create a new Java class: Open your project in your preferred IDE and create a new Java class to serve as your REST controller. This class will define the endpoints and handle the incoming HTTP requests.

2. Annotate the class: Add the `@RestController` annotation to the class declaration. This annotation indicates that the class is a REST controller and that its methods will handle HTTP requests and produce responses.

   ```java
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class MyRestController {

   }
   ```

3. Define endpoints: Add methods to the REST controller class and annotate them with appropriate HTTP method annotations such as `@GetMapping`, `@PostMapping`, `@PutMapping`, or `@DeleteMapping`. These annotations specify the URL path for the endpoint and the HTTP method it handles.

   ```java
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;

   @RestController
   public class MyRestController {

       @GetMapping("/hello")
       public String getHello() {
           return "Hello, World!";
       }

       @PostMapping("/data")
       public void postData(@RequestBody Data data) {
           // Process and handle the received data
       }
   }
   ```

4. Customize endpoint mappings: You can specify additional path segments and parameters in the `@GetMapping`, `@PostMapping`, and other method annotations to further customize your endpoint mappings. You can also use path variables, request parameters, or request headers in your method parameters.

   ```java
   @GetMapping("/users/{id}")
   public User getUserById(@PathVariable("id") Long userId) {
       // Retrieve and return the user with the specified ID
   }

   @GetMapping("/search")
   public List<Product> searchProducts(@RequestParam("query") String searchQuery) {
       // Perform product search based on the provided query
   }
   ```

5. Handle request payloads: You can handle request payloads by including the `@RequestBody` annotation on method parameters. This annotation binds the request body to the specified parameter, allowing you to access and process the incoming data.

   ```java
   @PostMapping("/data")
   public void postData(@RequestBody Data data) {
       // Process and handle the received data
   }
   ```

6. Return responses: The methods in your REST controller can return various types of responses, such as plain text, JSON, XML, or custom objects. Spring Boot automatically serializes the response into the appropriate format based on the `Accept` header provided by the client.

   ```java
   @GetMapping("/hello")
   public String getHello() {
       return "Hello, World!";
   }

   @GetMapping("/users")
   public List<User> getAllUsers() {
       // Retrieve and return a list of users
   }
   ```

7. Run your application: Start your Spring Boot application, and the REST endpoints you defined will be accessible through the configured server (e.g., Tomcat or Jetty). You can test the endpoints using a web browser, cURL, or API testing tools like Postman.

That's it! You have created REST controllers in Spring Boot to handle incoming HTTP requests and produce responses. You can define additional endpoints and customize the behavior of your controllers based on your application requirements.

### @RestController

The `@RestController` annotation in Spring Boot is used to mark a class as a REST controller. It combines the functionality of the `@Controller` and `@ResponseBody` annotations.

When a class is annotated with `@RestController`, it indicates that the class will handle incoming HTTP requests and produce HTTP responses, typically in JSON or XML format. The methods within the class are responsible for defining the endpoints and processing the requests.

Key features of the `@RestController` annotation:

1. Combination of `@Controller` and `@ResponseBody`: By annotating a class with `@RestController`, there's no need to explicitly annotate each method with `@ResponseBody`. The `@RestController` annotation implies that the return values of the methods will be directly serialized into the response body.

2. Automatic Content Negotiation: When a request is made to a `@RestController` endpoint, Spring Boot automatically performs content negotiation based on the `Accept` header sent by the client. It determines the appropriate format (such as JSON, XML, or others) to serialize the response.

3. Simplified REST API development: Using `@RestController` helps streamline the development of RESTful APIs. It eliminates the need to manually handle the serialization and deserialization of objects to and from JSON or XML format, as Spring Boot takes care of it automatically.

Example usage of `@RestController`:

```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;

@RestController
public class MyRestController {

    @GetMapping("/hello")
    public String getHello() {
        return "Hello, World!";
    }
}
```

In the above example, the `MyRestController` class is annotated with `@RestController`, indicating that it handles HTTP requests. The `getHello()` method is annotated with `@GetMapping` and defines the `/hello` endpoint. When a request is made to `/hello`, the method returns the string "Hello, World!" as the response, which will be automatically serialized into the response body.

By using `@RestController`, you can create concise and straightforward REST controllers in Spring Boot, focusing on handling request mappings and business logic without worrying about response serialization.

### @RequestMapping

The `@RequestMapping` annotation in Spring Boot is a versatile annotation used to map web requests to specific controller methods. It is a fundamental annotation for defining the URL mapping and request handling behavior in Spring MVC and Spring WebFlux applications.

Key features of the `@RequestMapping` annotation:

1. Mapping URL paths: The `@RequestMapping` annotation is used to map a specific URL path or pattern to a controller method. It specifies the URL at which the method should be invoked when a request is made.

2. Handling various HTTP methods: The `@RequestMapping` annotation supports multiple HTTP methods, including GET, POST, PUT, DELETE, and more. By default, if no explicit method is specified, the method will handle all HTTP methods for the specified URL.

3. Request parameter mapping: The `@RequestMapping` annotation allows you to map and handle requests based on specific request parameters. You can define required or optional parameters and even specify their values using the `params` attribute.

4. Path variables mapping: The `@RequestMapping` annotation supports path variables, allowing you to extract dynamic values from the URL and use them as method parameters. Path variables are defined within the URL pattern using curly braces `{}` and can be accessed through method parameters with the `@PathVariable` annotation.

Example usage of `@RequestMapping`:

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class MyController {

    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String sayHello() {
        return "Hello, World!";
    }

    @RequestMapping(value = "/users/{id}", method = RequestMethod.GET)
    public User getUserById(@PathVariable("id") Long userId) {
        // Retrieve and return the user with the specified ID
    }

    @RequestMapping(value = "/search", method = RequestMethod.GET, params = "query")
    public List<Product> searchProducts(@RequestParam("query") String searchQuery) {
        // Perform product search based on the provided query
    }
}
```

In the above example, the `@RequestMapping` annotation is used to define different endpoints:

- `/api/hello`: When a GET request is made to `/api/hello`, the `sayHello()` method is invoked, which returns the string "Hello, World!" as the response.
- `/api/users/{id}`: The `{id}` is a path variable that captures a dynamic value. The `getUserById()` method retrieves a user based on the provided `id` value and returns the user object as the response.
- `/api/search`: The `searchProducts()` method accepts a request parameter called `query`. It performs a product search based on the provided query value and returns a list of products as the response.

By using `@RequestMapping`, you have more flexibility in mapping requests to specific methods, handling different HTTP methods, working with request parameters, and capturing dynamic values from the URL in your Spring MVC or Spring WebFlux application.


### @GetMapping, @PostMapping, @PutMapping and @DeleteMapping

The `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping` annotations are specialized versions of the more general `@RequestMapping` annotation in Spring Boot. They are used to handle HTTP GET, POST, PUT, and DELETE requests, respectively, for specific endpoint mappings in a Spring MVC or Spring WebFlux application.

Here's an explanation of each annotation:

1. `@GetMapping`: This annotation is used to handle HTTP GET requests. It maps a specific URL path or pattern to a controller method that will be invoked when a GET request is made to that URL.

Example:
```java
@GetMapping("/users")
public List<User> getUsers() {
    // Retrieve and return a list of users
}
```

2. `@PostMapping`: This annotation is used to handle HTTP POST requests. It maps a specific URL path or pattern to a controller method that will be invoked when a POST request is made to that URL.

Example:
```java
@PostMapping("/users")
public User createUser(@RequestBody User newUser) {
    // Create a new user based on the provided request body and return the created user
}
```

3. `@PutMapping`: This annotation is used to handle HTTP PUT requests. It maps a specific URL path or pattern to a controller method that will be invoked when a PUT request is made to that URL.

Example:
```java
@PutMapping("/users/{id}")
public User updateUser(@PathVariable("id") Long userId, @RequestBody User updatedUser) {
    // Update the user with the specified ID using the provided request body and return the updated user
}
```

4. `@DeleteMapping`: This annotation is used to handle HTTP DELETE requests. It maps a specific URL path or pattern to a controller method that will be invoked when a DELETE request is made to that URL.

Example:
```java
@DeleteMapping("/users/{id}")
public void deleteUser(@PathVariable("id") Long userId) {
    // Delete the user with the specified ID
}
```

In each of the above examples, the respective annotation is used to map a specific URL to a controller method. The method is responsible for processing the request, performing any necessary operations, and returning an appropriate response. Additional annotations like `@PathVariable` and `@RequestBody` may be used to access path variables or request bodies sent along with the requests.

By using these specialized mapping annotations, you can handle specific HTTP methods in a more readable and concise manner, enhancing the clarity and maintainability of your Spring Boot application's code.

### @PathVariable

The `@PathVariable` annotation in Spring Boot is used to bind a path variable from the URL to a method parameter in a controller method. It allows you to extract dynamic values from the URL and use them in your request handling logic.

When a URL contains a path variable, such as `/users/{id}`, the `@PathVariable` annotation can be used to capture the value of `id` and pass it as an argument to the corresponding method parameter.

Here's an example of using `@PathVariable`:

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUserById(@PathVariable("id") Long userId) {
    // Retrieve the user with the specified ID
    User user = userService.getUserById(userId);
    
    if (user != null) {
        return ResponseEntity.ok(user);
    } else {
        return ResponseEntity.notFound().build();
    }
}
```

In the above example, the `@PathVariable("id")` annotation is used to bind the value of the `id` path variable from the URL to the `userId` method parameter. When a GET request is made to `/users/123`, the `getUserById` method is invoked with `userId` set to `123`.

The `@PathVariable` annotation supports various data types for method parameters, such as `String`, `int`, `Long`, or custom types. You can also specify the variable name in the annotation if it differs from the method parameter name, as shown in the example.

It's important to note that the name of the path variable in the annotation should match the name defined in the URL mapping. Additionally, the path variable can be optional by using the `required` attribute of `@PathVariable`, such as `@PathVariable(value = "id", required = false)`. In this case, if the path variable is not present in the URL, the method parameter will be set to `null` or the default value of the corresponding data type.

By using `@PathVariable`, you can easily capture dynamic values from the URL and incorporate them into your controller methods, enabling flexible and parameterized request handling in your Spring Boot application.

### @RequestBody

The `@RequestBody` annotation in Spring Boot is used to bind the request body of an HTTP request to a method parameter in a controller method. It allows you to extract data sent in the request body and use it in your request handling logic.

When a request is made with a body containing JSON, XML, or other data formats, the `@RequestBody` annotation can be applied to a method parameter to automatically deserialize the request body into an object of the specified type.

Here's an example of using `@RequestBody`:

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User newUser) {
    // Create a new user based on the provided request body
    User savedUser = userService.createUser(newUser);

    // Return the saved user in the response
    return ResponseEntity.ok(savedUser);
}
```

In the above example, the `@RequestBody` annotation is used to bind the request body to the `newUser` method parameter of type `User`. When a POST request is made to `/users` with a request body containing user data in JSON format, the `createUser` method is invoked, and Spring automatically converts the request body into a `User` object.

The `@RequestBody` annotation supports various data types for method parameters, including custom objects, collections, and primitives. When the request body is deserialized, Spring uses the configured `HttpMessageConverter` to convert the request body into the appropriate object type.

It's important to note that the `@RequestBody` annotation is typically used with HTTP methods that have request bodies, such as POST, PUT, and PATCH. It is not applicable to methods handling GET or DELETE requests, as they usually do not have request bodies.

By using `@RequestBody`, you can easily retrieve and use the data sent in the request body, allowing you to process and manipulate it within your controller methods. This annotation simplifies the handling of complex data structures and enables seamless integration of RESTful APIs in your Spring Boot application.

### ResponseEntity

In Spring Boot, `ResponseEntity` is a class that represents the entire HTTP response, including the status code, headers, and body. It provides a way to customize the response sent back to the client from a controller method.

`ResponseEntity` allows you to specify the HTTP status code, headers, and body of the response in a flexible manner. It provides more control and flexibility compared to the default response handling in Spring MVC or Spring WebFlux.

Here's a deeper explanation of `ResponseEntity` and its usage:

1. Customizing the HTTP status code: With `ResponseEntity`, you can set the desired HTTP status code for the response. You can use the `ResponseEntity.status(HttpStatus)` method to specify the status code explicitly or use convenience methods like `ok()`, `badRequest()`, `notFound()`, etc., to set commonly used status codes.

2. Setting headers: `ResponseEntity` allows you to set custom headers for the response. You can use the `header(String, String)` method to add a single header or the `headers(HttpHeaders)` method to set multiple headers at once.

3. Defining the response body: `ResponseEntity` provides various methods to set the response body. You can use the `body(T)` method to set a single object as the response body, or you can use the `body(Object, MediaType)` method to set the body along with the desired media type. Additionally, you can return `ResponseEntity<Void>` if you don't need to include a response body.

4. Returning the `ResponseEntity` from a controller method: In a controller method, you can return `ResponseEntity` as the result. This allows you to have fine-grained control over the response, including the status code, headers, and body. If you return `ResponseEntity<T>`, Spring will automatically serialize the response body using the configured `HttpMessageConverter`.

Here's an example of using `ResponseEntity` in a controller method:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/hello")
    public ResponseEntity<String> sayHello() {
        String message = "Hello, World!";
        return ResponseEntity.ok(message);
    }

    @GetMapping("/notfound")
    public ResponseEntity<Void> notFound() {
        return ResponseEntity.notFound().build();
    }

    @GetMapping("/custom")
    public ResponseEntity<String> customResponse() {
        HttpHeaders headers = new HttpHeaders();
        headers.add("Custom-Header", "Custom Value");

        String body = "Custom response body";

        return ResponseEntity
                .status(HttpStatus.CREATED)
                .headers(headers)
                .body(body);
    }
}
```

In the above example:
- The `sayHello()` method returns a `ResponseEntity<String>` with a response body of "Hello, World!" and the default HTTP status code of 200 (OK).
- The `notFound()` method returns a `ResponseEntity<Void>` with the HTTP status code of 404 (Not Found) and no response body.
- The `customResponse()` method constructs a custom `ResponseEntity<String>` with a status code of 201 (Created), adds a custom header, and sets a custom response body.

By using `ResponseEntity`, you have fine-grained control over the HTTP response sent back to the client. It allows you to customize the status code, headers, and body according to your application's specific needs.



## Example

Sure! Here's an example of building a RESTful API that emits JSON using Spring Boot and the mentioned annotations:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private List<User> userList = new ArrayList<>();

    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(userList);
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable("id") Long id) {
        for (User user : userList) {
            if (user.getId().equals(id)) {
                return ResponseEntity.ok(user);
            }
        }
        return ResponseEntity.notFound().build();
    }

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        // Generate a unique ID for the new user
        Long id = generateUniqueId();

        // Set the generated ID for the user
        user.setId(id);

        // Add the user to the list
        userList.add(user);

        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }

    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable("id") Long id, @RequestBody User updatedUser) {
        for (User user : userList) {
            if (user.getId().equals(id)) {
                user.setName(updatedUser.getName());
                user.setEmail(updatedUser.getEmail());
                return ResponseEntity.ok(user);
            }
        }
        return ResponseEntity.notFound().build();
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable("id") Long id) {
        for (User user : userList) {
            if (user.getId().equals(id)) {
                userList.remove(user);
                return ResponseEntity.noContent().build();
            }
        }
        return ResponseEntity.notFound().build();
    }

    private Long generateUniqueId() {
        // Logic to generate a unique ID (e.g., using an atomic counter or UUID)
        return System.currentTimeMillis();
    }
}
```

In this example, we have a `UserController` class that handles CRUD operations for a `User` entity. Here's a breakdown of the annotations used:

- `@RestController`: Indicates that the class is a REST controller, allowing it to handle HTTP requests and produce JSON responses.
- `@RequestMapping("/api/users")`: Specifies the base URL mapping for all the endpoints defined in the controller.
- `@GetMapping`: Handles HTTP GET requests.
- `@PostMapping`: Handles HTTP POST requests.
- `@PutMapping`: Handles HTTP PUT requests.
- `@DeleteMapping`: Handles HTTP DELETE requests.
- `@PathVariable`: Binds a path variable from the URL to a method parameter.
- `@RequestBody`: Binds the request body to a method parameter, deserializing it into an object.
- `ResponseEntity<T>`: Represents the HTTP response, allowing you to customize the status code, headers, and body.

Make sure to define the `User` class with appropriate attributes (e.g., `id`, `name`, `email`) to match your application's requirements.

You can test the API by running the Spring Boot application and making HTTP requests to the corresponding endpoints (`/api/users`) using tools like cURL or Postman.


