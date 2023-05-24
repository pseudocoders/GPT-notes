# Dependency injection and Services

## DI

Dependency Injection (DI) is a design pattern and a core concept in Angular that promotes loose coupling and modular development. It is a mechanism for providing the dependencies that a class or component needs from external sources, rather than creating them internally. The main idea behind DI is to separate the creation of dependencies from their usage, resulting in more maintainable, testable, and reusable code.

In Angular, DI is primarily used to manage the dependencies of components, services, and other objects within the application. Instead of manually creating instances of dependencies within a class, Angular's DI container takes care of creating and providing the necessary dependencies. Here's how DI works in Angular:

1. Provider Registration:
   Dependencies are registered as providers within Angular's dependency injection system. A provider is a configuration object that maps a token (a unique identifier) to a value or a factory function that creates the value.

2. Dependency Injection Tree:
   Angular maintains a hierarchical tree-like structure known as the dependency injection tree. The tree represents the relationships between the various components and services in an application.

3. Injection Token:
   Each dependency is associated with an injection token, which serves as a unique identifier for that dependency. The injection token is used to look up and retrieve the dependency from the DI container.

4. Dependency Resolution:
   When a class or component requests a dependency, Angular's DI container resolves it by traversing up the dependency injection tree. It searches for the requested dependency by using the associated injection token.

5. Dependency Creation:
   Once the DI container finds the provider associated with the injection token, it either creates a new instance of the dependency or retrieves a cached instance (depending on the provider configuration).

6. Dependency Injection:
   Finally, the resolved dependency is injected into the requesting class or component. This allows the class to use the dependency without having to worry about its creation or management.

The benefits of using DI in Angular include:

- Modular and reusable code: DI enables the easy replacement of dependencies, making it simpler to reuse components and services across the application.
- Testability: With DI, dependencies can be easily mocked or replaced during unit testing, allowing for more effective and isolated testing.
- Loose coupling: By separating the creation and usage of dependencies, DI reduces the coupling between classes, promoting better separation of concerns.
- Scalability and maintainability: DI simplifies the addition or modification of dependencies, making the application more flexible and maintainable.

Overall, DI in Angular promotes better software design and improves the overall architecture of an application by providing a flexible and robust way to manage dependencies.

## Services

In Angular, a service is a class that is responsible for providing specific functionality and data to other parts of the application. Services play a crucial role in Angular applications as they help in implementing business logic, data manipulation, communication with servers, and sharing data between different components.

Services are typically used to encapsulate reusable and common functionality that is independent of any specific component. They promote code reusability, maintainability, and separation of concerns by keeping the business logic separate from the components.

Here are some key characteristics and purposes of services in Angular:

1. **Data Sharing:** Services act as a central point for sharing data between components. They can store and manage application state or act as a data source for components.

2. **Dependency Injection:** Services are typically injected into components or other services using Angular's Dependency Injection mechanism. This allows components to access the functionality and data provided by the service.

3. **Separation of Concerns:** Services help in separating the business logic and data manipulation from the presentation logic of components. This improves code organization and maintainability.

4. **Reusability:** Services can be reused across multiple components or even multiple applications. This eliminates code duplication and promotes modular development.

5. **Asynchronous Operations:** Services can handle asynchronous operations such as HTTP requests, timers, or WebSocket connections. They provide methods and observables for components to subscribe to and receive the asynchronous results.

6. **State Management:** Services can be used for managing application state, especially in larger applications. They can store and update data that needs to be shared and synchronized across multiple components.

7. **Testing:** Services can be easily tested in isolation, making it simpler to write unit tests for the application's business logic and data manipulation.

Overall, services are a key building block in Angular applications. They encapsulate reusable functionality, enable data sharing, promote separation of concerns, and enhance code reusability and maintainability. By using services effectively, you can create modular, scalable, and maintainable Angular applications.

### Custom services

To create a custom service in Angular and inject it into your components, you can follow these steps:

1. Generate a new service file:
   - Use the Angular CLI command `ng generate service service-name` to generate a new service file. Replace `service-name` with the name you want to give your service.

2. Open the generated service file:
   - Open the generated service file, typically located at `src/app/service-name.service.ts`.

3. Define the service class:
   - In the service file, define your service class and its methods. For example:

   ```typescript
   import { Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root' // Register the service at the root level
   })
   export class ServiceNameService {
     constructor() { }

     // Define your service methods here
     public someMethod() {
       // ...
     }
   }
   ```

   In the above example, the `@Injectable` decorator is used to make the service injectable. The `providedIn: 'root'` option registers the service at the root level, making it available for injection throughout the application. When the providedIn property is set to "root", the service is registered at the root level. This means that a single instance of the service is shared across the entire application. The service becomes a singleton and is available for injection in any component or service within the application. The providedIn property can also be set to the name of a specific module or any other injectable type. In this case, the service is registered at the level of that specific module or injectable type. This creates a separate instance of the service for each module or injectable type, providing component-level or feature-level scoping.

4. Inject the service into a component:
   - To use the service in a component, you need to inject it. Open the component file where you want to use the service, typically located at `src/app/component-name.component.ts`.
   - Import the service and inject it into the component's constructor:

   ```typescript
   import { Component } from '@angular/core';
   import { ServiceNameService } from '../service-name.service';

   @Component({
     // Component configuration
   })
   export class ComponentNameComponent {
     constructor(private serviceName: ServiceNameService) { }

     // Component logic
   }
   ```

   In the above example, the `ServiceNameService` is imported and injected into the component's constructor as a private property. This allows the component to access and use the methods provided by the service.

5. Use the service in the component:
   - With the service injected, you can now use its methods and properties within your component:

   ```typescript
   export class ComponentNameComponent {
     constructor(private serviceName: ServiceNameService) { }

     public someMethod() {
       this.serviceName.someMethod(); // Call the service method
     }
   }
   ```

   In the above example, the `someMethod()` of the service is called within the component.

By following these steps, you can create a custom service in Angular and inject it into your components. This allows you to encapsulate reusable logic and share data and functionality across multiple components in your application.



#### Custom injectable services example

Let's analyze an example of Dependency Injection (DI) in Angular.

Consider the following scenario: We have an Angular component called `UserService` that requires an instance of an `HttpService` to make HTTP requests. Instead of creating an instance of `HttpService` within the `UserService` class, we'll use DI to inject the dependency.

First, we need to register the `HttpService` as a provider in the Angular module or component where it is available. We can do this by providing it in the `providers` array of the module or component decorator.

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class HttpService {
  // Implementation of HttpService methods
}
```

Next, we have the `UserService` component that requires an instance of `HttpService`. We can achieve this by injecting the `HttpService` dependency in the constructor of the `UserService` class.

```typescript
import { Injectable } from '@angular/core';
import { HttpService } from './http.service';

@Injectable()
export class UserService {
  constructor(private httpService: HttpService) {
    // ...
  }

  // UserService methods that use HttpService
}
```

In the above code, we see that the `HttpService` is passed as a parameter to the constructor of the `UserService` class. The `private` access modifier indicates that the `httpService` parameter should be assigned as an instance variable of the `UserService` class.

Now, when the `UserService` is instantiated or used elsewhere in the application, Angular's DI mechanism automatically resolves the `HttpService` dependency and provides an instance of it to the `UserService`. This means that the `UserService` can now access and use the methods and properties of the `HttpService` without having to create it explicitly.

By using DI, we achieve loose coupling between the `UserService` and `HttpService`, making it easier to maintain and test the code. Additionally, if we need to replace or modify the behavior of the `HttpService` in the future, we can simply provide a different implementation or mock it without affecting the `UserService`.

Overall, DI in Angular simplifies the management of dependencies, promotes modularity, and enhances testability and maintainability in Angular applications.

#### Custom injectable services example

Let's analyze an example of injecting a simple service into a component to understand Dependency Injection (DI) in Angular.

Consider the following example:

1. **SimpleService**
   ```typescript
   import { Injectable } from '@angular/core';

   @Injectable()
   export class SimpleService {
     getMessage(): string {
       return 'Hello from SimpleService!';
     }
   }
   ```

   Here, we have a simple service called `SimpleService` with a single method `getMessage()` that returns a greeting message.

2. **AppComponent**
   ```typescript
   import { Component } from '@angular/core';
   import { SimpleService } from './simple.service';

   @Component({
     selector: 'app-root',
     template: `
       <h1>{{ message }}</h1>
     `,
   })
   export class AppComponent {
     message: string;

     constructor(private simpleService: SimpleService) {
       this.message = this.simpleService.getMessage();
     }
   }
   ```

   In the `AppComponent`, we import the `SimpleService` and inject it into the component's constructor. The `private` access modifier signifies that the `simpleService` parameter should be assigned as an instance variable of the `AppComponent` class. In the constructor, we use the `simpleService` to retrieve the greeting message and assign it to the `message` property.

3. **AppModule**
   ```typescript
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { AppComponent } from './app.component';
   import { SimpleService } from './simple.service';

   @NgModule({
     declarations: [AppComponent],
     imports: [BrowserModule],
     providers: [SimpleService],
     bootstrap: [AppComponent],
   })
   export class AppModule {}
   ```

   In the `AppModule`, we import the `SimpleService` and include it in the `providers` array. This registers `SimpleService` as a provider within the Angular dependency injection system. The provider configuration allows the DI system to create an instance of `SimpleService` when requested by components.

Now, when the `AppComponent` is created, Angular's DI mechanism automatically resolves the `SimpleService` dependency and provides an instance of it to the `AppComponent`. The `getMessage()` method of `SimpleService` is called within the constructor, and the returned message is assigned to the `message` property.

As a result, when the component is rendered, the `message` property value is displayed in the template.

This example demonstrates how DI allows the `AppComponent` to use the functionality of the `SimpleService` without explicitly creating an instance of it. The service is provided to the component through DI, enabling loose coupling and code reusability.

By using DI in Angular, we can easily inject services, manage dependencies, and promote modular and maintainable application development.


### Built-in injectable services

Certainly! Here are the commonly used built-in injectable services in Angular:

1. **HttpClient**: The `HttpClient` service is used for making HTTP requests and handling responses. It provides methods like `get()`, `post()`, `put()`, and `delete()` to perform various types of HTTP requests.

2. **HttpInterceptor**: The `HttpInterceptor` interface allows you to intercept and modify HTTP requests and responses. Interceptors are used to add custom headers, handle authentication, modify request/response bodies, and perform other preprocessing or postprocessing tasks related to HTTP communication.

3. **Router**: The `Router` service provides navigation and routing functionality in Angular applications. It allows you to define routes, navigate between different views, and handle URL parameters. The `Router` service is essential for implementing client-side routing.

4. **ActivatedRoute**: The `ActivatedRoute` service provides information about the currently activated route. It gives you access to route parameters, query parameters, and other route-related data. It is typically used within components to access and react to route changes.

5. **FormBuilder**: The `FormBuilder` service provides a convenient way to create and manage Angular forms. It simplifies the process of creating form controls, groups, and arrays, and provides methods for form validation and manipulation.

6. **NgZone**: The `NgZone` service is responsible for handling Angular's change detection mechanism and triggering updates to the application's views. It allows you to run code within a specific zone, ensuring that Angular is aware of any changes made within that zone and performs the necessary change detection and view updates.

7. **Title**: The `Title` service allows you to dynamically set the title of the HTML document. It provides methods to update the title of the page based on the current state or content of your application.

8. **Location**: The `Location` service provides an API for interacting with the browser's URL. It allows you to read the current URL, navigate to different URLs, and listen for URL changes. This service is particularly useful when working with deep linking, bookmarking, or managing browser history.

1. **Renderer2**: The `Renderer2` service provides a way to manipulate the DOM in a platform-independent manner. It allows you to create, modify, and remove DOM elements, apply styles, and listen for events. The `Renderer2` service is commonly used when you need to perform DOM manipulations in Angular.

2. **ElementRef**: The `ElementRef` service provides a reference to the underlying DOM element of a component or directive. It allows you to access and manipulate the properties and methods of the DOM element directly. The `ElementRef` service is often used when you need to work with a specific DOM element within a component or directive.

3. **ChangeDetectorRef**: The `ChangeDetectorRef` service is responsible for triggering change detection manually in Angular. It allows you to mark a component or directive as dirty and initiate the change detection process. The `ChangeDetectorRef` service is useful in scenarios where changes occur outside of Angular's default change detection mechanism.

4. **ErrorHandler**: The `ErrorHandler` service is used to handle errors that occur within an Angular application. It allows you to customize error handling and implement your own error logging, reporting, or recovery logic. By extending the `ErrorHandler` class, you can provide a centralized error handling strategy for your application.

5. **DOCUMENT**: The `DOCUMENT` token represents the global document object in a browser environment. It allows you to access and manipulate the document directly within your Angular application. The `DOCUMENT` token is often used when you need to perform operations on the document that are not covered by other Angular services.

1. **HttpParams**: The `HttpParams` class is used for creating and manipulating HTTP parameters for requests made with the `HttpClient` service. It provides methods to set, append, and delete individual parameters, and to generate a URL-encoded string representation of the parameters.

2. **HttpHeaders**: The `HttpHeaders` class is used for creating and manipulating HTTP headers for requests made with the `HttpClient` service. It allows you to set, append, and delete individual headers, and provides convenience methods for working with common headers like content type, authorization, and cache control.

3. **HttpClientTestingModule**: The `HttpClientTestingModule` is a module provided by Angular's testing framework for mocking HTTP requests and responses during unit testing. It allows you to simulate HTTP interactions and write tests for components and services that use the `HttpClient` service.

4. **TestBed**: The `TestBed` service is used for configuring and creating instances of Angular components and services during unit testing. It provides methods for creating testing modules, compiling components, and injecting dependencies. The `TestBed` service is a key component of Angular's testing infrastructure.

5. **RouterTestingModule**: The `RouterTestingModule` is a module provided by Angular's testing framework for testing components and services that rely on Angular's router. It provides a mock implementation of the router, allowing you to simulate route changes and test navigation-related functionality.

####  Built-in injectable service example

The `Title` service in Angular allows you to dynamically set the title of the HTML document. Here's an example of how to use the `Title` service:

1. Import the `Title` service from `@angular/platform-browser`:

```typescript
import { Title } from '@angular/platform-browser';
```

2. Inject the `Title` service into your component's constructor:

```typescript
constructor(private titleService: Title) { }
```

3. Set the title using the `setTitle()` method of the `Title` service. You can set the title in the `ngOnInit()` lifecycle hook or any other appropriate place in your component:

```typescript
ngOnInit() {
  this.titleService.setTitle('My Angular App');
}
```

In the above example, the `setTitle()` method is used to set the title of the HTML document to "My Angular App". This will update the title displayed in the browser's title bar or tab.

By dynamically setting the title using the `Title` service, you can customize the title based on the current state or content of your application. This can be particularly useful for improving SEO, providing context-aware titles, or enhancing the user experience by displaying relevant information in the browser's title area.






