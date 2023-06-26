# Databases, connection pools and transactions

>
> :red_circle: Explore and learn: [**Databases in GPT-notes**](../basics/databases.md)
> 

## JDBC

JDBC (Java Database Connectivity) is a Java API (Application Programming Interface) that provides a standard way for Java applications to interact with databases. It allows developers to execute SQL queries, retrieve and manipulate data, and perform database operations using Java code.

Here are the key components and concepts related to JDBC:

1. JDBC Driver: A JDBC driver is a software component that acts as an interface between the Java application and the database. It provides the necessary functionality to establish a connection, send SQL statements to the database, and retrieve the results. JDBC drivers are typically provided by the database vendors and need to be loaded and registered in the Java application.

2. Connection: The Connection object represents a connection to the database. It is responsible for establishing a connection, authenticating with the database, and managing the communication channel with the database server. The Connection object is obtained by calling the `DriverManager.getConnection()` method, passing the database URL, username, and password.

3. Statement: The Statement object is used to execute SQL queries and statements on the database. It can be used to perform operations like executing SELECT queries, updating data with INSERT, UPDATE, and DELETE statements, or executing any other SQL command. Statements are created using the `Connection.createStatement()` method.

4. PreparedStatement: The PreparedStatement object is an extension of the Statement interface that provides a way to execute parameterized SQL queries. It allows you to create SQL statements with placeholders for parameters and then set the values of those parameters before executing the query. PreparedStatement helps prevent SQL injection attacks and improves performance by reusing the query execution plan.

5. ResultSet: The ResultSet object represents the result of a SQL query executed against the database. It allows you to navigate through the rows of the result set and retrieve the data returned by the query. You can access the columns of the result set using the `getXXX()` methods, where `XXX` represents the data type of the column.

6. Transaction Management: JDBC supports transaction management, which allows you to group multiple database operations into a single logical unit. You can start a transaction, perform database operations, and commit the changes or rollback if an error occurs.

7. Exception Handling: JDBC throws various exceptions, such as SQLException, to handle database-related errors. Proper exception handling is essential to handle errors gracefully and take appropriate actions.

JDBC provides a standardized way to interact with databases, allowing Java applications to be database-independent and easily switch between different database vendors. It has become a foundational technology for Java database programming and is widely used in various Java-based applications, including web applications, desktop applications, and enterprise systems.

## Statements and prepared statements

In Java, when accessing databases using SQL, there are two common approaches: using regular statements and using prepared statements. Let's explore the differences between these two approaches:

1. Regular Statement:
A regular statement is a straightforward way of executing SQL queries in Java. It allows you to write SQL statements directly within your Java code as strings. For example:

```java
Statement statement = connection.createStatement();
String sql = "SELECT * FROM users WHERE id = 1";
ResultSet resultSet = statement.executeQuery(sql);
```

With regular statements, the SQL query is compiled and executed each time it is executed. If you have dynamic values or parameters to be included in your query, you need to concatenate them as strings, which can be error-prone and potentially vulnerable to SQL injection attacks.

2. Prepared Statement:
A prepared statement is a pre-compiled SQL statement that can be parameterized. It is typically used when you have a query that needs to be executed multiple times with different parameter values. Prepared statements offer several advantages over regular statements:

- Improved Performance: Prepared statements are pre-compiled and stored in a compiled form in the database. This allows the database to reuse the query execution plan, resulting in better performance compared to regular statements, especially when executing the same query multiple times with different parameter values.

- Parameter Binding: Prepared statements support parameter binding, allowing you to set parameter values dynamically without having to concatenate them into the SQL query as strings. This helps to prevent SQL injection attacks and ensures proper handling of special characters and data types. Here's an example:

```java
String sql = "SELECT * FROM users WHERE id = ?";
PreparedStatement statement = connection.prepareStatement(sql);
statement.setInt(1, 1); // Bind the value for the parameter
ResultSet resultSet = statement.executeQuery();
```

- Readability and Maintainability: Prepared statements separate the SQL code from the data values, making the code more readable and easier to maintain. The SQL query remains intact, while the parameters can be changed without modifying the query itself.

In summary, while regular statements are suitable for simple queries or one-time executions, prepared statements are preferred for queries that are executed multiple times or involve dynamic parameters. Prepared statements offer better performance, parameter binding for security, and improved code readability.

### Injection atack example

An SQL injection attack occurs when an attacker maliciously manipulates the input data in an SQL query to execute unintended commands or gain unauthorized access to the database. Let's consider an example using a regular statement without proper input validation or parameter binding:

```java
String username = request.getParameter("username");
String password = request.getParameter("password");

String sql = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(sql);
```

In the code above, the username and password inputs from a user are concatenated directly into the SQL query string. An attacker can exploit this vulnerability by providing crafted input, such as:

Username: `' OR '1'='1'--`
Password: `anything`

The resulting SQL query would be:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1'--' AND password = 'anything'
```

The attacker's intention is to make the condition `1=1` always true, effectively bypassing the username and password check. The double hyphen `--` denotes a comment, which comments out the rest of the original query.

With this manipulated input, the attacker could gain unauthorized access to the system by logging in as any user or retrieving sensitive information from the database.

To prevent SQL injection attacks, it is crucial to use prepared statements or employ proper input validation and sanitization techniques to ensure that user input is treated as data and not executable SQL code.

## Connection and data retrieval example

Here's an example of a Java servlet that connects to a database, retrieves data based on a request, and returns the results in JSON format using the Google Gson library:

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.google.gson.Gson;

public class DataServlet extends HttpServlet {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String DB_USER = "username";
    private static final String DB_PASSWORD = "password";

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        try {
            // Connect to the database
            conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);

            // Execute the query
            stmt = conn.createStatement();
            rs = stmt.executeQuery("SELECT * FROM mytable");

            // Create a list to hold the results
            List<DataObject> data = new ArrayList<>();

            // Iterate through the result set and retrieve data
            while (rs.next()) {
                DataObject obj = new DataObject();
                obj.setId(rs.getInt("id"));
                obj.setName(rs.getString("name"));
                // Set other attributes as needed
                data.add(obj);
            }

            // Set the response type to JSON
            response.setContentType("application/json");
            PrintWriter out = response.getWriter();

            // Use Gson to convert the list to JSON
            Gson gson = new Gson();
            String json = gson.toJson(data);

            // Write the JSON response
            out.print(json);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // Close the database resources
            try {
                if (rs != null) {
                    rs.close();
                }
                if (stmt != null) {
                    stmt.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

In this example, we assume that you have a MySQL database running locally with a table named "mytable" containing columns "id" and "name". Make sure to replace the DB_URL, DB_USER, and DB_PASSWORD with your actual database connection details.

The servlet uses JDBC to connect to the database and execute the query. The retrieved data is then stored in a list of `DataObject` instances, where `DataObject` is a simple POJO class representing the structure of your data.

After retrieving the data, the servlet sets the response type to JSON and uses the Gson library to convert the list of `DataObject` instances into a JSON string. Finally, it writes the JSON response back to the client.

Please note that this is a basic example, and in a real-world application, you would typically handle exceptions more gracefully, use connection pooling, and possibly incorporate frameworks like Spring to handle dependency injection and simplify the database access code.

### Taking out database information into a resorces file

Here's a modified version of the previous example where the database connection parameters are stored in a resource file:

**db.properties:**
```
db.url=jdbc:mysql://localhost:3306/mydatabase
db.user=username
db.password=password
```

**DataServlet.java:**
```java
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.google.gson.Gson;

public class DataServlet extends HttpServlet {
    private static final String DB_PROPERTIES_FILE = "db.properties";

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        try {
            // Load the database connection properties
            Properties properties = loadProperties(DB_PROPERTIES_FILE);

            // Connect to the database
            conn = DriverManager.getConnection(properties.getProperty("db.url"),
                    properties.getProperty("db.user"),
                    properties.getProperty("db.password"));

            // Execute the query
            stmt = conn.createStatement();
            rs = stmt.executeQuery("SELECT * FROM mytable");

            // Create a list to hold the results
            List<DataObject> data = new ArrayList<>();

            // Iterate through the result set and retrieve data
            while (rs.next()) {
                DataObject obj = new DataObject();
                obj.setId(rs.getInt("id"));
                obj.setName(rs.getString("name"));
                // Set other attributes as needed
                data.add(obj);
            }

            // Set the response type to JSON
            response.setContentType("application/json");
            PrintWriter out = response.getWriter();

            // Use Gson to convert the list to JSON
            Gson gson = new Gson();
            String json = gson.toJson(data);

            // Write the JSON response
            out.print(json);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // Close the database resources
            try {
                if (rs != null) {
                    rs.close();
                }
                if (stmt != null) {
                    stmt.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    private Properties loadProperties(String fileName) throws IOException {
        Properties properties = new Properties();
        try (InputStream inputStream = getClass().getClassLoader().getResourceAsStream(fileName)) {
            properties.load(inputStream);
        }
        return properties;
    }
}
```

In this modified example, the database connection parameters are stored in a resource file named `db.properties`. The `loadProperties()` method is responsible for loading the properties from the file.

The servlet loads the properties from the file and then establishes a database connection using the retrieved parameters. The rest of the code remains the same as the previous example.

Make sure to place the `db.properties` file in the appropriate location, such as the `src` folder of your web application, so that it can be loaded correctly by the `loadProperties()` method.

By using a resource file, you can easily modify the connection parameters without modifying the servlet code, which provides greater flexibility and separation of concerns.

### Parameterized queries based on the request parameters

Here's a modified version of the previous example where the SQL query is parameterized based on the request parameters:

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.google.gson.Gson;

public class DataServlet extends HttpServlet {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String DB_USER = "username";
    private static final String DB_PASSWORD = "password";

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;

        try {
            // Connect to the database
            conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);

            // Get the request parameters
            String name = request.getParameter("name");

            // Prepare the SQL statement
            String sql = "SELECT * FROM mytable WHERE name = ?";
            stmt = conn.prepareStatement(sql);
            stmt.setString(1, name);

            // Execute the query
            rs = stmt.executeQuery();

            // Create a list to hold the results
            List<DataObject> data = new ArrayList<>();

            // Iterate through the result set and retrieve data
            while (rs.next()) {
                DataObject obj = new DataObject();
                obj.setId(rs.getInt("id"));
                obj.setName(rs.getString("name"));
                // Set other attributes as needed
                data.add(obj);
            }

            // Set the response type to JSON
            response.setContentType("application/json");
            PrintWriter out = response.getWriter();

            // Use Gson to convert the list to JSON
            Gson gson = new Gson();
            String json = gson.toJson(data);

            // Write the JSON response
            out.print(json);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // Close the database resources
            try {
                if (rs != null) {
                    rs.close();
                }
                if (stmt != null) {
                    stmt.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

In this modified example, the SQL query is parameterized based on the `name` request parameter. The `name` parameter is retrieved using `request.getParameter("name")`, and the SQL query is modified accordingly to filter the results based on the provided name.

The `PreparedStatement` is used to set the parameter value using `stmt.setString(1, name)`, where `1` represents the index of the parameter in the SQL query.

Please note that in this example, the assumption is made that you have a single request parameter named "name." You can modify the code to handle multiple parameters or adjust it according to your specific requirements.

## Hikari Connection pool

HikariCP is a high-performance, lightweight, and extremely fast connection pooling library for Java applications. It is widely regarded as one of the best connection pooling options available due to its exceptional performance, low latency, and efficient resource management.

Here are some key features and characteristics of HikariCP:

1. Performance: HikariCP is known for its excellent performance and high throughput. It achieves this by utilizing a combination of optimized algorithms and techniques, such as lock-free data structures, asynchronous processing, and minimal thread context switching.

2. Lightweight and Low Footprint: HikariCP has a small footprint, making it suitable for resource-constrained environments. It has minimal dependencies, resulting in faster startup times and efficient memory utilization.

3. Connection Reuse and Recycling: HikariCP emphasizes connection reuse and recycling. It maintains a pool of pre-established connections to the database and efficiently manages their reuse. Reusing connections eliminates the overhead of creating new connections for each request, resulting in improved performance.

4. Configurability: HikariCP provides extensive configuration options to fine-tune its behavior based on specific application requirements. Configuration parameters include maximum pool size, connection timeout, connection validation, and many more.

5. Connection Health Monitoring: HikariCP actively monitors the health and validity of connections in the pool. It performs periodic checks to ensure that connections are still valid and available. Unhealthy or idle connections are automatically removed from the pool to maintain optimal performance.

6. Connection Pool Statistics: HikariCP collects and exposes various statistics related to connection pooling, including the number of active connections, idle connections, connection acquisition times, and more. These statistics can be useful for monitoring and performance tuning.

7. Integration and Compatibility: HikariCP is compatible with various database drivers and frameworks. It can be easily integrated into Java applications using popular frameworks such as Spring, Hibernate, and Java Database Connectivity (JDBC).

Overall, HikariCP offers an efficient, high-performance, and developer-friendly solution for connection pooling in Java applications. Its focus on performance, configurability, and lightweight design makes it a popular choice for optimizing database interactions and improving overall application performance.

