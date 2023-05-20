# Data validation and exception control 

## Data validation

In a Spring Boot project, data validation from the request should typically be placed in the controller layer. The controller layer is responsible for handling incoming requests, validating the request data, and forwarding the data to the appropriate service or business logic layer.

Here's a recommended approach for data validation in a Spring Boot project:

1. Controller Layer: In the controller layer, you can utilize the validation annotations provided by Spring, such as `@Valid` and `@RequestBody`, to perform data validation on the incoming request. These annotations can be applied to the method parameters or the request body object.

   ```java
   @RestController
   public class UserController {
       @PostMapping("/users")
       public ResponseEntity<?> createUser(@Valid @RequestBody UserDto userDto) {
           // Handle the validated request data
           // Call the service layer to create a user
           // Return appropriate response
       }
   }
   ```

   In the above example, the `@Valid` annotation is used to trigger the validation process for the `UserDto` object received in the request body. This ensures that the request data adheres to the validation rules defined in the `UserDto` class.

2. Validation Annotations: Inside the DTO (Data Transfer Object) or request object classes, you can use validation annotations such as `@NotNull`, `@NotBlank`, `@Size`, `@Pattern`, etc., to define the validation constraints for each field.

   ```java
   public class UserDto {
       @NotNull
       @NotBlank
       @Size(min = 2, max = 50)
       private String name;
   
       @NotNull
       @Email
       private String email;
   
       // Getters and setters
   }
   ```

   In this example, the `name` field is annotated with `@NotNull`, `@NotBlank`, and `@Size` annotations, specifying that it must not be null or blank and must have a length between 2 and 50 characters. The `email` field is annotated with `@NotNull` and `@Email`, indicating that it must not be null and must be a valid email address.

3. Validation Errors Handling: If validation errors occur during the validation process, Spring Boot automatically generates a response with the appropriate error messages. You can customize the error handling by using the `@ExceptionHandler` annotation to handle validation-related exceptions and return a meaningful error response to the client.

   ```java
   @RestControllerAdvice
   public class GlobalExceptionHandler {
       @ExceptionHandler(MethodArgumentNotValidException.class)
       public ResponseEntity<ErrorResponse> handleValidationException(MethodArgumentNotValidException ex) {
           // Extract validation errors from the exception
           // Build an appropriate error response
           // Return the error response with a suitable status code
       }
   }
   ```

   In this example, the `GlobalExceptionHandler` class handles the `MethodArgumentNotValidException`, which is thrown when validation errors occur. It extracts the validation errors from the exception, builds a custom error response, and returns it with an appropriate status code.

By placing the data validation in the controller layer, you can ensure that the incoming request data is validated before further processing. This helps maintain the integrity and consistency of the data and provides meaningful error responses to clients in case of validation failures.

### Example

Here's an example of how you can perform data validation in the controller layer of a Spring Boot application:

```java
@RestController
public class UserController {
    
    @Autowired
    private UserService userService;

    @PostMapping("/users")
    public ResponseEntity<User> createUser(@Valid @RequestBody CreateUserRequest request) {
        // Validate request data
        if (!isValidEmail(request.getEmail())) {
            throw new IllegalArgumentException("Invalid email format");
        }

        // Convert request to user entity
        User user = convertToUserEntity(request);

        // Save the user
        User savedUser = userService.createUser(user);

        return ResponseEntity.ok(savedUser);
    }

    // Other controller methods...

    private boolean isValidEmail(String email) {
        // Perform email validation logic
        // Return true if email is valid, false otherwise
    }

    private User convertToUserEntity(CreateUserRequest request) {
        // Convert request data to User entity object
        // Return the converted User entity
    }
}
```

In this example, we have a `UserController` class with a `createUser` method that handles the creation of a user. The `@Valid` annotation is used to enable data validation on the `CreateUserRequest` object received in the request body.

Inside the method, you can perform additional data validation checks based on your requirements. In this case, we have a validation check for the email format using the `isValidEmail` method. If the email is determined to be invalid, we throw an `IllegalArgumentException` with an appropriate error message.

If the request data passes the validation checks, we then convert the `CreateUserRequest` object to a `User` entity using the `convertToUserEntity` method. This method handles the conversion logic specific to your application.

Finally, we save the user using the `userService` and return the saved user as the response with a status of 200 OK.

By implementing data validation in the controller layer, you can ensure that the incoming request data meets the required criteria before further processing and persisting the data. This helps in maintaining data integrity and preventing the storage of invalid or inconsistent data in your Spring Boot application.

### Example

Here's an example of how you can use various validation annotations in a DTO (Data Transfer Object) class:

```java
public class UserDTO {
    
    @NotBlank(message = "Name is required")
    private String name;

    @Email(message = "Invalid email format")
    private String email;

    @Size(min = 6, max = 20, message = "Password must be between 6 and 20 characters")
    private String password;

    @Pattern(regexp = "\\d{3}-\\d{3}-\\d{4}", message = "Phone number must be in the format XXX-XXX-XXXX")
    private String phoneNumber;

    @NotNull(message = "Date of birth is required")
    @Past(message = "Date of birth must be in the past")
    private LocalDate dateOfBirth;

    // Getters and Setters
}
```

In this example, the `UserDTO` class represents a Data Transfer Object that contains various fields representing user data. We apply different validation annotations to these fields:

- `@NotBlank` ensures that the `name` field is not blank or empty.
- `@Email` validates that the `email` field is a valid email address.
- `@Size` specifies the minimum and maximum length of the `password` field.
- `@Pattern` applies a regular expression pattern to validate the `phoneNumber` field.
- `@NotNull` ensures that the `dateOfBirth` field is not null.
- `@Past` checks if the `dateOfBirth` field represents a date in the past.

These validation annotations provide built-in validation logic that is automatically triggered during request processing. If any of the validations fail, Spring Boot will automatically handle the validation errors and return a proper response with the error messages.

By using these validation annotations in your DTO class, you can enforce data validation rules on the incoming request data, ensuring that it meets the specified criteria before further processing or persisting it.

## Using exceptions

### ErrorResponse

`ErrorResponse` is a custom class used to represent an error response in a Spring Boot application. It typically contains information about an error that occurred during the processing of a request.

The structure and content of the `ErrorResponse` class can vary depending on the specific requirements of your application. However, it generally includes the following common elements:

1. Error Code: A unique code or identifier that represents the specific type of error. It can be useful for categorizing and identifying different types of errors in the system.

2. Error Message: A descriptive message that provides additional information about the error. It helps in understanding the cause or context of the error.

3. Additional Details: Optional additional information or data related to the error. This can include details such as error timestamps, stack traces, error fields, or any other relevant information that can assist in troubleshooting or resolving the error.

By encapsulating the error details in an `ErrorResponse` object, you can provide a standardized format for returning error responses from your API. This helps in consistent error handling and communication with clients consuming your API.

Here's an example of a simple `ErrorResponse` class:

```java
public class ErrorResponse {

    private String errorCode;
    private String errorMessage;
    private String additionalDetails;

    // Constructors, getters, and setters

    // ...

}
```

In this example, the `ErrorResponse` class has three properties: `errorCode`, `errorMessage`, and `additionalDetails`. You can provide appropriate constructors, getters, and setters to create and access the error response properties.

When an error occurs, you can create an instance of the `ErrorResponse` class, populate its properties with the relevant error information, and return it as the response from your API endpoint. The client consuming the API can then parse the response and extract the error details to handle the error appropriately.

Using a custom `ErrorResponse` class allows you to have more control over the structure and content of your error responses, making it easier to communicate meaningful error information to clients and facilitating the debugging and resolution of issues in your Spring Boot application.

### @ExceptionHandler

The `@ExceptionHandler` annotation in Spring Boot is used to handle exceptions in a centralized manner within a controller or an advice class. It allows you to define methods that specifically handle exceptions thrown during the execution of request mappings.

Here are the key aspects and use cases of `@ExceptionHandler`:

1. Custom Exception Handling: By annotating a method with `@ExceptionHandler`, you can define a custom handler for a specific exception or a group of related exceptions. This method will be invoked when the specified exception occurs, allowing you to provide custom logic to handle the exception and generate an appropriate response.

2. Fine-grained Exception Handling: The `@ExceptionHandler` annotation allows you to handle different types of exceptions separately. You can define multiple methods, each handling a specific exception type or a hierarchy of exception types. This allows for fine-grained control over the exception handling process.

3. Exception Handling Order: When multiple exception handling methods are defined, Spring Boot follows a resolution algorithm to determine which method to invoke based on the exception type. The algorithm considers the inheritance hierarchy of the exception types, ensuring that the most specific handler is chosen. If no matching handler is found, an exception can be propagated to a global exception handler.

4. Access to Exception Information: Exception handling methods annotated with `@ExceptionHandler` can have the exception object as a method parameter, allowing you to access the details of the exception. You can use this information to generate error messages, log the exception, or perform any other necessary actions.

Here's an example illustrating the usage of `@ExceptionHandler`:

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFoundException(ResourceNotFoundException ex) {
        ErrorResponse errorResponse = new ErrorResponse("Resource not found", ex.getMessage());
        return new ResponseEntity<>(errorResponse, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(Exception ex) {
        ErrorResponse errorResponse = new ErrorResponse("Internal Server Error", ex.getMessage());
        return new ResponseEntity<>(errorResponse, HttpStatus.INTERNAL_SERVER_ERROR);
    }

    // Other exception handling methods...

}
```

In this example, the `GlobalExceptionHandler` class is annotated with `@ControllerAdvice`, which allows it to handle exceptions thrown by controllers within the application. It contains two methods annotated with `@ExceptionHandler`.

The first method, `handleResourceNotFoundException`, handles the `ResourceNotFoundException` specifically. It creates an `ErrorResponse` object and returns a `ResponseEntity` with the error response and an HTTP status code of `NOT_FOUND`.

The second method, `handleGenericException`, acts as a fallback handler for any other exceptions not handled explicitly. It creates a generic error response and returns a `ResponseEntity` with an HTTP status code of `INTERNAL_SERVER_ERROR`.

By using `@ExceptionHandler`, you can handle exceptions in a centralized and consistent manner, providing appropriate error responses to clients. It helps to decouple the exception handling logic from the controller methods and promotes better error management in your Spring Boot application.

### @RestControllerAdvice

The `@RestControllerAdvice` annotation in Spring Boot is used to define a global exception handler and to apply advice to all `@RestController` classes in your application. It combines the functionalities of `@ControllerAdvice` and `@ResponseBody`, making it suitable for building RESTful APIs.

Here are the key aspects and use cases of `@RestControllerAdvice`:

1. Global Exception Handling: By annotating a class with `@RestControllerAdvice`, you can define methods that handle exceptions thrown by any `@RestController` within your application. These methods can be used to customize the error response sent back to the client when an exception occurs. You can handle specific exceptions or provide a generic fallback handler for any unhandled exceptions.

2. Fine-grained Control: `@RestControllerAdvice` allows you to selectively handle exceptions based on their types, making it easy to provide different error handling strategies for different types of exceptions. You can define multiple methods within the advice class, each handling a specific exception or a group of related exceptions.

3. Global Data Transformations: Besides exception handling, `@RestControllerAdvice` can also be used to perform global data transformations before the response is sent back to the client. For example, you can add common response headers, format the response payload, or perform any other necessary modifications.

4. Integration with Validation Errors: When used in combination with validation annotations, `@RestControllerAdvice` allows you to handle validation errors globally. You can define a method that handles `MethodArgumentNotValidException` or `ConstraintViolationException` and customize the error response with the validation error details.

Here's an example illustrating the usage of `@RestControllerAdvice`:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ErrorResponse handleResourceNotFoundException(ResourceNotFoundException ex) {
        // Create a custom error response for resource not found exception
        return new ErrorResponse("Resource not found", ex.getMessage());
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorResponse handleValidationException(MethodArgumentNotValidException ex) {
        // Extract validation errors and create a custom error response
        List<FieldError> fieldErrors = ex.getBindingResult().getFieldErrors();
        List<String> errorMessages = fieldErrors.stream()
                .map(fieldError -> fieldError.getField() + ": " + fieldError.getDefaultMessage())
                .collect(Collectors.toList());
        return new ErrorResponse("Validation Error", errorMessages);
    }

    // Other exception handling methods...

    // Other global data transformations methods...

}
```

In this example, the `GlobalExceptionHandler` class is annotated with `@RestControllerAdvice`. It contains methods annotated with `@ExceptionHandler` to handle specific exceptions, such as `ResourceNotFoundException` and `MethodArgumentNotValidException`. Each method handles the exception and returns a custom error response with the appropriate HTTP status code.

By using `@RestControllerAdvice`, you can centralize the exception handling and data transformations logic for your RESTful APIs, ensuring consistent error responses and simplifying error management across your application.

### Example

1. Create a User-Defined Exception:
   ```java
   public class CustomException extends RuntimeException {
       public CustomException(String message) {
           super(message);
       }
   }
   ```

2. Create an `ErrorResponse` Class:
   ```java
   public class ErrorResponse {
       private String errorCode;
       private String errorMessage;
       // Getters and Setters
   }
   ```

3. Create a Global Exception Handler:
   ```java
   @RestControllerAdvice
   public class GlobalExceptionHandler {
       @ExceptionHandler(CustomException.class)
       public ResponseEntity<ErrorResponse> handleCustomException(CustomException ex) {
           ErrorResponse errorResponse = new ErrorResponse();
           errorResponse.setErrorCode("CUSTOM_ERROR");
           errorResponse.setErrorMessage(ex.getMessage());
           return new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST);
       }
   }
   ```

4. Use the Custom Exception in a Controller:
   ```java
   @RestController
   public class UserController {
       @GetMapping("/users/{id}")
       public User getUser(@PathVariable Long id) {
           // Simulating a scenario where a user is not found
           throw new CustomException("User not found");
       }
   }
   ```

In this example, we have defined a `CustomException` class, which extends the `RuntimeException` class to create a user-defined exception. We have also created an `ErrorResponse` class to encapsulate the error details.

The `GlobalExceptionHandler` class is annotated with `@RestControllerAdvice` and contains a method annotated with `@ExceptionHandler(CustomException.class)`. This method handles the `CustomException` specifically and creates an `ErrorResponse` object with the appropriate error code and error message. It returns a `ResponseEntity` with the error response and an HTTP status code of `BAD_REQUEST`.

In the `UserController` class, we have a GET mapping for retrieving a user. In this example, we simulate a scenario where the requested user is not found, and we throw the `CustomException` with an appropriate error message.

When the `CustomException` is thrown from the `UserController`, the `handleCustomException` method in the `GlobalExceptionHandler` is invoked due to the `@ExceptionHandler` annotation. It creates an `ErrorResponse` object and returns it as the response with the specified HTTP status code.

This way, you can create and use a user-defined exception in your Spring Boot project, handle it globally using `@ExceptionHandler`, and customize the error response using the `ErrorResponse` class.

## Example

Here's an example of how you can handle data validation errors using Exceptions:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorResponse handleValidationException(MethodArgumentNotValidException ex) {
        BindingResult bindingResult = ex.getBindingResult();
        List<FieldError> fieldErrors = bindingResult.getFieldErrors();

        // Create an ErrorResponse object to hold the validation errors
        ErrorResponse errorResponse = new ErrorResponse();
        errorResponse.setErrorCode("VALIDATION_ERROR");

        // Loop through the field errors and add them to the error response
        for (FieldError fieldError : fieldErrors) {
            String fieldName = fieldError.getField();
            String errorMessage = fieldError.getDefaultMessage();
            errorResponse.addValidationError(fieldName, errorMessage);
        }

        return errorResponse;
    }
}
```

In this example, we have a `GlobalExceptionHandler` class annotated with `@RestControllerAdvice`, which allows it to handle exceptions thrown across multiple controllers.

The `handleValidationException` method is annotated with `@ExceptionHandler(MethodArgumentNotValidException.class)`, indicating that it will handle `MethodArgumentNotValidException` instances, which are thrown when request data fails validation.

Inside the method, we retrieve the `BindingResult` from the exception to get access to the validation errors. We loop through the field errors using `getFieldErrors()` and create an `ErrorResponse` object to hold the validation errors.

For each field error, we extract the field name and error message and add them to the `ErrorResponse` object using the `addValidationError()` method.

Finally, we return the populated `ErrorResponse` object with an HTTP status of `BAD_REQUEST`. The `ErrorResponse` class can be customized to suit your needs and may contain fields such as error code, timestamp, and a list of validation errors.

By using `@ExceptionHandler` in conjunction with `MethodArgumentNotValidException`, you can intercept and handle validation errors centrally in your Spring Boot project. This ensures consistent error handling and allows you to provide informative error responses to the client when data validation fails.

