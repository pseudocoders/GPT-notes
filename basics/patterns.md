# Software patterns

## Introduction

A software pattern, also known as a design pattern, is a reusable solution to a common problem or a recurring design challenge in software development. It provides a structured approach to solving specific problems by capturing best practices and proven solutions that have been developed and refined over time by experienced software engineers.

Patterns are not complete implementations or ready-made code snippets. Instead, they are high-level descriptions of the problem, its context, and the proposed solution. They serve as templates or blueprints that can be applied to different situations, guiding developers in designing software systems that are flexible, maintainable, and scalable.

Software patterns have several key characteristics:

1. Problem and Context: Each pattern addresses a specific problem in a particular context. It identifies the issues or challenges commonly encountered in software development and provides a way to resolve them effectively.

2. Solution: A pattern describes a general solution or a set of guidelines to tackle the identified problem. It defines the components, relationships, and interactions required to achieve the desired outcome.

3. Reusability: Patterns are reusable and can be applied to various software systems. They provide a common vocabulary and shared understanding among developers, facilitating communication and collaboration.

4. Best Practices: Patterns embody best practices and proven techniques that have been refined and validated by the software development community. They capture the collective wisdom of experienced practitioners and help new developers learn from their expertise.

5. Trade-offs: Patterns often involve trade-offs between different design principles, such as flexibility, performance, and maintainability. They help developers make informed decisions by weighing the pros and cons of different design choices.

By understanding and applying software patterns, developers can benefit in several ways:

- Improved Design: Patterns promote good design practices and help create software systems that are modular, extensible, and easier to understand and maintain.
- Reusability: Patterns allow developers to reuse tested and proven solutions, reducing development time and effort.
- Communication: Patterns provide a common language and vocabulary for discussing software design, facilitating communication and collaboration among team members.
- Learning: Studying patterns helps developers broaden their knowledge and gain insights into effective software design principles and techniques.

It's important to note that patterns should not be blindly applied in every situation. They should be used judiciously, taking into account the specific requirements and constraints of the project at hand. Additionally, patterns should evolve and adapt over time as new technologies and practices emerge in the software development field.

## Software pattern index

Here's an index of software patterns:

1. Creational Patterns:
   - Singleton Pattern
   - Factory Method Pattern
   - Abstract Factory Pattern
   - Builder Pattern
   - Prototype Pattern

2. Structural Patterns:
   - Adapter Pattern
   - Decorator Pattern
   - Proxy Pattern
   - Composite Pattern
   - Facade Pattern
   - Bridge Pattern

3. Behavioral Patterns:
   - Observer Pattern
   - Strategy Pattern
   - Template Method Pattern
   - Command Pattern
   - Iterator Pattern
   - State Pattern
   - Visitor Pattern
   - Mediator Pattern
   - Chain of Responsibility Pattern
   - Interpreter Pattern
   - Memento Pattern

4. Architectural Patterns:
   - Model-View-Controller (MVC) Pattern
   - Model-View-ViewModel (MVVM) Pattern
   - Layered Architecture Pattern
   - Event-Driven Architecture Pattern
   - Microservices Architecture Pattern
   - Hexagonal Architecture Pattern
   - Clean Architecture Pattern

5. Concurrency Patterns:
   - Producer-Consumer Pattern
   - Reader-Writer Pattern
   - Semaphore Pattern
   - Barrier Pattern
   - Monitor Pattern
   - Thread Pool Pattern

6. Data Access Patterns:
   - Repository Pattern
   - Unit of Work Pattern
   - Data Mapper Pattern
   - Active Record Pattern

7. Integration Patterns:
   - Publish-Subscribe Pattern
   - Message Queue Pattern
   - Service Bus Pattern
   - Event-Driven Architecture Pattern

8. Presentation Patterns:
   - Model-View-Presenter (MVP) Pattern
   - Model-View-Controller (MVC) Pattern
   - Model-View-ViewModel (MVVM) Pattern
   - Presentation-Abstraction-Control (PAC) Pattern

9. Testing Patterns:
   - Mock Object Pattern
   - Test Double Pattern
   - Data-Driven Test Pattern
   - Page Object Pattern

10. Security Patterns:
    - Authentication Pattern
    - Authorization Pattern
    - Secure Session Pattern
    - Secure Pipe Pattern

These patterns represent a variety of concepts and techniques commonly used in software development. Mastering them can greatly enhance your ability to design and develop robust and maintainable software systems.

We're going to explore the most relevant of them.


## Singleton

The Singleton pattern is a creational design pattern that ensures the existence of only one instance (object) of a class in the entire application. It restricts the instantiation of a class to a single object, providing global access to that object.

Key features of the Singleton pattern:

1. Single Instance: The Singleton class allows for the creation of only one instance of itself. This instance is typically stored as a static member variable within the class.

2. Global Access: The Singleton instance is globally accessible, meaning that any part of the application can access it without needing to create a new instance or pass references around.

3. Lazy Initialization: The Singleton instance is created when it is first requested or accessed. This approach ensures that the instance is created only when needed, rather than being created upfront.

4. Thread Safety: Singleton implementations often need to consider thread safety to ensure that multiple threads accessing the Singleton instance simultaneously do not result in inconsistent or incorrect behavior.

### Java example

Common use cases for the Singleton pattern include:

- Logging: A Singleton Logger class can be used throughout the application to maintain a centralized logging mechanism.
- Database Connections: A Singleton Database class can handle all database-related operations, ensuring that only one connection is established throughout the application.
- Configuration Settings: A Singleton Configuration class can manage application settings and provide access to them from various parts of the system.

Here's an example implementation of a Singleton in Java:

```java
public class Singleton {
    private static Singleton instance;
    
    // Private constructor to prevent instantiation from other classes
    private Singleton() { }
    
    // Public static method to access the Singleton instance
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
    
    // Other methods and properties of the Singleton class
    // ...
}
```

In this example, the Singleton class has a private constructor to prevent direct instantiation. The `getInstance()` method serves as the entry point to access the Singleton instance. It uses double-checked locking and synchronization to ensure thread safety during the initialization phase.

It's important to note that while Singleton pattern can be useful in certain scenarios, it can also introduce challenges such as tight coupling and difficulties with unit testing. Therefore, it should be used judiciously and considered in light of the specific requirements of the application.


### Javascript example

In JavaScript, the Singleton pattern can be implemented in a similar way as in other object-oriented languages. Here's an example of a Singleton implementation in JavaScript:

```javascript
const Singleton = (function() {
  let instance; // Private variable to hold the single instance

  function createInstance() {
    // Private method to create the instance
    const object = new Object('I am the Singleton instance');
    return object;
  }

  return {
    getInstance: function() {
      // Public method to access the Singleton instance
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

// Usage:
const instance1 = Singleton.getInstance();
console.log(instance1); // Output: Object { name: 'I am the Singleton instance' }

const instance2 = Singleton.getInstance();
console.log(instance2); // Output: Object { name: 'I am the Singleton instance' }

console.log(instance1 === instance2); // Output: true (both variables reference the same instance)
```

In this example, we create a closure that encapsulates the Singleton logic. The `createInstance` function is a private function that creates the Singleton object. The `getInstance` method is a public method that provides access to the Singleton instance. It checks if the instance already exists and only creates a new instance if one doesn't already exist. The closure ensures that the `instance` variable is private and not accessible outside the Singleton module.

When we invoke `Singleton.getInstance()`, it returns the same instance every time it is called. The `instance1` and `instance2` variables reference the same object, demonstrating that only one instance is created and used throughout the application.

It's important to note that in modern JavaScript, the Singleton pattern is not as commonly used as in other languages, mainly because JavaScript is a highly dynamic and flexible language that allows for alternative approaches to managing global state and object creation. Nonetheless, the Singleton pattern can still be employed in JavaScript when a single instance is explicitly desired and necessary.


## Factory

The Factory pattern is a creational design pattern that provides an interface for creating objects without specifying the exact class of the object being instantiated. It encapsulates the object creation logic within a separate method or class, called the factory, which is responsible for creating and returning instances of different classes based on specific conditions or parameters.

The main goal of the Factory pattern is to promote loose coupling between the client code and the objects it needs to create. Instead of directly instantiating objects using the `new` operator or knowing the specific implementation classes, the client code delegates the object creation responsibility to the factory. This allows for flexibility in changing or extending the object creation process without modifying the client code.

Key elements of the Factory pattern:

1. Factory Interface/Class: This defines the interface or abstract class that declares the factory methods responsible for creating objects. The factory methods can have different variations based on the specific conditions or parameters.

2. Concrete Implementations: These are the actual classes that implement the factory interface and provide the implementation logic for creating objects. Each concrete factory class is responsible for creating a specific type of object.

3. Client: The client code interacts with the factory through the factory interface, without knowing the specific class of the objects being created. It requests objects from the factory, which handles the creation and returns the appropriate instances.

Benefits of using the Factory pattern:

- Encapsulation: The client code is decoupled from the object creation process and does not need to know the specific implementation details of the objects it uses.

- Flexibility and Extensibility: It's easy to introduce new object types by creating additional concrete factory classes or modifying the existing ones, without affecting the client code.

- Code Reusability: The factory logic can be reused across different parts of the application, allowing for centralized object creation and promoting code maintenance and consistency.

- Dependency Injection: The factory pattern can be combined with dependency injection techniques to manage object creation and resolve dependencies in a more modular and flexible manner.

### Java example

Here's an example of the Factory pattern implemented in Java:

```java
// Factory interface
interface AnimalFactory {
    Animal createAnimal();
}

// Concrete implementations of the factory
class DogFactory implements AnimalFactory {
    public Animal createAnimal() {
        return new Dog();
    }
}

class CatFactory implements AnimalFactory {
    public Animal createAnimal() {
        return new Cat();
    }
}

// Product interface
interface Animal {
    void speak();
}

// Concrete product classes
class Dog implements Animal {
    public void speak() {
        System.out.println("Woof!");
    }
}

class Cat implements Animal {
    public void speak() {
        System.out.println("Meow!");
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        AnimalFactory dogFactory = new DogFactory();
        Animal dog = dogFactory.createAnimal();
        dog.speak();  // Output: Woof!

        AnimalFactory catFactory = new CatFactory();
        Animal cat = catFactory.createAnimal();
        cat.speak();  // Output: Meow!
    }
}
```

In this example, we have the `AnimalFactory` interface that declares the `createAnimal` method, which serves as the factory method for creating animal objects. The `DogFactory` and `CatFactory` classes implement the `AnimalFactory` interface and provide the specific object creation logic for dogs and cats, respectively.

The `Animal` interface defines the common behavior of animals, and the `Dog` and `Cat` classes implement this interface with their respective `speak` methods.

In the client code (`Main` class), we create instances of animals using the appropriate factory. We create a `Dog` instance using `DogFactory` and a `Cat` instance using `CatFactory`. Then, we invoke the `speak` method on each object, which outputs the corresponding animal sound.

By using the Factory pattern, the client code is decoupled from the specific classes of animals and relies on the factory to create the appropriate objects. This promotes flexibility, modularity, and easier maintenance of the codebase.

### Javascript example

Example of the Factory pattern in JavaScript:

```javascript
// Factory interface
class AnimalFactory {
  createAnimal() {
    throw new Error('createAnimal() method must be implemented');
  }
}

// Concrete implementations of the factory
class DogFactory extends AnimalFactory {
  createAnimal() {
    return new Dog();
  }
}

class CatFactory extends AnimalFactory {
  createAnimal() {
    return new Cat();
  }
}

// Product classes
class Dog {
  speak() {
    console.log('Woof!');
  }
}

class Cat {
  speak() {
    console.log('Meow!');
  }
}

// Client code
function performAnimalSound(factory) {
  const animal = factory.createAnimal();
  animal.speak();
}

// Usage
const dogFactory = new DogFactory();
performAnimalSound(dogFactory); // Output: Woof!

const catFactory = new CatFactory();
performAnimalSound(catFactory); // Output: Meow!
```

In this example, we have an abstract `AnimalFactory` class that defines the factory interface with a `createAnimal` method. The concrete factory classes `DogFactory` and `CatFactory` implement the factory interface and provide their own object creation logic.

The client code uses the factory interface to create instances of animals without directly knowing the specific classes. It calls the `performAnimalSound` function, passing the appropriate factory object, which in turn creates the specific animal instance and invokes the `speak` method.

The Factory pattern allows for easy extension by adding new concrete factory classes for different types of animals without modifying the client code. It promotes flexibility and modularity in object creation, making it a powerful tool for managing complex object creation scenarios.
