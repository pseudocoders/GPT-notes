# Object oriented programming

Object-oriented programming (OOP) is a programming paradigm that uses objects and classes for organizing code. TypeScript, being a superset of JavaScript, supports OOP principles. Here's an introduction to OOP in TypeScript:

### Classes and Objects:

In TypeScript, you can define classes using the `class` keyword. A class is a blueprint for creating objects.

```typescript
class Car {
  // Properties
  brand: string;
  model: string;

  // Constructor
  constructor(brand: string, model: string) {
    this.brand = brand;
    this.model = model;
  }

  // Method
  startEngine() {
    console.log(`${this.brand} ${this.model} is starting...`);
  }
}

// Creating an object
const myCar = new Car('Toyota', 'Camry');

// Accessing properties and calling methods
console.log(myCar.brand); // Output: Toyota
myCar.startEngine(); // Output: Toyota Camry is starting...
```

### Inheritance:

Inheritance allows a class to inherit properties and methods from another class. In TypeScript, you use the `extends` keyword for inheritance.

```typescript
class ElectricCar extends Car {
  batteryCapacity: number;

  constructor(brand: string, model: string, batteryCapacity: number) {
    super(brand, model);
    this.batteryCapacity = batteryCapacity;
  }

  chargeBattery() {
    console.log(`Charging ${this.brand} ${this.model} battery...`);
  }
}

// Creating an object of the derived class
const myElectricCar = new ElectricCar('Tesla', 'Model 3', 75);

// Accessing properties and methods from the base class
console.log(myElectricCar.brand); // Output: Tesla
myElectricCar.startEngine(); // Output: Tesla Model 3 is starting...

// Accessing properties and methods from the derived class
console.log(myElectricCar.batteryCapacity); // Output: 75
myElectricCar.chargeBattery(); // Output: Charging Tesla Model 3 battery...
```

```typescript
class Box {
  content: string = "";
  sameAs(other: Box) {
    return other.content === this.content;
  }
}

class DerivedBox extends Box {
  otherContent: string = "?";
}

const base = new Box();
const derived = new DerivedBox();
console.log(derived.sameAs(base)); //true
```
### Encapsulation:

Encapsulation is the bundling of data and the methods that operate on that data into a single unit, i.e., a class. TypeScript uses public, private, and protected access modifiers.

```typescript
class Person {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}

const person = new Person('John');
person.greet(); // Output: Hello, my name is John.
// Uncommenting the line below will result in a compilation error
// console.log(person.name); // Error: Property 'name' is private and only accessible within class 'Person'.
```

### Polymorphism:

Polymorphism allows objects of different classes to be treated as objects of a common base class. TypeScript supports polymorphism through method overriding.

```typescript
class Animal {
  makeSound() {
    console.log('Some generic sound');
  }
}

class Dog extends Animal {
  makeSound() {
    console.log('Woof! Woof!');
  }
}

class Cat extends Animal {
  makeSound() {
    console.log('Meow!');
  }
}

// Polymorphic behavior
const animals: Animal[] = [new Dog(), new Cat()];

animals.forEach(animal => animal.makeSound());
// Output:
// Woof! Woof!
// Meow!
```

These are the basic concepts of OOP in TypeScript. They provide a foundation for organizing and structuring your code in a more modular and reusable way.

### this

In TypeScript, the `this` keyword in classes is used to refer to the current instance of the class. It plays a crucial role in object-oriented programming, allowing you to access and modify the properties and methods of the current instance within class methods.

Here's a brief overview of how `this` is used in classes in TypeScript:

1. **Instance Properties and Methods:**
   - `this` is commonly used within class methods to refer to instance properties and methods.
   - It helps distinguish between instance members and local variables or parameters.

    ```typescript
    class MyClass {
      private myProperty: number;

      constructor(initialValue: number) {
        this.myProperty = initialValue;
      }

      updateProperty(newValue: number): void {
        this.myProperty = newValue;
      }
    }

    const instance = new MyClass(42);
    instance.updateProperty(100);
    ```

2. **Method Chaining:**
   - `this` enables method chaining, allowing you to call multiple methods on the same instance in a concise manner.

    ```typescript
    class ChainingExample {
      private value: number;

      constructor(initialValue: number) {
        this.value = initialValue;
      }

      add(value: number): this {
        this.value += value;
        return this;
      }

      multiply(value: number): this {
        this.value *= value;
        return this;
      }

      value(): number {
        return this.value;
      }
    }

    const chainInstance = new ChainingExample(2);
    chainInstance.add(3).multiply(4).value();
    ```

3. **Arrow Functions:**
   - When using arrow functions as class methods, the lexical scoping of `this` is preserved, avoiding the need to use function binding or workarounds.

    ```typescript
    class ArrowFunctionExample {
      private value: number;

      constructor(initialValue: number) {
        this.value = initialValue;
      }

      updateValue = (newValue: number): void => {
        this.value = newValue;
      }
    }

    const arrowInstance = new ArrowFunctionExample(10);
    arrowInstance.updateValue(20);
    ```

4. **Function Binding:**
   - In some cases, you might need to explicitly bind a function to a specific context using `.bind(this)`.

    ```typescript
    class BindingExample {
      private value: number;

      constructor(initialValue: number) {
        this.value = initialValue;
        this.printValue = this.printValue.bind(this);
      }

      printValue(): void {
        console.log(this.value);
      }
    }

    const bindingInstance = new BindingExample(42);
    bindingInstance.printValue();
    ```

Understanding and correctly using `this` in TypeScript classes is essential for writing maintainable and object-oriented code. It ensures that you are working with the correct instance and allows for clear and readable class definitions.

### super

In TypeScript, the `super` keyword is used to call the constructor or methods of the superclass (parent class) in a derived class (child class). It is essential when you extend a class and want to invoke the behavior of the superclass along with any additional behavior in the subclass. Here are the main use cases for `super` in TypeScript classes:

### 1. Calling the Superclass Constructor:

When a subclass extends a superclass, it must call the constructor of the superclass using `super()` in its own constructor. This ensures that the initialization logic in the superclass is executed before the initialization of the subclass.

```typescript
class Animal {
  constructor(public name: string) {}
}

class Dog extends Animal {
  // Call the superclass constructor using super()
  constructor(name: string, public breed: string) {
    super(name); // Call the constructor of the superclass
  }
}

const myDog = new Dog('Buddy', 'Labrador');
console.log(myDog.name); // Access property from the superclass
console.log(myDog.breed); // Access property from the subclass
```

### 2. Invoking Superclass Methods:

You can use `super` to invoke methods from the superclass within the subclass. This is particularly useful when you want to extend or override the behavior of methods in the subclass.

```typescript
class Vehicle {
  startEngine(): void {
    console.log('Engine started');
  }
}

class Car extends Vehicle {
  // Override the startEngine method
  startEngine(): void {
    super.startEngine(); // Call the method of the superclass
    console.log('Car engine started');
  }
}

const myCar = new Car();
myCar.startEngine();
```

### 3. Accessing Superclass Properties:

You can use `super` to access properties or methods from the superclass in the subclass. This is useful when you want to reuse functionality from the superclass.

```typescript
class Shape {
  protected color: string;

  constructor(color: string) {
    this.color = color;
  }
}

class Circle extends Shape {
  private radius: number;

  constructor(color: string, radius: number) {
    super(color); // Call the constructor of the superclass
    this.radius = radius;
  }

  getArea(): number {
    const area = Math.PI * Math.pow(this.radius, 2);
    console.log(`Area of the ${this.color} circle: ${area}`);
    return area;
  }
}

const myCircle = new Circle('red', 5);
myCircle.getArea();
```

In the example above, the `Circle` class extends the `Shape` class, and `super(color)` is used to invoke the constructor of the superclass, ensuring that the `color` property is properly initialized.

Using `super` helps maintain the integrity of the class hierarchy and facilitates code reuse when working with inheritance in TypeScript.
