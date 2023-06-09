# Google Gson

Google Gson is a Java library developed by Google that allows for the serialization and deserialization of Java objects to and from JSON (JavaScript Object Notation) format. JSON is a popular data interchange format that is widely used for communication between different systems.

Gson provides a simple and flexible API for converting Java objects to JSON and vice versa. It can handle complex object graphs with nested objects and arrays, and supports various data types such as strings, numbers, booleans, and null values. Gson also provides customization options for handling specific serialization and deserialization requirements.

With Gson, you can easily convert Java objects to JSON strings for transmitting or storing data, and convert JSON strings back into Java objects for processing or manipulation within your Java applications. It simplifies the process of working with JSON data in Java by abstracting away the details of parsing and generating JSON.

Here's a basic example of using Gson to serialize a Java object to JSON:

```java
import com.google.gson.Gson;

public class MyClass {
    private int id;
    private String name;

    public MyClass(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public static void main(String[] args) {
        MyClass obj = new MyClass(1, "John Doe");
        Gson gson = new Gson();
        String json = gson.toJson(obj);
        System.out.println(json);
    }
}
```

The output of this code would be a JSON string representation of the `MyClass` object:

```json
{"id":1,"name":"John Doe"}
```

You can also deserialize a JSON string back into a Java object using Gson. Here's an example:

```java
import com.google.gson.Gson;

public class MyClass {
    private int id;
    private String name;

    public MyClass(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public static void main(String[] args) {
        String json = "{\"id\":1,\"name\":\"John Doe\"}";
        Gson gson = new Gson();
        MyClass obj = gson.fromJson(json, MyClass.class);
        System.out.println(obj.getId()); // Output: 1
        System.out.println(obj.getName()); // Output: John Doe
    }

    // Getters and setters omitted for brevity
}
```

In this example, the JSON string is deserialized into a `MyClass` object, allowing you to access its properties and perform operations on the data.

Overall, Gson is a powerful and widely used library for working with JSON data in Java, providing a convenient way to convert between Java objects and JSON representations.

## Selective serialization / deserialization

To configure Google Gson to selectively serialize or deserialize a field while excluding it from the opposite operation, you can combine the `@Expose` and `@SerializedName` annotations along with custom serialization and deserialization strategies. This approach allows you to have fine-grained control over field serialization and deserialization.

Here's an example demonstrating how to configure Gson to achieve this:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
import com.google.gson.JsonElement;
import com.google.gson.JsonSerializationContext;
import com.google.gson.JsonSerializer;
import com.google.gson.JsonDeserializationContext;
import com.google.gson.JsonDeserializer;
import java.lang.reflect.Type;

public class MyClass {
    @Expose(serialize = true, deserialize = false)
    private int id;

    @Expose(serialize = true, deserialize = true)
    @SerializedName("fullName")
    private String name;

    private transient String password; // Excluded from serialization and deserialization

    public MyClass(int id, String name, String password) {
        this.id = id;
        this.name = name;
        this.password = password;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getPassword() {
        return password;
    }

    public static void main(String[] args) {
        MyClass obj = new MyClass(1, "John Doe", "pass123");

        Gson gson = new GsonBuilder()
                .excludeFieldsWithoutExposeAnnotation()
                .registerTypeAdapter(MyClass.class, new MyClassDeserializer())
                .registerTypeAdapter(MyClass.class, new MyClassSerializer())
                .create();

        // Serialization
        String json = gson.toJson(obj);
        System.out.println(json);

        // Deserialization
        String jsonInput = "{\"id\": 1, \"fullName\": \"John Doe\", \"password\": \"pass123\"}";
        MyClass deserializedObj = gson.fromJson(jsonInput, MyClass.class);
        System.out.println(deserializedObj.getId()); // Output: 1
        System.out.println(deserializedObj.getName()); // Output: John Doe
        System.out.println(deserializedObj.getPassword()); // Output: null
    }
}

class MyClassSerializer implements JsonSerializer<MyClass> {
    @Override
    public JsonElement serialize(MyClass src, Type typeOfSrc, JsonSerializationContext context) {
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("id", src.getId());
        jsonObject.addProperty("fullName", src.getName());
        return jsonObject;
    }
}

class MyClassDeserializer implements JsonDeserializer<MyClass> {
    @Override
    public MyClass deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) {
        JsonObject jsonObject = json.getAsJsonObject();
        int id = jsonObject.get("id").getAsInt();
        String fullName = jsonObject.get("fullName").getAsString();
        return new MyClass(id, fullName, null);
    }
}
```

In this example, the `@Expose` annotation is used on the `id` and `name` fields to specify their serialization and deserialization behavior. By setting `serialize = true` and `deserialize = false` for the `id` field, Gson will only serialize the field and exclude it during deserialization. Similarly, by setting `serialize = true` and `deserialize = true` for the `name` field, Gson will serialize and deserialize it.

The `@SerializedName` annotation is used to provide a custom name ("fullName") for the serialized `name` field.

Additionally, custom serialization and deserialization strategies are implemented using the `JsonSerializer` and `JsonDeserializer` interfaces to handle the serialization and deserialization of the `MyClass` object. These strategies allow you to customize the serialization and

 deserialization process according to your specific requirements.

Remember to configure Gson using `GsonBuilder` and register the custom serializer and deserializer before creating the Gson instance.

## Custom serialization

To configure Google Gson to serialize or deserialize a field, you can use annotations and custom serialization/deserialization strategies provided by Gson. Here's an example of how to achieve this:

1. Create a custom serialization and deserialization strategy: Implement a custom `JsonSerializer` and `JsonDeserializer` for the field you want to handle differently during serialization and deserialization.

```java
import com.google.gson.*;

import java.lang.reflect.Type;

public class CustomFieldAdapter implements JsonSerializer<String>, JsonDeserializer<String> {
    @Override
    public JsonElement serialize(String value, Type typeOfSrc, JsonSerializationContext context) {
        // Logic for custom serialization
        if (value != null && value.startsWith("custom:")) {
            return new JsonPrimitive(value.substring(7));
        }
        return new JsonPrimitive(value);
    }

    @Override
    public String deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) throws JsonParseException {
        // Logic for custom deserialization
        String value = json.getAsString();
        if (value != null && !value.isEmpty()) {
            return "custom:" + value;
        }
        return value;
    }
}
```

In this example, the custom serialization strategy removes the "custom:" prefix from the field value if it exists. The custom deserialization strategy adds the "custom:" prefix to the field value during deserialization.

2. Configure Gson with the custom adapter: Create a Gson instance and register the custom adapter for the field using `GsonBuilder`.

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class MyClass {
    private String customField;

    public MyClass(String customField) {
        this.customField = customField;
    }

    public static void main(String[] args) {
        MyClass obj = new MyClass("custom:Value");

        Gson gson = new GsonBuilder()
                .registerTypeAdapter(String.class, new CustomFieldAdapter())
                .create();

        // Serialization
        String json = gson.toJson(obj);
        System.out.println(json); // Output: {"customField":"Value"}

        // Deserialization
        String jsonInput = "{\"customField\":\"Value\"}";
        MyClass deserializedObj = gson.fromJson(jsonInput, MyClass.class);
        System.out.println(deserializedObj.getCustomField()); // Output: custom:Value
    }

    public String getCustomField() {
        return customField;
    }
}
```

In this example, the `CustomFieldAdapter` is registered with Gson using `registerTypeAdapter()` on the `GsonBuilder` instance. The custom adapter is associated with the `String` type, so any fields of type `String` will be handled by this adapter during serialization and deserialization.

During serialization, the custom adapter removes the "custom:" prefix from the `customField` value. During deserialization, the custom adapter adds the "custom:" prefix to the `customField` value.

By using a custom adapter, you can selectively apply serialization and deserialization logic to specific fields, allowing you to handle them differently as per your requirements.

### Customization with Dates example

To customize Google Gson for serializing and deserializing a date field, you can use Gson's `JsonSerializer` and `JsonDeserializer` interfaces to define your custom logic for date conversion. Here's an example:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonElement;
import com.google.gson.JsonPrimitive;
import com.google.gson.JsonSerializationContext;
import com.google.gson.JsonDeserializer;
import com.google.gson.JsonDeserializationContext;
import java.lang.reflect.Type;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class MyClass {
    private Date date;

    public MyClass(Date date) {
        this.date = date;
    }

    public Date getDate() {
        return date;
    }

    public static void main(String[] args) {
        Gson gson = new GsonBuilder()
                .registerTypeAdapter(Date.class, new DateSerializer())
                .registerTypeAdapter(Date.class, new DateDeserializer())
                .create();

        // Serialization
        Date currentDate = new Date();
        MyClass obj = new MyClass(currentDate);
        String json = gson.toJson(obj);
        System.out.println(json);

        // Deserialization
        String jsonInput = "{\"date\":\"2023-06-09\"}";
        MyClass deserializedObj = gson.fromJson(jsonInput, MyClass.class);
        System.out.println(deserializedObj.getDate()); // Output: Fri Jun 09 00:00:00 UTC 2023
    }
}

class DateSerializer implements JsonSerializer<Date> {
    private static final DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

    @Override
    public JsonElement serialize(Date src, Type typeOfSrc, JsonSerializationContext context) {
        String formattedDate = dateFormat.format(src);
        return new JsonPrimitive(formattedDate);
    }
}

class DateDeserializer implements JsonDeserializer<Date> {
    private static final DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

    @Override
    public Date deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) {
        String dateString = json.getAsString();
        try {
            return dateFormat.parse(dateString);
        } catch (ParseException e) {
            // Handle parsing exception
            e.printStackTrace();
        }
        return null;
    }
}
```

In this example, the `MyClass` contains a `date` field of type `java.util.Date`. We define two custom classes, `DateSerializer` and `DateDeserializer`, to handle the serialization and deserialization of the `Date` field, respectively.

The `DateSerializer` class implements the `JsonSerializer<Date>` interface and overrides the `serialize` method. It formats the `Date` object using a `DateFormat` and returns a `JsonPrimitive` containing the formatted date string.

The `DateDeserializer` class implements the `JsonDeserializer<Date>` interface and overrides the `deserialize` method. It retrieves the date string from the JSON input, parses it using the same `DateFormat`, and returns the resulting `Date` object.

In the `main` method, we create a `Gson` instance using `GsonBuilder` and register the custom serializer and deserializer for the `Date` type. We then perform serialization and deserialization operations on a `MyClass` object containing a `Date` field.

By customizing the serializer and deserializer for the `Date` field, you can control the format and behavior of date serialization and deserialization in Gson according to your specific requirements.
