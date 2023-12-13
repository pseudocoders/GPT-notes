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
