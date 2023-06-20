# Auth

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

## Sharing information between servlets at context level

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
                FileWriter writer

 = new FileWriter(file);
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
