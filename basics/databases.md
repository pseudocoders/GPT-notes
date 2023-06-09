# Databases

A database is an organized collection of structured data that is stored and managed using a database management system (DBMS). It is designed to efficiently store, retrieve, update, and manage large volumes of data, providing a structured and controlled environment for data management.

Common types of databases include relational databases (RDBMS), where data is organized in tables with predefined relationships, and non-relational databases (NoSQL), which provide flexible data models suitable for unstructured or semi-structured data.

Overall, databases serve as a critical component in modern software systems, enabling efficient data storage, retrieval, and management for a wide range of applications and industries.

Using a database offers several advantages over traditional file-based systems or other data storage approaches. Here are some key advantages of using a database:

1. Data Centralization: Databases allow for centralized storage of data, eliminating the need for redundant copies of data scattered across multiple files or applications. This centralization ensures data consistency and reduces data duplication, making it easier to manage and update data.

2. Data Integrity and Consistency: Databases enforce data integrity by applying constraints and rules to maintain the accuracy and validity of the data. These constraints, such as primary keys, unique keys, and foreign keys, prevent inconsistent or invalid data from being stored. This ensures the reliability and quality of the data.

3. Data Security: Databases provide built-in security mechanisms to control access to data. User authentication, authorization, and access control features ensure that only authorized users can view, modify, or manipulate the data. Additionally, databases often offer encryption capabilities to protect sensitive data from unauthorized access.

4. Data Sharing and Collaboration: Databases enable multiple users or applications to access and share the same data concurrently. This facilitates collaboration and improves data integration across different departments or systems within an organization. It ensures that users can access the most up-to-date and consistent information.

5. Data Consistency and Data Redundancy: Databases support transaction management, which ensures that a set of database operations is either completed successfully or rolled back as a single unit. This maintains data consistency and prevents data corruption or partial updates. Additionally, databases allow for controlled redundancy, where data can be replicated or distributed across multiple servers or locations for improved availability and fault tolerance.

6. Data Scalability: Databases are designed to handle large volumes of data and can scale horizontally (adding more servers) or vertically (increasing resources of existing servers) to accommodate growing data needs. This scalability allows databases to handle increased data volumes and user demands without sacrificing performance.

7. Data Recovery and Backup: Databases provide mechanisms for data backup and recovery to protect against data loss or system failures. Regular backups ensure that data can be restored to a consistent state in the event of accidental deletion, hardware failures, or other disasters.

8. Efficient Data Retrieval and Querying: Databases offer query languages like SQL (Structured Query Language) that provide powerful and efficient ways to retrieve, filter, and manipulate data. Indexing and optimization techniques in databases help speed up data retrieval, even for large datasets.

9. Data Analysis and Reporting: Databases support advanced querying and analytical capabilities, allowing users to perform complex data analysis, generate reports, and gain insights from the data. Features like aggregate functions, grouping, and joins enable users to extract meaningful information from the data.

By leveraging these advantages, databases provide a reliable, secure, and efficient solution for data storage, management, and analysis, making them indispensable for a wide range of applications and industries.

## RDNMS

An RDBMS, or Relational Database Management System, is a software system designed to manage and organize structured data in a relational database. It provides a systematic way of storing, retrieving, managing, and manipulating data, utilizing the principles of the relational model.

The relational model organizes data into tables, which consist of rows and columns. Each table represents a specific entity or concept, such as customers, orders, or products. The columns, also known as attributes or fields, define the characteristics of the data, while the rows represent individual instances or records.

The RDBMS ensures data integrity and enforces relationships between tables through the use of constraints and rules. It supports ACID properties (Atomicity, Consistency, Isolation, Durability) to maintain data consistency and transactional integrity.

Key features of an RDBMS include:

1. Data Definition Language (DDL): It provides commands to define the database schema, such as creating tables, specifying columns, setting constraints, and establishing relationships between tables.

2. Data Manipulation Language (DML): It allows users to retrieve, insert, update, and delete data from the database. The commonly used DML commands are SELECT, INSERT, UPDATE, and DELETE.

3. Query Optimization: RDBMS systems have query optimizers that analyze SQL queries and determine the most efficient execution plan to retrieve the requested data.

4. Data Integrity and Constraints: RDBMS enforces data integrity rules through constraints such as primary keys, unique keys, foreign keys, and check constraints. These constraints ensure the consistency and validity of the data stored in the database.

5. Transaction Management: RDBMS systems provide transactional capabilities, allowing multiple database operations to be grouped together as a single unit of work. Transactions ensure that either all operations are completed successfully or none of them are applied, maintaining data consistency.

6. Security and Access Control: RDBMS systems offer mechanisms to control access to the database and its objects. Users can be granted specific privileges to perform operations like querying, modifying, or managing the database.

Some popular examples of RDBMSs include Oracle Database, MySQL, PostgreSQL, Microsoft SQL Server, and SQLite. These systems are widely used in various applications, ranging from small-scale personal projects to large enterprise systems, due to their reliability, scalability, and well-established query languages like SQL (Structured Query Language).

### Database design with ER model

Database design using the Entity-Relationship (ER) model involves creating a conceptual representation of the database structure. The ER model helps capture the relationships between different entities and defines the attributes associated with each entity. Here's an overview of the process:

1. Identify Entities: Identify the main entities or objects of interest within the domain you are modeling. For example, in a university database, entities could include students, courses, professors, and departments.

2. Define Attributes: For each entity, determine the attributes that describe the characteristics or properties of that entity. Attributes are represented as columns in the database table. For example, for the "student" entity, attributes could be student ID, name, date of birth, and contact information.

3. Establish Relationships: Identify the relationships between entities. Relationships represent how entities are related or connected to each other. There are three types of relationships: one-to-one, one-to-many, and many-to-many. For example, a student can enroll in multiple courses, creating a one-to-many relationship between the "student" and "course" entities.

4. Cardinality and Modality: Specify the cardinality and modality of the relationships. Cardinality defines the number of instances that can be associated with each entity in the relationship (e.g., one or many), while modality defines whether the relationship is mandatory or optional for each entity. For example, a course may have many students (one-to-many) and it may be mandatory for a student to be enrolled in at least one course.

5. Primary Keys: Identify the primary key for each entity. The primary key is a unique identifier for each record in the entity. It can be a single attribute or a combination of attributes that uniquely identify a record.

6. Entity-Relationship Diagram (ERD): Represent the entities, attributes, relationships, and primary keys in an Entity-Relationship Diagram. An ERD is a graphical representation that uses boxes (entities), lines (relationships), and attributes to illustrate the database structure. It provides a visual overview of the database design.

7. Refine and Normalize: Refine the design by analyzing the entities, attributes, and relationships for potential redundancies or anomalies. Apply normalization techniques to ensure data integrity and eliminate data duplication. Normalization involves breaking down tables into smaller, more efficient structures by applying specific rules to eliminate data redundancy and dependency.

8. Implement the Database: Convert the ER model into a physical database schema using a database management system (DBMS). Map the entities, attributes, and relationships into database tables, columns, and foreign keys. Create the necessary indexes, constraints, and relationships based on the defined cardinality and modality.

The ER model serves as a blueprint for designing a database that accurately represents the real-world domain. It helps ensure data integrity, optimize data storage, and facilitate efficient querying and data manipulation.
#### Example

Let's consider an example of designing a database for a university system using the Entity-Relationship (ER) model, focusing on a one-to-many relationship.

Entities:
1. Department: Attributes may include DepartmentID (primary key), Name, Location, and Chairperson.
2. Course: Attributes may include CourseID (primary key), Name, Credits, and DepartmentID (foreign key).
3. Student: Attributes may include StudentID (primary key), Name, Major, and DepartmentID (foreign key).

Relationship:
1. Department-Course: Represents the relationship between a department and its courses. It is a one-to-many relationship, as one department can offer multiple courses, but each course is associated with only one department.

Cardinality and Modality:
- For the Department-Course relationship, the cardinality is one-to-many (one department can offer many courses, but each course is associated with only one department). The modality is mandatory on the Department side (a department must offer courses), and optional on the Course side (a course may or may not have a department associated with it).

Entity-Relationship Diagram (ERD):
Here is a simplified representation of the ER model for the university system:

```
+------------+         +---------+
| Department |         | Course  |
+------------+         +---------+
| DepartmentID|◄--------| CourseID|
| Name       |         | Name    |
| Location   |         | Credits |
| Chairperson|         +---------+
+------------+
      ▲
      |
      |     +---------+
      +-----| Student |
            +---------+
            | StudentID|
            | Name     |
            | Major    |
            +---------+
```

In this example, the Department entity has a one-to-many relationship with the Course entity through the DepartmentID attribute. This relationship indicates that a department can offer multiple courses, but each course is associated with only one department.

The Student entity is not directly related to the Department entity in a one-to-many relationship. However, it can have a foreign key attribute, DepartmentID, which indicates the student's major department.

This ER model provides a foundation for designing a database that captures the structure of a university system, allowing for efficient management of departments, courses, and student information.

#### Example

Let's consider an example of designing a database for a library management system using the Entity-Relationship (ER) model.

Entities:
1. Book: Attributes may include BookID (primary key), Title, Author, Publisher, Publication Year, and ISBN.
2. Member: Attributes may include MemberID (primary key), Name, Address, Email, and Phone Number.
3. Borrowing: Attributes may include BorrowingID (primary key), BookID (foreign key), MemberID (foreign key), Borrow Date, and Return Date.

Relationships:
1. Book-Borrowing: Represents the relationship between a book and a borrowing. It is a one-to-many relationship, as one book can be borrowed by multiple members, but each borrowing is associated with only one book. This relationship helps track which books are borrowed by which members.

2. Member-Borrowing: Represents the relationship between a member and a borrowing. It is also a one-to-many relationship, as one member can borrow multiple books, but each borrowing is associated with only one member. This relationship helps track which members have borrowed which books.

Cardinality and Modality:
- For the Book-Borrowing relationship, the cardinality is one-to-many (one book can be borrowed by many members, but each borrowing is associated with only one book). The modality is mandatory on the Book side (a book must be borrowed), and optional on the Borrowing side (a borrowing may or may not have a book associated with it).
- For the Member-Borrowing relationship, the cardinality is one-to-many (one member can have multiple borrowings, but each borrowing is associated with only one member). The modality is mandatory on the Member side (a member must have a borrowing), and optional on the Borrowing side (a borrowing may or may not have a member associated with it).

Entity-Relationship Diagram (ERD):
Here is a simplified representation of the ER model for the library management system:

```
+-----------+          +-------------+          +------------+
|   Book    |          |   Borrowing |          |   Member   |
+-----------+          +-------------+          +------------+
| BookID    |◄---------| BorrowingID |          | MemberID   |
| Title     |          | BookID      |----------| Name       |
| Author    |◄---------| MemberID    |◄---------| Address    |
| Publisher |          | BorrowDate  |          | Email      |
| Pub_Year  |          | ReturnDate  |          | Phone      |
| ISBN      |          +-------------+          +------------+
+-----------+
```

In this example, the Book entity has a one-to-many relationship with the Borrowing entity through the BookID attribute. Similarly, the Member entity has a one-to-many relationship with the Borrowing entity through the MemberID attribute.

This ER model captures the structure of the database for a library management system, allowing for efficient storage and management of books, members, and their borrowings.



### Normalization

#### 1NF

First Normal Form (1NF) is the fundamental rule in database normalization. It establishes the basic requirements for eliminating data redundancy and organizing data in a tabular format. The goal of 1NF is to ensure that each column in a table contains only atomic values (indivisible data) and that there are no repeating groups of data.

To comply with 1NF, the following conditions must be met:

1. Atomic Values: Each attribute (column) in a table should hold atomic values, meaning that it cannot be further divided into smaller data elements. This eliminates the storage of multiple values within a single attribute. For example, if you have a "Phone Numbers" attribute, it should not contain multiple phone numbers separated by commas or semicolons. Instead, each phone number should be stored in a separate attribute or a separate row.

2. Unique Column Names: Each column in a table must have a unique name, ensuring that each attribute represents a distinct piece of information. This helps prevent confusion and ambiguity when querying or manipulating the data.

3. Rows as Uniquely Identifiable: Each row in a table should be uniquely identifiable. This is typically achieved by including a primary key column in the table, which uniquely identifies each record. The primary key ensures that there are no duplicate rows in the table and provides a means to establish relationships between tables.

By adhering to 1NF, a table is structured in a way that allows for efficient storage, retrieval, and manipulation of data. It reduces data redundancy and improves data integrity by avoiding the insertion, deletion, or update anomalies that can occur when duplicate or multi-valued data is present.

It's important to note that 1NF is the foundational level of database normalization, and subsequent normalization forms build upon its principles to achieve higher levels of data organization and integrity.

As a 1NF example, let's consider an example of a table that violates the First Normal Form (1NF), and then we'll normalize it to comply with 1NF.

Suppose we have a table called "Employees" with the following structure:

| EmployeeID | Name       | Department          | Skills             |
|------------|------------|---------------------|-------------------|
| 1          | John Smith | Sales, Marketing    | Sales, Marketing   |
| 2          | Jane Doe   | Finance, HR         | Finance, HR        |
| 3          | Bob Johnson| Operations, Finance | Operations, Finance|

In this example, the "Department" and "Skills" columns contain multiple values separated by commas. This violates 1NF because the values in these columns are not atomic, and it leads to data redundancy.

To bring the table into 1NF, we need to eliminate the repeating groups and ensure atomicity. We can achieve this by splitting the table into two separate tables:

Table: Employees
| EmployeeID | Name       |
|------------|------------|
| 1          | John Smith |
| 2          | Jane Doe   |
| 3          | Bob Johnson|

Table: EmployeeDepartments
| EmployeeID | Department |
|------------|------------|
| 1          | Sales      |
| 1          | Marketing  |
| 2          | Finance    |
| 2          | HR         |
| 3          | Operations |
| 3          | Finance    |

Table: EmployeeSkills
| EmployeeID | Skill      |
|------------|------------|
| 1          | Sales      |
| 1          | Marketing  |
| 2          | Finance    |
| 2          | HR         |
| 3          | Operations |
| 3          | Finance    |

In the normalized form, the "Employees" table contains only the EmployeeID and Name columns, with each row representing a unique employee. The "EmployeeDepartments" table stores the EmployeeID and Department values, where each row represents a department associated with an employee. Similarly, the "EmployeeSkills" table stores the EmployeeID and Skill values, with each row representing a skill associated with an employee.

By separating the repeating values into separate tables, we have eliminated the redundancy and achieved atomicity. Each column now contains atomic values, and the relationships between employees, departments, and skills are established through the use of shared EmployeeID values.

This normalized structure adheres to the First Normal Form (1NF) and allows for efficient storage, retrieval, and manipulation of data without redundancy or multi-valued attributes.

#### 2NF

Second Normal Form (2NF) is a database normalization rule that builds upon the principles of the First Normal Form (1NF). It addresses the issue of partial dependencies in a table by ensuring that non-key attributes are dependent on the entire primary key rather than just a part of it.

To comply with 2NF, the following conditions must be met:

1. Meet the requirements of 1NF: The table must already conform to the rules of the First Normal Form (1NF), which includes having atomic values and unique column names.

2. Have a Primary Key: The table must have a primary key defined, which uniquely identifies each record in the table.

3. Remove Partial Dependencies: Every non-key attribute in the table must be fully functionally dependent on the entire primary key, not on only a subset of the key.

To illustrate the concept of 2NF, let's consider an example of a table that violates 2NF:

Table: Orders
| OrderID | CustomerID | CustomerName | ProductID | ProductName | Quantity |
|---------|------------|--------------|-----------|-------------|----------|
| 1       | 1001       | John Smith   | 1         | Widget A    | 5        |
| 2       | 1002       | Jane Doe     | 2         | Widget B    | 3        |
| 3       | 1001       | John Smith   | 3         | Widget C    | 2        |

In this example, the table has a composite primary key consisting of both OrderID and ProductID. However, the non-key attributes CustomerName and Quantity are not fully dependent on the entire primary key. CustomerName is functionally dependent only on CustomerID, while Quantity is functionally dependent only on ProductID.

To bring the table into 2NF, we need to separate the data into two separate tables to eliminate the partial dependencies:

Table: Customers
| CustomerID | CustomerName |
|------------|--------------|
| 1001       | John Smith   |
| 1002       | Jane Doe     |

Table: Products
| ProductID | ProductName |
|-----------|-------------|
| 1         | Widget A    |
| 2         | Widget B    |
| 3         | Widget C    |

Table: Orders
| OrderID | CustomerID | ProductID | Quantity |
|---------|------------|-----------|----------|
| 1       | 1001       | 1         | 5        |
| 2       | 1002       | 2         | 3        |
| 3       | 1001       | 3         | 2        |

In this normalized form, the Customers table contains unique CustomerID and CustomerName values. The Products table contains unique ProductID and ProductName values. The Orders table now includes the OrderID, CustomerID, ProductID, and Quantity columns, with each row representing a unique order.

By separating the data into multiple tables, we have eliminated the partial dependencies. CustomerName is now dependent on the CustomerID, and Quantity is dependent on the ProductID. Each non-key attribute is fully functionally dependent on the entire primary key.

This normalized structure adheres to the Second Normal Form (2NF), ensuring data integrity and avoiding data redundancy.

#### 3NF

Third Normal Form (3NF) is a database normalization rule that helps eliminate data redundancy and ensure data integrity by addressing transitive dependencies within a table. It builds upon the principles of the First Normal Form (1NF) and Second Normal Form (2NF).

To comply with 3NF, the following conditions must be met:

1. Meet the requirements of 2NF: The table must already conform to the rules of the Second Normal Form (2NF), which includes meeting the requirements of 1NF and having a primary key defined.

2. Eliminate Transitive Dependencies: Every non-key attribute in the table must be directly dependent on the primary key and not on other non-key attributes.

Transitive dependency refers to a relationship where a non-key attribute is functionally dependent on another non-key attribute, rather than directly on the primary key. By eliminating transitive dependencies, we ensure that the data in the table is organized efficiently and does not contain redundant information.

To illustrate the concept of 3NF, let's consider an example of a table that violates 3NF:

Table: Employees
| EmployeeID | EmployeeName | Department   | DepartmentLocation | Manager       |
|------------|--------------|--------------|--------------------|---------------|
| 1          | John Smith   | Sales        | Building A         | Sarah Johnson |
| 2          | Jane Doe     | Finance      | Building B         | John Smith    |
| 3          | Bob Johnson  | Operations   | Building A         | Jane Doe      |

In this example, the table has a primary key of EmployeeID. However, there are transitive dependencies present. The DepartmentLocation attribute depends on the Department attribute, and the Manager attribute depends on the EmployeeName attribute.

To bring the table into 3NF, we need to separate the data into multiple tables to eliminate the transitive dependencies:

Table: Employees
| EmployeeID | EmployeeName |
|------------|--------------|
| 1          | John Smith   |
| 2          | Jane Doe     |
| 3          | Bob Johnson  |

Table: Departments
| Department   | DepartmentLocation |
|--------------|--------------------|
| Sales        | Building A         |
| Finance      | Building B         |
| Operations   | Building A         |

Table: EmployeeDepartments
| EmployeeID | Department   |
|------------|--------------|
| 1          | Sales        |
| 2          | Finance      |
| 3          | Operations   |

Table: Managers
| EmployeeID | Manager       |
|------------|---------------|
| 1          | Sarah Johnson |
| 2          | John Smith    |
| 3          | Jane Doe      |

In this normalized form, the Employees table contains the EmployeeID and EmployeeName columns, with each row representing a unique employee. The Departments table includes the Department and DepartmentLocation columns, storing unique department information. The EmployeeDepartments table establishes the relationship between employees and departments. The Managers table associates each employee with their respective manager.

By separating the data into multiple tables, we have eliminated the transitive dependencies. Each non-key attribute is directly dependent on the primary key, and data redundancy is minimized. This structure adheres to the Third Normal Form (3NF) and ensures efficient data organization and integrity.

### DDL & DML

DDL and DML are two types of SQL (Structured Query Language) statements used to manipulate and manage databases. 

1. DDL - Data Definition Language:
DDL stands for Data Definition Language. It consists of SQL statements used to define and manage the structure of the database objects, such as tables, views, indexes, and constraints. DDL statements include:

- CREATE: Used to create database objects such as tables, views, indexes, or constraints.
- ALTER: Used to modify the structure of existing database objects.
- DROP: Used to delete or remove database objects.
- TRUNCATE: Used to remove all data from a table, while keeping the structure intact.
- RENAME: Used to rename database objects.

DDL statements are typically executed by database administrators or developers responsible for database design and schema management.

2. DML - Data Manipulation Language:
DML stands for Data Manipulation Language. It includes SQL statements used to manipulate and retrieve data within the database objects. DML statements include:

- SELECT: Used to retrieve data from one or more tables.
- INSERT: Used to insert new records or data into a table.
- UPDATE: Used to modify existing data in a table.
- DELETE: Used to delete records from a table.
- MERGE: Used to perform an "upsert" operation, combining INSERT and UPDATE functionality.

DML statements are primarily used by application developers and end-users to perform operations on the data stored in the database.

In summary, DDL is concerned with defining and managing the structure of the database objects, while DML is focused on manipulating and retrieving the data within those objects. Both DDL and DML are essential components of SQL and work together to facilitate efficient database management and data manipulation.

## Transactions

A transaction is a logical unit of work that consists of one or more database operations. It represents a sequence of actions that need to be executed as a single, indivisible unit. The purpose of a transaction is to ensure data integrity and consistency within a database.

Transactions are used to group related database operations together and treat them as a single entity. The concept of a transaction is based on the ACID properties, which stand for Atomicity, Consistency, Isolation, and Durability:

1. Atomicity: Atomicity ensures that a transaction is treated as an all-or-nothing operation. Either all the database operations within the transaction are successfully completed, or none of them are applied. If any part of the transaction fails, the entire transaction is rolled back, and the database is left unchanged.

2. Consistency: Consistency ensures that a transaction brings the database from one valid state to another. The database must satisfy all defined integrity constraints and business rules both before and after the transaction. If a transaction violates any integrity constraints, it is rolled back to maintain consistency.

3. Isolation: Isolation ensures that concurrent transactions do not interfere with each other. Each transaction operates independently and is not affected by the actions of other concurrent transactions. Isolation prevents issues such as dirty reads, non-repeatable reads, and phantom reads.

4. Durability: Durability ensures that once a transaction is committed and the changes are made to the database, they persist even in the event of system failures or power outages. The changes become permanent and are not lost.

A transaction typically follows a sequence of steps:

1. Begin Transaction: The transaction is initiated.

2. Perform Database Operations: The desired database operations, such as inserts, updates, or deletes, are executed.

3. Transaction Validation: The database checks if the transaction satisfies all integrity constraints and business rules.

4. Commit: If the transaction passes the validation and all operations are successful, the changes made by the transaction are permanently saved in the database.

5. Rollback: If any part of the transaction fails or violates constraints, the changes made by the transaction are undone, and the database is rolled back to its state before the transaction started.

Transactions provide reliability, data integrity, and consistency in multi-user database environments. They ensure that data modifications are handled in a controlled and reliable manner, allowing concurrent access to the database while preserving its integrity.
