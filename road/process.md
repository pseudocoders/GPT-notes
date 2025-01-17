# Process

Let's break down the differences between these concepts Static Pages and Dynamic Pages:

## Static Pages
Static pages are pre-built web pages that are delivered to the user's browser exactly as they were created. They are typically written in HTML, CSS, and JavaScript. The content of static pages remains the same unless manually updated. When a user requests a static page, the server simply retrieves and sends the requested file to the user's browser without any processing or manipulation.

Here's an example of a simple "Hello, World!" static page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Hello, World!</title>
</head>
<body>
  <h1>Hello, World!</h1>
  <p>This is a simple static page.</p>
</body>
</html>
```

In this example, we have a basic HTML structure. The `<title>` tag sets the title of the web page, which will appear in the browser's title bar or tab. Inside the `<body>` tag, we have an `<h1>` heading that displays the "Hello, World!" message and a `<p>` paragraph that provides some additional text.

When you save this code as an HTML file (e.g., `hello.html`) and open it in a web browser, you will see the "Hello, World!" message displayed on the page. This is a classic example used to introduce beginners to web development and serves as a starting point for more complex web applications.

This page is a static page because its content remains the same regardless of user interactions or external factors. The HTML markup and the "Hello, World!" message are predefined and fixed within the HTML file itself. When the file is served to a user's browser, it is displayed exactly as it was created, without any changes or modifications.

Static pages are typically used for content that doesn't require frequent updates or real-time interactions. They are useful for displaying information that doesn't need to be generated dynamically or personalized for each user. Since static pages don't involve server-side processing or dynamic data retrieval, they are relatively simple and can be served quickly to users.

In contrast, dynamic web pages involve executing code to generate customized content based on user requests. These pages can change their content dynamically based on various factors. Dynamic web pages can involve both server-side processing, where the server generates dynamic content before sending it to the browser, and front-end processing, where JavaScript code running in the browser manipulates the page's content and interacts with the user in real-time. Both server-side and front-end processing contribute to the dynamic nature of a web page.



> 📓 solvantes
> * Static multiplication quadrant
> * :arrow_right: HTML
> * [https://rafaelaznar.github.io/solvantes/](https://rafaelaznar.github.io/solvantes/)
> * [https://github.com/rafaelaznar/solvantes](https://github.com/rafaelaznar/solvantes)




## Dynamic Pages:

A dynamic page is a web page whose content can change or be customized based on various factors, such as user input, database queries, or real-time data updates. Unlike static pages, which have fixed content that remains the same for all users, dynamic pages adapt and display different content based on specific conditions or user interactions.

Dynamic pages can be generated using server-side processing or client-side processing, or a combination of both:

1. Server-Side Dynamic Pages: In server-side dynamic pages, the server executes code to generate the web page content before sending it to the user's browser. The server may process user input, retrieve data from databases or external APIs, and apply business logic to generate dynamic content tailored to each user. Server processed pages, also known as server-side rendering (SSR), involve the server executing code to generate dynamic content before sending it to the user's browser. When a user requests a server processed page, the server retrieves the necessary data, performs computations or manipulations, and generates an HTML page as a response. The server executes code written in languages such as PHP, Java, Python, or Javascript over Node.js to process the request and create the HTML page that is then sent to the user's browser.

2. Client-Side Dynamic Pages: In client-side dynamic pages, the bulk of the page rendering and content manipulation happens within the user's browser using JavaScript. The web page is initially loaded with static content, and then JavaScript code dynamically updates and modifies the page based on user interactions. So, the front-end process refers to the execution of code on the client-side, within the user's browser. It involves rendering the user interface, handling user interactions, and manipulating the page's content without the need to communicate with the server. The front-end process typically includes HTML, CSS, and JavaScript. It runs on the user's device and is responsible for creating an interactive and engaging user experience. 

3. Both AJAX Back-End and Front-End Dynamic Processed Web Pages:
AJAX (Asynchronous JavaScript and XML) is a technique that enables asynchronous communication between the client-side (front-end) and server-side (back-end) processes. It allows for dynamic content updates without requiring a full page reload. In an AJAX back-end process, JavaScript code on the front-end sends asynchronous requests to the server, typically using the XMLHttpRequest or Fetch API. The server processes these requests, performs necessary operations, retrieves data from databases or other sources, and sends back a response, usually in JSON format. The front-end JavaScript code handles the response and updates the user interface accordingly without reloading the entire page. In an AJAX front-end process, JavaScript code on the front-end makes asynchronous requests to the server to retrieve data or perform specific actions. These requests are sent without refreshing the entire page. The front-end code handles the responses and updates the user interface dynamically based on the received data.

To summarize, static pages are pre-built and don't change dynamically. Server processed pages involve server-side code execution to generate dynamic content. The front-end process executes code on the client-side to handle user interactions and render the user interface. AJAX processes allow for asynchronous communication between the front-end and back-end, enabling dynamic updates without full page reloads.

Dynamic pages offer several advantages:

- Personalization: Dynamic pages allow for customized content based on user preferences, profiles, or previous interactions, providing a personalized experience.

- Real-Time Updates: Dynamic pages can display real-time information, such as live data feeds, chat messages, or collaborative editing, without requiring the user to manually refresh the page.

- Interactive User Experience: With dynamic pages, users can actively interact with the content, submit forms, make selections, and receive immediate feedback.

- Scalability: Dynamic pages can handle large amounts of data or complex computations on the server-side, providing scalability for web applications with growing user bases.

Dynamic pages are widely used in various web applications, including e-commerce websites, social media platforms, news portals, and collaborative tools, where content needs to be constantly updated and personalized to enhance the user experience.




### Server Processed Web Pages

Server Processed Web Pages, also known as server-side rendering (SSR), refer to the process of generating and serving web pages on the server before sending them to the client's browser. In server-side rendering, the server executes code and processes data to dynamically generate the HTML content of a web page.

Here's an overview of how Server Processed Web Pages work:

1. User Request: A user initiates a request for a web page by entering a URL or clicking on a link.

2. Server Processing: The web server receives the request and executes server-side code written in languages like PHP, Python, or Node.js. This code may interact with databases, external APIs, or perform other computations to gather the necessary data and generate the HTML content for the requested page.

3. HTML Generation: Based on the requested page and the processed data, the server dynamically generates an HTML document that represents the web page. The HTML may include placeholders or template tags to be replaced with specific data.

4. Sending Response: The server sends the generated HTML document as a response to the user's browser.

5. Browser Rendering: The user's browser receives the HTML response and renders it as a web page. This rendering process includes interpreting the HTML structure, applying CSS styles, and executing any JavaScript included in the page.

Server Processed Web Pages offer several benefits:

1. Search Engine Optimization (SEO): By generating HTML content on the server, the complete web page is readily available to search engine crawlers, making it easier for search engines to index and rank the page.

2. Initial Page Load: Since the server generates the complete HTML content, the user's browser can quickly render and display the page, providing a faster initial loading experience.

3. Consistent Content: Server-side rendering ensures that all users receive the same content since the HTML generation is centralized on the server. This is particularly useful for content that doesn't require personalization or real-time updates.

However, server-side rendering also has some limitations. It can put a heavier load on the server, as each user request requires server-side processing and HTML generation. Additionally, dynamic interactions or updates that require frequent communication with the server may still require additional front-end processing or AJAX requests.

Overall, Server Processed Web Pages are a fundamental approach to generating dynamic content on the server before sending it to the client's browser, providing advantages such as SEO benefits and faster initial page loading.

#### PHP example

Certainly! Here's an example of a simple web page generated using PHP:

```php
<!DOCTYPE html>
<html>
<head>
  <title>Server-Generated Web Page</title>
</head>
<body>
  <h1>Welcome to my website!</h1>

  <?php
  // Server-side PHP code
  $currentDate = date("Y-m-d");
  $currentTime = date("H:i:s");
  ?>

  <p>Today's date is: <?php echo $currentDate; ?></p>
  <p>The current time is: <?php echo $currentTime; ?></p>
</body>
</html>
```

In this example, we have an HTML structure with PHP code embedded within the HTML markup. The PHP code is executed on the server before sending the generated web page to the user's browser.

Inside the PHP section, we have two variables, `$currentDate` and `$currentTime`, which use PHP's `date()` function to retrieve the current date and time, respectively.

These variables are then displayed within `<p>` paragraphs using PHP's `echo` statement to output the values. So when the page is loaded, the server processes the PHP code, retrieves the current date and time, and generates the HTML content dynamically with the updated values.

When a user accesses this PHP file on a server, the resulting web page will display a heading saying "Welcome to my website!" followed by the dynamically generated current date and time information.

Please note that in order to execute PHP code on a server, you would need a server environment with PHP installed and configured.

#### JSP example

Here's an example of a web page generated using JSP (JavaServer Pages):

```jsp
<!DOCTYPE html>
<html>
<head>
  <title>Server-Generated Web Page</title>
</head>
<body>
  <h1>Welcome to my website!</h1>

  <%-- Server-side JSP code --%>
  <%@ page import="java.util.Date" %>
  <% Date currentDate = new Date(); %>

  <p>Today's date is: <%= currentDate %></p>
</body>
</html>
```

In this example, we have an HTML structure with JSP code embedded within HTML comments (`<%--` and `--%>`). The JSP code is executed on the server before sending the generated web page to the user's browser.

Inside the JSP section, we import the `java.util.Date` class and create a `currentDate` variable of type `Date`. This variable is then assigned the current date and time using `new Date()`.

To display the value of `currentDate` within the web page, we use the `<%= %>` expression tag. This tag allows us to output the value of a variable or expression directly into the HTML content.

When a user accesses this JSP file on a server, the server processes the JSP code, creates an HTML page, and sends it to the user's browser. The resulting web page will display a heading saying "Welcome to my website!" followed by the dynamically generated current date and time.

Please note that to run JSP code, you need a server environment that supports Java servlets and JSP, such as Apache Tomcat.

### Front-End Processed Web Pages

> :red_circle: Explore and learn: [**Javascript ES6 notes in GPT-notes**](../javascript/index.md)


Front-End Processed Web Pages refer to web pages where the majority of processing and rendering tasks are performed on the client-side, within the user's browser. In this approach, the web page's interactivity and content updates are primarily handled by JavaScript code running in the browser.

Here's an overview of how Front-End Processed Web Pages work:

1. User Request: A user initiates a request for a web page by entering a URL or clicking on a link.

2. Server Response: The web server sends the HTML, CSS, and JavaScript files that make up the web page to the user's browser.

3. HTML Rendering: The browser receives the HTML file and starts rendering the structure of the web page. It parses the HTML, constructs the Document Object Model (DOM), and renders the initial visual representation of the page.

4. CSS Styling: The browser applies CSS styles to the HTML elements, modifying their appearance and layout.

5. JavaScript Execution: The browser executes the JavaScript code embedded within the web page. This JavaScript code can interact with the DOM, handle user interactions, make asynchronous requests to the server, and manipulate the content of the web page dynamically.

6. Dynamic Updates: With the help of JavaScript, Front-End Processed Web Pages can fetch data from servers using AJAX requests, respond to user actions such as clicks or form submissions, and update the page content without requiring a full page reload. JavaScript frameworks and libraries like React, Angular, or Vue.js are often used to simplify the development of interactive web applications.

Front-End Processed Web Pages provide several benefits:

1. Interactivity: By executing JavaScript code in the browser, web pages can respond to user interactions in real-time, providing a more interactive and engaging experience.

2. Dynamic Content: JavaScript can fetch data from servers, update page content, and display real-time information without reloading the entire page.

3. Enhanced User Experience: Front-End Processing enables smooth transitions, animations, and dynamic behavior, resulting in a more responsive and seamless user experience.

4. Reduced Server Load: With Front-End Processing, certain tasks that would have required server-side processing can be offloaded to the client-side, reducing the load on the server and improving scalability.

However, Front-End Processed Web Pages also have some considerations:

1. Dependency on JavaScript: Front-End Processed Web Pages heavily rely on JavaScript execution. If JavaScript is disabled or encounters errors, the functionality of the page may be compromised.

2. SEO Challenges: Search engine crawlers may have difficulty parsing and indexing content generated dynamically through JavaScript. Proper implementation of server-side rendering or techniques like prerendering can address these SEO challenges.

Front-End Processed Web Pages are commonly used for building web applications, where interactivity, real-time updates, and a rich user experience are desired.

#### Example

Here's an example of front-end processing in a web page using JavaScript:

HTML:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Front-End Process Example</title>
</head>
<body>
  <h1>Front-End Process Example</h1>

  <p id="message">Click the button to change the message!</p>

  <button id="changeButton">Change Message</button>

  <script src="script.js"></script>
</body>
</html>
```

JavaScript (script.js):
```javascript
document.addEventListener("DOMContentLoaded", function() {
  var changeButton = document.getElementById("changeButton");
  var messageElement = document.getElementById("message");

  changeButton.addEventListener("click", function() {
    messageElement.textContent = "The message has been changed!";
  });
});
```

In this example, the JavaScript code is placed in an external file called "script.js" and is linked to the HTML page using the `<script>` tag.

The JavaScript code is wrapped in an event listener that waits for the DOMContentLoaded event, which indicates that the HTML document has finished loading and is ready for manipulation.

Inside the event listener, the JavaScript code retrieves the button element with the ID "changeButton" and the paragraph element with the ID "message" using `document.getElementById()`.

It then adds an event listener to the button element, listening for the click event. When the button is clicked, the event listener function is executed, and it updates the content of the paragraph by modifying the `textContent` property, changing the message to "The message has been changed!".

### Introduction to AJAX

AJAX (Asynchronous JavaScript and XML) is a technique used in web development to send and receive data from a server asynchronously without requiring a full page reload. It enables the dynamic updating of web content, providing a smoother and more interactive user experience.

Ajax mixes Back-End and Front-End process.

Here's a step-by-step explanation of how AJAX (Asynchronous JavaScript and XML) works:

1. User Interaction: The user performs an action on a web page, such as clicking a button or submitting a form, which triggers an AJAX request.

2. AJAX Request Initialization: The JavaScript code running in the browser initiates an AJAX request using an API like XMLHttpRequest, Fetch API, or Axios. The request includes information such as the URL of the server endpoint, the HTTP method (GET, POST, etc.), and any data to be sent.

3. Sending the Request: The AJAX request is sent from the client-side JavaScript code to the server. It is typically sent asynchronously, meaning that the browser can continue executing other tasks while waiting for the server response.

4. Server Processing: The server receives the AJAX request and processes it based on the server-side code (e.g., PHP, Python, Java) handling the request. This can involve executing server-side logic, accessing databases, or interacting with external APIs.

5. Server Response: The server generates a response to the AJAX request. The response is usually in a structured format such as JSON, XML, or plain text. It can contain data, status codes, error messages, or any other information that the server intends to send back to the client.

6. Receiving the Response: Once the server processes the AJAX request and generates the response, the response is sent back to the client-side JavaScript code.

7. Response Handling: The JavaScript code in the browser receives the server response. It can then process the response based on its format and content. Common actions include parsing JSON or XML, extracting data, manipulating the Document Object Model (DOM) to update specific elements on the web page, or triggering other JavaScript functions based on the response data.

8. Updating the Web Page: With the data and information obtained from the server response, the client-side JavaScript code updates the web page dynamically. This can involve adding or removing elements, updating text or images, displaying error messages, or performing any other necessary modifications to reflect the new data.

9. Optional Error Handling: If an error occurs during the AJAX request or server processing, appropriate error handling can be implemented. This can involve displaying error messages, retrying the request, or taking any other necessary actions to handle the error gracefully.

By following these steps, AJAX enables the exchange of data between the client and server asynchronously, without requiring a full page reload. This allows for dynamic updates, real-time interactions, and a more responsive user experience in web applications.

#### AJAX PHP example

Here's a very simple example of AJAX using PHP and the Fetch API:

index.html:
```html
<!DOCTYPE html>
<html>
<head>
  <title>AJAX Example with Fetch API</title>
</head>
<body>
  <h1>AJAX Example with Fetch API</h1>

  <button id="loadButton">Load Data</button>

  <div id="result"></div>

  <script src="script.js"></script>
</body>
</html>
```

script.js:
```javascript
document.addEventListener("DOMContentLoaded", function() {
  var loadButton = document.getElementById("loadButton");
  var resultDiv = document.getElementById("result");

  loadButton.addEventListener("click", function() {
    fetch("data.php")
      .then(function(response) {
        if (response.ok) {
          return response.text();
        } else {
          throw new Error("Error: " + response.status);
        }
      })
      .then(function(data) {
        resultDiv.textContent = data;
      })
      .catch(function(error) {
        console.log(error);
      });
  });
});
```

data.php:
```php
<?php
  // Simulating a delay
  sleep(2);

  // Generate a random number
  $randomNumber = rand(1, 100);

  // Send the random number as the response
  echo "Random Number: " . $randomNumber;
?>
```

In this example, we have an HTML file (`index.html`) with a button and a `div` element to display the result. The JavaScript code (`script.js`) is placed in an external file and linked to the HTML file using the `<script>` tag.

The JavaScript code uses the `addEventListener` method to attach an event listener to the "Load Data" button. When the button is clicked, the event listener function is executed. Inside the function, an AJAX request is made using the Fetch API.

The `fetch` function is called with the URL of the PHP script (`data.php`). It returns a Promise that resolves to the response from the server.

The response is then checked for success (response status 200) using the `ok` property of the response. If the response is successful, the data is extracted from the response using the `text` method.

The extracted data is then displayed in the `result` `div` element by setting its `textContent`.

If any errors occur during the AJAX request or response handling, they are caught and logged to the console using the `catch` method.

Upon clicking the "Load Data" button, an AJAX request is made to `data.php`. After a delay of 2 seconds (simulated using `sleep`), the PHP script generates a random number and sends it back as the response. The JavaScript code updates the web page dynamically by displaying the received data in the `result` `div` element.

This example demonstrates a simple implementation of AJAX using the Fetch API in JavaScript and PHP. It showcases how you can use the Fetch API to make asynchronous requests and handle responses in a concise and readable manner.

#### AJAX JSP example

> :red_circle: Explore and learn: [**Java in GPT-notes**](../java/index.md)
>
> :red_circle: Explore and learn: [**Servlets & JSP in GPT-notes**](../java/servlets.md)
>
> :red_circle: Explore and learn: [**Java for the web development in GPT-notes**](../java4backend/index.md)


Here's a very simple example of AJAX using JSP (JavaServer Pages) and the Fetch API:

index.html:
```html
<!DOCTYPE html>
<html>
<head>
  <title>AJAX Example with JSP and Fetch API</title>
</head>
<body>
  <h1>AJAX Example with JSP and Fetch API</h1>

  <button id="loadButton">Load Data</button>

  <div id="result"></div>

  <script src="script.js"></script>
</body>
</html>
```

script.js:
```javascript
document.addEventListener("DOMContentLoaded", function() {
  var loadButton = document.getElementById("loadButton");
  var resultDiv = document.getElementById("result");

  loadButton.addEventListener("click", function() {
    fetch("data.jsp")
      .then(function(response) {
        if (response.ok) {
          return response.text();
        } else {
          throw new Error("Error: " + response.status);
        }
      })
      .then(function(data) {
        resultDiv.textContent = data;
      })
      .catch(function(error) {
        console.log(error);
      });
  });
});
```

data.jsp:
```jsp
<%@ page contentType="text/plain;charset=UTF-8" %>
<%
  // Simulating a delay
  Thread.sleep(2000);

  // Generate a random number
  int randomNumber = (int) (Math.random() * 100) + 1;

  // Send the random number as the response
  out.println("Random Number: " + randomNumber);
%>
```

In this example, we have an HTML file (`index.html`) with a button and a `div` element to display the result. The JavaScript code (`script.js`) is placed in an external file and linked to the HTML file using the `<script>` tag.

The JavaScript code uses the `addEventListener` method to attach an event listener to the "Load Data" button. When the button is clicked, the event listener function is executed. Inside the function, an AJAX request is made using the Fetch API.

The `fetch` function is called with the URL of the JSP file (`data.jsp`). It returns a Promise that resolves to the response from the server.

The response is then checked for success (response status 200) using the `ok` property of the response. If the response is successful, the data is extracted from the response using the `text` method.

The extracted data is then displayed in the `result` `div` element by setting its `textContent`.

If any errors occur during the AJAX request or response handling, they are caught and logged to the console using the `catch` method.

The JSP file (`data.jsp`) receives the AJAX request, simulates a delay of 2 seconds, generates a random number, and sends it back as the response using the `out.println()` method.

Upon clicking the "Load Data" button, an AJAX request is made to `data.jsp`. After a delay of 2 seconds (simulated using `Thread.sleep()`), the JSP script generates a random number and sends it back as the response. The JavaScript code updates the web page dynamically by displaying the received data in the `result` `div` element.

This example demonstrates a simple implementation of AJAX using the Fetch API in JavaScript and JSP. It showcases how you can use the Fetch API to make asynchronous requests and handle responses in a concise and readable manner with JSP as the server-side technology.
