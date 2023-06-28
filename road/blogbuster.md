# BlogBUSTER

## Get

I will guide you through creating a Spring Boot project named "blogBUSTER," creating the entity class for the "post" table, creating a repository, service, and implementing an API endpoint to retrieve a post by ID and return it in JSON format.

1. Start a Spring Boot project:
You can start a new Spring Boot project with the name "blogBUSTER" using your preferred IDE or by using Spring Initializr (https://start.spring.io/).

2. Create the entity class:
Create a new class called "Post" in the "blogBUSTER" project's package with the following fields and annotations:
```java
import javax.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "post")
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String title;
    private String body;
    private LocalDateTime date;
    private String labels;
    private boolean visible;

    // Constructors, getters, and setters
}
```

3. Create the repository:
Create an interface called "PostRepository" in the "blogBUSTER" project's package that extends JpaRepository:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface PostRepository extends JpaRepository<Post, Integer> {
}
```

4. Create the service:
Create a service class called "PostService" in the "blogBUSTER" project's package to handle the business logic:
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.Optional;

@Service
public class PostService {
    private final PostRepository postRepository;

    @Autowired
    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public Post getPostById(int id) {
        Optional<Post> postOptional = postRepository.findById(id);
        if (postOptional.isPresent()) {
            return postOptional.get();
        }
        throw new RuntimeException("Post not found for ID: " + id);
    }
}
```

5. Create the API endpoint:
Create a controller class called "PostController" in the "blogBUSTER" project's package to expose the API endpoint:
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    @Autowired
    public PostController(PostService postService) {
        this.postService = postService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<Post> getPostById(@PathVariable int id) {
        Post post = postService.getPostById(id);
        return new ResponseEntity<>(post, HttpStatus.OK);
    }
}
```

In the example above, the `PostService` class handles the business logic and uses the `PostRepository` to interact with the database. The `PostController` exposes the `/posts/{id}` endpoint for retrieving a post by ID and returns it in JSON format.

Now, when you run the Spring Boot application and send a GET request to `/posts/{id}`, where `{id}` is the ID of the post you want to retrieve, it will return the corresponding post in JSON format.


## Count

To add an endpoint called "count" to the "blogBUSTER" project that returns the number of posts in the database, you can follow these steps:

1. Modify the `PostRepository` interface:
```java
@Repository
public interface PostRepository extends JpaRepository<Post, Integer> {
    long count();
}
```
By adding the `count()` method to the `PostRepository`, you can retrieve the number of posts from the database.

2. Modify the `PostService` class:
```java
@Service
public class PostService {
    private final PostRepository postRepository;

    @Autowired
    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public Post getPostById(int id) {
        Optional<Post> postOptional = postRepository.findById(id);
        if (postOptional.isPresent()) {
            return postOptional.get();
        }
        throw new RuntimeException("Post not found for ID: " + id);
    }

    public long countPosts() {
        return postRepository.count();
    }
}
```
In the `PostService` class, the `countPosts()` method utilizes the `count()` method from the `PostRepository` to get the total number of posts in the database.

3. Update the `PostController` class:
```java
@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    @Autowired
    public PostController(PostService postService) {
        this.postService = postService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<Post> getPostById(@PathVariable int id) {
        Post post = postService.getPostById(id);
        return new ResponseEntity<>(post, HttpStatus.OK);
    }

    @GetMapping("/count")
    public ResponseEntity<Long> countPosts() {
        long count = postService.countPosts();
        return new ResponseEntity<>(count, HttpStatus.OK);
    }
}
```
In the `PostController` class, a new `countPosts()` method is added as a GET endpoint at `/posts/count`. This endpoint retrieves the count of posts using the `countPosts()` method from the `PostService` and returns it in the response body.

Now, when you run the Spring Boot application and send a GET request to `/posts/count`, it will return the number of posts in the database.

## getPage

To modify the "blogBUSTER" project and add a `getPage` API endpoint you can follow these steps:

1. Update the `PostService` class:
Update the `PostService` class to include a method for retrieving a paginated list of posts based on the request parameters:
```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

@Service
public class PostService {
    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public Page<Post> getPostsByPage(int pageNumber, int pageSize, String orderBy, String direction) {
        Sort sort = Sort.by(Sort.Direction.fromString(direction), orderBy);
        Pageable pageable = PageRequest.of(pageNumber, pageSize, sort);
        return postRepository.findAll(pageable);
    }
}
```

2. Update the `PostController` class:
Update the `PostController` class to include the new `getPage` endpoint:
```java
import org.springframework.data.domain.Page;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    public PostController(PostService postService) {
        this.postService = postService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<Post> getPostById(@PathVariable int id) {
        Post post = postService.getPostById(id);
        return new ResponseEntity<>(post, HttpStatus.OK);
    }

    @GetMapping("/count")
    public ResponseEntity<Long> countPosts() {
        long count = postService.countPosts();
        return new ResponseEntity<>(count, HttpStatus.OK);
    }

    @GetMapping("/page")
    public ResponseEntity<Page<Post>> getPostsByPage(@RequestParam(defaultValue = "0") int pageNumber,
                                                     @RequestParam(defaultValue = "10") int pageSize,
                                                     @RequestParam(defaultValue = "id") String orderBy,
                                                     @RequestParam(defaultValue = "asc") String direction) {
        Page<Post> postPage = postService.getPostsByPage(pageNumber, pageSize, orderBy, direction);
        return new ResponseEntity<>(postPage, HttpStatus.OK);
    }
}
```

In the updated code:
- The `getPostsByPage` method in the `PostService` class now accepts the `pageNumber`, `pageSize`, `orderBy`, and `direction` parameters.
- The `PostController` class now uses the `Pageable` object from Spring Boot's `PageRequest` to create the pagination configuration. The default values are specified for the request parameters `pageNumber`, `pageSize`, `orderBy`, and `direction`, but you can modify them as needed.
- The `getPostsByPage` endpoint in the `PostController` returns a `ResponseEntity` containing the paginated `Page` of `Post` objects.

Now, when you run the Spring Boot application and send a GET request to `/posts/page` with the appropriate query parameters, it will return a paginated response of posts based on the specified page number, page size, field order, and field direction using the `Pageable` functionality provided by Spring Boot.
