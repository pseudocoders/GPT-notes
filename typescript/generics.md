# Generics

TypeScript generics allow you to create functions, classes, and interfaces that can work with different types while maintaining type safety. Generics provide flexibility by allowing you to define placeholders for types that will be determined when the function is called. This enables you to write reusable code that can handle a variety of data types. Here's how you can use TypeScript generics to develop generic functions:

**Generic Function Syntax:**
To define a generic function, you use angle brackets (`<>`) with a placeholder type parameter inside the function signature. This type parameter represents a placeholder for the actual type that will be provided when the function is called. Here's the generic function syntax:

```typescript
function functionName<T>(parameter: T): ReturnType {
  // Function body
}
```

- `functionName` is the name of the generic function.
- `<T>` specifies the placeholder type parameter. You can use any valid identifier as the type parameter name.
- `parameter` is a parameter of type `T` that will be passed to the function.
- `ReturnType` represents the return type of the function. You can explicitly specify it or rely on TypeScript's type inference to determine it.

**Example: Identity Function:**
Let's start with a simple example of an identity function that returns the same value as the input. We'll make it generic to work with any type:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

In this example, `T` is the type parameter representing the placeholder type. The function takes a parameter of type `T` and returns a value of the same type `T`. The actual type will be inferred based on the type of the argument passed when calling the function.

**Using Generic Functions:**
When you call a generic function, you can specify the type argument explicitly or rely on TypeScript's type inference. Here are a few examples:

```typescript
const result1 = identity<number>(5); // Explicitly specifying type argument
console.log(result1); // Output: 5

const result2 = identity("Hello"); // Type inference infers the argument type as string
console.log(result2); // Output: Hello

const result3 = identity<boolean>(true); // Explicitly specifying type argument
console.log(result3); // Output: true
```

In the examples above, we call the `identity` function with different types (`number`, `string`, and `boolean`). The type parameter `T` is resolved based on the provided type argument or inferred by TypeScript.

**Constraints on Generics:**
You can also apply constraints on generics to restrict the types that can be used with the type parameter. For example, you can ensure that the type parameter extends a specific interface or is a subtype of a certain class.

```typescript
interface Lengthy {
  length: number;
}

function logLength<T extends Lengthy>(value: T): void {
  console.log(value.length);
}
```

In this example, the `logLength` function accepts a generic parameter `T` that extends the `Lengthy` interface. The constraint ensures that the passed value must have a `length` property of type `number`.


## Classes and Interfaces

Generics can also be used with classes and interfaces in TypeScript, allowing for the creation of reusable and flexible code that can work with multiple types. Here's an explanation of how generics can be applied to classes and interfaces:

**Generics in Classes:**
To use generics in a class, you can declare a type parameter that represents a placeholder for a specific type. This type parameter can then be used within the class to define the types of properties, methods, and other class members. Here's an example:

```typescript
class Box<T> {
  private contents: T;

  constructor(contents: T) {
    this.contents = contents;
  }

  getContents(): T {
    return this.contents;
  }
}
```

In the `Box` class above, `T` is a type parameter that represents the type of the contents stored in the box. The type parameter is declared within angle brackets (`<>`) after the class name. It can then be used to define the type of the `contents` property and the return type of the `getContents` method. When creating an instance of the `Box` class, you can specify the actual type for `T`. For example:

```typescript
const numberBox = new Box<number>(42);
console.log(numberBox.getContents()); // Output: 42

const stringBox = new Box<string>("Hello");
console.log(stringBox.getContents()); // Output: Hello
```

In this example, `numberBox` is an instance of `Box` with `T` resolved to `number`, and `stringBox` is an instance with `T` resolved to `string`. The type parameter `T` allows the class to work with different types while maintaining type safety.

**Generics in Interfaces:**
Generics can also be applied to interfaces, allowing you to define interfaces that work with various types. Similar to classes, you declare a type parameter within angle brackets and use it to specify the types of properties, methods, or other members of the interface. Here's an example:

```typescript
interface Repository<T> {
  getById(id: number): T | undefined;
  save(item: T): void;
}
```

In the `Repository` interface above, `T` is a type parameter representing the type of items stored in the repository. The type parameter is declared within angle brackets after the interface name. The interface defines two methods, `getById` and `save`, both of which operate on values of type `T`. When implementing the `Repository` interface, you can specify the actual type for `T`. For example:

```typescript
class UserRepository implements Repository<User> {
  getById(id: number): User | undefined {
    // Implementation
  }

  save(user: User): void {
    // Implementation
  }
}
```

In this example, `UserRepository` implements the `Repository` interface with `T` resolved to the `User` type. The type parameter `T` allows the interface to be used with different types of repositories while providing type safety.

Using generics in classes and interfaces enables you to write reusable and flexible code that can work with various types, promoting code modularity and type safety. It allows you to create generic data structures, algorithms, and patterns that can be adapted to different data types without sacrificing type checking.


### Example

```typescript
interface Printable {
  print(): void;
}

interface Serializable {
  serialize(): string;
}

interface PrintableSerializable extends Printable, Serializable {
  // Inherits both print() and serialize() methods
}

class Data<T extends PrintableSerializable> {
  private data: T;

  constructor(data: T) {
    this.data = data;
  }

  printData(): void {
    this.data.print();
  }

  serializeData(): string {
    return this.data.serialize();
  }
}

class Person implements PrintableSerializable {
  constructor(private name: string, private age: number) {}

  print(): void {
    console.log(`Name: ${this.name}, Age: ${this.age}`);
  }

  serialize(): string {
    return JSON.stringify({ name: this.name, age: this.age });
  }
}
```

In the example above, we have three interfaces: `Printable`, `Serializable`, and `PrintableSerializable`. `PrintableSerializable` extends both `Printable` and `Serializable` interfaces, inheriting their methods.

The `Data` class is a generic class that accepts a type parameter `T` which must be a subtype of `PrintableSerializable`. It has a constructor that takes an object of type `T` and stores it in the `data` property. The class provides two methods: `printData()` which calls the `print()` method of the stored object, and `serializeData()` which calls the `serialize()` method of the stored object.

The `Person` class implements the `PrintableSerializable` interface. It has a constructor that accepts `name` and `age` as arguments and implements the `print()` and `serialize()` methods. In this case, the `print()` method logs the name and age of the person, and the `serialize()` method returns a JSON string representation of the person object.

Here's how you can use these classes and interfaces:

```typescript
const person = new Person("Alice", 25);
const data = new Data(person);

data.printData(); // Output: Name: Alice, Age: 25
console.log(data.serializeData()); // Output: {"name":"Alice","age":25}
```

In this example, we create an instance of the `Person` class named `person` and pass it to the `Data` class constructor. We then call the `printData()` method of the `Data` instance, which in turn calls the `print()` method of the `Person` instance, printing the person's name and age. Finally, we call the `serializeData()` method of the `Data` instance, which calls the `serialize()` method of the `Person` instance, returning a JSON string representation of the person object.

## Factory pattern using interfaces and generics




```typescript
// Define a common interface for the product using generics
interface Product<T> {
    operation(): T;
}

// Define an interface for the factory using generics
interface Factory<T> {
    createProduct(): Product<T>;
}

// Implementations of the product interface
class ConcreteProductA implements Product<string> {
    operation(): string {
        return 'ConcreteProductA operation: ' + Math.floor(Math.random() * 10000000);
    }
}

class ConcreteProductB implements Product<number> {
    operation(): number {
        return Math.floor(Math.random() * 10000000);
    }
}

class ConcreteProductC implements Product<boolean> {
    operation(): boolean {
        if (Math.random() <= 0.5) {
            return true;
        } else {
            return false;
        }
    }
}

// Implementations of the factory interface
class ConcreteFactoryA implements Factory<string> {
    createProduct(): Product<string> {
        return new ConcreteProductA();
    }
}

class ConcreteFactoryB implements Factory<number> {
    createProduct(): Product<number> {
        return new ConcreteProductB();
    }
}

class ConcreteFactoryC implements Factory<boolean> {
    createProduct(): Product<boolean> {
        return new ConcreteProductC();
    }
}

// Client code using the factory
function genericProduction<T>(factory: Factory<T>): void {
    const product: Product<T> = factory.createProduct();
    console.log(product.operation());
}

// Usage
for (let i: number = 1; i <= 10; i++) {
    let f: Factory<any>;
    if (Math.random() <= 0.33) {
        f = new ConcreteFactoryA();
    } else {
        if (Math.random() <= 0.5) {
            f = new ConcreteFactoryC();
        } else {
            f = new ConcreteFactoryB();
        }
    }
    genericProduction(f);
}
```


