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

### Connection

To establish a JDBC connection to a database, you need to follow these general steps:

1. Load the JDBC Driver: The first step is to load the JDBC driver class provided by the database vendor. The driver class is responsible for handling the communication between the Java application and the database. You can load the driver class using the `Class.forName()` method, providing the fully qualified name of the driver class. For example:

```java
Class.forName("com.mysql.jdbc.Driver");
```

2. Create a Connection URL: The connection URL specifies the necessary details to establish a connection to the database. The URL format depends on the database vendor and the specific database instance you are connecting to. It typically includes information such as the database host, port, database name, and any additional connection parameters. For example, for a MySQL database:

```java
String url = "jdbc:mysql://localhost:3306/mydatabase";
```

3. Establish the Connection: Use the `DriverManager.getConnection()` method to establish a connection to the database. Pass the connection URL, username, and password as arguments to this method. For example:

```java
Connection connection = DriverManager.getConnection(url, "username", "password");
```

4. Perform Database Operations: Once the connection is established, you can create statements or prepared statements to execute SQL queries and perform database operations. You can use the `Connection.createStatement()` method to create a Statement object or `Connection.prepareStatement()` to create a PreparedStatement object.

5. Process Results and Manage Resources: After executing queries or database operations, you can retrieve the results using a ResultSet object and process the data as needed. Make sure to close the ResultSet, Statement, and Connection objects when you are finished to release the resources and free up connections.

Here's a simplified example that demonstrates the steps above:

```java
import java.sql.*;

public class JDBCExample {
    public static void main(String[] args) {
        try {
            // Load the JDBC driver
            Class.forName("com.mysql.jdbc.Driver");
            
            // Create the connection URL
            String url = "jdbc:mysql://localhost:3306/mydatabase";
            
            // Establish the connection
            Connection connection = DriverManager.getConnection(url, "username", "password");
            
            // Create a statement
            Statement statement = connection.createStatement();
            
            // Execute a query
            ResultSet resultSet = statement.executeQuery("SELECT * FROM users");
            
            // Process the results
            while (resultSet.next()) {
                String username = resultSet.getString("username");
                System.out.println("Username: " + username);
            }
            
            // Close resources
            resultSet.close();
            statement.close();
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Remember to handle exceptions appropriately and close the resources in a `finally` block or use try-with-resources construct to ensure proper resource cleanup.

By following these steps, you can establish a JDBC connection to a database, execute queries, and perform database operations within your Java application.

### Statements and prepared statements

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

#### Injection atack example

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

### ResultSets

The ResultSet interface in JDBC represents the result of a database query. It provides methods to navigate through the rows of the result set and retrieve the data returned by the query. Here are the key aspects and operations related to ResultSet:

1. Retrieving Data: ResultSet allows you to access the data in the current row of the result set. You can use methods like `getInt()`, `getString()`, `getDouble()`, etc., to retrieve the values of specific columns in the current row. These methods take the column index or column name as arguments.

2. Navigating Through Rows: ResultSet maintains a cursor that points to the current row in the result set. Initially, the cursor is positioned before the first row. You can use `next()` method to move the cursor to the next row. It returns true if there is a next row, and false if there are no more rows.

3. Getting Metadata: ResultSet provides methods to retrieve metadata about the result set, such as the number of columns, the name of columns, and their data types. You can use methods like `getMetaData()` to obtain the ResultSetMetaData object and then access the metadata information.

4. Scrollable Result Sets: Some JDBC drivers and database systems support scrollable result sets, which allow navigating to any row in the result set. You can use methods like `beforeFirst()`, `absolute()`, `relative()`, etc., to move the cursor to a specific row or position in the result set.

5. Updating Result Sets: If the ResultSet is created with a type that supports updates (e.g., `ResultSet.TYPE_SCROLL_SENSITIVE`), you can modify the data in the result set using the appropriate update methods, such as `updateInt()`, `updateString()`, `deleteRow()`, etc. These changes can be propagated to the database using the `updateRow()` or `insertRow()` methods.

6. Closing the ResultSet: After you have finished working with the result set, it is important to close it to release any resources associated with it. You can use the `close()` method to close the ResultSet.

Here's a simple example that demonstrates retrieving data from a ResultSet:

```java
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery("SELECT * FROM employees");

while (resultSet.next()) {
    int id = resultSet.getInt("id");
    String name = resultSet.getString("name");
    double salary = resultSet.getDouble("salary");
    
    System.out.println("ID: " + id + ", Name: " + name + ", Salary: " + salary);
}

resultSet.close();
statement.close();
```

In the code above, we execute a SELECT query and retrieve data from the ResultSet using methods like `getInt()`, `getString()`, and `getDouble()`.

Note that ResultSet is forward-only by default, meaning you can only move forward through the rows. If you need more advanced functionality like scrolling or updatable result sets, you can specify the appropriate ResultSet type and concurrency when creating the Statement or PreparedStatement object.

Remember to handle exceptions appropriately and close the ResultSet, Statement, and Connection objects to release resources and avoid resource leaks.

### Example: Connection and data retrieval example using a ResultSet

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

#### Taking out database information into a resorces file

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

#### Parameterized queries based on the request parameters

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

### Example

Here's a complete example of configuring and using HikariCP in a servlet:

1. Add HikariCP Dependency: First, you need to include the HikariCP library in your project. You can do this by adding the dependency to your project's build file (e.g., Maven or Gradle). Here's an example for Maven:

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>4.0.3</version>
</dependency>
```

2. Configure HikariCP in Servlet Initialization: In your servlet, you can configure HikariCP in the `init()` method by creating a HikariDataSource object and setting the necessary configuration parameters. Here's an example:

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class MyServlet extends HttpServlet {
    private HikariDataSource dataSource;

    @Override
    public void init() throws ServletException {
        super.init();

        // Create HikariCP configuration
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:mysql://localhost:3306/mydatabase");
        config.setUsername("username");
        config.setPassword("password");
        // Configure other HikariCP parameters as needed

        // Create the HikariCP data source
        dataSource = new HikariDataSource(config);
    }

    // Override other servlet methods (doGet, doPost, etc.)

    @Override
    public void destroy() {
        super.destroy();
        
        // Close the HikariCP data source
        if (dataSource != null) {
            dataSource.close();
        }
    }
}
```

In the `init()` method, you create a HikariConfig object and set the JDBC URL, username, password, and any other desired configuration parameters.

3. Use HikariCP Connection in Servlet Methods: In your servlet methods (e.g., `doGet()`, `doPost()`), you can obtain a connection from the HikariCP data source and use it to perform database operations. Here's an example:

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.zaxxer.hikari.HikariDataSource;

public class MyServlet extends HttpServlet {
    private HikariDataSource dataSource;

    @Override
    public void init() throws ServletException {
        super.init();
        // HikariCP configuration (as shown in the previous step)
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try (Connection connection = dataSource.getConnection()) {
            // Use the connection for database operations
            PreparedStatement statement = connection.prepareStatement("SELECT * FROM users");
            ResultSet resultSet = statement.executeQuery();

            // Process the result set
            while (resultSet.next()) {
                String username = resultSet.getString("username");
                // ...
            }

            resultSet.close();
            statement.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }

        // Continue with servlet logic and response handling
    }

    @Override
    public void destroy() {
        super.destroy();
        // Closing HikariCP data source (as shown in the previous step)
    }
}
```

In the `doGet()` method (or any other servlet method), you can obtain a connection from the HikariCP data source using `

dataSource.getConnection()`. Then, you can use the connection to prepare statements, execute queries, and process the result set.

Remember to properly handle exceptions and close the ResultSet, PreparedStatement, and Connection objects to release resources and avoid resource leaks.

This example demonstrates how to configure and use HikariCP in a servlet, including obtaining connections from the connection pool and performing database operations.

### Configuration

HikariCP provides a variety of configuration parameters that allow you to fine-tune its behavior and performance. These parameters can be set in the HikariConfig object when configuring the HikariCP data source. Here are some of the commonly used configuration parameters:

1. JDBC URL (`jdbcUrl`): The URL of the database to connect to. It typically includes the database type, host, port, and database name.

2. Username (`username`) and Password (`password`): The credentials used to authenticate with the database.

3. Connection Pool Size (`maximumPoolSize`): The maximum number of connections that can be created in the connection pool. This determines the maximum concurrent connections that can be established with the database.

4. Connection Timeout (`connectionTimeout`): The maximum time (in milliseconds) that HikariCP will wait for a connection to be established with the database. If the timeout is exceeded, an exception will be thrown.

5. Minimum Idle Connections (`minimumIdle`): The minimum number of idle connections that HikariCP will maintain in the connection pool. Idle connections are ready-to-use connections that are kept in the pool to improve performance.

6. Maximum Lifetime of Connections (`maxLifetime`): The maximum time (in milliseconds) that a connection can remain in the pool before being closed and replaced with a new connection. This helps to prevent connections from becoming stale or unreliable.

7. Idle Connection Timeout (`idleTimeout`): The maximum time (in milliseconds) that an idle connection can remain in the pool before being closed and removed.

8. Connection Validation (`connectionTestQuery`): A SQL query or statement used to validate the health of a connection before it is given to the application. If the query fails, the connection is considered invalid and will be discarded.

9. Auto-commit Mode (`autoCommit`): Specifies whether connections obtained from the pool should be in auto-commit mode by default. Auto-commit mode automatically commits each database operation as a separate transaction.

10. Data Source Properties: Additional properties specific to the database driver or data source can be set using the `dataSourceProperties` property of the HikariConfig object. These properties are driver-specific and can be used to configure driver-specific behavior.

These are just a few examples of the configuration parameters available in HikariCP. You can refer to the HikariCP documentation or the HikariConfig class for a comprehensive list of all available configuration parameters and their detailed descriptions.

By adjusting these configuration parameters according to your application's needs and the database environment, you can optimize the performance and resource utilization of your HikariCP connection pool.

### Configuration in a Resources file 

To configure HikariCP parameters using a resource file, you can use a properties file or a YAML file to store the configuration. Here's an example of how you can modify the previous example to load the HikariCP configuration from a properties file:

1. Create a properties file (e.g., `hikari.properties`) and define the HikariCP configuration parameters. For example:

```
jdbcUrl=jdbc:mysql://localhost:3306/mydatabase
username=username
password=password
maximumPoolSize=10
connectionTimeout=30000
minimumIdle=5
maxLifetime=1800000
idleTimeout=600000
connectionTestQuery=SELECT 1
autoCommit=true
```

2. Update the servlet's `init()` method to load the configuration from the properties file:

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class MyServlet extends HttpServlet {
    private HikariDataSource dataSource;

    @Override
    public void init() throws ServletException {
        super.init();

        // Load the properties file
        Properties properties = new Properties();
        try (InputStream input = getClass().getClassLoader().getResourceAsStream("hikari.properties")) {
            properties.load(input);
        } catch (IOException e) {
            throw new ServletException("Failed to load HikariCP configuration", e);
        }

        // Create HikariCP configuration
        HikariConfig config = new HikariConfig(properties);

        // Create the HikariCP data source
        dataSource = new HikariDataSource(config);
    }

    // Override other servlet methods (doGet, doPost, etc.)

    @Override
    public void destroy() {
        super.destroy();
        // Closing HikariCP data source (as shown in the previous example)
    }
}
```

In this example, we load the properties file (`hikari.properties`) using the `getClass().getClassLoader().getResourceAsStream()` method. We then create a `Properties` object and load the file contents into it.

Next, we create a `HikariConfig` object and pass the `Properties` object to it. HikariCP will automatically parse and apply the properties to configure the data source.

With this modification, you can configure HikariCP parameters in a separate properties file, making it easier to manage and modify the configuration without modifying the Java code directly.






