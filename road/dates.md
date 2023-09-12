# Dealing with dates

## Spring Boot Backend

1. **Set up Spring Boot with MySQL**:

   Make sure you have the necessary dependencies in your `pom.xml` file for Spring Boot and MySQL:

   ```xml
   <dependencies>
       <!-- Spring Boot Starter Data JPA -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-data-jpa</artifactId>
       </dependency>
       <!-- MySQL Database Driver -->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
       </dependency>
       <!-- Spring Boot Starter Web -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   </dependencies>
   ```

   Configure your `application.properties` or `application.yml` to connect to your MySQL database:

   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/dataexample
   spring.datasource.username=yourUsername
   spring.datasource.password=yourPassword
   ```

2. **Create an Entity Class**:

   Create a Java class to represent the data you want to store in the database. In this case, you mentioned `id`, `name`, and `birth` (as a date). Here's an example:

   ```java
   @Entity
   public class Person {
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       private String name;
       private LocalDate birth;

       // Constructors, getters, setters, and other methods...
   }
   ```

3. **Create a Controller**:

   Create a controller class to handle incoming HTTP requests from the frontend:

   ```java
   @RestController
   @RequestMapping("/api/persons")
   public class PersonController {
       private final PersonRepository personRepository;

       @Autowired
       public PersonController(PersonRepository personRepository) {
           this.personRepository = personRepository;
       }

       // Define your API endpoints here...
   }
   ```

4. **Implement the API Endpoint**:

   Implement an endpoint to receive data from the frontend and store it in the database:

   ```java
   @PostMapping("/add")
   public ResponseEntity<String> addPerson(@RequestBody PersonRequest personRequest) {
       try {
           // Convert data from PersonRequest to a Person entity
           Person person = new Person();
           person.setName(personRequest.getName());
           person.setBirth(personRequest.getBirth());

           // Save the person entity to the database
           personRepository.save(person);

           return ResponseEntity.ok("Person added successfully");
       } catch (Exception e) {
           return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error adding person");
       }
   }
   ```

   In this code, `PersonRequest` is a DTO (Data Transfer Object) that you can create to receive data from the frontend. Make sure to validate the input data and handle exceptions appropriately in a production environment.

5. **DTO for Receiving Data**:

   Create a DTO class to receive data from the frontend:

   ```java
   public class PersonRequest {
       private String name;
       private LocalDate birth;

       // Getters and setters...
   }
   ```

6. **Create a Repository Interface**:

   Create a repository interface for the `Person` entity to perform database operations:

   ```java
   public interface PersonRepository extends JpaRepository<Person, Long> {
   }
   ```

You now have a Spring Boot API with an endpoint that can receive name and birthdate data from the frontend and store it in the MySQL database. You can test this endpoint by making a POST request to `/api/persons/add` with the JSON payload containing the `name` and `birth` fields.



## Angular FrontEnd with PrimeNg

To create an Angular component that allows you to select a date using PrimeNG's calendar component and send it to the server, you can follow these steps:

1. **Install PrimeNG**:

   If you haven't already, make sure you have PrimeNG installed in your Angular project. You can install it using npm:

   ```
   npm install primeng --save
   npm install primeicons --save
   ```

2. **Import Required Modules**:

   In your Angular module (e.g., `app.module.ts`), import the necessary modules for PrimeNG and Angular forms:

   ```typescript
   import { NgModule } from '@angular/core';
   import { FormsModule } from '@angular/forms';
   import { BrowserModule } from '@angular/platform-browser';
   import { CalendarModule } from 'primeng/calendar';

   @NgModule({
     imports: [BrowserModule, FormsModule, CalendarModule],
     // ...
   })
   export class AppModule {}
   ```

3. **Create the Date Selection Component**:

   Create a new component where you want to select a date. You can use the Angular CLI to generate a component:

   ```
   ng generate component date-selector
   ```

4. **Component Template**:

   In the HTML template of your component (`date-selector.component.html`), use PrimeNG's calendar component to select a date:

```html
<div>
  <p-calendar
    [(ngModel)]="selectedDate"
    dateFormat="dd/mm/yy"
    [showIcon]="true"
    [locale]="es"
  ></p-calendar>
</div>
<div>
  <button (click)="sendDate()">Submit Date</button>
</div>
```

5. **Component Class**:

   In the TypeScript file (`date-selector.component.ts`) of your component, define the `selectedDate` property and a method to send the selected date to the server:

   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-date-selector',
     templateUrl: './date-selector.component.html',
     styleUrls: ['./date-selector.component.css'],
   })
   export class DateSelectorComponent {
     selectedDate: Date;

     sendDate() {
       // You can now send this.selectedDate to your server using an HTTP service
       // For example, using Angular's HttpClient
       // Make an HTTP POST request to your server endpoint with the selected date.
     }
   }
   ```

6. **Sending Data to the Server**:

   To send the selected date to the server, you'll need to use Angular's `HttpClient` module to make an HTTP request. Import `HttpClient` in your component and use it within the `sendDate()` method to send the selected date to your server's API endpoint.

   Here's an example of how to make an HTTP POST request:

   ```typescript
   import { Component } from '@angular/core';
   import { HttpClient } from '@angular/common/http';

   @Component({
     // ...
   })
   export class DateSelectorComponent {
     selectedDate: Date;

     constructor(private http: HttpClient) {}

     sendDate() {
       const dateToSend = this.selectedDate.toISOString(); // Convert to ISO format or adjust as needed
       const apiUrl = 'your-server-api-url'; // Replace with your server's API endpoint

       this.http.post(apiUrl, { date: dateToSend }).subscribe(
         (response) => {
           // Handle the success response from the server
           console.log('Date sent successfully:', response);
         },
         (error) => {
           // Handle any errors that occur during the request
           console.error('Error sending date:', error);
         }
       );
     }
   }
   ```

7. **Use the Component**:

   Finally, include the `app-date-selector` selector in your application's template to use the date selection component.

   ```html
   <app-date-selector></app-date-selector>
   ```

Now, when you select a date using the PrimeNG calendar component and click the "Send Date" button, the selected date will be sent to your server's API endpoint. Make sure to replace `'your-server-api-url'` with the actual URL where your server's API is listening for date submissions.



