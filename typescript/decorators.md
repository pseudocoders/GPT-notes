# Decorators 

In TypeScript, decorators are a powerful feature that allows you to annotate and modify classes, methods, properties, or parameters at design time. They provide a way to extend or alter the behavior of elements in your code. Decorators are applied using the `@decorator` syntax and are evaluated when your code is declared, not when it's executed. Here are some common use cases for decorators in TypeScript classes:

### 1. **Class Decorators:**

Class decorators are applied to class declarations and receive the constructor of the class as an argument. They can be used to modify the behavior or metadata of the class.

```typescript
function MyLoggerDecorator(constructor: Function) {
  console.log(`Class ${constructor.name} is decorated.`);
}

@MyLoggerDecorator
class ExampleClass {
  // Class definition
}
```

In this example, the `MyLoggerDecorator` class decorator logs a message when the class is decorated.

### 2. **Method Decorators:**

Method decorators are applied to method declarations and receive the target (prototype), the method name, and a property descriptor as arguments. They are often used to extend or modify the behavior of methods.

```typescript
function MyMethodDecorator(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  console.log(`Method ${propertyKey} is decorated.`);
}

class ExampleClass {
  @MyMethodDecorator
  myMethod() {
    // Method implementation
  }
}
```

The `MyMethodDecorator` logs a message when the method is decorated.

### 3. **Property Decorators:**

Property decorators are applied to property declarations and receive the target (prototype) and the property name as arguments. They can be used to modify the behavior or metadata of properties.

```typescript
function MyPropertyDecorator(target: any, propertyKey: string) {
  console.log(`Property ${propertyKey} is decorated.`);
}

class ExampleClass {
  @MyPropertyDecorator
  myProperty: string;
}
```

Here, the `MyPropertyDecorator` logs a message when the property is decorated.

### 4. **Parameter Decorators:**

Parameter decorators are applied to parameters in a method or constructor and receive the target (prototype), the method or constructor name, and the parameter index as arguments. They are commonly used in frameworks like Angular for dependency injection.

```typescript
function MyParameterDecorator(target: any, methodName: string | symbol, parameterIndex: number) {
  console.log(`Parameter at index ${parameterIndex} is decorated.`);
}

class ExampleClass {
  myMethod(@MyParameterDecorator param1: string, @MyParameterDecorator param2: number) {
    // Method implementation
  }
}
```

The `MyParameterDecorator` logs a message when parameters are decorated.

### 5. **Decorator Factories:**

Decorator factories are functions that return decorator functions. This allows you to pass arguments to your decorators.

```typescript
function MyParameterDecoratorFactory(arg1: string) {
  return function (target: any, methodName: string | symbol, parameterIndex: number) {
    console.log(`Parameter decorator called with argument: ${arg1}`);
  };
}

class ExampleClass {
  myMethod(@MyParameterDecoratorFactory('ArgumentValue') param: string) {
    // Method implementation
  }
}
```

Here, the `MyParameterDecoratorFactory` returns a decorator function with an argument.

### 6. **Decorators in Frameworks:**

Many modern web frameworks and libraries, such as Angular, heavily use decorators for various purposes like dependency injection, component metadata, routing, and more.

Understanding and using decorators effectively can lead to more modular and maintainable code, especially when working with frameworks and libraries that leverage this feature.
