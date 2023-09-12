# Sending emails from server

To create a Spring Boot Java server that can send emails through an API, you'll need to follow these steps:

1. Set up a Spring Boot Project:
   Start by creating a new Spring Boot project or use an existing one. You can use Spring Initializr to quickly generate a Spring Boot project with the necessary dependencies.

2. Add Required Dependencies:
   In your project's `pom.xml` file, add the following dependencies for sending emails using Spring:

   ```xml
   <dependencies>
       <!-- Spring Boot Starter -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter</artifactId>
       </dependency>

       <!-- Spring Boot Starter for Mail -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-mail</artifactId>
       </dependency>
   </dependencies>
   ```

3. Configure Email Properties:
   Configure your email server properties in the `application.properties` or `application.yml` file. For example:

   ```properties
   spring.mail.host=your-smtp-server.com
   spring.mail.port=587
   spring.mail.username=your-email@example.com
   spring.mail.password=your-email-password
   spring.mail.properties.mail.smtp.auth=true
   spring.mail.properties.mail.smtp.starttls.enable=true
   ```

   Be sure to replace `your-smtp-server.com`, `your-email@example.com`, and `your-email-password` with your actual SMTP server details and email credentials.

4. Create an Email Service:
   Create a service class that handles email sending. Here's an example of a simple `EmailService`:

   ```java
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.mail.javamail.JavaMailSender;
   import org.springframework.mail.javamail.MimeMessageHelper;
   import org.springframework.stereotype.Service;

   import javax.mail.MessagingException;
   import javax.mail.internet.MimeMessage;

   @Service
   public class EmailService {

       private final JavaMailSender javaMailSender;

       @Autowired
       public EmailService(JavaMailSender javaMailSender) {
           this.javaMailSender = javaMailSender;
       }

       public void sendEmail(String to, String subject, String body) {
           MimeMessage message = javaMailSender.createMimeMessage();
           MimeMessageHelper helper = new MimeMessageHelper(message);

           try {
               helper.setTo(to);
               helper.setSubject(subject);
               helper.setText(body, true); // true indicates HTML content

               javaMailSender.send(message);
           } catch (MessagingException e) {
               e.printStackTrace();
               // Handle exceptions as needed
           }
       }
   }
   ```

5. Create an API Endpoint:
   Create a RESTful API endpoint that allows you to send emails. You can do this by creating a controller class:

   ```java
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class EmailController {

       private final EmailService emailService;

       @Autowired
       public EmailController(EmailService emailService) {
           this.emailService = emailService;
       }

       @PostMapping("/send-email")
       public void sendEmail(@RequestBody EmailRequest emailRequest) {
           emailService.sendEmail(emailRequest.getTo(), emailRequest.getSubject(), emailRequest.getBody());
       }
   }
   ```

   Here, `EmailRequest` is a simple DTO (Data Transfer Object) class that holds the email details like recipient, subject, and body.

6. Run Your Application:
   Start your Spring Boot application, and it will run a RESTful API endpoint at `/send-email` that allows you to send emails by making POST requests.

Remember to handle exceptions and security properly in a production environment, such as implementing authentication and authorization for your email sending API. Also, consider using environment variables or a configuration server for sensitive information like email credentials instead of hardcoding them in your properties file.
