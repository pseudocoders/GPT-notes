# Interfaces

Interfaces in TypeScript provide a way to define the structure and shape of an object or a class. They allow you to enforce a contract on objects, specifying the names and types of their properties and the signatures of their methods. Interfaces are primarily used to achieve type checking and ensure consistency across different parts of your codebase. Here are the key aspects of interfaces in TypeScript:

1. **Interface Declaration**: Interfaces are declared using the `interface` keyword, followed by the name of the interface. For example:
   ```typescript
   interface Person {
     name: string;
     age: number;
     sayHello(): void;
   }
   ```

2. **Property Declarations**: Inside an interface, you can define properties with their names and corresponding types. For example, the `Person` interface above defines `name` as a property of type `string` and `age` as a property of type `number`.

3. **Method Declarations**: Interfaces can also include method declarations. Method signatures specify the name, parameter types, and return type of the method. For example, the `Person` interface above declares a `sayHello` method with no parameters and a return type of `void`.

4. **Optional Properties**: You can make properties optional in an interface by adding a question mark (`?`) after their names. Optional properties can be assigned `undefined` or omitted when creating objects that implement the interface. For example:
   ```typescript
   interface Person {
     name: string;
     age?: number;
   }
   ```

5. **Readonly Properties**: You can make properties readonly in an interface by using the `readonly` modifier. Readonly properties can only be assigned a value at the time of object creation and cannot be modified later. For example:
   ```typescript
   interface Person {
     readonly name: string;
   }
   ```

6. **Extending Interfaces**: Interfaces can inherit from other interfaces using the `extends` keyword. This allows you to create new interfaces that include the properties and methods from existing interfaces, along with additional members. For example:
   ```typescript
   interface Student extends Person {
     studentId: number;
   }
   ```

7. **Implementing Interfaces**: Classes and objects can implement interfaces to ensure that they adhere to a specific structure. The `implements` keyword is used to declare that a class or object implements an interface. For example:
   ```typescript
   class Student implements Person {
     name: string;
     age: number;
     sayHello(): void {
       console.log("Hello!");
     }
   }
   ```

8. **Type Checking**: Interfaces provide type checking during compile-time. When an object or class implements an interface, TypeScript checks if it conforms to the structure defined by the interface. It verifies that all required properties and methods are present and have the correct types.

9. **Duck Typing**: Duck typing is a concept in programming languages like TypeScript where the type or class of an object is determined by its behavior (methods and properties) rather than its explicit type or inheritance. In TypeScript, this means that objects are considered compatible with an interface as long as they have the same structure and shape, regardless of their explicit declaration of implementing the interface. Here's an example:

```typescript
// Define an interface
interface Quackable {
  quack(): void;
}

// An object with the same structure as the interface
const duck1 = {
  quack: () => console.log('Quack!'),
};

// Another object with the same structure as the interface
const duck2 = {
  quack: () => console.log('Quack! Quack!'),
};

// Function that takes a Quackable parameter
function makeDuckQuack(duck: Quackable) {
  duck.quack();
}

// Both objects can be passed to the function
makeDuckQuack(duck1); // Output: Quack!
makeDuckQuack(duck2); // Output: Quack! Quack!

// A different object structure, but still quackable
const robotDuck = {
  makeSound: () => console.log('Beep beep!'),
  quack: () => console.log('Quack! (from a robot)'),
};

// This object can also be passed to the function
makeDuckQuack(robotDuck); // Output: Quack! (from a robot)
```

In this example, `duck1` and `duck2` are objects that conform to the `Quackable` interface, even though they haven't explicitly stated that they implement the interface. The `makeDuckQuack` function takes a parameter of type `Quackable`, and it can accept any object that has a `quack` method, regardless of its declared type.

The `robotDuck` object is not explicitly declared to implement the `Quackable` interface, but since it has the same structure (a `quack` method), it can be used wherever a `Quackable` is expected. This demonstrates the concept of duck typing in TypeScript, where the compatibility of an object with an interface is based on its shape rather than its explicit type or class declaration.


Interfaces in TypeScript play a crucial role in defining contracts and ensuring type safety in your code. They promote code reusability, modularity, and consistency, making it easier to work with complex data structures and collaborate in larger projects.

### Extending interfaces

Extending an interface in TypeScript allows you to create a new interface that inherits properties and methods from one or more existing interfaces. This enables you to create a combination of interfaces, combining their members into a single interface. Here's how you can extend an interface in TypeScript:

```typescript
interface Interface1 {
  property1: string;
  method1(): void;
}

interface Interface2 {
  property2: number;
  method2(): void;
}

interface CombinedInterface extends Interface1, Interface2 {
  additionalProperty: boolean;
}
```

In the example above, we have two interfaces, `Interface1` and `Interface2`, each with their own properties and methods. To create a combination of these interfaces, we use the `extends` keyword followed by the names of the interfaces we want to inherit from.

The `CombinedInterface` interface extends `Interface1` and `Interface2`, and it also introduces its own `additionalProperty` of type `boolean`. As a result, `CombinedInterface` includes all the properties and methods from both `Interface1` and `Interface2`, along with the new property.

By extending interfaces, you can create reusable and modular code by defining smaller interfaces and then combining them as needed. This approach promotes code organization, reduces duplication, and allows for better type checking and code consistency.


Here's an example of interface inheritance with classes and objects:

```typescript
// Base interface
interface Vehicle {
  start(): void;
  stop(): void;
}

// Derived interface inheriting from the base interface
interface ElectricVehicle extends Vehicle {
  charge(): void;
}

// Implementing the base interface in a class
class Car implements Vehicle {
  start() {
    console.log('Car is starting...');
  }

  stop() {
    console.log('Car is stopping...');
  }
}

// Implementing the derived interface in another class
class ElectricCar implements ElectricVehicle {
  start() {
    console.log('Electric car is starting...');
  }

  stop() {
    console.log('Electric car is stopping...');
  }

  charge() {
    console.log('Charging electric car...');
  }
}

// Creating objects
const regularCar = new Car();
const electricCar = new ElectricCar();

// Using objects based on interfaces
regularCar.start(); // Output: Car is starting...
regularCar.stop();  // Output: Car is stopping...

electricCar.start(); // Output: Electric car is starting...
electricCar.stop();  // Output: Electric car is stopping...
electricCar.charge();// Output: Charging electric car...
```

In this example, the `Vehicle` interface defines the basic methods `start` and `stop`. The `ElectricVehicle` interface extends the `Vehicle` interface and adds an additional method `charge`. The `Car` class implements the `Vehicle` interface, and the `ElectricCar` class implements the `ElectricVehicle` interface, which means it must provide implementations for all the methods in both interfaces.

This demonstrates how you can use interface inheritance to create a hierarchy of interfaces, and then implement those interfaces in classes, allowing you to structure your code in a way that ensures certain methods are present in classes that implement those interfaces.
