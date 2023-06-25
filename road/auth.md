# Auth

## Introduction

>
> :red_circle: Explore and learn: [**HTTP in GPT-notes**](../basics/http.md)
>
> :red_circle: Explore and learn: [**Authentication in GPT-notes**](../basics/auth.md)
>

## Cookies based sessions

To authenticate an AJAX frontend and maintain a session until a logout request, you can follow these general steps:

1. Server-side Implementation:
   - When the user logs in, the server should create a session for that user.
   - The session could be managed using a server-side technology like PHP, Node.js with Express, or any other framework you're using.
   - Typically, the session information is stored on the server-side, and a session ID is sent back to the client as a cookie.

2. Client-side Implementation:
   - On the client-side, you need to send the session ID with each subsequent AJAX request to authenticate the user.
   - You can extract the session ID from the cookie or use other methods depending on your implementation.
   - Add the session ID to the headers or parameters of your AJAX requests, depending on the API design.

3. Server-side Session Verification:
   - On the server-side, you need to validate the session for each AJAX request.
   - When a request is received, the server should check if the session ID provided in the request matches a valid session in the session store.
   - If the session ID is valid, it means the user is authenticated, and you can proceed with the request. Otherwise, you can return an error response.

4. Session Timeout and Renewal:
   - To maintain the session until a logout request, you should handle session timeouts.
   - Set an appropriate expiration time for the session when it is created.
   - Whenever a request is made with a valid session ID, you can update the session's expiration time to extend the session's duration.
   - If a session expires, subsequent AJAX requests will fail the session verification, and the user will need to log in again.

5. Logout Request:
   - When the user initiates a logout request, the server should destroy the corresponding session.
   - This could involve removing the session from the session store and clearing the session ID cookie on the client-side.
   - After a successful logout, subsequent AJAX requests with the previous session ID should fail the session verification.

By following these steps, you can authenticate an AJAX frontend using session-based authentication and maintain the session until a logout request is received. Remember to consider security measures like protecting against session hijacking, using HTTPS for secure communication, and implementing other relevant security best practices in your application.


### Example: Java cookies based sessions

Let's continue with the example of using a Java servlet on the backend and ES6 on the frontend. In this example, the servlet will handle three types of requests: login, logout, and check.

Backend (Java Servlet):

1. Create a servlet class, for example, `SessionServlet.java`, and map it to a URL pattern, such as `/session`.

```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class SessionServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        String action = request.getParameter("action");
        if (action != null && !action.isEmpty()) {
            HttpSession session = request.getSession();

            if (action.equals("login")) {
                String username = request.getParameter("username");
                String password = request.getParameter("password");
                boolean isAuthenticated = authenticate(username, password);

                if (isAuthenticated) {
                    session.setAttribute("username", username);
                    response.setStatus(HttpServletResponse.SC_OK);
                } else {
                    response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
                }
            } else if (action.equals("logout")) {
                session.invalidate();
                response.setStatus(HttpServletResponse.SC_OK);
            }
        } else {
            response.setStatus(HttpServletResponse.SC_BAD_REQUEST);
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        HttpSession session = request.getSession(false);

        if (session != null && session.getAttribute("username") != null) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        }
    }

    private boolean authenticate(String username, String password) {
        // Perform authentication logic here
        // Return true if authentication is successful, false otherwise
    }
}
```

Frontend (ES6):

1. Create an HTML file, for example, `index.html`, with the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Session Example</title>
</head>
<body>
    <h1>Session Example</h1>

    <button id="loginButton">Login</button>
    <button id="logoutButton">Logout</button>
    <button id="checkButton">Check</button>

    <script src="script.js"></script>
</body>
</html>
```

2. Create a JavaScript file, for example, `script.js`, and add the following code:

```javascript
document.getElementById('loginButton').addEventListener('click', function () {
    fetch('/session', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        body: 'action=login&username=myusername&password=mypassword',
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // Login successful
                console.log('Login successful!');
            } else {
                // Login failed
                console.log('Login failed!');
            }
        })
        .catch(error => {
            console.error('An error occurred during login:', error);
        });
});

document.getElementById('logoutButton').addEventListener('click', function () {
    fetch('/session', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        body: 'action=logout',
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // Logout successful
                console.log('Logout successful!');
            } else {
                // Logout failed
                console.log('Logout failed!');
           

 }
        })
        .catch(error => {
            console.error('An error occurred during logout:', error);
        });
});

document.getElementById('checkButton').addEventListener('click', function () {
    fetch('/session', {
        method: 'GET',
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // User has an active session
                console.log('User has an active session.');
            } else {
                // User does not have a session
                console.log('User does not have a session.');
            }
        })
        .catch(error => {
            console.error('An error occurred during session check:', error);
        });
});
```

In this example, the JavaScript code uses the Fetch API to send AJAX requests to the `/session` URL for login, logout, and session check.

The login request sends the username and password as parameters in the request body. If the authentication is successful, the server creates a session and stores the username in the session. If the authentication fails, an unauthorized status is returned.

The logout request invalidates the session on the server.

The session check request uses the GET method and checks if the user has an active session. If the user has a session, an OK status is returned; otherwise, an unauthorized status is returned.

Remember to adjust the code to fit your project structure and requirements, and implement appropriate security measures, such as secure communication (HTTPS) and protecting against session hijacking or session fixation vulnerabilities.

Note: Don't forget to configure and deploy your servlet on a servlet container, such as Tomcat, to handle the requests.


### Example: PHP cookies based sessions

Here's an example of using PHP on the backend instead of a servlet.

Backend (PHP):

1. Create a PHP file, for example, `session.php`, and place the following code:

```php
<?php

// Start or resume the session
session_start();

// Function to authenticate the user
function authenticate($username, $password)
{
    // Perform authentication logic here
    // Return true if authentication is successful, false otherwise
}

// Handle login, logout, and check requests
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $action = $_POST['action'] ?? '';

    if ($action === 'login') {
        $username = $_POST['username'] ?? '';
        $password = $_POST['password'] ?? '';

        if (authenticate($username, $password)) {
            $_SESSION['username'] = $username;
            http_response_code(200); // Login successful
        } else {
            http_response_code(401); // Unauthorized
        }
    } elseif ($action === 'logout') {
        session_destroy();
        http_response_code(200); // Logout successful
    } else {
        http_response_code(400); // Bad request
    }
} elseif ($_SERVER['REQUEST_METHOD'] === 'GET') {
    if (isset($_SESSION['username'])) {
        http_response_code(200); // User has an active session
    } else {
        http_response_code(401); // User does not have a session
    }
}
?>
```

Frontend (ES6):

1. Create an HTML file, for example, `index.html`, with the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Session Example</title>
</head>
<body>
    <h1>Session Example</h1>

    <button id="loginButton">Login</button>
    <button id="logoutButton">Logout</button>
    <button id="checkButton">Check</button>

    <script src="script.js"></script>
</body>
</html>
```

2. Create a JavaScript file, for example, `script.js`, and add the following code:

```javascript
document.getElementById('loginButton').addEventListener('click', function () {
    fetch('session.php', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        body: 'action=login&username=myusername&password=mypassword',
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // Login successful
                console.log('Login successful!');
            } else {
                // Login failed
                console.log('Login failed!');
            }
        })
        .catch(error => {
            console.error('An error occurred during login:', error);
        });
});

document.getElementById('logoutButton').addEventListener('click', function () {
    fetch('session.php', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        body: 'action=logout',
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // Logout successful
                console.log('Logout successful!');
            } else {
                // Logout failed
                console.log('Logout failed!');
            }
        })
        .catch(error => {
            console.error('An error occurred during logout:', error);
        });
});

document.getElementById('checkButton').addEventListener('click', function () {
    fetch('session.php', {
        method: 'GET',
        credentials: 'include' // Send cookies with the request
    })
        .then(response

 => {
            if (response.ok) {
                // User has an active session
                console.log('User has an active session.');
            } else {
                // User does not have a session
                console.log('User does not have a session.');
            }
        })
        .catch(error => {
            console.error('An error occurred during session check:', error);
        });
});
```

In this example, the PHP code handles the login, logout, and check requests. The `session_start()` function is used to start or resume the session. The `authenticate()` function is responsible for the authentication logic.

The login request checks the provided username and password using the `authenticate()` function. If the authentication is successful, the username is stored in the session. If the authentication fails, an unauthorized status is returned.

The logout request destroys the session using `session_destroy()`.

The session check request checks if the user has an active session by verifying the presence of the `username` value in the session array.

Remember to adjust the code to fit your project structure and requirements, and implement appropriate security measures, such as secure communication (HTTPS) and protecting against session hijacking or session fixation vulnerabilities.

Make sure to have a web server (such as Apache) configured to interpret PHP files and serve them correctly.


### Example: Node.js cookies based sessions


Certainly! Here's an example of using Node.js on the backend instead of a servlet or PHP.

Backend (Node.js):

1. Install the required dependencies by running the following command in your project directory:

```
npm install express
```

2. Create a new JavaScript file, for example, `server.js`, and add the following code:

```javascript
const express = require('express');
const session = require('express-session');

const app = express();

// Session middleware
app.use(session({
    secret: 'your-secret-key', // Change this to a secure secret key
    resave: false,
    saveUninitialized: true,
    cookie: { secure: false } // Set 'secure: true' if using HTTPS
}));

// Function to authenticate the user
function authenticate(username, password) {
    // Perform authentication logic here
    // Return true if authentication is successful, false otherwise
}

// Handle login, logout, and check requests
app.post('/session', (req, res) => {
    const action = req.body.action;

    if (action === 'login') {
        const { username, password } = req.body;

        if (authenticate(username, password)) {
            req.session.username = username;
            res.sendStatus(200); // Login successful
        } else {
            res.sendStatus(401); // Unauthorized
        }
    } else if (action === 'logout') {
        req.session.destroy();
        res.sendStatus(200); // Logout successful
    } else {
        res.sendStatus(400); // Bad request
    }
});

app.get('/session', (req, res) => {
    if (req.session.username) {
        res.sendStatus(200); // User has an active session
    } else {
        res.sendStatus(401); // User does not have a session
    }
});

// Start the server
app.listen(3000, () => {
    console.log('Server listening on port 3000');
});
```

Frontend (ES6):

1. Create an HTML file, for example, `index.html`, with the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Session Example</title>
</head>
<body>
    <h1>Session Example</h1>

    <button id="loginButton">Login</button>
    <button id="logoutButton">Logout</button>
    <button id="checkButton">Check</button>

    <script src="script.js"></script>
</body>
</html>
```

2. Create a JavaScript file, for example, `script.js`, and add the following code:

```javascript
document.getElementById('loginButton').addEventListener('click', function () {
    fetch('/session', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            action: 'login',
            username: 'myusername',
            password: 'mypassword'
        }),
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // Login successful
                console.log('Login successful!');
            } else {
                // Login failed
                console.log('Login failed!');
            }
        })
        .catch(error => {
            console.error('An error occurred during login:', error);
        });
});

document.getElementById('logoutButton').addEventListener('click', function () {
    fetch('/session', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            action: 'logout'
        }),
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (

response.ok) {
                // Logout successful
                console.log('Logout successful!');
            } else {
                // Logout failed
                console.log('Logout failed!');
            }
        })
        .catch(error => {
            console.error('An error occurred during logout:', error);
        });
});

document.getElementById('checkButton').addEventListener('click', function () {
    fetch('/session', {
        method: 'GET',
        credentials: 'include' // Send cookies with the request
    })
        .then(response => {
            if (response.ok) {
                // User has an active session
                console.log('User has an active session.');
            } else {
                // User does not have a session
                console.log('User does not have a session.');
            }
        })
        .catch(error => {
            console.error('An error occurred during session check:', error);
        });
});
```

In this example, we use the Express.js framework to handle the backend routing and session management. The `express-session` middleware is used for session management.

The `authenticate()` function is responsible for the authentication logic. You can customize it to fit your authentication requirements.

The login request checks the provided username and password using the `authenticate()` function. If the authentication is successful, the username is stored in the session. If the authentication fails, an unauthorized status is returned.

The logout request destroys the session using `req.session.destroy()`.

The session check request checks if the user has an active session by verifying the presence of the `username` value in the session object.

Remember to adjust the code to fit your project structure and requirements, and implement appropriate security measures, such as secure communication (HTTPS) and protecting against session hijacking or session fixation vulnerabilities.

To run the Node.js server, make sure you have Node.js installed. Then, in the project directory, run the following command:

```
node server.js
```

This will start the server on port 3000. Open the HTML file in a web browser, and you can interact with the login, logout, and session check buttons.

## Sharing information between servlets at servlet-thread level

In a servlet-based application, instances of the same servlet can share variable values and communicate with each other by utilizing shared class variables. These shared variables are initialized using the `init()` method of the servlet. However, it's important to note that this technique is not inherently thread-safe, and additional synchronization mechanisms need to be implemented to ensure thread safety if multiple threads will access and modify the shared variables concurrently.

To initialize shared variables using the `init()` method, you can follow these steps:

1. Declare the shared variables as class variables (static variables) within the servlet class. For example:

```java
public class MyServlet extends HttpServlet {
    private static int sharedVariable;
    // ...
}
```

2. Override the `init()` method of the servlet and initialize the shared variables within it. This method is called by the servlet container when the servlet is first loaded or initialized. For example:

```java
public class MyServlet extends HttpServlet {
    private static int sharedVariable;

    @Override
    public void init() throws ServletException {
        super.init();

        // Initialize shared variables
        sharedVariable = 0;
    }

    // ...
}
```

3. Access and modify the shared variables within the servlet's methods. Since the shared variables are static, their values are shared across all instances of the servlet. For example:

```java
public class MyServlet extends HttpServlet {
    private static int sharedVariable;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Access the shared variable
        int value = sharedVariable;

        // Modify the shared variable
        sharedVariable++;

        // ...
    }

    // ...
}
```

It's important to note that this technique is not thread-safe by default. If multiple threads access and modify the shared variables concurrently, race conditions and other synchronization issues may occur. To ensure thread safety, you can use synchronization mechanisms such as `synchronized` blocks or locks to control access to the shared variables and ensure that only one thread can modify them at a time.

For example, you can use a `synchronized` block to synchronize access to the shared variable within the servlet's methods:

```java
public class MyServlet extends HttpServlet {
    private static int sharedVariable;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        synchronized (MyServlet.class) {
            // Access the shared variable
            int value = sharedVariable;

            // Modify the shared variable
            sharedVariable++;

            // ...
        }
    }

    // ...
}
```

By synchronizing access to the shared variable, you ensure that only one thread can read or modify it at a time, thus preventing concurrent access issues.

In summary, instances of the same servlet can share variable values and communicate with each other by utilizing shared class variables. These variables are initialized using the `init()` method. However, to ensure thread safety when multiple threads access and modify the shared variables, additional synchronization mechanisms, such as `synchronized` blocks or locks, should be implemented.

### Counter example

Here's an example of a servlet-based application where two instances of the same servlet share a requests counter using a synchronized shared class variable:

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class RequestCounterServlet extends HttpServlet {
    private static int requestCounter = 0;

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        incrementRequestCounter();

        // Generate the response
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<h1>Request Counter Servlet</h1>");
        out.println("<p>Total Requests: " + getRequestCounter() + "</p>");
    }

    private synchronized void incrementRequestCounter() {
        requestCounter++;
    }

    private synchronized int getRequestCounter() {
        return requestCounter;
    }
}
```

In this example, we have a servlet named `RequestCounterServlet` that extends `HttpServlet`. The servlet contains a shared class variable `requestCounter`, which will store the total number of requests received by both instances of the servlet.

The `doGet()` method is overridden to handle HTTP GET requests. Inside the method, we have two synchronized methods: `incrementRequestCounter()` and `getRequestCounter()`. These methods are used to safely increment the `requestCounter` variable and retrieve its value, respectively.

By using synchronized methods, we ensure that only one thread can access and modify the shared `requestCounter` variable at a time, preventing concurrency issues.

To deploy and test this application, you need to configure the servlet mapping in the `web.xml` file, similar to the previous example.

Once the application is deployed, you can access the servlet using the URL `http://localhost:8080/<context-path>/requestCounter`.

When multiple clients make requests to the servlet, the `requestCounter` variable will be shared between the servlet instances, and the total number of requests will be displayed accurately.

By synchronizing the access to the shared variable, you ensure thread safety and prevent concurrency issues that could arise from multiple threads accessing and modifying the variable simultaneously.

To deploy and test this application, you need to configure the servlet mapping in the `web.xml` file:

```xml
<web-app>
    <servlet>
        <servlet-name>RequestCounterServlet</servlet-name>
        <servlet-class>RequestCounterServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>RequestCounterServlet</servlet-name>
        <url-pattern>/requestCounter</url-pattern>
    </servlet-mapping>
</web-app>
```

## Sharing information between servlets at context level

Communication between servlets at the context level can be achieved using the ServletContext object provided by the servlet container. The ServletContext object represents a context, which is a web application's environment. It allows servlets within the same web application to share information and communicate with each other.

Each servlet has access to its ServletContext object via the getServletContext() method inherited from the HttpServlet class. ServletContext provides a shared storage space where data can be stored and accessed by multiple servlets. This storage space is commonly referred to as the "context attributes." Servlets can store data in the ServletContext using the setAttribute() method, and retrieve the stored data using the getAttribute() method.

It's important to note that the ServletContext is shared across all servlets within the same web application. Therefore, proper synchronization mechanisms should be implemented when accessing or modifying shared data to ensure thread safety, especially in scenarios with concurrent access.

### Example

To explain the use of sessions at the level of servlet context, I will provide an example where we implement three servlets and maintain a visitor counter for each servlet. The visitor counters will be persisted in a file.

Let's begin by creating the three servlets:

1. Create a servlet named `Servlet1.java`:

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class Servlet1 extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Get the servlet context
        ServletContext context = getServletContext();

        // Get the visitor counter for Servlet1 from the context
        Integer counter = (Integer) context.getAttribute("counter1");

        // Increment the counter
        if (counter == null) {
            counter = 1;
        } else {
            counter++;
        }

        // Update the visitor counter in the context
        context.setAttribute("counter1", counter);

        // Write the response
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<h1>Servlet 1</h1>");
        out.println("<p>Visitor Count: " + counter + "</p>");
    }
}
```

2. Create a servlet named `Servlet2.java`:

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class Servlet2 extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Get the servlet context
        ServletContext context = getServletContext();

        // Get the visitor counter for Servlet2 from the context
        Integer counter = (Integer) context.getAttribute("counter2");

        // Increment the counter
        if (counter == null) {
            counter = 1;
        } else {
            counter++;
        }

        // Update the visitor counter in the context
        context.setAttribute("counter2", counter);

        // Write the response
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<h1>Servlet 2</h1>");
        out.println("<p>Visitor Count: " + counter + "</p>");
    }
}
```

3. Create a servlet named `Servlet3.java`:

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class Servlet3 extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Get the servlet context
        ServletContext context = getServletContext();

        // Get the visitor counter for Servlet3 from the context
        Integer counter = (Integer) context.getAttribute("counter3");

        // Increment the counter
        if (counter == null) {
            counter = 1;
        } else {
            counter++;
        }

        // Update the visitor counter in the context
        context.setAttribute("counter3", counter);

        // Write the response
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<h1>Servlet 3</h1>");
        out.println("<p>Visitor Count: " + counter + "</p>");
    }
}
```

Next, we will persist the visitor counters in a file. Let's modify the `ServletContextListener` class:

1. Create a class named `CounterContextListener.java`:

```java
import java.io.*;
import javax.servlet.*;

public class CounterContextListener implements ServletContextListener {
    private String filePath;

    public void contextInitialized(ServletContextEvent event) {
        ServletContext context = event.getServletContext();
        filePath = context.getRealPath("/") + "counter.txt";

        try {
            File file = new File(filePath);

            // Create the file if it doesn't exist
            if (!file.exists()) {
                file.createNewFile();
                FileWriter writer = new FileWriter(file);
                writer.write("0\n0\n0\n");
                writer.close();
            }

            // Read the visitor counters from the file
            BufferedReader reader = new BufferedReader(new FileReader(file));
            String line;
            int i = 1;
            while ((line = reader.readLine()) != null) {
                int counter = Integer.parseInt(line.trim());
                context.setAttribute("counter" + i, counter);
                i++;
            }
            reader.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void contextDestroyed(ServletContextEvent event) {
        ServletContext context = event.getServletContext();

        try {
            // Write the visitor counters to the file
            FileWriter writer = new FileWriter(filePath);
            for (int i = 1; i <= 3; i++) {
                Integer counter = (Integer) context.getAttribute("counter" + i);
                writer.write(counter + "\n");
            }
            writer.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Finally, we need to configure the `web.xml` file to map the servlets and configure the listener:

1. Open the `web.xml` file and add the following code:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
    id="WebApp_ID" version="4.0">

    <display-name>Session Example</display-name>

    <listener>
        <listener-class>CounterContextListener</listener-class>
    </listener>

    <servlet>
        <servlet-name>Servlet1</servlet-name>
        <servlet-class>Servlet1</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Servlet1</servlet-name>
        <url-pattern>/servlet1</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>Servlet2</servlet-name>
        <servlet-class>Servlet2</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Servlet2</servlet-name>
        <url-pattern>/servlet2</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>Servlet3</servlet-name>
        <servlet-class>Servlet3</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Servlet3</servlet-name>
        <url-pattern>/servlet3</url-pattern>
    </servlet-mapping>

</web-app>
```

Make sure to adjust the paths and package names according to your project structure.

Now, when the servlets are accessed, the visitor counters for each servlet will be incremented and persisted in the `counter.txt` file. The `CounterContextListener` class initializes the counters from the file when the application starts and saves the counters back to the file when the application is destroyed.

Remember to place the `counter.txt` file in the appropriate location based on your project setup.

That's it! You have now implemented three servlets with visitor counters persisted in a file using session context in a servlet-based application.


## JWT

### Server Example

Here's an example of implementing JWT authentication without using Spring Security:

1. Add the necessary dependencies to your project:
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.2</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.2</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.2</version>
    <scope>runtime</scope>
</dependency>
```

2. Create a JWTUtil class to handle JWT token generation and validation:
```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;

import javax.crypto.SecretKey;
import java.util.Date;

public class JWTUtil {
    private static final String SECRET_KEY = "yourSecretKey";
    private static final long EXPIRATION_TIME = 86400000; // 24 hours

    public static String generateToken(String username) {
        SecretKey key = Keys.hmacShaKeyFor(SECRET_KEY.getBytes());

        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + EXPIRATION_TIME);

        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(now)
                .setExpiration(expiryDate)
                .signWith(key, SignatureAlgorithm.HS512)
                .compact();
    }

    public static String extractUsername(String token) {
        return extractClaims(token).getSubject();
    }

    public static boolean validateToken(String token) {
        return extractClaims(token).getExpiration().after(new Date());
    }

    private static Claims extractClaims(String token) {
        SecretKey key = Keys.hmacShaKeyFor(SECRET_KEY.getBytes());

        return Jwts.parserBuilder()
                .setSigningKey(key)
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
}
```

3. Create a controller to handle authentication requests:
```java
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AuthController {
    @PostMapping("/login")
    public ResponseEntity<?> authenticateUser(@RequestBody LoginRequest loginRequest) {
        String username = loginRequest.getUsername();
        String password = loginRequest.getPassword();

        // Implement your authentication logic here (e.g., check credentials against a database)
        if (username.equals("admin") && password.equals("password")) {
            String token = JWTUtil.generateToken(username);
            return ResponseEntity.ok(new JwtResponse(token));
        } else {
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Invalid username or password");
        }
    }
}
```

In this example, we have a `JWTUtil` class that handles JWT token generation and validation. The `generateToken()` method generates a JWT token using the provided username. The `extractUsername()` method extracts the username from a JWT token. The `validateToken()` method checks if a JWT token is valid.

The `AuthController` class contains an endpoint for user authentication (`/login`). In the example, the authentication logic is simplified to check if the provided username and password are equal to "admin" and "password" respectively. If the credentials are valid, a JWT token is generated using the `JWTUtil` class and returned in the response.

Remember to customize the authentication logic in the `Auth

Controller` class to suit your specific needs, such as integrating with a user repository or service for authentication.

### ES6 FrontEnd Example

Here's an example of implementing the frontend code for JWT authentication using ES6:

```html
<!DOCTYPE html>
<html>
<head>
    <title>JWT Authentication</title>
</head>
<body>
    <h1>Login</h1>
    <div id="loginForm">
        <input type="text" id="usernameInput" placeholder="Username">
        <input type="password" id="passwordInput" placeholder="Password">
        <button id="loginButton">Login</button>
    </div>

    <h1>Welcome</h1>
    <div id="welcomeMessage" style="display: none;">
        <p id="usernameText"></p>
        <button id="logoutButton">Logout</button>
    </div>

    <script>
        // Function to make an HTTP POST request
        async function postData(url, data) {
            const response = await fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            });

            if (!response.ok) {
                throw new Error(response.status);
            }

            return response.json();
        }

        // Function to handle the login process
        async function login() {
            const username = document.getElementById('usernameInput').value;
            const password = document.getElementById('passwordInput').value;

            try {
                const response = await postData('/login', { username, password });

                // Store the token in localStorage
                localStorage.setItem('token', response.token);

                // Hide the login form and show the welcome message
                document.getElementById('loginForm').style.display = 'none';
                document.getElementById('welcomeMessage').style.display = 'block';
                document.getElementById('usernameText').textContent = `Welcome, ${username}!`;
            } catch (error) {
                console.error(error);
                alert('Failed to login. Please try again.');
            }
        }

        // Function to handle the logout process
        function logout() {
            // Remove the token from localStorage
            localStorage.removeItem('token');

            // Show the login form and hide the welcome message
            document.getElementById('loginForm').style.display = 'block';
            document.getElementById('welcomeMessage').style.display = 'none';

            // Clear the username input field
            document.getElementById('usernameInput').value = '';
        }

        // Function to check if a user is authenticated
        function checkAuth() {
            const token = localStorage.getItem('token');
            if (token) {
                // Hide the login form and show the welcome message
                document.getElementById('loginForm').style.display = 'none';
                document.getElementById('welcomeMessage').style.display = 'block';

                // Decode the token to get the username
                const decodedToken = JSON.parse(atob(token.split('.')[1]));
                const username = decodedToken.sub;

                document.getElementById('usernameText').textContent = `Welcome, ${username}!`;
            } else {
                // Show the login form and hide the welcome message
                document.getElementById('loginForm').style.display = 'block';
                document.getElementById('welcomeMessage').style.display = 'none';
            }
        }

        // Add event listeners to the login and logout buttons
        document.getElementById('loginButton').addEventListener('click', login);
        document.getElementById('logoutButton').addEventListener('click', logout);

        // Check authentication on page load
        checkAuth();
    </script>
</body>
</html>
```

In this example, we have an HTML page that contains a login form and a welcome message. The JavaScript code utilizes ES6 features to handle the login and logout processes.

The `postData()` function is an asynchronous function that sends a POST request to the server with the provided data. It

 handles the response and stores the JWT token in the localStorage.

The `login()` function is called when the login button is clicked. It retrieves the entered username and password, calls the `postData()` function to send the login request, and handles the response by storing the token and updating the UI accordingly.

The `logout()` function is called when the logout button is clicked. It removes the token from localStorage and updates the UI to display the login form again.

The `checkAuth()` function is called on page load. It checks if a token exists in localStorage and updates the UI accordingly.

Note that this is a simplified example and doesn't cover all aspects of JWT authentication, such as token expiration and refresh. It's recommended to use a library or framework that provides built-in support for JWT authentication to handle these aspects more robustly.


### Angular FrontEnd Example

Here's an example of implementing the frontend code for JWT authentication using Angular:

1. Create a login component to handle the login functionality:

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

interface LoginResponse {
  token: string;
}

@Component({
  selector: 'app-login',
  template: `
    <h1>Login</h1>
    <div>
      <input type="text" [(ngModel)]="username" placeholder="Username">
      <input type="password" [(ngModel)]="password" placeholder="Password">
      <button (click)="login()">Login</button>
    </div>
  `,
})
export class LoginComponent {
  username: string;
  password: string;

  constructor(private http: HttpClient) {}

  login(): void {
    this.http
      .post<LoginResponse>('/login', { username: this.username, password: this.password })
      .subscribe(
        (response) => {
          localStorage.setItem('token', response.token);
          // Handle successful login
        },
        (error) => {
          console.error(error);
          // Handle login error
        }
      );
  }
}
```

2. Create an authentication service to handle token management and authentication status:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class AuthService {
  isAuthenticated(): boolean {
    return !!localStorage.getItem('token');
  }

  logout(): void {
    localStorage.removeItem('token');
  }
}
```

3. Create a component to display the authenticated content:

```typescript
import { Component } from '@angular/core';
import { AuthService } from './auth.service';

@Component({
  selector: 'app-welcome',
  template: `
    <h1>Welcome</h1>
    <div *ngIf="authService.isAuthenticated()">
      <p>Welcome, {{ getUsernameFromToken() }}!</p>
      <button (click)="logout()">Logout</button>
    </div>
    <div *ngIf="!authService.isAuthenticated()">
      <p>Please login to access this page.</p>
    </div>
  `,
})
export class WelcomeComponent {
  constructor(public authService: AuthService) {}

  getUsernameFromToken(): string {
    const token = localStorage.getItem('token');
    if (token) {
      const tokenPayload = JSON.parse(atob(token.split('.')[1]));
      return tokenPayload.sub;
    }
    return '';
  }

  logout(): void {
    this.authService.logout();
  }
}
```

4. Add the components to your app module:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { LoginComponent } from './login.component';
import { WelcomeComponent } from './welcome.component';

@NgModule({
  imports: [BrowserModule, FormsModule, HttpClientModule],
  declarations: [AppComponent, LoginComponent, WelcomeComponent],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Make sure to import the necessary modules and components, and add them to the `declarations` and `bootstrap` arrays.

5. Update the main `app.component.html` template:

```html
<app-login></app-login>
<app-welcome></app-welcome>
```


## oAuth2 login with Google

Here's an example of a simple authentication web application using a servlet and Google's OAuth 2.0 Java library:

1. Create a `LoginServlet` to handle the login functionality:

```java
import com.google.api.client.auth.oauth2.AuthorizationCodeFlow;
import com.google.api.client.auth.oauth2.AuthorizationCodeRequestUrl;
import com.google.api.client.auth.oauth2.Credential;
import com.google.api.client.extensions.servlet.auth.oauth2.AbstractAuthorizationCodeServlet;
import com.google.api.client.googleapis.auth.oauth2.GoogleAuthorizationCodeFlow;
import com.google.api.client.googleapis.auth.oauth2.GoogleCredential;
import com.google.api.client.googleapis.auth.oauth2.GoogleTokenResponse;
import com.google.api.client.googleapis.auth.oauth2.GoogleTokenResponseException;
import com.google.api.client.http.BasicAuthentication;
import com.google.api.client.http.GenericUrl;
import com.google.api.client.http.javanet.NetHttpTransport;
import com.google.api.client.json.JsonFactory;
import com.google.api.client.json.jackson2.JacksonFactory;
import com.google.api.services.oauth2.Oauth2;
import com.google.api.services.oauth2.model.Userinfoplus;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class LoginServlet extends AbstractAuthorizationCodeServlet {

    private static final String CLIENT_ID = "your_client_id";
    private static final String CLIENT_SECRET = "your_client_secret";
    private static final String REDIRECT_URI = "http://localhost:8080/login/callback";
    private static final List<String> SCOPES = Arrays.asList("email", "profile");

    private final JsonFactory jsonFactory = new JacksonFactory();
    private final NetHttpTransport httpTransport = new NetHttpTransport();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        AuthorizationCodeFlow flow = createAuthorizationCodeFlow();

        AuthorizationCodeRequestUrl authorizationUrl = flow.newAuthorizationUrl();
        String redirectUrl = authorizationUrl.setRedirectUri(REDIRECT_URI).build();
        resp.sendRedirect(redirectUrl);
    }

    @Override
    protected String getRedirectUri(HttpServletRequest req) throws ServletException, IOException {
        return REDIRECT_URI;
    }

    @Override
    protected AuthorizationCodeFlow initializeFlow() throws IOException {
        return createAuthorizationCodeFlow();
    }

    @Override
    protected String getUserId(HttpServletRequest req) throws ServletException, IOException {
        Credential credential = getFlow().loadCredential(getUserId(req));
        if (credential != null) {
            return ((GoogleCredential) credential).getUserId();
        }
        return null;
    }

    @Override
    protected void onSuccess(HttpServletRequest req, HttpServletResponse resp, Credential credential)
            throws ServletException, IOException {
        Oauth2 oauth2 = new Oauth2.Builder(httpTransport, jsonFactory, credential).build();
        Userinfoplus userInfo = oauth2.userinfo().get().execute();

        // Access user information using `userInfo` object

        // Redirect to the home page or perform any other action
        resp.sendRedirect("/home");
    }

    @Override
    protected void onError(HttpServletRequest req, HttpServletResponse resp, AuthorizationCodeResponseUrl errorResponse)
            throws ServletException, IOException {
        // Handle error during authentication
        resp.getWriter().print("Authentication error: " + errorResponse.getError());
    }

    private AuthorizationCodeFlow createAuthorizationCodeFlow() {
        return new GoogleAuthorizationCodeFlow.Builder(httpTransport, jsonFactory,
                CLIENT_ID, CLIENT_SECRET, SCOPES).setAccessType("offline").build();
    }
}
```

2. Create a `LoginCallbackServlet` to handle the OAuth 2.0 callback:

```java
import com.google.api.client.auth.oauth2.AuthorizationCodeFlow;
import com.google.api.client.auth.oauth2.AuthorizationCodeResponse

Url;
import com.google.api.client.auth.oauth2.Credential;
import com.google.api.client.extensions.servlet.auth.oauth2.AbstractAuthorizationCodeCallbackServlet;
import com.google.api.client.googleapis.auth.oauth2.GoogleAuthorizationCodeFlow;
import com.google.api.client.googleapis.auth.oauth2.GoogleTokenResponse;
import com.google.api.client.googleapis.auth.oauth2.GoogleTokenResponseException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class LoginCallbackServlet extends AbstractAuthorizationCodeCallbackServlet {

    private static final String CLIENT_ID = "your_client_id";
    private static final String CLIENT_SECRET = "your_client_secret";
    private static final String REDIRECT_URI = "http://localhost:8080/login/callback";
    private static final List<String> SCOPES = Arrays.asList("email", "profile");

    @Override
    protected void onSuccess(HttpServletRequest req, HttpServletResponse resp, Credential credential)
            throws ServletException, IOException {
        // Redirect to the login success page or perform any other action
        resp.sendRedirect("/login/success");
    }

    @Override
    protected void onError(HttpServletRequest req, HttpServletResponse resp, AuthorizationCodeResponseUrl errorResponse)
            throws ServletException, IOException {
        // Handle error during OAuth callback
        resp.getWriter().print("Authentication error: " + errorResponse.getError());
    }

    @Override
    protected String getRedirectUri(HttpServletRequest req) throws ServletException, IOException {
        return REDIRECT_URI;
    }

    @Override
    protected AuthorizationCodeFlow initializeFlow() throws IOException {
        return new GoogleAuthorizationCodeFlow.Builder(HTTP_TRANSPORT, JSON_FACTORY,
                CLIENT_ID, CLIENT_SECRET, SCOPES).setAccessType("offline").build();
    }
}
```

3. Configure the `web.xml` file to map the servlets:

```xml
<web-app>
    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>your.package.name.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>
    <servlet>
        <servlet-name>LoginCallbackServlet</servlet-name>
        <servlet-class>your.package.name.LoginCallbackServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LoginCallbackServlet</servlet-name>
        <url-pattern>/login/callback</url-pattern>
    </servlet-mapping>
</web-app>
```

Make sure to replace `your.package.name` with the actual package name where you place the servlet classes.

4. Build and deploy the application to your servlet container.

This example demonstrates a basic implementation of OAuth 2.0 authentication using Google's OAuth 2.0 Java library and servlets. When the user visits the `/login` URL, they will be redirected to the Google login page. After successful authentication, the callback URL `/login/callback` will receive the authorization code, exchange it for an access token, and store it in the user's session. You can customize the success and error handling as per your requirements.

Note: To use Google's OAuth 2.0 Java library, you need to include the necessary dependencies in your project. You can download the library from the Google API Client Library for Java website or use a dependency management tool like Maven or Gradle to include the library in your project.

Make sure to replace `your_client_id` and `your_client_secret` with your actual Google OAuth 2.0 client ID and client secret.

Remember to properly configure your Google OAuth 2.0 credentials in the Google Cloud Console for the authentication to work correctly.
This will render the login component and the welcome component in the main app component template.

That's it! This example demonstrates a basic implementation of JWT authentication in an Angular application. The login component handles the login request and stores the JWT token in localStorage. The welcome component displays the authenticated content based on the token's presence in localStorage and provides a logout functionality.
