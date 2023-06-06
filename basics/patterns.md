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

```java
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

// Factory class
class AnimalFactory {
    public static Animal createAnimal(String animalType) {
        if (animalType.equalsIgnoreCase("dog")) {
            return new Dog();
        } else if (animalType.equalsIgnoreCase("cat")) {
            return new Cat();
        } else {
            throw new IllegalArgumentException("Invalid animal type");
        }
    }
}

// Main program
public class Main {
    public static void main(String[] args) {
        String animalType = java.util.Arrays.asList("dog", "cat").get(new java.util.Random().nextInt(2));

        // Create animal based on the parameter using the factory method
        Animal animal = AnimalFactory.createAnimal(animalType);
        animal.speak(); // Output: Woof! or Meow! depending on the animalType
    }
}
```

In this example, the `AnimalFactory` class contains a static factory method called `createAnimal`. This method takes an `animalType` parameter to determine the type of animal to create.

The main program receives an `animalType` parameter, and it calls the `createAnimal` factory method of the `AnimalFactory` class to create an animal object. The factory method handles the creation logic based on the parameter and returns an instance of the appropriate concrete animal class (`Dog` or `Cat`).

The program assigns the returned animal object to the `Animal` interface, allowing the program to interact with the animal without knowing its specific type. Finally, the program invokes the `speak` method on the animal object, which will output "Woof!" if the `animalType` is "dog" or "Meow!" if the `animalType` is "cat". This demonstrates how the Factory pattern with a factory method allows for dynamic creation of objects and interaction with them through a common interface, without explicitly knowing their specific types.

### Javascript example

```javascript

function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}

// Product classes
class Animal {
  speak() {
    throw new Error('speak() method must be implemented');
  }
}

class Dog extends Animal {
  speak() {
    console.log('Woof!');
  }
}

class Cat extends Animal {
  speak() {
    console.log('Meow!');
  }
}

// Factory function
function createAnimal(animalType) {
  if (animalType.toLowerCase() === 'dog') {
    return new Dog();
  } else if (animalType.toLowerCase() === 'cat') {
    return new Cat();
  } else {
    throw new Error('Invalid animal type');
  }
}

// Main program
const animalType = shuffle(['dog', 'cat'])[0];

// Create animal based on the parameter using the factory function
const animal = createAnimal(animalType);
animal.speak(); // Output: Woof! or Meow! depending on the animalType
```

In this JavaScript example, we have the `Animal` class as the base class for different animal types. The `Dog` and `Cat` classes extend the `Animal` class and provide their own implementations of the `speak` method.

The `createAnimal` function serves as the factory function. It takes an `animalType` parameter and returns an instance of the appropriate animal class based on the parameter.

In the main program, we receive an `animalType` parameter and call the `createAnimal` factory function to create an animal object. The factory function handles the creation logic based on the parameter and returns an instance of the appropriate animal class.

Finally, we invoke the `speak` method on the animal object, which will output "Woof!" if the `animalType` is "dog" or "Meow!" if the `animalType` is "cat". This demonstrates how the Factory pattern with a factory function allows for dynamic creation of objects and interaction with them through a common interface, without explicitly knowing their specific types.


## Decorator

The Decorator pattern is a structural design pattern that allows you to dynamically add new behaviors or functionalities to an object without modifying its underlying structure. It follows the principle of open-closed design, where classes are open for extension but closed for modification.

The pattern involves two main components: the base component (also known as the component interface) and the decorator classes. The base component defines the common interface or behavior that both the original object and its decorators implement.

Decorators are classes that wrap the base component and provide additional functionalities by adding new behavior before or after the execution of the base component's methods. Decorators have the same interface as the base component, allowing them to be used interchangeably.

The key idea behind the Decorator pattern is that the decorators maintain a reference to the base component and enhance its behavior by adding their own functionality. They delegate calls to the base component's methods and may perform additional actions before or after the delegation.

By stacking multiple decorators on top of each other, it is possible to combine and compose various functionalities dynamically. This allows for flexible and fine-grained behavior customization of objects at runtime.

The Decorator pattern offers several benefits, including:

1. Enhanced flexibility: Decorators allow for the dynamic addition of behaviors, making it easy to extend the functionality of an object without modifying its code.
2. Simplified customization: With decorators, you can add or remove functionalities as needed, providing a more flexible and customizable approach compared to subclassing.
3. Better code reusability: Decorators can be combined and reused to create new combinations of behaviors, promoting code reuse and modularity.
4. Preserved interface: Both the base component and decorators implement the same interface, ensuring that the clients can interact with them uniformly.

Overall, the Decorator pattern provides a way to add or modify functionalities of objects at runtime while preserving code flexibility and adhering to the open-closed principle.

### Java Example

Here's an example of the Decorator pattern in Java:

```java
// Component interface
interface Beverage {
    double getCost();
    String getDescription();
}

// Concrete component
class DarkRoast implements Beverage {
    public double getCost() {
        return 2.0;
    }

    public String getDescription() {
        return "Dark Roast Coffee";
    }
}

class HouseBlend implements Beverage {
    public double getCost() {
        return 1.5;
    }

    public String getDescription() {
        return "House Blend Coffee";
    }
}

// Decorator
abstract class Addon implements Beverage {
    protected Beverage beverage;

    public Addon(Beverage beverage) {
        this.beverage = beverage;
    }
}

// Concrete decorators
class Milk extends Addon {
    public Milk(Beverage beverage) {
        super(beverage);
    }

    public double getCost() {
        return beverage.getCost() + 0.5;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Milk";
    }
}

class Sugar extends Addon {
    public Sugar(Beverage beverage) {
        super(beverage);
    }

    public double getCost() {
        return beverage.getCost() + 0.3;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Sugar";
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        Beverage darkRoast = new DarkRoast();
        System.out.println("Dark Roast Coffee: " + darkRoast.getDescription() + ", Cost: $" + darkRoast.getCost());

        Beverage houseBlendWithMilk = new Milk(new HouseBlend());
        System.out.println("House Blend Coffee with Milk: " + houseBlendWithMilk.getDescription() + ", Cost: $" + houseBlendWithMilk.getCost());

        Beverage darkRoastWithMilkAndSugar = new Sugar(new Milk(new DarkRoast()));
        System.out.println("Dark Roast Coffee with Milk and Sugar: " + darkRoastWithMilkAndSugar.getDescription() + ", Cost: $" + darkRoastWithMilkAndSugar.getCost());
    }
}
```

In this example, we have a `Beverage` interface that defines the `getCost` and `getDescription` methods. The `DarkRoast` and `HouseBlend` classes are concrete implementations of the `Beverage` interface representing different coffee varieties.

The `Addon` class is the abstract decorator class that implements the `Beverage` interface and maintains a reference to the decorated beverage object.

The `Milk` and `Sugar` classes are concrete decorators that extend the `Addon` class. They add additional functionalities by overriding the `getCost` and `getDescription` methods and calling the base implementation.

In the client code, we create instances of different coffee varieties and wrap them with decorators to add addons. The addons modify the cost and description of the coffee accordingly.

When we run the `main` method, it outputs the description and cost of each coffee with the specified addons. For example, "Dark Roast Coffee with Milk and Sugar: Dark Roast Coffee, Milk, Sugar, Cost: $3.3".

### Javascript Example

Here's the example of the Decorator pattern in JavaScript:

```javascript
// Component interface
class Beverage {
  getCost() {
    throw new Error("Method 'getCost()' must be implemented.");
  }

  getDescription() {
    throw new Error("Method 'getDescription()' must be implemented.");
  }
}

// Concrete component
class DarkRoast extends Beverage {
  getCost() {
    return 2.0;
  }

  getDescription() {
    return "Dark Roast Coffee";
  }
}

class HouseBlend extends Beverage {
  getCost() {
    return 1.5;
  }

  getDescription() {
    return "House Blend Coffee";
  }
}

// Decorator
class Addon extends Beverage {
  constructor(beverage) {
    super();
    this.beverage = beverage;
  }

  getCost() {
    return this.beverage.getCost();
  }

  getDescription() {
    return this.beverage.getDescription();
  }
}

// Concrete decorators
class Milk extends Addon {
  constructor(beverage) {
    super(beverage);
  }

  getCost() {
    return this.beverage.getCost() + 0.5;
  }

  getDescription() {
    return this.beverage.getDescription() + ", Milk";
  }
}

class Sugar extends Addon {
  constructor(beverage) {
    super(beverage);
  }

  getCost() {
    return this.beverage.getCost() + 0.3;
  }

  getDescription() {
    return this.beverage.getDescription() + ", Sugar";
  }
}

// Client code
const darkRoast = new DarkRoast();
console.log("Dark Roast Coffee: " + darkRoast.getDescription() + ", Cost: $" + darkRoast.getCost());

const houseBlendWithMilk = new Milk(new HouseBlend());
console.log("House Blend Coffee with Milk: " + houseBlendWithMilk.getDescription() + ", Cost: $" + houseBlendWithMilk.getCost());

const darkRoastWithMilkAndSugar = new Sugar(new Milk(new DarkRoast()));
console.log("Dark Roast Coffee with Milk and Sugar: " + darkRoastWithMilkAndSugar.getDescription() + ", Cost: $" + darkRoastWithMilkAndSugar.getCost());
```

In this example, we use JavaScript classes to represent the component, concrete components, decorators, and concrete decorators. The syntax and structure of the code are adjusted accordingly.

When we run the code, it outputs the description and cost of each coffee with the specified addons, similar to the Java example. For example:
```
Dark Roast Coffee: Dark Roast Coffee, Cost: $2
House Blend Coffee with Milk: House Blend Coffee, Milk, Cost: $2.5
Dark Roast Coffee with Milk and Sugar: Dark Roast Coffee, Milk, Sugar, Cost: $3.3
```

The Decorator pattern is implemented in a similar manner as in Java, but with the appropriate syntax for JavaScript classes and method definitions.


