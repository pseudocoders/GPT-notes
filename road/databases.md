# Databases, connection pools and transactions

>
> :red_circle: Explore and learn: [**Databases in GPT-notes**](../basics/databases.md)
> 

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


