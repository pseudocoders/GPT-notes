# Components

## Introduction

In Angular, a component is a fundamental building block of an application that encapsulates the logic, data, and view templates required to render a specific part of the user interface. It is a TypeScript class decorated with the `@Component` decorator, which provides metadata and configuration for the component.

Components in Angular follow the concept of component-based architecture, where the user interface is broken down into smaller, reusable, and self-contained units. Each component represents a distinct part of the UI and is responsible for its own functionality and rendering.

Key characteristics of an Angular component include:

1. Class: A component is defined as a TypeScript class with properties, methods, and event handlers. The class contains the logic and behavior of the component.

2. Template: Components have an associated template that defines the structure and layout of the component's view. The template is written in HTML and may include Angular-specific syntax, such as directives and bindings.

3. Metadata: The `@Component` decorator provides metadata for the component, including selector, template or templateUrl, style or styleUrls, and other configuration options. It allows Angular to understand and process the component correctly.

4. Data Binding: Components enable data binding, allowing data to flow between the component class and the template. Data binding allows you to interpolate values, bind to properties, listen to events, and establish two-way data binding between the component and the template.

5. Lifecycle Hooks: Angular provides lifecycle hooks that allow you to tap into specific stages of a component's lifecycle, such as creation, rendering, updating, and destruction. These hooks, such as `ngOnInit` and `ngOnDestroy`, provide opportunities to perform custom initialization, update data, and clean up resources.

6. Input and Output: Components can accept input data through input properties, allowing parent components to pass data to child components. Similarly, components can emit events through output properties, enabling child components to communicate with their parent components.

7. Styles: Components can have their own styles defined using CSS or preprocessor syntax like SCSS. Styles can be defined inline using the `styles` property or externally using the `styleUrls` property in the component metadata.

Components form the building blocks of Angular applications and enable the creation of modular, reusable, and maintainable code. They promote separation of concerns, code organization, and reusability, making it easier to develop and maintain complex user interfaces. They play a crucial role in Angular applications as they are the building blocks for creating user interfaces. They encapsulate the logic, data, and view templates required to render a specific part of the user interface. Here's a breakdown of the key roles components play in Angular applications:

1. Modularity and Reusability: Components promote modularity by breaking down the user interface into smaller, self-contained units. They encapsulate specific functionality and can be reused across different parts of the application. This reusability simplifies maintenance, promotes code organization, and improves overall development efficiency.

2. View Rendering: Components define the visual representation of the user interface. They have associated templates that contain HTML markup, bindings, and directives. The template is rendered to generate the actual DOM elements displayed to the user. Components control what is rendered, how it looks, and how it responds to user interactions.

3. Component Logic: Components define the behavior and logic behind the user interface. They encapsulate the data, properties, and methods required to interact with the template and respond to user actions. Component classes in Angular are written in TypeScript and can define properties, methods, event handlers, and lifecycle hooks.

4. Data Binding: Components enable data binding, allowing data to flow between the component class and the template. Angular supports various types of data binding, such as property binding, event binding, and two-way binding. This enables seamless communication between the component and its associated template, keeping the UI in sync with the component's state.

5. Component Lifecycle: Components have a lifecycle that consists of various stages, such as creation, rendering, updating, and destruction. Angular provides lifecycle hooks that allow you to tap into these stages and execute custom logic. This helps manage component initialization, updates, resource cleanup, and interactions with other components.

6. Dependency Injection: Components can declare dependencies on services or other components. Angular's dependency injection system ensures that these dependencies are resolved and provided to the component when it is created. This promotes separation of concerns and allows for easier testing and maintainability.

7. Routing and Navigation: Components are often associated with specific routes in Angular applications. They play a key role in defining the views and behavior for different routes, allowing users to navigate between different parts of the application.

Overall, components form the foundation of Angular applications, enabling the creation of modular, reusable, and interactive user interfaces. They combine the visual representation, data handling, and behavior logic required to build robust and dynamic web applications.


## Structure

In Angular, the structure of a component consists of several parts that work together to define the component's behavior, data, and view. Let's explore the structure of a typical Angular component:

1. Import Statements: At the top of the component file, you'll find import statements for the necessary dependencies. This includes imports from Angular modules, third-party libraries, services, and other components.

2. Component Decorator: The `@Component` decorator is used to decorate the component class and provide metadata and configuration options. It includes properties such as:
   - `selector`: A selector that identifies how the component will be used in HTML templates.
   - `template` or `templateUrl`: The HTML template that defines the component's view.
   - `style` or `styleUrls`: The CSS styles that apply to the component's view.
   - `inputs` and `outputs`: Declarations of input and output properties for data binding.
   - `providers`: Configuration for dependency injection, specifying the services and dependencies required by the component.

3. Component Class: The component class is defined using the `class` keyword and typically has the same name as the file. It contains the properties, methods, event handlers, and lifecycle hooks that define the component's behavior. The class may import services or other dependencies needed by the component.

4. Component Properties: Properties are defined within the component class to store and manage the component's data. These properties can be accessed in the template for rendering and data binding purposes. Properties can be decorated with `@Input()` or `@Output()` decorators to establish data binding with other components.

5. Lifecycle Hooks: Angular provides a set of lifecycle hooks that allow you to tap into specific stages of a component's lifecycle. These hooks include `ngOnInit()`, `ngOnChanges()`, `ngDoCheck()`, `ngAfterViewInit()`, and `ngOnDestroy()`, among others. You can define methods in the component class corresponding to these hooks to perform custom logic at various stages.

6. Methods and Event Handlers: Component methods are defined within the component class to handle events, perform calculations, or provide additional functionality. These methods can be called from the template or other parts of the component class.

7. Template: The template defines the structure and layout of the component's view. It is typically written in HTML and can include Angular-specific syntax, such as directives (`*ngFor`, `*ngIf`) and bindings (`{{}}`, `[property]`, `(event)`). The template can access component properties and methods to render data and respond to user interactions.

8. Styles: Styles define the appearance and visual styling of the component. They can be defined inline using the `styles` property in the component decorator or externally using the `styleUrls` property to reference an external CSS file or preprocessor syntax file (e.g., SCSS).

The structure of an Angular component follows a modular and encapsulated approach, where the class, template, and styles work together to define the component's behavior and appearance. By separating concerns and leveraging data binding and event handling, components provide a flexible and maintainable way to build complex user interfaces in Angular applications.

## Examples

Here are some examples of components:

1. Using `selector` and `template`:
   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-greeting',
     template: `
       <h1>Welcome to Angular!</h1>
       <p>Thank you for using our app.</p>
     `,
   })
   export class GreetingComponent {
     // Component logic goes here
   }
   ```

   In this example, the component is defined with the `selector` property set to `'app-greeting'`. It means that this component can be used as `<app-greeting></app-greeting>` in other templates. The content of the component is defined inline using the `template` property.

2. Using `selector` and `templateUrl`:
   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-greeting',
     templateUrl: './greeting.component.html',
   })
   export class GreetingComponent {
     // Component logic goes here
   }
   ```

   In this example, the component's content is defined in a separate HTML file. The `templateUrl` property points to the external HTML file, which will be loaded and used as the template for the component. For example, `greeting.component.html` contains the HTML structure of the component.

3. Using `selector`, `template`, and `style`:
   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-greeting',
     template: `
       <h1>Welcome to Angular!</h1>
       <p>Thank you for using our app.</p>
     `,
     styles: [
       `
         h1 {
           color: blue;
         }
         p {
           font-size: 16px;
         }
       `,
     ],
   })
   export class GreetingComponent {
     // Component logic goes here
   }
   ```

   In this example, the component has an inline style defined using the `styles` property. The styles are provided as an array of strings, where each string represents CSS rules for the component. Here, the `h1` element will have blue color, and the `p` element will have a font size of 16 pixels.

4. Using `selector`, `templateUrl`, and `styleUrls`:
   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-greeting',
     templateUrl: './greeting.component.html',
     styleUrls: ['./greeting.component.css'],
   })
   export class GreetingComponent {
     // Component logic goes here
   }
   ```

   In this example, the component's template is defined in an external HTML file (`greeting.component.html`), and the styles are defined in an external CSS file (`greeting.component.css`). The `templateUrl` property points to the HTML file, and the `styleUrls` property is an array of CSS file paths.

These examples showcase different ways of defining the component's template and styles using the `@Component` decorator in Angular. Depending on your needs, you can choose to define the template and styles inline using the `template` and `style` properties or externally using the `templateUrl` and `styleUrls` properties.

## Lifecycle Hooks

In Angular, lifecycle hooks are methods provided by the Angular framework that allow you to tap into specific stages of a component's lifecycle. These hooks provide opportunities to execute custom logic and perform tasks at various points in the component's lifecycle. Let's explore the different lifecycle hooks available in Angular:

1. `ngOnChanges()`:
   - Description: Called when one or more input properties of the component change.
   - Purpose: Allows you to react to changes in input properties and perform actions based on the new values.
   - Parameters: `changes` (a `SimpleChanges` object containing the previous and current values of the input properties).

2. `ngOnInit()`:
   - Description: Called once after the component's constructor and `ngOnChanges()` have been called.
   - Purpose: Used for initialization tasks, such as fetching initial data, subscribing to observables, or setting up the component's initial state.
   - No parameters.

3. `ngDoCheck()`:
   - Description: Called during every change detection cycle, right after `ngOnChanges()` and `ngOnInit()`.
   - Purpose: Allows you to perform custom change detection and react to changes in component properties or its child components.
   - No parameters.

4. `ngAfterContentInit()`:
   - Description: Called after Angular projects external content into the component's view (e.g., projected content from a parent component or dynamic component creation).
   - Purpose: Used to perform additional initialization tasks that depend on the component's content.
   - No parameters.

5. `ngAfterContentChecked()`:
   - Description: Called after every change detection cycle when Angular has checked the projected content within the component's view.
   - Purpose: Used for tasks that need to be performed after the projected content has been checked and updated.
   - No parameters.

6. `ngAfterViewInit()`:
   - Description: Called after Angular initializes the component's view and child views.
   - Purpose: Used for DOM manipulation, interacting with child components, or accessing elements in the component's view.
   - No parameters.

7. `ngAfterViewChecked()`:
   - Description: Called after every change detection cycle when Angular has checked the component's view and child views.
   - Purpose: Used for tasks that need to be performed after the component's view has been checked and updated.
   - No parameters.

8. `ngOnDestroy()`:
   - Description: Called just before the component is destroyed.
   - Purpose: Used for cleanup tasks such as unsubscribing from observables, canceling timers, or releasing resources.
   - No parameters.

These lifecycle hooks provide points of control and flexibility during the component's lifecycle. By implementing these hooks in your component, you can customize the behavior and perform tasks at specific stages, ensuring proper initialization, updates, and cleanup of resources.

It's important to note that the lifecycle hooks are not always required for every component. Use the appropriate hooks based on your component's needs and the tasks you want to perform at different stages of the lifecycle.

###  Example

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `
    <h1>{{ message }}</h1>
  `,
})
export class ExampleComponent implements OnInit {
  message: string = '';

  constructor() {
    console.log('Constructor executed');
    // This code runs when the component is instantiated
    // However, it is not the recommended place for component initialization tasks
    // Avoid complex initialization or data fetching here
    // Constructor should be used for dependency injection and basic property initialization
  }

  ngOnInit(): void {
    console.log('ngOnInit executed');
    // This code runs when the component is initialized
    // It is the recommended place for component initialization tasks
    // Perform any setup logic, data fetching, or subscription initialization here
    this.message = 'Hello, Angular!';
  }
}
```

In this example:

- The `ExampleComponent` class implements the `OnInit` interface, indicating that it will use the `ngOnInit()` hook.

- The component has a `message` property that will be displayed in the template using interpolation (`{{ message }}`).

- The constructor is used for basic property initialization and dependency injection. It is executed when the component is instantiated. In the example, we log a message to the console to indicate that the constructor has been executed. However, it is not the recommended place for complex component initialization tasks or data fetching.

- The `ngOnInit()` method is called when the component is initialized. It is the recommended place for component initialization tasks. In the example, we log a message to the console to indicate that `ngOnInit()` has been executed, and we set the `message` property to 'Hello, Angular!'.

The key difference between the constructor and `ngOnInit()` is the timing of their execution. The constructor is executed when the component is instantiated, while `ngOnInit()` is called when the component is being initialized. 

It is generally recommended to perform complex initialization tasks, such as data fetching or subscription initialization, in the `ngOnInit()` method. This allows for better separation of concerns and ensures that the component is fully initialized before executing any additional logic. The constructor, on the other hand, should be used primarily for dependency injection and basic property initialization.

In summary, use the constructor for basic setup and dependency injection, and utilize `ngOnInit()` for more advanced component initialization tasks.

## Template bindings

In Angular projects, template bindings are a powerful feature that allows you to establish a connection between component properties or expressions and elements within the HTML template. Bindings enable you to dynamically update and display data, respond to user interactions, and control the behavior of your application. There are different types of template bindings available in Angular:

1. Interpolation ({{ }}):
   - Interpolation is the most common type of binding used in Angular.
   - Syntax: `{{ expression }}`
   - Purpose: Allows you to embed component properties or expressions within the HTML template.
   - Example: `<h1>{{ title }}</h1>`

2. Property Binding ([ ]):
   - Property binding allows you to bind component properties to element properties.
   - Syntax: `[property]="expression"`
   - Purpose: Dynamically sets the value of an element's property based on a component property or expression.
   - Example: `<input [value]="username">`

3. Event Binding (( ) or on-):
   - Event binding enables you to bind component methods to element events.
   - Syntax: `(event)="expression"` or `on-event="expression"`
   - Purpose: Executes a component method when the specified event occurs on the element.
   - Example: `<button (click)="onSubmit()">Submit</button>`

4. Two-Way Binding ([( )]):
   - Two-way binding allows you to establish a bi-directional data flow between a component property and an element property.
   - Syntax: `[(property)]="expression"`
   - Purpose: Updates both the component property and the element property simultaneously when either of them changes.
   - Example: `<input [(ngModel)]="username">`

5. Attribute Binding ([attr.]):
   - Attribute binding allows you to bind component properties to HTML attributes.
   - Syntax: `[attr.attributeName]="expression"`
   - Purpose: Sets the value of an HTML attribute based on a component property or expression.
   - Example: `<div [attr.role]="userRole">...</div>`

6. Class Binding ([class.]):
   - Class binding allows you to dynamically add or remove CSS classes on elements based on component properties or expressions.
   - Syntax: `[class.className]="expression"`
   - Purpose: Adds or removes the specified CSS class based on the truthiness of the component property or expression.
   - Example: `<div [class.highlighted]="isActive">...</div>`

7. Style Binding ([style.]):
   - Style binding enables you to dynamically set CSS styles on elements based on component properties or expressions.
   - Syntax: `[style.styleName]="expression"`
   - Purpose: Sets the value of a CSS style property based on the component property or expression.
   - Example: `<div [style.backgroundColor]="bgColor">...</div>`

These template bindings provide a declarative and flexible way to interact with and manipulate the HTML template based on the component's data and behavior. By leveraging these bindings, you can create dynamic, interactive, and data-driven Angular applications.

### Example

Here's an example in Angular:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-binding-example',
  template: `
    <h1>Binding Example</h1>

    <!-- Interpolation -->
    <p>Welcome, {{ name }}!</p>

    <!-- Property Binding -->
    <img [src]="imageUrl" [alt]="imageAlt">

    <!-- Event Binding -->
    <button (click)="onButtonClick()">Click Me</button>

    <!-- Two-Way Binding -->
    <input type="text" [(ngModel)]="inputValue">
    <p>You entered: {{ inputValue }}</p>
  `,
})
export class BindingExampleComponent {
  name: string = 'John Doe';
  imageUrl: string = 'https://example.com/image.jpg';
  imageAlt: string = 'Example Image';
  inputValue: string = '';

  onButtonClick(): void {
    console.log('Button clicked!');
    // Perform any desired actions here
  }
}
```

In this example:

- Interpolation: The `name` property is displayed within double curly braces (`{{ name }}`). Angular evaluates the expression and replaces it with the corresponding value from the component, resulting in "Welcome, John Doe!" being displayed.

- Property Binding: The `src` and `alt` attributes of the `<img>` element are bound to the `imageUrl` and `imageAlt` properties, respectively. The values of these properties are dynamically bound to the corresponding attributes, allowing the image source and alt text to be set based on the component's values.

- Event Binding: The (click) event of the <button> element is bound to the onButtonClick() method of the component. When the button is clicked, the associated method is called, and in this example, it logs a message to the console. You can perform any desired actions within the event handler method.

- Two-Way Binding: The `<input>` element is bound using the `[(ngModel)]` syntax, which enables two-way data binding. The `inputValue` property serves as the source of the input value, and any changes made to the input will update the `inputValue` property. Similarly, changes to the `inputValue` property will update the value in the input field.

These binding techniques enable dynamic data rendering, property synchronization, and user input updates in an Angular component. Interpolation allows values to be displayed within the template, property binding allows properties to be bound to element attributes, and two-way binding facilitates data synchronization between the component and the template. The event binding syntax (event)="handler()" allows you to bind component methods to various events like click, keypress, mouseover, etc. This enables you to respond to user interactions and trigger specific actions in your component.

## Built-in Angular directives

Here is a list of some commonly used built-in directives in Angular:

1. **ngIf**: Conditionally renders an element based on a condition.
   ```html
   <div *ngIf="condition">Content to be displayed when condition is true</div>
   ```

2. **ngFor**: Iterates over a collection and generates multiple instances of an element.
   ```html
   <ul>
     <li *ngFor="let item of items">{{ item }}</li>
   </ul>
   ```

3. **ngSwitch**: Conditionally renders elements based on a provided value.
   ```html
   <div [ngSwitch]="value">
     <div *ngSwitchCase="'A'">Value is A</div>
     <div *ngSwitchCase="'B'">Value is B</div>
     <div *ngSwitchDefault>Value is neither A nor B</div>
   </div>
   ```

4. **ngClass**: Adds or removes CSS classes dynamically based on conditions or expressions.
   ```html
   <div [ngClass]="{ 'active': isActive, 'disabled': isDisabled }">Content</div>
   ```

5. **ngStyle**: Sets inline styles dynamically based on component properties or expressions.
   ```html
   <div [ngStyle]="{ 'color': textColor, 'background-color': bgColor }">Content</div>
   ```

6. **ngModel**: Creates a two-way data binding between an input element and a component property.
   ```html
   <input [(ngModel)]="property" />
   ```

7. **ngTemplateOutlet**: Renders a template dynamically based on a provided context.
   ```html
   <ng-container *ngTemplateOutlet="templateRef; context: { prop: value }"></ng-container>
   ```

8. **ngContainer**: A non-rendering element used as a placeholder to group multiple elements.
   ```html
   <ng-container>Content</ng-container>
   ```

9. **ngNonBindable**: Prevents Angular from compiling or evaluating the contents of an element.
   ```html
   <div ngNonBindable>{{ someExpression }}</div>
   ```

10. **ngClass**: Adds or removes CSS classes dynamically based on conditions or expressions.
    ```html
    <div [ngClass]="{ 'active': isActive, 'disabled': isDisabled }">Content</div>
    ```

11. **ngIfElse**: Conditionally renders different content based on a condition.
   ```html
   <div *ngIf="condition; else elseBlock">Content to be displayed when condition is true</div>
   <ng-template #elseBlock>Content to be displayed when condition is false</ng-template>
   ```

12. **ngTemplateOutlet**: Renders a template dynamically based on a provided context.
   ```html
   <ng-container *ngTemplateOutlet="templateRef; context: { prop: value }"></ng-container>
   ```

13. **ng-content**: Represents the content projection slot for a component.
   ```html
   <app-custom-component>
     <h1>Content to be projected into the component</h1>
   </app-custom-component>
   ```

14. **ngNonBindable**: Prevents Angular from compiling or evaluating the contents of an element.
    ```html
    <div ngNonBindable>{{ someExpression }}</div>
    ```

These are just a few examples of the commonly used built-in directives in Angular. Each directive has its own purpose and provides a powerful way to manipulate the DOM, conditionally render elements, and apply dynamic styling. By leveraging these directives, you can create dynamic and interactive user interfaces in your Angular applications.

### Example

Here's a complete example of an Angular component that demonstrates the usage of directives:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `
    <h1>Welcome to the Example Component</h1>

    <div *ngIf="showMessage">
      <p>{{ message }}</p>
    </div>

    <ul>
      <li *ngFor="let item of items">{{ item }}</li>
    </ul>

    <input type="text" [(ngModel)]="inputValue" [ngClass]="{ 'highlight': inputValue.length > 5 }">
    <p [ngClass]="{ 'red-text': isError }">Current Value: {{ inputValue }}</p>
  `,
  styles: [`
    .highlight {
      background-color: yellow;
    }

    .red-text {
      color: red;
    }
  `]
})
export class ExampleComponent {
  showMessage: boolean = true;
  message: string = "This message is shown conditionally";
  items: string[] = ['Item 1', 'Item 2', 'Item 3'];
  inputValue: string = '';
  isError: boolean = false;
}
```

In this example:

- The `ngIf` directive is used to conditionally display the `<div>` element based on the value of the `showMessage` property.
- The `ngFor` directive is used to iterate over the `items` array and render `<li>` elements for each item.
- The `ngModel` directive is used for two-way data binding, binding the value of the `<input>` element to the `inputValue` property.
- The `ngClass` directive is used to conditionally apply CSS classes to elements. In this case, it adds the `highlight` class to the `<input>` element if the `inputValue` has a length greater than 5 characters. It also adds the `red-text` class to the `<p>` element if the `isError` property is `true`.
- The component has an associated CSS style block that defines the styles for the `highlight` and `red-text` classes.

Please note that for this code to work, you'll need to import the necessary Angular modules in your application and ensure that the component is properly included in your module's declarations.

This example demonstrates how you can use `ngIf` to conditionally show content, `ngFor` to iterate over a collection, `ngModel` for two-way data binding, and `ngClass` for conditional styling in an Angular component.

## Events

In Angular, events play a crucial role in capturing user interactions and triggering specific actions in response. Events in Angular components are used to handle user input, such as clicks, keyboard inputs, mouse movements, and more. Angular provides several mechanisms to handle events and bind them to component methods or expressions.

1. Event Binding:
   Event binding is a way to capture and handle events in Angular components. It allows you to bind an event from the template to a method or expression in the component class. Event binding is denoted by the parentheses `()` syntax in the template.

   Example:
   ```html
   <button (click)="handleClick()">Click Me</button>
   ```

   In the above example, the `(click)` event is bound to the `handleClick()` method in the component class. When the button is clicked, the associated method will be invoked.

2. Event Filtering:
   Angular provides the ability to filter events based on certain conditions using event modifiers. Event modifiers are appended to the event binding syntax and allow you to specify additional conditions for event handling.

   Example:
   ```html
   <input (keyup.enter)="submitForm()" />
   ```

   In the above example, the `(keyup.enter)` event binding is used to trigger the `submitForm()` method only when the Enter key is pressed within the input field.

3. Event Objects:
   When an event occurs, Angular provides an event object that contains information about the event. You can capture this event object by passing it as a parameter to the event handler method.

   Example:
   ```html
   <button (click)="handleClick($event)">Click Me</button>
   ```

   In the above example, the `$event` object is passed as a parameter to the `handleClick()` method, allowing you to access additional event details such as the event type, target element, or event payload.

4. Two-Way Event Binding:
   Angular supports two-way event binding, which allows you to establish a bi-directional data flow between an input element and a component property. It combines event binding and property binding into a single syntax using the `[(ngModel)]` directive.

   Example:
   ```html
   <input [(ngModel)]="name" />
   ```

   In the above example, the input element is bound to the `name` property of the component. Changes in the input value will update the `name` property, and vice versa.

Events are essential for capturing user interactions and making components interactive in Angular applications. By utilizing event binding, filtering, event objects, and two-way event binding, you can respond to user actions and trigger appropriate logic in your Angular components.

Here is a list of commonly used bindable events in Angular along with their filtering options:

1. **Click Event**
   - Event: `(click)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

2. **Input Event**
   - Event: `(input)`
   - Filter: `.debounceTime`, `.distinctUntilChanged`

3. **Change Event**
   - Event: `(change)`
   - Filter: `.debounceTime`, `.distinctUntilChanged`

4. **Submit Event**
   - Event: `(submit)`
   - Filter: `.preventDefault`

5. **Key Event**
   - Event: `(keyup)`, `(keydown)`, `(keypress)`
   - Filter: `.enter`, `.escape`, `.space`, `.tab`, `.arrowUp`, `.arrowDown`, `.arrowLeft`, `.arrowRight`

6. **Mouse Event**
   - Event: `(mouseover)`, `(mouseout)`, `(mouseenter)`, `(mouseleave)`, `(mousedown)`, `(mouseup)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

7. **Focus Event**
   - Event: `(focus)`, `(blur)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

8. **Scroll Event**
   - Event: `(scroll)`
   - Filter: `.debounceTime`, `.throttleTime`

9. **DoubleClick Event**
   - Event: `(dblclick)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`
   
10. **Contextmenu Event**
   - Event: `(contextmenu)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

11. **Drag Event**
   - Event: `(drag)`, `(dragstart)`, `(dragend)`, `(dragover)`, `(dragenter)`, `(dragleave)`, `(drop)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

12. **Resize Event**
   - Event: `(resize)`
   - Filter: `.debounceTime`, `.throttleTime`

13. **Window Event**
   - Event: `(window:resize)`, `(window:scroll)`
   - Filter: `.debounceTime`, `.throttleTime`

14. **Form Events**
   - Event: `(focusin)`, `(focusout)`, `(select)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

15. **Touch Events**
   - Event: `(touchstart)`, `(touchend)`, `(touchmove)`, `(touchcancel)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

16. **Animation Events**
   - Event: `(animationstart)`, `(animationend)`, `(animationiteration)`, `(transitionend)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

17. **Custom Events**
   - Event: `(customEvent)`
   - Filter: `.stopPropagation`, `.preventDefault`, `.stopPropagation.preventDefaut`, `.stopPropagation.preventDefaut.self`

These are just a few examples of commonly used bindable events in Angular. Each event can be filtered using various modifiers to further refine the event handling based on specific conditions. The filters allow you to control event propagation, prevent default behavior, apply time-based delays, and more. By understanding these events and their filtering options, you can effectively handle user interactions and customize event handling behavior in your Angular applications.

