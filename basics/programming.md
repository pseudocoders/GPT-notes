# Structured Programming

## Spaghetti code

Unstructured programming, also known as "spaghetti code," refers to a programming style that lacks clear organization and control flow structures. It emerged prior to the development of structured programming and is characterized by its unrestricted use of control flow statements, such as "goto" statements, and the absence of modularization.

In unstructured programming, the control flow of the program can jump to arbitrary points within the code using "goto" statements. This often leads to code that is difficult to read, understand, and maintain. The lack of structured control flow can make it challenging to follow the execution path of the program, making it prone to bugs and errors.

Unstructured programming can result in the following issues:

1. Spaghetti code: The program's control flow becomes tangled and convoluted, resembling a plate of spaghetti. The execution jumps between various code sections in a non-linear and unpredictable manner, making it hard to discern the program's logic.

2. Code duplication: Without proper modularization, code segments are often duplicated in multiple places throughout the program. This redundancy makes maintenance and updates more difficult, as changes must be made in multiple locations.

3. Difficulty in debugging: Unstructured programs are harder to debug due to the lack of clear control flow and scattered code. Identifying the root cause of errors becomes challenging as the program lacks a clear structure and hierarchy.

4. Poor readability: Unstructured code tends to be less readable and comprehensible. It lacks the visual cues provided by structured programming constructs, such as loops and conditionals, which aid in understanding the flow and purpose of the code.

Unstructured programming was prevalent in the early days of computer programming when the concept of structured programming had not yet been widely adopted. As software development matured, structured programming emerged as a response to the challenges posed by unstructured code.

Structured programming brought forth the principles of sequence, selection, iteration, and modularization to improve code organization, readability, and maintainability. It provided structured control flow constructs, such as loops and conditionals, as alternatives to unstructured "goto" statements.

Today, unstructured programming is generally discouraged due to its drawbacks. Most programming languages and development practices promote the use of structured programming principles and discourage the unrestricted use of "goto" statements. The focus is on writing code that is easier to understand, debug, and maintain, leading to more reliable and efficient software development.

## Example

Here's an example of spaghetti code with line numbers and "goto" statements in a hypothetical programming language to calculate the value of pi using the Buffon's needle experiment:

```
10 count_crossed = 0
20 count_total = 0
30 PRINT "Enter the number of iterations: "
40 INPUT iterations
50 i = 1
60 GOTO 100
70 GOTO 150
100 angle = RANDOM(0, 90)
110 x_distance = RANDOM(0, needle_length)
120 y_distance = SIN(angle) * needle_length / 2
130 IF y_distance > x_distance THEN GOTO 140 ELSE GOTO 170
140 count_crossed = count_crossed + 1
150 count_total = count_total + 1
160 IF i < iterations THEN i = i + 1 : GOTO 100 ELSE GOTO 180
170 count_total = count_total + 1
180 pi = (2 * needle_length * count_total) / (needle_gap * count_crossed)
190 PRINT "The approximate value of pi is: " + pi
200 END
```

In this example, line numbers are used to reference specific lines of code, and "goto" statements are used to control the flow of the program. The code iterates through the specified number of iterations, performs the Buffon's needle experiment, and counts the number of crossed needles.

However, this spaghetti code style can make the program flow difficult to follow and understand, especially as the codebase grows larger. It can lead to decreased readability, maintainability, and modularity.

Structured programming principles, such as breaking the code into functions or procedures, utilizing loops and conditionals instead of "goto" statements, and promoting modularization, can greatly enhance the code's organization and maintainability.

## Structured programming

Structured programming is a programming paradigm that emphasizes the use of structured control flow constructs to improve the clarity, readability, and maintainability of computer programs. It was developed in the late 1960s as a response to the perceived shortcomings of earlier programming styles, such as unstructured or spaghetti code.

The key principles of structured programming include:

1. Sequence: Programs are organized as a sequence of instructions executed in a linear order. Each instruction is executed one after the other, unless explicitly directed otherwise.

2. Selection: Structured programming introduces conditional statements, such as "if-else" or "switch-case," to selectively execute blocks of code based on certain conditions. These constructs allow the program to take different paths depending on the evaluation of logical expressions.

3. Iteration: Structured programming provides looping constructs, like "for" and "while" loops, to repeatedly execute a block of code until a specific condition is met or until a certain number of iterations have been performed. This allows for efficient repetition and looping within a program.

4. Modularization: Programs are divided into modular units, typically called functions or procedures, which encapsulate a specific set of tasks. Each function has a well-defined purpose and inputs/outputs, promoting code reuse and modular design.

By adhering to these principles, structured programming minimizes the use of unstructured control flow mechanisms like "goto" statements, which can make code difficult to understand and maintain. Instead, it promotes a more disciplined approach to programming, improving code readability, maintainability, and reducing bugs.

One of the most popular languages that heavily influenced the adoption of structured programming is C. The introduction of C and its structured constructs, such as loops, conditionals, and functions, allowed programmers to write clearer and more manageable code.

Structured programming has laid the foundation for subsequent programming paradigms, such as object-oriented programming (OOP) and procedural programming. OOP extends structured programming by introducing the concept of objects and classes, while procedural programming focuses on organizing code around procedures or functions.

Overall, structured programming provides a logical and systematic approach to software development, promoting code organization, clarity, and maintainability. It has become a fundamental concept in computer science and software engineering, shaping the way programs are designed and implemented.

## Program modularization

Program modularization refers to the process of breaking down a computer program into smaller, self-contained modules or units of code. Each module focuses on a specific task or functionality and can be developed, tested, and maintained independently. Modularization is a key principle in software engineering and promotes code reuse, readability, maintainability, and scalability.

Modularization offers several benefits:

1. Encapsulation: Each module encapsulates a set of related functions or procedures, hiding the internal details and providing a well-defined interface. This allows other parts of the program to interact with the module without needing to know its internal implementation, promoting information hiding and reducing dependencies.

2. Code Reusability: Modularization facilitates code reuse by creating self-contained modules that can be easily integrated into different programs or used by other modules. Instead of rewriting code from scratch, developers can leverage existing modules, saving time and effort.

3. Readability and Maintainability: Breaking a program into modular units enhances code readability. Each module focuses on a specific task, making it easier to understand and reason about the code. Furthermore, maintaining and updating the program becomes more manageable as changes can be localized to specific modules rather than affecting the entire codebase.

4. Collaboration: Modularization facilitates collaboration among developers working on different parts of a program. Modules can be assigned to different team members, enabling parallel development and reducing conflicts during integration.

To achieve modularization, various techniques and principles can be applied:

a. Functions and Procedures: Dividing a program into functions or procedures is a fundamental way to modularize code. Each function or procedure handles a specific task and can be called from other parts of the program when needed.

b. Libraries and Packages: Libraries or packages provide a collection of related modules that can be used across multiple programs. They offer a higher level of abstraction and allow for easier distribution and sharing of reusable code.

c. Object-Oriented Programming (OOP): OOP takes modularization a step further by organizing code around objects and classes. Objects encapsulate data and behavior, and classes define the blueprint for creating objects. Inheritance, polymorphism, and encapsulation are used to achieve modularization and code reuse in an OOP context.

d. Application Programming Interfaces (APIs): APIs define a set of rules and protocols that allow different software components or modules to interact with each other. By providing well-defined interfaces, APIs enable modular code development and integration.

e. Modular Design Patterns: Design patterns, such as the module pattern, provide standardized approaches for structuring and organizing code. These patterns help define relationships between modules and promote modularity.

In summary, program modularization involves breaking down a program into smaller, self-contained modules to promote code reuse, readability, maintainability, and collaboration. It is an essential practice in software engineering, enabling efficient development and maintenance of complex software systems.

## Structured design

Structured design is a systematic approach to designing software systems that emphasizes organization, modularity, and clarity. It is a top-down design methodology that aims to break down a complex system into smaller, more manageable modules or components.

The key principles of structured design include:

1. Modularity: The system is divided into separate modules or components, each responsible for a specific functionality or task. Modularity promotes code reusability, maintainability, and ease of understanding.

2. Abstraction: The design focuses on capturing the essential characteristics of each module while hiding unnecessary implementation details. Abstraction allows developers to focus on the high-level functionality of each module without getting lost in the implementation specifics.

3. Encapsulation: Modules are encapsulated, meaning that their internal details are hidden from other modules. This promotes information hiding, reduces dependencies, and allows for independent development and maintenance of different modules.

4. Cohesion: Modules are designed to have high cohesion, meaning that the elements within a module are closely related and work together to achieve a specific objective. High cohesion reduces complexity and improves the maintainability and reusability of modules.

5. Coupling: The level of coupling between modules is minimized to reduce dependencies and promote independence. Low coupling enables changes in one module to have minimal impact on other modules.

6. Hierarchical Structure: The design follows a hierarchical structure, with modules organized in a layered or hierarchical manner. The higher-level modules coordinate the activities of lower-level modules and provide a clear overall system architecture.

7. Structured Control Flow: The design utilizes structured control flow constructs, such as loops, conditionals, and structured branching, to ensure clear and predictable control flow within and between modules.

Structured design techniques often involve the use of graphical notations, such as structure charts, data flow diagrams, and entity-relationship diagrams, to visualize the relationships and interactions between modules.

By following structured design principles, developers can create software systems that are modular, maintainable, and extensible. The structured design approach helps in managing complexity, promoting code reusability, and enhancing overall software quality.

High cohesion and low coupling are desirable design qualities. High cohesion ensures that modules have a single, well-defined purpose, while low coupling reduces dependencies between modules, promoting modularity and maintainability. Striking the right balance between cohesion and coupling leads to more robust and flexible software systems.

### Cohesion 

Cohesion and coupling are two important concepts in software design that describe the relationships and interactions between modules or components within a system.

Cohesion:
Cohesion refers to the degree to which the elements within a module or component are related and work together to achieve a common objective. It measures the functional strength and unity within a module. High cohesion is desirable as it indicates that the elements within a module are closely related and focused on a single task or responsibility.

Types of Cohesion:
1. Functional Cohesion: All elements within a module contribute to performing a single, well-defined function or task.
2. Sequential Cohesion: The elements within a module are organized in a specific sequence, where the output of one element becomes the input for the next element.
3. Communicational Cohesion: The elements within a module operate on the same input or output data.
4. Procedural Cohesion: The elements within a module are grouped together based on a specific sequence of steps or procedure.
5. Temporal Cohesion: The elements within a module are activated or executed together at a specific time.
6. Logical Cohesion: The elements within a module are related based on a common logic or condition.
7. Coincidental Cohesion: The elements within a module have no meaningful relationship to each other.

Here are a few examples of modules with poor cohesion:

1. Mixed Responsibility:
A module that handles both user interface rendering and data manipulation violates cohesion principles. It would be better to separate the user interface logic and the data manipulation logic into separate modules with clear responsibilities.

2. Excessive Functionality:
A module that performs multiple unrelated functions or tasks exhibits low cohesion. For example, a module that handles file I/O, performs calculations, and sends network requests would benefit from being divided into separate modules, each responsible for a specific task.

3. Low-Level Operations:
A module that contains low-level implementation details, such as memory management or hardware interactions, alongside higher-level business logic, lacks cohesion. The low-level operations should be encapsulated in separate modules to isolate and abstract those concerns.

4. God Object:
A module that acts as a central hub and handles almost all aspects of the system, controlling numerous functionalities, violates cohesion principles. This monolithic module should be decomposed into smaller, more focused modules that handle specific responsibilities.

5. Feature Envy:
A module that frequently accesses or modifies data from another module, indicating a strong dependency, demonstrates poor cohesion. The data and operations related to that functionality should be consolidated into a cohesive module rather than scattered across different modules.

6. Shotgun Surgery:
A module that requires modifications in multiple places whenever a specific feature or functionality needs to be changed suffers from low cohesion. Ideally, each module should have a single, well-defined responsibility, reducing the need for widespread modifications.

These examples illustrate situations where the responsibilities within a module are not well-aligned, resulting in poor cohesion. By identifying and addressing these issues, software designers can improve the maintainability, reusability, and readability of their codebase.

High cohesion is desirable as it leads to more maintainable and reusable modules. Modules with high cohesion are easier to understand, modify, and test because they have a clear and focused purpose.

### Coupling

Coupling refers to the degree of interdependence or interaction between modules or components within a system. It measures the strength of the relationships between modules. Low coupling is desirable as it indicates that modules are loosely connected and have minimal dependencies on each other.

Types of Coupling:
1. Data Coupling: Modules communicate by passing data parameters.
2. Stamp Coupling: Modules communicate by passing a data structure or record.
3. Control Coupling: One module controls the execution of another module through parameters or flags.
4. Common Coupling: Modules share global data or variables.
5. Content Coupling: One module directly accesses or modifies the internal data of another module.
6. External Coupling: Modules communicate through shared external storage or files.
7. Message Coupling: Modules communicate by sending messages or using a messaging system.

Low coupling is beneficial as it improves the maintainability and flexibility of a system. Modules with low coupling can be modified or replaced independently without affecting other modules. It promotes modular design, code reuse, and ease of testing.

Here are a few examples of modules with poor coupling:

1. Tight Coupling:
Tight coupling occurs when modules have strong dependencies on each other, making it difficult to modify or replace one module without affecting others. For example, if Module A directly accesses and manipulates the internal data structures of Module B, they are tightly coupled.

2. Circular Dependency:
Circular dependency happens when two or more modules depend on each other in a circular manner. This can lead to a tangled web of dependencies and make it challenging to understand and maintain the code. For instance, Module A depends on Module B, which in turn depends on Module C, and Module C depends back on Module A.

3. Inappropriate Dependency:
Inappropriate dependency occurs when a module depends on another module that does not logically or semantically align with its functionality. For example, if a module responsible for handling user authentication also depends on a module related to graphic rendering, it signifies an inappropriate coupling between unrelated concerns.

4. External Dependency:
Modules that rely heavily on external libraries, frameworks, or services introduce external dependencies. Excessive reliance on external dependencies can make the system more fragile, as any changes or issues in those external dependencies may have cascading effects on the modules using them.

5. Shared State:
Modules that share mutable global variables or common data structures introduce coupling through shared state. Changes in one module's use of shared state can impact other modules that rely on the same data, leading to potential bugs and unintended side effects.

6. Message Format Dependency:
Modules that rely on specific message formats or protocols for communication can lead to tight coupling. If one module expects the messages from another module to follow a specific format, it becomes challenging to change the message structure without affecting the consuming module.

Addressing these coupling issues is essential for creating more modular and maintainable software systems. By minimizing dependencies, promoting loose coupling, and employing proper abstraction and encapsulation techniques, developers can enhance the flexibility, scalability, and robustness of their codebase.

When designing you have to be careful with high coupling:

1. Direct Method Invocation:
Module A directly invokes methods or functions defined in Module B, tightly coupling the two modules. Any changes in the method signatures or implementation details of Module B may require corresponding modifications in Module A.

2. Shared Data Structures:
Modules that share mutable data structures, such as global variables or shared memory, introduce high coupling. Changes to the structure or representation of the shared data can impact multiple modules, leading to potential bugs and maintenance challenges.

3. Shared Database Tables:
If multiple modules directly access and modify the same database tables, it creates high coupling between those modules. Modifying the structure or schema of the shared tables can affect the functioning of multiple modules and require synchronized changes.

4. Strong Inter-Module Dependencies:
When one module heavily relies on the internal workings or specific functionalities of another module, it introduces high coupling. This dependency can make it difficult to modify or replace the dependent module without impacting the module it relies on.

5. Order-Based Execution:
If modules depend on the specific order of execution, where one module must run before or after another module, it creates high coupling. Changing the order of execution or introducing new modules can disrupt the system's functioning.

6. Inheritance Dependencies:
In object-oriented programming, modules that inherit from or depend on specific classes or hierarchies tightly couple to those classes. Any changes to the superclass or base classes can have cascading effects on the dependent modules.

It's important to minimize high coupling between modules to improve software maintainability, flexibility, and modularity. By reducing dependencies, encapsulating functionality, and promoting loose coupling through interfaces, abstractions, and dependency injection, developers can create more resilient and modular software systems.


### Top-down and bottom-up strategies 

Top-down and bottom-up strategies are two complementary approaches to structured program design. They offer different perspectives on how to tackle the design and development of a program.

1. Top-Down Design:
Top-down design, also known as stepwise refinement, starts with an overview of the entire problem and gradually breaks it down into smaller, more manageable subproblems. The design process begins with a high-level view of the problem domain and then progressively refines the solution by decomposing it into smaller modules or functions.

Here's an overview of the top-down design process:

- Problem Analysis: Understand the problem requirements and constraints.
- System Design: Identify the major components or modules required to solve the problem.
- Functional Decomposition: Decompose each major component into smaller, more detailed subcomponents.
- Refinement: Continue decomposing the subcomponents until they can be implemented as individual functions or procedures.
- Implementation: Write the code for each function or procedure, gradually building up the complete program.
- Integration: Combine the individual functions or procedures to create the final program.

The top-down approach allows you to focus on the big picture first and gradually refine the design as you move toward more specific details. It promotes modularity, code reusability, and maintainability by dividing the problem into manageable pieces and encapsulating related functionality into separate modules.

2. Bottom-Up Design:
Bottom-up design takes a different approach by starting with the smaller, individual components and gradually combining them to build larger, more complex modules and the overall program. It begins with the implementation of low-level functions or procedures and then incrementally integrates them to create higher-level modules.

Here's an overview of the bottom-up design process:

- Component Implementation: Implement the lower-level functions or procedures that solve specific subproblems.
- Component Testing: Test and verify the correctness of each component in isolation.
- Component Integration: Combine the tested components to create larger modules.
- Module Integration: Continue integrating modules until the complete program is built.

The bottom-up approach focuses on the specific implementation details first and then assembles them to form larger units. It can be useful when dealing with complex systems where the functionality of individual components is well-defined and understood.

In practice, a combination of both top-down and bottom-up strategies is often employed. The top-down approach provides a high-level structure and understanding of the problem, while the bottom-up approach ensures that individual components are properly designed and tested before integration.

By employing a balanced approach that combines top-down and bottom-up strategies, programmers can create well-structured, modular programs that are easier to develop, understand, and maintain.

### Refactoring

Refactoring is the process of making improvements to the structure, design, and implementation of existing code without changing its external behavior. It involves modifying the code to enhance readability, maintainability, extensibility, and performance while preserving the functionality of the program.

The primary goals of refactoring are:

1. Code Readability: Refactoring aims to improve the clarity and understandability of the code. By restructuring and simplifying the code, it becomes easier to read and comprehend, leading to improved maintainability and reduced bugs.

2. Code Maintainability: Refactoring helps in maintaining the codebase by making it more modular and organized. It eliminates duplication, reduces complex code structures, and improves the overall structure, making it easier to modify, extend, and debug.

3. Code Extensibility: Refactoring makes the code more flexible and extensible, allowing for easier addition of new features or functionalities. It promotes the use of design patterns and modularization, enabling the code to adapt to future requirements with minimal effort.

4. Code Performance: Refactoring can also target performance improvements by optimizing critical sections of the code. This may involve identifying and removing performance bottlenecks, improving algorithm efficiency, or utilizing better data structures.

Common refactoring techniques include:

1. Extract Method/Function: Breaking down a chunk of code into a separate method or function to improve code reuse, readability, and maintainability.

2. Rename: Giving more meaningful and descriptive names to variables, functions, or classes, making the code self-explanatory.

3. Simplify Conditionals: Simplifying complex conditional statements or loops to make them more concise and easier to understand.

4. Eliminate Code Duplication: Identifying duplicated code segments and consolidating them into reusable functions or modules.

5. Reorganize Code Structure: Rearranging code to improve logical flow, grouping related functions or classes together, and promoting better organization.

6. Improve Modularity: Identifying cohesive units of code and restructuring them into separate modules or classes, reducing interdependencies and improving encapsulation.

It's important to note that refactoring should be done incrementally and with proper testing in place to ensure that the behavior of the code remains unchanged. Automated tests, such as unit tests or integration tests, provide confidence that the refactored code behaves as expected.

Refactoring is an ongoing process throughout the software development lifecycle. As requirements change, new insights emerge, or as code quality issues are identified, refactoring allows developers to continuously improve the codebase while maintaining its functionality and reducing technical debt.



