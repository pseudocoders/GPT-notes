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

