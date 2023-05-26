# CORS implementation in Spring Boot

CORS (Cross-Origin Resource Sharing) is a mechanism that allows web browsers to securely make cross-origin requests from one domain to another. It is an important consideration when implementing a Spring Boot RESTful API because it controls access to your API from different origins (domains).

Here's why CORS is important:

1. Cross-Origin Requests: CORS enables clients (such as web browsers) to make requests to your RESTful API from a different domain, port, or protocol. Without CORS, browsers restrict cross-origin requests due to security concerns, known as the Same-Origin Policy.

2. Web Application Integration: In modern web development, it is common to have web applications that consume APIs from different origins. For example, an API hosted on `api.example.com` may be accessed by a web application hosted on `app.example.com`. CORS allows these applications to communicate and exchange data securely.

3. Security: CORS provides a secure way to control and restrict access to your API. By specifying the allowed origins, methods, headers, and other CORS configuration options, you can define a whitelist of trusted origins that are allowed to access your API. This helps prevent unauthorized access or abuse of your API from malicious domains.

4. Flexibility: CORS allows you to define granular access control policies for your API. You can specify which HTTP methods (e.g., GET, POST, PUT, DELETE) are allowed, which headers can be included in the requests, and other options to fine-tune the behavior of cross-origin requests. This gives you control over how your API is consumed by clients.

By implementing CORS correctly in your Spring Boot RESTful API, you can ensure that your API is accessible to authorized clients from different domains while maintaining security and control. It promotes interoperability and integration between web applications and APIs, enabling a more seamless and efficient development process.

To implement CORS (Cross-Origin Resource Sharing) in a Spring Boot project, you can use the `@CrossOrigin` annotation or configure CORS globally.

1. Using `@CrossOrigin` annotation:
   - Apply the `@CrossOrigin` annotation on specific controller methods or controller classes.
   - Specify the allowed origins, methods, headers, and other CORS configuration options.
   - For example:

     ```java
     @RestController
     @RequestMapping("/api/users")
     @CrossOrigin(origins = "http://example.com", methods = {RequestMethod.GET, RequestMethod.POST})
     public class UserController {
         // Controller methods
     }
     ```

   - In the above example, the `@CrossOrigin` annotation allows requests from `http://example.com` and allows only `GET` and `POST` methods.

2. Configuring CORS globally:
   - Create a configuration class that implements `WebMvcConfigurer` or extend `WebMvcConfigurerAdapter`.
   - Override the `addCorsMappings` method to configure global CORS settings.
   - For example:

     ```java
     @Configuration
     public class CorsConfig implements WebMvcConfigurer {
         @Override
         public void addCorsMappings(CorsRegistry registry) {
             registry.addMapping("/api/**")
                     .allowedOrigins("http://example.com")
                     .allowedMethods("GET", "POST")
                     .allowedHeaders("*");
         }
     }
     ```

   - In the above example, requests to paths starting with `/api/` are allowed from `http://example.com`, with `GET` and `POST` methods allowed, and all headers permitted.

By implementing CORS, you allow cross-origin requests from specified origins to access your RESTful API. This helps in enabling communication between different domains or ports, ensuring secure and controlled data exchange.

Remember to configure CORS according to your specific requirements, such as specifying the allowed origins, methods, headers, and other options to meet your application's needs.


## withCredentials

When accessing a Spring Boot API from a frontend application, the `withCredentials` option refers to whether or not to include cookies and HTTP authentication credentials (e.g., session ID, JWT) in the cross-origin requests made by the frontend application.

Here are the key differences between using `withCredentials` and not using it:

1. Without `withCredentials`:
   - Cookies and authentication credentials are not included in cross-origin requests.
   - The API server may not recognize the client as authenticated, and certain protected resources or endpoints may be inaccessible.
   - This option is suitable when the API and frontend application are hosted on the same origin (domain, protocol, and port).

2. With `withCredentials`:
   - Cookies and authentication credentials are included in cross-origin requests.
   - The API server recognizes the client as authenticated and grants access to protected resources or endpoints.
   - This option is necessary when the API and frontend application are hosted on different origins and require authentication and session management across domains.

The choice between using `withCredentials` or not depends on your specific use case and security requirements:

- If the frontend application and API are hosted on the same origin, it's generally safer and more desirable to omit `withCredentials`. This ensures that cookies and authentication credentials are not exposed unnecessarily to potential cross-site scripting (XSS) attacks.
- If the frontend application and API are hosted on different origins and require authentication and session management across domains, you should use `withCredentials`. This allows the frontend application to send the necessary cookies and authentication credentials with cross-origin requests, ensuring a seamless and secure user experience.

In summary, the more desirable option depends on whether you need to authenticate and maintain session state across different origins. If not, it's generally recommended to omit `withCredentials` to limit the exposure of cookies and authentication credentials. However, if cross-origin authentication is required, using `withCredentials` is necessary to ensure proper authentication and access to protected resources.

So, if your Spring Boot AJAX application use authentication via JSESSION ID or JWT, you should set the `withCredentials` option to `true` when configuring CORS. This allows the browser to include the authentication credentials (e.g., cookies, authorization headers) in cross-origin requests.

Here's why `withCredentials` is necessary in this both scenarios:

1. JSESSION ID: When using session-based authentication with JSESSION ID, the server typically issues a session cookie upon successful authentication. The browser automatically sends this cookie with subsequent requests to the same origin. However, for cross-origin requests, the browser does not include the session cookie by default. By setting `withCredentials` to `true`, the browser includes the session cookie in the cross-origin requests, allowing the server to recognize the user's session and authenticate the requests.

2. JWT: When using JWT (JSON Web Tokens) for authentication, the token is usually sent in the `Authorization` header of the HTTP request. The browser needs to include this header in cross-origin requests for the server to authenticate the requests. Setting `withCredentials` to `true` ensures that the browser includes the `Authorization` header and any other necessary headers when making cross-origin requests.

In summary, when using session-based authentication with JSESSION ID or JWT in a Spring Boot AJAX application, you should configure CORS with `withCredentials` set to `true`. This allows the browser to include the necessary authentication credentials (cookies or headers) in cross-origin requests, ensuring that the server can authenticate and authorize the requests correctly.

## preflight

Preflight requests are a mechanism used by modern web browsers to check if a cross-origin request (a request sent from one domain to another) is allowed by the server. These preflight requests are sent automatically by the browser as an initial step before sending the actual request.

When making cross-origin requests with certain HTTP methods or custom headers, the browser first sends a preflight OPTIONS request to the server to check if the actual request is allowed. The server needs to respond to this preflight request with appropriate CORS headers to indicate whether the actual request is allowed.

The purpose of preflight requests is to ensure that the server is configured to handle cross-origin requests safely and securely. It helps prevent unauthorized access to resources and protects against potential security vulnerabilities.

Preflight requests are typically sent in the following scenarios:

1. Request methods other than simple methods: When making requests with methods other than GET, POST, or HEAD, the browser sends a preflight request to check if the server allows the requested method.

2. Request headers other than simple headers: When including custom headers in the request, the browser sends a preflight request to check if the server allows those headers.

The preflight request is an HTTP OPTIONS request that includes additional headers, such as `Access-Control-Request-Method` and `Access-Control-Request-Headers`, to indicate the intended method and headers of the actual request.

The server should respond to the preflight request with appropriate CORS (Cross-Origin Resource Sharing) headers that specify which origins, methods, and headers are allowed for the actual request. These headers include `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`, and others.

Once the browser receives a successful response to the preflight request, it proceeds to send the actual request if it is allowed based on the CORS headers received. If the preflight request fails or the server does not respond with the necessary CORS headers, the browser will prevent the actual request from being sent.

By conducting preflight requests, browsers ensure that cross-origin requests comply with CORS policies and enhance security by preventing unauthorized access to sensitive resources. Servers need to handle these preflight requests correctly by returning appropriate CORS headers to allow or deny the actual request based on the defined policies.

## CORS in a filter using withCredentials with preflight

Here's an example of implementing a CORS filter in a Spring Boot application to allow cross-origin requests with credentials (`withCredentials` set to `true`) for an AJAX SB application:

1. Create a class `CorsFilter` and implement the `javax.servlet.Filter` interface:

```java
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class CorsFilter implements Filter {

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;

        response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
        response.setHeader("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept, Authorization");

        if ("OPTIONS".equalsIgnoreCase(request.getMethod())) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            chain.doFilter(req, res);
        }
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // Initialization logic, if needed
    }

    @Override
    public void destroy() {
        // Cleanup logic, if needed
    }
}
```

The code checks if the current request method is OPTIONS. If it is, it sets the response status to `HttpServletResponse.SC_OK` (HTTP status code 200) and bypasses the remaining filter chain. This indicates to the browser that the preflight request is successful and the actual request can be sent.

If the request method is not OPTIONS, meaning it is an actual request, the code proceeds with the filter chain by invoking `chain.doFilter(req, res)`. This allows the request to pass through the remaining filters and reach the appropriate controller or endpoint for processing.

By handling the OPTIONS requests separately in the CORS filter, you ensure that the proper CORS headers are returned for preflight requests, allowing the browser to determine if the actual request should be allowed. This is an essential step in enabling cross-origin requests and adhering to the CORS mechanism.


2. Register the filter in your application configuration:

```java
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public FilterRegistrationBean<CorsFilter> corsFilterRegistration() {
        FilterRegistrationBean<CorsFilter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new CorsFilter());
        registrationBean.addUrlPatterns("/*");
        registrationBean.setName("corsFilter");
        registrationBean.setOrder(Ordered.HIGHEST_PRECEDENCE);
        return registrationBean;
    }
}
```

In this example, the `CorsFilter` intercepts incoming requests and adds the necessary CORS headers to allow cross-origin requests. The `Access-Control-Allow-Origin` header is set dynamically based on the request's `Origin` header. The `Access-Control-Allow-Credentials` header is set to `true` to enable credentials in cross-origin requests.

The `FilterRegistrationBean` is used to register the `CorsFilter` in the application configuration, with a URL pattern of `/*` to apply the CORS filter to all requests.

With this configuration, your Spring Boot application should allow cross-origin requests from AJAX applications, including requests with credentials (`withCredentials` set to `true`).

Remember to customize the CORS configuration according to your specific requirements, such as allowed methods, headers, and origins. Additionally, ensure that the Spring Boot application is properly secured and handles authentication and authorization as needed.
