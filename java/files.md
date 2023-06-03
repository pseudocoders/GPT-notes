# Files

## Working with text files

This is an example on how to to open a text file, read its contents, add some text, and save it using Java. You can follow these general steps:

1. **Create a File object**: Start by creating a `File` object that represents the text file you want to open. You need to provide the file's path as a parameter to the `File` constructor.

```java
File file = new File("path/to/your/text/file.txt");
```

2. **Read the existing text**: Use a `BufferedReader` to read the existing text from the file. Open the file using a `FileReader` and wrap it in a `BufferedReader` for efficient reading.

```java
BufferedReader reader = new BufferedReader(new FileReader(file));
String line;
StringBuilder content = new StringBuilder();

while ((line = reader.readLine()) != null) {
    content.append(line).append("\n");
}

reader.close();
```

The `content` StringBuilder will store the existing text of the file.

3. **Add new text**: Now, you can add the desired text to the existing content. You can use the `append()` method of the `StringBuilder` class to add text.

```java
String newText = "This is the new text to be added.";
content.append(newText).append("\n");
```

4. **Save the modified text**: To save the modified text back to the file, you can use a `BufferedWriter` to write the content to the file. Open the file using a `FileWriter` and wrap it in a `BufferedWriter` for efficient writing.

```java
BufferedWriter writer = new BufferedWriter(new FileWriter(file));
writer.write(content.toString());
writer.close();
```

Make sure to handle any potential exceptions that may occur during file operations by using try-catch blocks or declaring them in a method's throws clause.

## Working with binary files

This is an example on how to open a binary file containing a JPG image, read its content, and close the file using Java. You can follow these steps:

1. **Create a File object**: Start by creating a `File` object that represents the binary file you want to open. Provide the file's path as a parameter to the `File` constructor.

```java
File file = new File("path/to/your/binary/file.jpg");
```

2. **Create a FileInputStream**: Use a `FileInputStream` to open the binary file for reading.

```java
FileInputStream fis = new FileInputStream(file);
```

3. **Read the file content**: Create a byte array to hold the contents of the file. Read the bytes from the file using the `read()` method of the `FileInputStream` class.

```java
byte[] content = new byte[(int) file.length()];
fis.read(content);
```

4. **Process the content**: Depending on your specific requirements, you can process the content in different ways. In the case of a JPG image, you may want to perform further operations such as decoding the image or saving it to another location.

For example, if you want to decode the image and create a `BufferedImage` object:

```java
BufferedImage image = ImageIO.read(new ByteArrayInputStream(content));
```

5. **Close the FileInputStream**: After you have finished reading and processing the content, make sure to close the `FileInputStream` to release the resources associated with it.

```java
fis.close();
```

By following these steps, you can open a binary file containing a JPG image, read its content, perform necessary operations, and then close the file using Java. Remember to handle any potential exceptions that may occur during file operations.

## Uploading a file

### Example: uploading a file to disk

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Part;

@WebServlet("/upload")
@MultipartConfig
public class FileUploadServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final String UPLOAD_DIRECTORY = "/path/to/upload/directory";
    
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // Create the upload directory if it doesn't exist
        File uploadDir = new File(UPLOAD_DIRECTORY);
        if (!uploadDir.exists()) {
            uploadDir.mkdirs();
        }
        
        try {
            Part filePart = request.getPart("file"); // Name of the upload form field
            String fileName = getFileName(filePart);
            
            // Validate the file extension
            if (!isAllowedExtension(fileName)) {
                response.getWriter().println("Error: Invalid file extension.");
                return;
            }
            
            // Validate the file name
            if (!isValidFileName(fileName)) {
                response.getWriter().println("Error: Invalid file name.");
                return;
            }
            
            // Write the file to the disk
            filePart.write(UPLOAD_DIRECTORY + File.separator + fileName);
            
            response.getWriter().println("File uploaded successfully.");
        } catch (IOException e) {
            response.getWriter().println("Error: " + e.getMessage());
        }
    }
    
    private String getFileName(Part part) {
        String contentDisposition = part.getHeader("content-disposition");
        String[] elements = contentDisposition.split(";");
        for (String element : elements) {
            if (element.trim().startsWith("filename")) {
                return element.substring(element.indexOf('=') + 1).trim().replace("\"", "");
            }
        }
        return null;
    }
    
    private boolean isAllowedExtension(String fileName) {
        String extension = fileName.substring(fileName.lastIndexOf('.') + 1);
        return extension.equalsIgnoreCase("txt") || extension.equalsIgnoreCase("pdf");
    }
    
    private boolean isValidFileName(String fileName) {
        // Perform your custom validation logic here
        return fileName.matches("[a-zA-Z0-9_]+\\.[a-zA-Z]{3}");
    }
}
```

In this example:
- The servlet is mapped to the URL pattern "/upload" using the `@WebServlet` annotation.
- The `@MultipartConfig` annotation is used to indicate that this servlet expects multipart/form-data requests, which is typically used for file uploads.
- The `doPost` method is overridden to handle the HTTP POST request for uploading the file.
- The file is retrieved from the request using the `getPart` method.
- The file name is extracted from the `content-disposition` header of the part.
- The file extension is validated to ensure it is allowed.
- The file name is validated to ensure it meets the desired criteria.
- The file is written to the specified upload directory using the `write` method of the `Part` object.

Make sure to replace "/path/to/upload/directory" with the actual path to the directory where you want to store the uploaded files. Also, customize the allowed extensions and the validation logic for the file name according to your requirements.

Note: Don't forget to configure the necessary `<form>` in your HTML page to submit the file to this servlet using the appropriate form field name ("file" in this example).

### Example: uploading a file to db

```java
import java.io.IOException;
import java.io.InputStream;
import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Part;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

@WebServlet("/upload")
@MultipartConfig
public class FileUploadServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // Database connection settings
    private static final String DB_URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String DB_USER = "username";
    private static final String DB_PASSWORD = "password";

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        try {
            Part filePart = request.getPart("file"); // Name of the upload form field
            String fileName = getFileName(filePart);
            InputStream fileContent = filePart.getInputStream();

            // Save the file in the database
            saveFileInDatabase(fileName, fileContent);

            response.getWriter().println("File uploaded and saved in the database successfully.");
        } catch (IOException | SQLException e) {
            response.getWriter().println("Error: " + e.getMessage());
        }
    }

    private String getFileName(Part part) {
        String contentDisposition = part.getHeader("content-disposition");
        String[] elements = contentDisposition.split(";");
        for (String element : elements) {
            if (element.trim().startsWith("filename")) {
                return element.substring(element.indexOf('=') + 1).trim().replace("\"", "");
            }
        }
        return null;
    }

    private void saveFileInDatabase(String fileName, InputStream fileContent) throws SQLException {
        Connection connection = null;
        PreparedStatement statement = null;

        try {
            // Establish the database connection
            connection = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);

            // Prepare the SQL statement
            String sql = "INSERT INTO files (name, content) VALUES (?, ?)";
            statement = connection.prepareStatement(sql);
            statement.setString(1, fileName);
            statement.setBlob(2, fileContent);

            // Execute the statement
            statement.executeUpdate();
        } finally {
            // Close the resources
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        }
    }
}
```

In this modified example:
- A database connection is established using JDBC, assuming you have a MySQL database with the appropriate credentials.
- The `saveFileInDatabase` method is added to handle the database operations.
- The `PreparedStatement` is used to execute the SQL INSERT statement, binding the file name and the file content as parameters.
- The `fileContent` is obtained from the `InputStream` of the uploaded file.

Make sure to replace the `DB_URL`, `DB_USER`, and `DB_PASSWORD` variables with your actual database connection details. Additionally, adjust the database table and column names as per your database schema.

Please note that the example provided is simplified and does not cover error handling, proper connection management, or other best practices. It's recommended to enhance the code according to your specific requirements and follow good practices when working with databases.

### Sending file


```html
<!DOCTYPE html>
<html>
<head>
    <title>File Upload</title>
    <script>
        function uploadFile() {
            const fileInput = document.getElementById('fileInput');
            const file = fileInput.files[0];
            
            const formData = new FormData();
            formData.append('file', file);

            const xhr = new XMLHttpRequest();
            xhr.open('POST', '/upload', true);
            xhr.onreadystatechange = function() {
                if (xhr.readyState === 4 && xhr.status === 200) {
                    console.log(xhr.responseText);
                }
            };
            xhr.send(formData);
        }
    </script>
</head>
<body>
    <input type="file" id="fileInput" accept=".jpg, .png, .gif">
    <button onclick="uploadFile()">Upload</button>
</body>
</html>
```

In this example:
- The user selects a file using the `<input type="file">` element with the `id` attribute set to "fileInput".
- The `uploadFile` function is triggered when the "Upload" button is clicked.
- Inside the function, the selected file is accessed using `fileInput.files[0]`.
- A new `FormData` object is created, and the file is appended to it using the `append` method.
- An AJAX request is created using the `XMLHttpRequest` object.
- The request is set to a POST method and the URL is set to "/upload" (change it according to your servlet mapping).
- The `FormData` object is sent as the request payload using the `send` method.
- The `onreadystatechange` event handler is used to handle the response when the request is completed.

Note that this example uses vanilla JavaScript and the XMLHttpRequest object. Alternatively, you can use libraries like Axios or Fetch API for simplified AJAX requests in modern JavaScript frameworks or libraries.

Make sure to adjust the URL in the `xhr.open` method to match the URL of your servlet.

### Multiupload

#### Frontend

```html
<!DOCTYPE html>
<html>
<head>
    <title>File Upload</title>
    <script>
        function uploadFiles() {
            const fileInput = document.getElementById('fileInput');
            const files = fileInput.files;
            
            const formData = new FormData();
            for (let i = 0; i < files.length; i++) {
                formData.append('files', files[i]);
            }

            const xhr = new XMLHttpRequest();
            xhr.open('POST', '/upload', true);
            xhr.onreadystatechange = function() {
                if (xhr.readyState === 4 && xhr.status === 200) {
                    console.log(xhr.responseText);
                }
            };
            xhr.send(formData);
        }
    </script>
</head>
<body>
    <input type="file" id="fileInput" accept=".jpg, .png, .gif" multiple>
    <button onclick="uploadFiles()">Upload</button>
</body>
</html>
```

In this updated example:
- The `<input type="file">` element has the `multiple` attribute added, allowing the selection of multiple files.
- The `uploadFiles` function is triggered when the "Upload" button is clicked.
- Inside the function, all the selected files are accessed using `fileInput.files`.
- A `for` loop is used to iterate over each file and append it to the `FormData` object using the same key 'files'.
- The rest of the code remains the same, including the AJAX request creation and sending the `FormData` object as the request payload.

With this modification, you can select multiple files using the file input and upload them to the server simultaneously. Adjust the URL in the `xhr.open` method to match the URL of your servlet.

#### Backend

```java
import java.io.File;
import java.io.IOException;
import java.util.List;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Part;

@WebServlet("/upload")
public class FileUploadServlet extends HttpServlet {
    private static final String UPLOAD_DIRECTORY = "/path/to/upload/directory";
    private static final long MAX_FILE_SIZE = 1024 * 1024 * 10; // 10MB
    private static final String[] ALLOWED_EXTENSIONS = { "jpg", "png", "gif" };

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        File uploadDir = new File(UPLOAD_DIRECTORY);
        if (!uploadDir.exists()) {
            uploadDir.mkdirs();
        }

        try {
            List<Part> fileParts = (List<Part>) request.getParts();
            for (Part part : fileParts) {
                String fileName = getFileName(part);
                long fileSize = part.getSize();
                String fileExtension = getFileExtension(fileName);

                if (fileSize <= MAX_FILE_SIZE && isAllowedExtension(fileExtension)) {
                    part.write(uploadDir + File.separator + fileName);
                    System.out.println("File uploaded: " + fileName);
                } else {
                    System.out.println("Invalid file: " + fileName);
                }
            }
        } catch (Exception ex) {
            System.out.println("Error uploading files: " + ex.getMessage());
            ex.printStackTrace();
        }
    }

    private String getFileName(Part part) {
        String contentDisposition = part.getHeader("content-disposition");
        String[] elements = contentDisposition.split(";");
        for (String element : elements) {
            if (element.trim().startsWith("filename")) {
                return element.substring(element.indexOf('=') + 1).trim().replace("\"", "");
            }
        }
        return null;
    }

    private String getFileExtension(String fileName) {
        int dotIndex = fileName.lastIndexOf('.');
        if (dotIndex > 0 && dotIndex < fileName.length() - 1) {
            return fileName.substring(dotIndex + 1).toLowerCase();
        }
        return null;
    }

    private boolean isAllowedExtension(String fileExtension) {
        for (String allowedExtension : ALLOWED_EXTENSIONS) {
            if (allowedExtension.equals(fileExtension)) {
                return true;
            }
        }
        return false;
    }
}
```

In this example:
- The servlet is mapped to the `/upload` URL using the `@WebServlet` annotation.
- The `doPost` method handles the POST request sent by the frontend.
- The file uploads are obtained from the request using `request.getParts()`.
- For each file part, the filename, file size, and file extension are extracted.
- The file size is validated against the maximum allowed file size (`MAX_FILE_SIZE`).
- The file extension is validated against the allowed extensions (`ALLOWED_EXTENSIONS`).
- If the file passes the validations, it is written to the disk in the specified upload directory.
- The `getFileName` method extracts the filename from the part's `content-disposition` header.
- The `getFileExtension` method extracts the file extension from the filename.
- The `isAllowedExtension` method checks if the file extension is in the allowed extensions list.

Make sure to update the `UPLOAD_DIRECTORY` constant with the desired path to your upload directory.
