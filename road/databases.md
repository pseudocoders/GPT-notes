# Databases, connection pools and transactions

>
> :red_circle: Explore and learn: [**Databases in GPT-notes**](../basics/databases.md)
> 

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
