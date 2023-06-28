# BlogBUSTER

```sql
CREATE TABLE `post` (
  `id` int(11) NOT NULL,
  `title` varchar(255) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL,
  `body` text CHARACTER SET utf8 COLLATE utf8_unicode_ci,
  `date` datetime DEFAULT NULL,
  `labels` varchar(255) CHARACTER SET utf8 COLLATE utf8_unicode_ci DEFAULT NULL,
  `visible` tinyint(1) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

ALTER TABLE `post`
  ADD PRIMARY KEY (`id`);

ALTER TABLE `post`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;
COMMIT;
```


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

## Create

To modify the "blogBUSTER" project and add a `create` API endpoint you can follow these steps:

1. Update the `Post` entity class:
Ensure that the `Post` entity class includes the necessary annotations and validation constraints. For example:
```java
import javax.persistence.*;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.Size;
import java.time.LocalDateTime;

@Entity
@Table(name = "post")
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @NotBlank
    @Size(max = 255)
    private String title;

    @Lob
    private String body;

    private LocalDateTime date;

    @Size(max = 255)
    private String labels;

    private boolean visible;

    // Constructors, getters, and setters
}
```

2. Update the `PostService` class:
Include a new method in the `PostService` class to handle the creation of posts:
```java
import org.springframework.stereotype.Service;

@Service
public class PostService {
    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public Post createPost(Post post) {
        return postRepository.save(post);
    }
}
```

3. Update the `PostController` class:
Add a new endpoint to the `PostController` class for handling the creation of posts:
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;

@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    public PostController(PostService postService) {
        this.postService = postService;
    }

    // Existing endpoints...

    @PostMapping
    public ResponseEntity<?> createPost(@RequestBody @Valid Post post, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            // Handle validation errors
            return ResponseEntity.badRequest().body(bindingResult.getAllErrors());
        }

        Post createdPost = postService.createPost(post);
        return new ResponseEntity<>(createdPost, HttpStatus.CREATED);
    }
}
```

4. Test the `create` API endpoint:
You can now test the `create` API endpoint by sending a POST request to `/posts` with a JSON payload containing the post data. The endpoint will validate the data based on the constraints defined in the `Post` entity class. If the validation passes, the post will be saved in the database and returned in the response with a status code of 201 (Created). If there are validation errors, the response will include the validation error details.

Make sure to handle any additional error scenarios and customize the response messages as needed.

Note: Don't forget to wire up the required dependencies (e.g., `PostRepository`) and configure the database connection properties in the Spring Boot project.

## Update

To modify the "blogBUSTER" project and add an `update` API endpoint you can follow these steps:

1. Update the `PostService` class:
Include a new method in the `PostService` class to handle the update of posts:
```java
import org.springframework.stereotype.Service;

@Service
public class PostService {
    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public Post updatePost(Post post) {
        if (!postRepository.existsById(post.getId())) {
            throw new RuntimeException("Post not found for ID: " + post.getId());
        }
        return postRepository.save(post);
    }
}
```

2. Update the `PostController` class:
Add a new endpoint to the `PostController` class for handling the update of posts:
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;

@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    public PostController(PostService postService) {
        this.postService = postService;
    }

    // Existing endpoints...

    @PutMapping("/{id}")
    public ResponseEntity<?> updatePost(@PathVariable int id, @RequestBody @Valid Post post,
                                        BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            // Handle validation errors
            return ResponseEntity.badRequest().body(bindingResult.getAllErrors());
        }

        if (post.getId() != id) {
            return ResponseEntity.badRequest().body("Mismatched ID in path and payload");
        }

        Post updatedPost = postService.updatePost(post);
        return new ResponseEntity<>(updatedPost, HttpStatus.OK);
    }
}
```

3. Test the `update` API endpoint:
You can now test the `update` API endpoint by sending a PUT request to `/posts/{id}` with the post ID in the URL path and a JSON payload containing the updated post data. The endpoint will validate the data based on the constraints defined in the `Post` entity class. If the validation passes and the post with the given ID exists in the database, the post will be updated and returned in the response with a status code of 200 (OK). If there are validation errors or the ID in the path and payload don't match, appropriate error responses will be returned.

Make sure to handle any additional error scenarios and customize the response messages as needed.

Note: Ensure that the necessary dependencies (e.g., `PostRepository`) are properly wired up and the database connection properties are configured in the Spring Boot project.


## Delete

To modify the "blogBUSTER" project and add a `delete` API endpoint that accepts a request with an ID and removes the corresponding post from the database, you can follow these steps:

1. Update the `PostService` class:
Include a new method in the `PostService` class to handle the deletion of posts:
```java
import org.springframework.stereotype.Service;

@Service
public class PostService {
    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public void deletePost(int id) {
        if (!postRepository.existsById(id)) {
            throw new RuntimeException("Post not found for ID: " + id);
        }
        postRepository.deleteById(id);
    }
}
```

2. Update the `PostController` class:
Add a new endpoint to the `PostController` class for handling the deletion of posts:
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    public PostController(PostService postService) {
        this.postService = postService;
    }

    // Existing endpoints...

    @DeleteMapping("/{id}")
    public ResponseEntity<?> deletePost(@PathVariable int id) {
        postService.deletePost(id);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    }
}
```

3. Test the `delete` API endpoint:
You can now test the `delete` API endpoint by sending a DELETE request to `/posts/{id}` with the post ID in the URL path. The endpoint will check if the post with the given ID exists in the database. If it does, the post will be deleted, and a response with a status code of 204 (No Content) will be returned. If the post is not found, an appropriate error response will be returned.

Make sure to handle any additional error scenarios and customize the response messages as needed.

Note: Ensure that the necessary dependencies (e.g., `PostRepository`) are properly wired up and the database connection properties are configured in the Spring Boot project.



## Generate

To modify the "blogBUSTER" project and add a `generate` API endpoint that creates random posts in the database, you can follow these steps:

1. Update the `PostService` class:
Include a new method in the `PostService` class to generate and save random posts:
```java
import org.springframework.stereotype.Service;

import java.time.LocalDateTime;
import java.util.Random;

@Service
public class PostService {
    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public void generateRandomPosts(int numberOfPosts) {
        Random random = new Random();

        for (int i = 0; i < numberOfPosts; i++) {
            Post post = new Post();
            post.setTitle("Random Post " + i);
            post.setBody("This is the body of random post " + i);
            post.setDate(LocalDateTime.now());
            post.setLabels("Random");
            post.setVisible(random.nextBoolean());

            postRepository.save(post);
        }
    }
}
```

2. Update the `PostController` class:
Add a new endpoint to the `PostController` class for handling the generation of random posts:
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/posts")
public class PostController {
    private final PostService postService;

    public PostController(PostService postService) {
        this.postService = postService;
    }

    // Existing endpoints...

    @PostMapping("/generate")
    public ResponseEntity<?> generateRandomPosts(@RequestParam int numberOfPosts) {
        postService.generateRandomPosts(numberOfPosts);
        return new ResponseEntity<>(HttpStatus.CREATED);
    }
}
```

3. Test the `generate` API endpoint:
You can now test the `generate` API endpoint by sending a POST request to `/posts/generate` with the `numberOfPosts` parameter specifying the desired number of random posts to be generated and saved in the database. The endpoint will create and save the specified number of random posts, and a response with a status code of 201 (Created) will be returned.

Make sure to handle any additional error scenarios and customize the response messages as needed.


Note: Ensure that the necessary dependencies (e.g., `PostRepository`) are properly wired up and the database connection properties are configured in the Spring Boot project.

