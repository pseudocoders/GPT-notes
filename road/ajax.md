# AJAX

Ajax, which stands for Asynchronous JavaScript and XML, has played a significant role in the history of web development. It revolutionized the way web applications functioned by allowing for asynchronous communication between the client-side and server-side, enabling dynamic and interactive web experiences.

Prior to Ajax, web applications relied on traditional request-response cycles, where a user would submit a form or click a link, and the entire web page would reload to display the updated content. This process often resulted in slow and clunky user experiences. However, in the late 1990s and early 2000s, developers began experimenting with new techniques to enhance web applications.

The term "Ajax" was coined in 2005 by Jesse James Garrett, who used it to describe a combination of existing technologies that enabled asynchronous data retrieval from a server without requiring a page reload. These technologies included JavaScript, the XMLHttpRequest object, XML, and CSS.

The XMLHttpRequest (XHR) object was the key component of Ajax. It allowed JavaScript code running in a web browser to make HTTP requests to the server and receive data in various formats, such as XML, JSON, or plain text. This enabled developers to retrieve data from the server in the background without disrupting the user's interaction with the web page.

Ajax quickly gained popularity among developers due to its ability to create more interactive and responsive web applications. Instead of waiting for entire pages to reload, users could now experience real-time updates, auto-suggestions, and interactive elements on web pages.

As web development progressed, frameworks and libraries emerged that simplified the implementation of Ajax techniques. Notably, jQuery, a popular JavaScript library, provided a simplified API for making Ajax requests, handling responses, and manipulating the DOM (Document Object Model).

The rise of Ajax paved the way for the development of modern web applications. It influenced the growth of dynamic web technologies such as single-page applications (SPAs) and rich internet applications (RIAs). Services like Google Maps and Gmail demonstrated the power of Ajax by delivering desktop-like experiences within web browsers.

Over time, the dominance of XML as the data format for Ajax responses gave way to JSON (JavaScript Object Notation) due to its simplicity and better integration with JavaScript. JSON became the de facto standard for data interchange in Ajax-based applications.

As web development progressed further, new technologies and techniques emerged, such as WebSocket, WebRTC, and server-side rendering (SSR). These technologies expanded the possibilities for real-time communication and enhanced the performance and scalability of web applications.

Today, Ajax remains a fundamental technique in web development, although it is often integrated seamlessly within more comprehensive frameworks and libraries. The principles of asynchronous communication and dynamic content loading introduced by Ajax continue to shape the way web applications are built, providing users with rich and interactive experiences.

## AJAX components

Let's dive deeper into Ajax and explore its key components and how they work together.

Ajax, as mentioned earlier, stands for Asynchronous JavaScript and XML. It is not a specific technology or framework but rather a combination of existing web technologies used together to achieve asynchronous communication between a web browser and a server.

The key components of Ajax are:

1. **HTML/CSS:** HTML (Hypertext Markup Language) provides the structure and content of a web page, while CSS (Cascading Style Sheets) is responsible for its visual presentation. These technologies are the foundation of web development and are essential for creating the user interface.

2. **JavaScript:** JavaScript is a programming language that runs in the web browser. It provides the necessary functionality to make web pages interactive and dynamic. Ajax relies heavily on JavaScript to initiate and handle asynchronous requests, manipulate the DOM, and update the web page content.

3. **XMLHttpRequest (XHR):** The XMLHttpRequest object is the heart of Ajax. It enables the browser to make HTTP requests to the server without reloading the entire web page. JavaScript code can create an instance of the XHR object, configure it with specific parameters, and send requests to the server. The XHR object also provides methods to handle the server's response asynchronously.

4. **Server-side Technologies:** On the server-side, various technologies handle the Ajax requests and generate appropriate responses. These technologies can include server-side scripting languages like PHP, Python, Ruby, or Java, as well as frameworks like Node.js. The server processes the request, retrieves the necessary data, and sends a response back to the browser.

5. **Data Formats:** Ajax supports multiple data formats for exchanging information between the client-side and server-side. Initially, XML (eXtensible Markup Language) was commonly used for data transmission, as indicated in the name "Ajax." However, JSON (JavaScript Object Notation) has become the preferred format due to its simplicity, compactness, and ease of integration with JavaScript. JSON is a lightweight data-interchange format that represents structured data as key-value pairs.

Now, let's explore the typical flow of an Ajax request:

1. **Event Triggering:** An event, such as a button click or form submission, occurs on the web page. JavaScript code associated with that event is executed.

2. **XHR Object Creation:** JavaScript creates an instance of the XMLHttpRequest object.

3. **Request Configuration:** The XHR object is configured with the necessary parameters, such as the HTTP method (GET, POST, etc.), the target URL, and optional request headers.

4. **Request Sending:** The XHR object sends the request to the server asynchronously. This means that the JavaScript code execution continues without waiting for the server's response.

5. **Server-side Processing:** On the server, the requested data or action is processed. The server may interact with databases, perform calculations, or execute other operations as needed.

6. **Response Handling:** Once the server processes the request, it sends a response back to the browser. The response can contain data, HTML snippets, or other relevant information.

7. **Response Handling in JavaScript:** JavaScript code handles the received response by using callback functions assigned to the XHR object. These callbacks are triggered when the response is received, allowing the JavaScript code to update the web page dynamically based on the server's response.

8. **DOM Manipulation:** JavaScript manipulates the Document Object Model (DOM) of the web page, updating the content, modifying styles, or adding new elements based on the received data. This allows for seamless updates without requiring a full page reload.

The asynchronous nature of Ajax allows web applications to provide real-time updates, validate form inputs without refreshing the page, perform auto-suggestions, and create interactive elements that enhance the user experience.

By combining HTML, CSS, JavaScript, the XMLHttpRequest object, and server-side technologies, Ajax empowers web developers to build dynamic, responsive, and interactive applications that closely resemble desktop applications.



## Asynchrony

> :red_circle: Explore and learn: [**JSON notation notes in GPT-notes**](../markups/json.md)
> 
> :red_circle: Explore and learn: [**Asynchronism javascript notes in GPT-notes**](../javascript/asynchronism.md)

In JavaScript programming, asynchronism refers to the ability of the code to execute tasks concurrently or non-blockingly, without waiting for a previous task to complete before moving on to the next one. This allows for efficient handling of time-consuming operations such as network requests, file I/O, or database queries, without blocking the execution of other code.

The main mechanism for achieving asynchronism in JavaScript is through the use of asynchronous functions and callbacks, as well as newer features like Promises and async/await.

Here are the key concepts related to asynchronism in JavaScript:

1. **Callbacks:** Callbacks are functions that are passed as arguments to other functions. They allow you to define what should happen after an asynchronous task completes. For example, when making an Ajax request, you provide a callback function that handles the server's response once it is received.

   ```javascript
   function fetchData(callback) {
     // Simulate an asynchronous task
     setTimeout(() => {
       const data = "Hello, world!";
       callback(data); // Invoke the callback with the retrieved data
     }, 1000);
   }

   function handleData(data) {
     console.log(data);
   }

   fetchData(handleData); // Pass the handleData function as a callback
   ```

   In this example, the `fetchData` function simulates an asynchronous task and invokes the callback function `handleData` after the task is complete.

2. **Promises:** Promises provide an enhanced way to handle asynchronism in JavaScript. They represent a future value that may not be available immediately. A Promise can be in one of three states: pending, fulfilled, or rejected. Promises allow you to chain asynchronous operations using `.then()` and `.catch()` methods.

   ```javascript
   function fetchData() {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         const data = "Hello, world!";
         resolve(data); // Resolve the promise with the retrieved data
       }, 1000);
     });
   }

   fetchData()
     .then((data) => {
       console.log(data);
     })
     .catch((error) => {
       console.error(error);
     });
   ```

   In this example, the `fetchData` function returns a Promise that resolves with the retrieved data. The `.then()` method is used to handle the resolved value, while the `.catch()` method handles any errors.

3. **Async/await:** Async/await is a syntax introduced in ES2017 that simplifies asynchronous programming by allowing you to write asynchronous code that looks like synchronous code. The `async` keyword is used to define an asynchronous function, and the `await` keyword is used to pause the function execution until a Promise is resolved.

   ```javascript
   function fetchData() {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         const data = "Hello, world!";
         resolve(data); // Resolve the promise with the retrieved data
       }, 1000);
     });
   }

   async function handleData() {
     try {
       const data = await fetchData(); // Wait for the promise to resolve
       console.log(data);
     } catch (error) {
       console.error(error);
     }
   }

   handleData();
   ```

   In this example, the `handleData` function is declared as an asynchronous function using the `async` keyword. The `await` keyword is used to pause the execution of the function until the Promise returned by `fetchData()` resolves.

Asynchronous programming in JavaScript allows for the execution of time-consuming tasks without blocking the main execution thread, ensuring that other parts of the code or the user interface remain responsive. It enables the handling of I/O operations, network requests, and other





## Example: AJAX calculator with jQuery and servlets

> :red_circle: Explore and learn: [**Revise jQuery notes in GPT-notes**](../jquery/index.md)

Here's an example of an Ajax calculator using jQuery on the client-side and Servlets on the server-side. This example performs basic arithmetic operations (addition, subtraction, multiplication, and division) asynchronously without reloading the entire page.

1. HTML Markup (index.html):
```html
<!DOCTYPE html>
<html>
<head>
  <title>Ajax Calculator</title>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
  <script>
    $(document).ready(function() {
      $("#calculate").click(function() {
        var num1 = $("#num1").val();
        var num2 = $("#num2").val();
        var operator = $("#operator").val();

        $.ajax({
          url: "calculator", // Servlet URL
          type: "POST",
          data: {
            num1: num1,
            num2: num2,
            operator: operator
          },
          success: function(result) {
            $("#result").text("Result: " + result);
          },
          error: function(error) {
            console.log(error);
          }
        });
      });
    });
  </script>
</head>
<body>
  <h2>Ajax Calculator</h2>
  <input type="number" id="num1" placeholder="Number 1"><br>
  <input type="number" id="num2" placeholder="Number 2"><br>
  <select id="operator">
    <option value="add">Addition</option>
    <option value="subtract">Subtraction</option>
    <option value="multiply">Multiplication</option>
    <option value="divide">Division</option>
  </select><br>
  <button id="calculate">Calculate</button><br>
  <div id="result"></div>
</body>
</html>
```

2. Servlet (CalculatorServlet.java):
```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class CalculatorServlet extends HttpServlet {
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    int num1 = Integer.parseInt(request.getParameter("num1"));
    int num2 = Integer.parseInt(request.getParameter("num2"));
    String operator = request.getParameter("operator");
    int result = 0;

    switch (operator) {
      case "add":
        result = num1 + num2;
        break;
      case "subtract":
        result = num1 - num2;
        break;
      case "multiply":
        result = num1 * num2;
        break;
      case "divide":
        if (num2 != 0)
          result = num1 / num2;
        else
          response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Cannot divide by zero.");
        break;
      default:
        response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Invalid operator.");
    }

    response.getWriter().print(result);
  }
}
```

Make sure to configure your servlet container (e.g., Apache Tomcat) properly and update the necessary servlet mapping in the web.xml file.

This example demonstrates the basic structure of an Ajax calculator. When the "Calculate" button is clicked, the JavaScript code retrieves the input values of the numbers and the selected operator. It then sends an Ajax POST request to the server-side servlet (CalculatorServlet) with the data.

The servlet receives the data, performs the corresponding arithmetic operation based on the operator, and sends back the result as the response.

Finally, the JavaScript code handles the successful response by updating the result div with the calculated value.

Please note that this is a simplified example for demonstration purposes, and in a production


## Example: AJAX calculator with ES6 and servlets

Here's an example of an Ajax calculator using ES6 syntax on the client-side and Servlets on the server-side. This example performs basic arithmetic operations (addition, subtraction, multiplication, and division) asynchronously without reloading the entire page.

1. HTML Markup (index.html):
```html
<!DOCTYPE html>
<html>
<head>
  <title>Ajax Calculator</title>
  <script>
    async function calculate() {
      const num1 = document.getElementById("num1").value;
      const num2 = document.getElementById("num2").value;
      const operator = document.getElementById("operator").value;

      try {
        const response = await fetch("calculator", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({ num1, num2, operator })
        });

        if (!response.ok) {
          throw new Error(response.statusText);
        }

        const result = await response.text();
        document.getElementById("result").textContent = `Result: ${result}`;
      } catch (error) {
        console.log(error);
      }
    }
  </script>
</head>
<body>
  <h2>Ajax Calculator</h2>
  <input type="number" id="num1" placeholder="Number 1"><br>
  <input type="number" id="num2" placeholder="Number 2"><br>
  <select id="operator">
    <option value="add">Addition</option>
    <option value="subtract">Subtraction</option>
    <option value="multiply">Multiplication</option>
    <option value="divide">Division</option>
  </select><br>
  <button onclick="calculate()">Calculate</button><br>
  <div id="result"></div>
</body>
</html>
```

2. Servlet (CalculatorServlet.java):
```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class CalculatorServlet extends HttpServlet {
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    int num1 = Integer.parseInt(request.getParameter("num1"));
    int num2 = Integer.parseInt(request.getParameter("num2"));
    String operator = request.getParameter("operator");
    int result = 0;

    switch (operator) {
      case "add":
        result = num1 + num2;
        break;
      case "subtract":
        result = num1 - num2;
        break;
      case "multiply":
        result = num1 * num2;
        break;
      case "divide":
        if (num2 != 0)
          result = num1 / num2;
        else
          response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Cannot divide by zero.");
        break;
      default:
        response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Invalid operator.");
    }

    response.getWriter().print(result);
  }
}
```

Make sure to configure your servlet container (e.g., Apache Tomcat) properly and update the necessary servlet mapping in the web.xml file.

In this example, we're using ES6 syntax, including async/await and the Fetch API for making the Ajax request. The `calculate()` function is an asynchronous function that retrieves the input values of the numbers and the selected operator. It then sends an Ajax POST request to the server-side servlet (CalculatorServlet) with the data in JSON format.

The servlet receives the data, performs the corresponding arithmetic operation based on the operator, and sends back the result as the response.

Finally, the JavaScript code handles the successful response by updating the result div with the calculated value.

Please note that this is a simplified example for demonstration purposes, and in a production environment, you would typically handle validation, error handling, and security considerations.

## Example: AJAX calculator with ES6 and a servlet and Gson.

Here's an example of an AJAX calculator using ES6 syntax on the client-side and a servlet on the server-side. The servlet will respond with JSON using the Google Gson library to convert a POJO that contains operators, the operation performed, and the response.

1. HTML Markup (index.html):
```html
<!DOCTYPE html>
<html>
<head>
  <title>AJAX Calculator</title>
</head>
<body>
  <h2>AJAX Calculator</h2>
  <input type="number" id="num1" placeholder="Number 1"><br>
  <input type="number" id="num2" placeholder="Number 2"><br>
  <select id="operator">
    <option value="add">Addition</option>
    <option value="subtract">Subtraction</option>
    <option value="multiply">Multiplication</option>
    <option value="divide">Division</option>
  </select><br>
  <button id="calculate">Calculate</button><br>
  <div id="operation"></div>
  <div id="response"></div>

  <script src="script.js"></script>
</body>
</html>
```

2. POJO (CalculationResult.java):
```java
public class CalculationResult {
  private String operator;
  private String operation;
  private int response;

  public CalculationResult(String operator, String operation, int response) {
    this.operator = operator;
    this.operation = operation;
    this.response = response;
  }

  // Getters and setters
}
```

3. JavaScript (script.js):
```javascript
document.addEventListener("DOMContentLoaded", function() {
  const calculateBtn = document.getElementById("calculate");
  const operationDiv = document.getElementById("operation");
  const responseDiv = document.getElementById("response");

  calculateBtn.addEventListener("click", function() {
    const num1 = document.getElementById("num1").value;
    const num2 = document.getElementById("num2").value;
    const operator = document.getElementById("operator").value;

    const data = {
      num1: num1,
      num2: num2,
      operator: operator
    };

    fetch("calculator", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(data)
    })
      .then(function(response) {
        if (!response.ok) {
          throw new Error(response.statusText);
        }
        return response.json();
      })
      .then(function(result) {
        operationDiv.textContent = `Operation: ${result.operation}`;
        responseDiv.textContent = `Response: ${result.response}`;
      })
      .catch(function(error) {
        console.log(error);
      });
  });
});
```

4. Servlet (CalculatorServlet.java):
```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.google.gson.Gson;

public class CalculatorServlet extends HttpServlet {
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    int num1 = Integer.parseInt(request.getParameter("num1"));
    int num2 = Integer.parseInt(request.getParameter("num2"));
    String operator = request.getParameter("operator");
    int result = 0;
    String operation = "";

    switch (operator) {
      case "add":
        result = num1 + num2;
        operation = num1 + " + " + num2;
        break;
      case "subtract":
        result = num1 - num2;
        operation = num1 + " - " + num2;
        break;
      case "multiply":
        result = num1 * num2;
        operation =

 num1 + " * " + num2;
        break;
      case "divide":
        if (num2 != 0) {
          result = num1 / num2;
          operation = num1 + " / " + num2;
        } else {
          response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Cannot divide by zero.");
          return;
        }
        break;
      default:
        response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Invalid operator.");
        return;
    }

    CalculationResult calculationResult = new CalculationResult(operator, operation, result);
    Gson gson = new Gson();
    String jsonResponse = gson.toJson(calculationResult);

    response.setContentType("application/json");
    PrintWriter out = response.getWriter();
    out.print(jsonResponse);
    out.flush();
  }
}
```

Please make sure to configure your servlet container (e.g., Apache Tomcat) properly and update the necessary servlet mapping in the web.xml file.

In this example, we're using ES6 syntax with unobtrusive JavaScript. The script.js file is loaded at the end of the HTML document, and it listens for the "DOMContentLoaded" event. When the user clicks the "Calculate" button, the event listener triggers, retrieves the input values, and sends an AJAX POST request to the server-side servlet (CalculatorServlet) with the data in JSON format.

The servlet receives the data, performs the corresponding arithmetic operation based on the operator, and creates a CalculationResult object with the operator, operation, and response. The Google Gson library is used to convert the CalculationResult object into JSON format.

Finally, the server responds with the JSON representation of the CalculationResult object, and the JavaScript code updates the respective div elements with the operation and response values.

Please note that this is a simplified example for demonstration purposes, and in a production environment, you would typically handle validation, error handling, and security considerations.

## Example: Synchronizing AJAX from 2 server requests:

Here's an example of an AJAX calculator where the operation is generated by a first servlet (S1) when the user presses a single button. The operation is requested when the button is pressed, not when the page is loaded. The first servlet calculates a random operation and sends it back to the frontend as JSON. When the response from S1 arrives at the frontend, it makes a second request to a second servlet (S2). S2 calculates the result based on the operation received from S1 and sends back the response containing the operator, operation, and result.

1. HTML Markup (index.html):
```html
<!DOCTYPE html>
<html>
<head>
  <title>AJAX Calculator</title>
</head>
<body>
  <h2>AJAX Calculator</h2>
  <div id="operation"></div>
  <div id="response"></div>
  <button id="calculate">Calculate</button>

  <script src="script.js"></script>
</body>
</html>
```

2. POJO (OperationRequest.java):
```java
public class OperationRequest {
  private String operator1;
  private String operator2;
  private String operation;

  // Getters and setters
}
```

3. First Servlet (S1.java):
```java
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Random;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.google.gson.Gson;

public class S1 extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String[] operators = { "+", "-", "*", "/" };
    Random random = new Random();
    String operator1 = operators[random.nextInt(operators.length)];
    String operator2 = operators[random.nextInt(operators.length)];
    String operation = "5 " + operator1 + " 3 " + operator2 + " 2";

    OperationRequest operationRequest = new OperationRequest();
    operationRequest.setOperator1(operator1);
    operationRequest.setOperator2(operator2);
    operationRequest.setOperation(operation);

    Gson gson = new Gson();
    String jsonResponse = gson.toJson(operationRequest);

    response.setContentType("application/json");
    PrintWriter out = response.getWriter();
    out.print(jsonResponse);
    out.flush();
  }
}
```

4. Second Servlet (S2.java):
```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.google.gson.Gson;

public class S2 extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String operationJson = request.getParameter("operationJson");

    Gson gson = new Gson();
    OperationRequest operationRequest = gson.fromJson(operationJson, OperationRequest.class);

    String operator1 = operationRequest.getOperator1();
    String operator2 = operationRequest.getOperator2();
    String operation = operationRequest.getOperation();

    int result = calculateResult(operator1, operator2, operation);

    OperationRequest operationResult = new OperationRequest();
    operationResult.setOperator1(operator1);
    operationResult.setOperator2(operator2);
    operationResult.setOperation(operation);
    operationResult.setResult(result);

    String jsonResponse = gson.toJson(operationResult);

    response.setContentType("application/json");
    PrintWriter out = response.getWriter();
    out.print(jsonResponse);
    out.flush();
  }

  private int calculateResult(String operator1, String operator2, String operation) {
    // Perform the calculation based on the operators and operation
    // Return the calculated result
    return 10; // Example result
  }
}
```

5. JavaScript (script.js):
```javascript
document.addEventListener("

DOMContentLoaded", function() {
  const operationDiv = document.getElementById("operation");
  const calculateBtn = document.getElementById("calculate");
  const responseDiv = document.getElementById("response");

  function generateOperation() {
    fetch("S1")
      .then(function(response) {
        if (!response.ok) {
          throw new Error(response.statusText);
        }
        return response.json();
      })
      .then(function(operationRequest) {
        const operator1 = operationRequest.operator1;
        const operator2 = operationRequest.operator2;
        const operation = operationRequest.operation;

        operationDiv.textContent = `Operation: ${operation}`;
        calculateBtn.addEventListener("click", function() {
          fetch("S2?operationJson=" + JSON.stringify(operationRequest))
            .then(function(response) {
              if (!response.ok) {
                throw new Error(response.statusText);
              }
              return response.json();
            })
            .then(function(operationResult) {
              const result = operationResult.result;
              responseDiv.textContent = `Result: ${result}`;
            })
            .catch(function(error) {
              console.log(error);
            });
        });
      })
      .catch(function(error) {
        console.log(error);
      });
  }

  generateOperation();
});
```

In this example, when the page loads, the `generateOperation()` function is called to fetch the initial operation from the first servlet (S1) when the user presses the button. The operation is displayed in the "operation" div. When the user clicks the "Calculate" button, the event listener triggers, and it makes a second request to the second servlet (S2) with the operation JSON as a query parameter. S2 calculates the result based on the received operation, and the response containing the result is displayed in the "response" div.

Please make sure to configure your servlet container (e.g., Apache Tomcat) properly and update the necessary servlet mappings in the web.xml file.

Note: This example assumes you have the necessary server-side setup and configuration in place to run the servlets.

