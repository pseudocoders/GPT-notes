# Advanced types

## type

In TypeScript, the `type` keyword is used to define custom types. It allows you to create aliases for existing types or create new types based on combinations of existing types. The `type` keyword is part of TypeScript's type system and helps improve code readability, maintainability, and reusability.

**Type Aliases:**
One common use of the `type` keyword is to create type aliases. A type alias allows you to assign a new name to an existing type, making it easier to refer to complex or lengthy types. Type aliases are useful when you want to create a shorthand notation for frequently used types or to provide more descriptive names for complex types.

Here's an example of creating a type alias using the `type` keyword:

```typescript
type Point = {
  x: number;
  y: number;
};

const point: Point = { x: 10, y: 20 };
```

In this example, we define a type alias `Point` that represents an object with `x` and `y` properties of type `number`. We can then use the `Point` alias to declare variables or parameters of that specific type.

When working with TypeScript, there are differences between using custom types and using objects created from classes. Here are some key distinctions:

**1. Definition and Structure:**
- Custom Types: Custom types, defined using the `type` keyword, are used to create aliases or combinations of existing types. They provide a way to assign a new name to an existing type or to create more complex types using union types, intersection types, or type literals. Custom types are used to define the shape or structure of data, but they don't have any runtime representation.
- Objects from Classes: Objects created from classes have a concrete representation at runtime. Classes define the blueprint or template for creating objects and specify their properties and methods. Objects created from classes have an instance state and can be instantiated using the `new` keyword.

**2. Instantiation:**
- Custom Types: Custom types, being just type aliases, cannot be instantiated directly. They are used for type annotations, to provide descriptive names, or to define complex type compositions.
- Objects from Classes: Objects created from classes can be instantiated using the `new` keyword. The instantiation process creates a new instance of the class, allocating memory for the object and executing the constructor to initialize its state.

**3. Behavior and Methods:**
- Custom Types: Custom types do not define behavior or methods. They focus solely on describing the shape and structure of data.
- Objects from Classes: Objects created from classes can encapsulate behavior through methods. Methods define the actions or operations that can be performed on the object. Class instances can invoke these methods to perform specific tasks or modify the object's state.

**4. Inheritance and Polymorphism:**
- Custom Types: Custom types, defined using the `type` keyword, do not support inheritance or polymorphism. They represent specific combinations or aliases of existing types but do not participate in inheritance hierarchies.
- Objects from Classes: Objects created from classes can participate in inheritance hierarchies, where a subclass can inherit properties and methods from a superclass. This allows for code reuse and promotes the principles of inheritance and polymorphism.

**5. Instantiation Flexibility:**
- Custom Types: Custom types are static and cannot be instantiated or modified at runtime. They are used primarily for static type checking and type annotations during the development process.
- Objects from Classes: Objects created from classes are dynamic and can be instantiated, modified, and have their state changed at runtime. They provide more flexibility for working with objects, allowing for data manipulation and behavior customization.

In summary, custom types are used to define the shape and structure of data, whereas objects created from classes provide a runtime representation with state and behavior. Custom types are primarily used for type annotations and type compositions, while objects from classes are instantiated, modified, and used to model and interact with real-world entities or concepts.



## Union & Intersection Types

Let's explore union and intersection types with examples:

**Union Types:**
Union types allow you to combine multiple types, stating that a value can be of any one of those types. You represent a union type using the `|` operator.

Example 1: Union of Primitive Types
```typescript
let age: number | string;
age = 25; // Valid
age = "twenty-five"; // Valid
age = true; // Error: Type 'boolean' is not assignable to type 'number | string'
```
In this example, the variable `age` can hold values of either `number` or `string` types. Assigning a value of either type to `age` is valid, but assigning a value of a different type, such as `boolean`, results in a type error.

Example 2: Union of Object Types
```typescript
type Square = { width: number };
type Rectangle = { width: number; height: number };

function calculateArea(shape: Square | Rectangle): number {
  return shape.width * (shape.height || shape.width);
}

const square: Square = { width: 5 };
console.log(calculateArea(square)); // Output: 25

const rectangle: Rectangle = { width: 4, height: 6 };
console.log(calculateArea(rectangle)); // Output: 24
```
In this example, we have two object types, `Square` and `Rectangle`, representing shapes with different properties. The `calculateArea` function takes a parameter `shape` of type `Square | Rectangle`, indicating that it can accept either a square or a rectangle object. Inside the function, we calculate the area based on the given shape's properties. We can then call `calculateArea` with both a `Square` object and a `Rectangle` object.

**Intersection Types:**
Intersection types allow you to combine multiple types, resulting in a new type that has all the properties and methods from each of the constituent types. You represent an intersection type using the `&` operator.

Example: Intersection of Object Types
```typescript
type Printable = { print: () => void };
type Serializable = { serialize: () => string };

type PrintableSerializable = Printable & Serializable;

class Book implements PrintableSerializable {
  print(): void {
    console.log("Printing book...");
  }

  serialize(): string {
    return "Serialized book data";
  }
}

function processDocument(doc: PrintableSerializable): void {
  doc.print();
  console.log("Serialized data:", doc.serialize());
}

const book = new Book();
processDocument(book);
```
In this example, we define three types: `Printable`, `Serializable`, and `PrintableSerializable`. The `Printable` and `Serializable` types represent objects with `print()` and `serialize()` methods, respectively. The `PrintableSerializable` type is an intersection type that combines both `Printable` and `Serializable` types.

We then create a `Book` class that implements the `PrintableSerializable` interface. The `Book` class provides implementations for both the `print()` and `serialize()` methods.

The `processDocument` function takes a parameter `doc` of type `PrintableSerializable` and uses the methods `print()` and `serialize()` of the given object.

Finally, we instantiate a `Book` object and pass it to the `processDocument` function, demonstrating the usage of the intersection type.

Union and intersection types in TypeScript provide flexibility in working with multiple types, allowing you to create more expressive and flexible type compositions.

## Type Guards

Type guards in TypeScript are mechanisms that allow you to narrow down the type of a variable within a conditional block, enabling more specific type checking and accessing properties or methods that are only available on certain types. Type guards help make your code more robust and prevent type-related errors.

There are several ways to perform type guards in TypeScript:

**1. typeof Type Guard:**
The `typeof` type guard allows you to narrow down the type of a variable based on its primitive value. It checks if a variable has a specific primitive type (e.g., `string`, `number`, `boolean`, `symbol`) and applies the appropriate type narrowing.

```typescript
function doSomething(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

In this example, the `typeof` type guard is used to determine if the `value` parameter is of type `string`. Inside the conditional block, TypeScript narrows down the type of `value` to `string`, allowing us to call the `toUpperCase()` method.

**2. instanceof Type Guard:**
The `instanceof` type guard checks if an object is an instance of a specific class or constructor function. It allows you to narrow down the type of an object based on its class.

```typescript
class Animal {
  sound: string;
  constructor(sound: string) {
    this.sound = sound;
  }
}

class Dog extends Animal {
  breed: string;
  constructor(sound: string, breed: string) {
    super(sound);
    this.breed = breed;
  }
}

function makeSound(animal: Animal): void {
  if (animal instanceof Dog) {
    console.log(`Barking (${(animal as Dog).breed}): ${animal.sound}`);
  } else {
    console.log(`Making sound: ${animal.sound}`);
  }
}

const dog = new Dog("Woof", "Labrador");
const cat = new Animal("Meow");

makeSound(dog); // Output: Barking (Labrador): Woof
makeSound(cat); // Output: Making sound: Meow
```

In this example, the `instanceof` type guard checks if the `animal` parameter is an instance of the `Dog` class. Inside the conditional block, TypeScript narrows down the type of `animal` to `Dog`, allowing us to access the `breed` property.

**3. Custom Type Guards:**
You can also create your own custom type guards using functions that return a type predicate. A type predicate is a function that returns a boolean value and narrows down the type based on that value.

```typescript
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}

function isCircle(shape: Circle | Square): shape is Circle {
  return shape.kind === "circle";
}

function calculateArea(shape: Circle | Square): number {
  if (isCircle(shape)) {
    return Math.PI * shape.radius ** 2;
  } else {
    return shape.sideLength ** 2;
  }
}
```

In this example, the `isCircle` function is a custom type guard that checks if the `shape` parameter has the property `kind` with a value of `"circle"`. The function returns a type predicate (`shape is Circle`), indicating that the type has been narrowed down to `Circle` if the condition is true.

Inside the `calculateArea` function, the type guard is used to determine whether `shape` is a `Circle` or a `Square`. TypeScript can now safely access the `radius` property if it's a `

## Type Assertions


Type assertions, also known as type casting, are a way to tell the TypeScript compiler about the specific type of a value when the compiler cannot infer it automatically. Type assertions are a mechanism to override the default type inference and provide more explicit type information.

Type assertions are denoted by using either the angle bracket syntax (`<Type>value`) or the "as" syntax (`value as Type`).

Here's an example of type assertions:

```typescript
let value: any = "Hello, TypeScript!";
let length1: number = (<string>value).length;
let length2: number = (value as string).length;

console.log(length1); // Output: 19
console.log(length2); // Output: 19
```

In this example, we have a variable `value` of type `any`. We know that the value is actually a string, but TypeScript doesn't infer it because of the `any` type. To access the `length` property of the string, we use type assertions.

Both `<string>value` and `value as string` tell TypeScript that `value` should be treated as a `string` type, allowing us to access the `length` property. The result is assigned to `length1` and `length2`, respectively, and they both correctly give the length of the string.

It's important to note that type assertions don't perform any runtime type checking or conversion. They are only a way to inform the TypeScript compiler about the type you expect the value to have. If the type assertion is incorrect and the value doesn't match the specified type, it can lead to runtime errors.

Type assertions are most commonly used when working with variables of type `any` or when interfacing with libraries or APIs where the type information is incomplete or inaccurate. However, it's generally recommended to use type assertions sparingly and rely on type inference and static typing as much as possible to ensure type safety and maintainability in TypeScript code.
