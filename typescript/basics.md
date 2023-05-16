# TypeScript Basics

## Data types 

In TypeScript, data types define the kind of values that variables, parameters, and functions can hold. TypeScript provides several built-in data types and allows you to define custom types. Here are the commonly used data types in TypeScript:

1. **Boolean**: Represents a logical value of true or false.

2. **Number**: Represents numeric values, including both integer and floating-point numbers.

3. **String**: Represents a sequence of characters enclosed in single quotes ('') or double quotes ("").

4. **Array**: Represents an ordered collection of values of a particular type. TypeScript arrays can be typed explicitly, such as `number[]` for an array of numbers or `string[]` for an array of strings, or using the generic `Array<ElementType>` syntax.

5. **Tuple**: Represents an array-like structure where the types of a fixed number of elements are known but need not be the same. Tuple types allow specifying the types for each element at specific indices, such as `[string, number, boolean]`.

6. **Enum**: Represents a set of named constant values. Enums provide a way to define a group of related values with more readable names.

7. **Any**: Represents a dynamic type where the value can be of any type. Variables of type `any` can be assigned values of any type, and they do not undergo type checking during compilation. It is often used when working with existing JavaScript code or for cases where the type is not known in advance.

8. **Void**: Represents the absence of a value. Used as the return type for functions that do not return a value.

9. **Null and Undefined**: Represents the absence of a value. `null` and `undefined` are subtypes of all other types. By default, TypeScript includes `null` and `undefined` in the domain of every type, unless the `--strictNullChecks` compiler flag is enabled.

10. **Object**: Represents a non-primitive type, i.e., anything that is not `number`, `string`, `boolean`, `symbol`, `null`, or `undefined`. It includes functions, arrays, classes, objects, etc.

11. **Union Types**: Represents a value that can be of one of several types. Union types are denoted using the `|` symbol between the types, for example, `number | string` represents a value that can be either a number or a string.

12. **Intersection Types**: Represents a combination of multiple types. Intersection types are denoted using the `&` symbol between the types. It allows creating new types that have all the properties of the intersected types.

13. **Literal Types**: Represents a specific value, rather than a range of values. Literal types can be string literals (`"hello"`, `'world'`), numeric literals (`42`, `3.14`), boolean literals (`true`, `false`), or even enum values.

14. **Type Aliases**: Allows creating custom names for types to make the code more readable and maintainable. Type aliases are created using the `type` keyword and can be used to refer to complex types or to give descriptive names to union or intersection types.

15. **Interfaces**: Defines the shape of an object or a class by specifying the names and types of its properties and methods. Interfaces can be used to enforce contracts and provide type checking for objects.

These are some of the primary data types in TypeScript. Understanding and utilizing these data types will help you write type-safe and maintainable code.

### Avoiding any

Avoiding the use of the `any` type in TypeScript offers several conveniences and benefits. Here are some reasons why it's recommended to minimize the use of `any`:

1. **Type Safety**: TypeScript's main objective is to provide static type checking and help catch potential errors during development. By explicitly specifying types for variables, parameters, and return values, you can benefit from type checking at compile-time, which helps identify type-related issues early on. Using `any` bypasses this type checking and may lead to runtime errors that could have been caught during compilation.

2. **Improved Code Maintainability**: Explicitly declaring types enhances code readability and maintainability. It provides a clear understanding of the expected data types for variables and function inputs and outputs. When reviewing or modifying code, having well-defined types makes it easier to understand the purpose and usage of variables and functions.

3. **Tooling Support**: TypeScript's type system enables robust tooling support, such as autocompletion, type inference, and refactoring capabilities in modern code editors and IDEs. By using specific types instead of `any`, you can leverage these features and improve productivity while coding.

4. **Documentation and Collaboration**: Specifying types helps in documenting your code, making it more understandable for other developers who may work on the project. It serves as self-documented code, reducing the need for additional comments or explanations about the expected data types.

5. **Error Prevention**: TypeScript's type system helps prevent certain runtime errors by catching type mismatches or incompatible operations at compile-time. It ensures that you use variables and functions in a way that aligns with their intended purpose and constraints. Relying on the `any` type circumvents these checks and increases the risk of encountering unexpected errors during runtime.

While there may be cases where using `any` is necessary, such as when dealing with external libraries or dynamically-typed JavaScript code, it's generally recommended to use specific types provided by TypeScript whenever possible. This way, you can fully leverage the benefits of static typing, improve code quality, and minimize the chances of encountering type-related issues in your projects.

## Variables

In TypeScript, variables are used to store and manipulate data. They are declared using the `let` or `const` keywords. Here are some important aspects of variables in TypeScript:

1. **Variable Declaration**: Variables are declared using the `let` keyword. For example:
   ```typescript
   let x: number;
   ```

2. **Variable Initialization**: Variables can be initialized at the time of declaration. For example:
   ```typescript
   let x: number = 5;
   ```

3. **Variable Assignment**: Variables can be assigned new values using the assignment operator (`=`). The assigned value must be compatible with the declared type. For example:
   ```typescript
   let x: number = 5;
   x = 10; // Assigning a new value to 'x'
   ```

4. **Type Inference**: TypeScript has a type inference system that can automatically infer the type of a variable based on its assigned value. This allows omitting the type annotation. For example:
   ```typescript
   let x = 5; // TypeScript infers the type of 'x' as number
   ```

5. **Const Variables**: The `const` keyword is used to declare variables that are read-only and cannot be reassigned once initialized. For example:
   ```typescript
   const PI = 3.14; // Declaring a constant variable
   ```

6. **Scoping**: Variables in TypeScript have block scope, which means they are accessible only within the block of code where they are defined. This includes blocks enclosed in curly braces `{}` such as if statements, loops, and function scopes.

7. **Variable Types**: TypeScript supports specifying types for variables using type annotations. This helps catch type-related errors and provides better tooling support. For example:
   ```typescript
   let name: string = "John";
   let age: number = 25;
   let isValid: boolean = true;
   ```

8. **Variable Naming**: Variables in TypeScript follow the same naming conventions as JavaScript. They must start with a letter, underscore, or dollar sign and can contain alphanumeric characters and underscores.

9. **Implicit Any**: By default, if you declare a variable without specifying its type and without initializing it, TypeScript infers it as the `any` type. The `any` type allows values of any type to be assigned to the variable, effectively disabling type checking for that variable. It is recommended to avoid using `any` when possible and provide explicit types.

10. **Type Annotations**: Type annotations are used to explicitly specify the type of a variable, overriding type inference. They can be placed after the variable name, separated by a colon (`:`). For example:
    ```typescript
    let age: number = 25; // Type annotation 'number' is explicitly provided
    ```

11. **Type Assertion**: Type assertion allows you to inform the TypeScript compiler about the specific type of a variable when the type inference system is unable to determine it. Type assertion is done using the syntax `<Type>` or `value as Type`. For example:
    ```typescript
    let someValue: any = "Hello, TypeScript!";
    let strLength: number = (someValue as string).length; // Type assertion to treat 'someValue' as string
    ```

Variables in TypeScript provide type safety and allow for better code organization and readability. By specifying types explicitly, you can catch errors early and benefit from enhanced tooling support, making your code more robust and maintainable.


### Example

Here's an example that demonstrates declaring and assigning variables of every type in TypeScript:

```typescript
let booleanVar: boolean = true;
let numberVar: number = 42;
let stringVar: string = "Hello, TypeScript!";
let arrayVar: number[] = [1, 2, 3];
let tupleVar: [string, number] = ["TypeScript", 2023];
let enumVar: Colors = Colors.Blue;
let anyVar: any = "This can be any type";

const PI: number = 3.14;

enum Colors {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE",
}

function multiply(a: number, b: number): number {
  return a * b;
}

let functionVar: (a: number, b: number) => number = multiply;

let undefinedVar: undefined = undefined;
let nullVar: null = null;
let objectVar: object = { prop: "value" };

console.log(booleanVar);
console.log(numberVar);
console.log(stringVar);
console.log(arrayVar);
console.log(tupleVar);
console.log(enumVar);
console.log(anyVar);
console.log(PI);
console.log(functionVar(5, 6));
console.log(undefinedVar);
console.log(nullVar);
console.log(objectVar);
```

In this example:

- `booleanVar` is a boolean variable set to `true`.
- `numberVar` is a number variable set to `42`.
- `stringVar` is a string variable set to `"Hello, TypeScript!"`.
- `arrayVar` is an array of numbers `[1, 2, 3]`.
- `tupleVar` is a tuple with a string and a number `["TypeScript", 2023]`.
- `enumVar` is an enum variable set to `Colors.Blue`.
- `anyVar` is an `any` variable that can hold a value of any type.
- `PI` is a constant number variable set to `3.14`.
- `multiply` is a function that takes two numbers and returns their product.
- `functionVar` is a variable that holds a reference to the `multiply` function.
- `undefinedVar` is a variable explicitly set to `undefined`.
- `nullVar` is a variable explicitly set to `null`.
- `objectVar` is an object variable with a property `prop` set to `"value"`.

The example demonstrates the usage of variables of different types, including basic types like boolean, number, string, and complex types like arrays, tuples, enums, and functions. It also includes variables with the `any` type, constants, and variables with explicit values of `undefined`, `null`, and object types.

You can run this code in a TypeScript environment to see the output of each variable.


## Functions and arrow functions

Functions in TypeScript allow you to define reusable blocks of code that can be executed multiple times with different inputs. TypeScript supports two syntaxes for defining functions: standard functions and arrow functions.

**Standard Functions:**
The syntax for a standard function declaration in TypeScript is as follows:
```typescript
function functionName(parameter1: type, parameter2: type, ...): return_type {
  // Function body
  return result;
}
```
- `functionName` is the name of the function.
- `parameter1`, `parameter2`, and so on are the input parameters with their specified types.
- `return_type` is the type of value that the function will return.
- The function body contains the code to be executed.
- The `return` statement specifies the value to be returned from the function.

Here's an example of a standard function that calculates the sum of two numbers:
```typescript
function sum(a: number, b: number): number {
  return a + b;
}

console.log(sum(2, 3)); // Output: 5
```

**Arrow Functions:**
Arrow functions provide a concise syntax for defining functions and capture the `this` value from the surrounding context. The syntax for an arrow function is as follows:
```typescript
const functionName = (parameter1: type, parameter2: type, ...): return_type => {
  // Function body
  return result;
};
```
- `functionName` is the optional name of the function. If omitted, the function is anonymous and can be assigned to a variable.
- `parameter1`, `parameter2`, and so on are the input parameters with their specified types.
- `return_type` is the type of value that the function will return.
- The function body contains the code to be executed.
- The `return` statement specifies the value to be returned from the function.

Here's an example of an arrow function that calculates the square of a number:
```typescript
const square = (x: number): number => {
  return x * x;
};

console.log(square(5)); // Output: 25
```

Arrow functions are often used in scenarios where concise function syntax is preferred, such as when working with array methods like `map`, `filter`, or `reduce`. They also provide lexical scoping of the `this` keyword, ensuring that it refers to the surrounding context where the function is defined.

Both standard functions and arrow functions have their uses, and the choice between them depends on the specific requirements and coding style preferences of your project.
