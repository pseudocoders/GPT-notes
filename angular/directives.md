# Custom Directives

In Angular, directives are a fundamental concept that extends HTML with custom behavior, allowing developers to manipulate the DOM, apply styles, or add functionality to elements. Directives can be categorized into two main types: built-in directives and custom directives.

1. Built-in Directives:
Angular comes with a set of built-in directives that provide powerful functionalities out of the box. These directives are part of the Angular framework and can be used directly without any additional setup. Some of the most common built-in directives are:

- `ngIf`: Conditionally adds or removes elements from the DOM based on a given expression.
- `ngFor`: Loops through a collection and generates elements for each item in the collection.
- `ngSwitch`: Conditionally displays elements based on multiple case expressions.
- `ngStyle`: Dynamically applies inline styles to elements based on given expressions.
- `ngClass`: Dynamically adds or removes CSS classes from elements based on given expressions.

To use a built-in directive, you simply need to include it in your template by using the appropriate attribute or structural syntax.

Example of using built-in directives:

```html
<!-- ngIf: Conditionally display content -->
<div *ngIf="showElement">This div will only be displayed if showElement is true.</div>

<!-- ngFor: Loop through an array -->
<ul>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>
```

2. Custom Directives:
Custom directives are directives created by the developers to provide specific functionality or behavior tailored to their application's needs. Custom directives allow you to extend the capabilities of Angular by encapsulating reusable functionalities.

To create a custom directive, you use the `@Directive` decorator in an Angular component or a TypeScript class. The decorator provides configuration options for the directive, such as the selector, inputs, outputs, and event listeners.

Example of a custom directive:

```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

In this example, we have created a custom directive called `appHighlight`, which changes the background color of the element to yellow when the mouse enters and removes the highlight when the mouse leaves.

To use the custom directive, you simply add the directive selector as an attribute to the HTML element.

```html
<p appHighlight>
  Hover over this paragraph to see the custom directive in action!
</p>
```

Custom directives are powerful as they allow you to create reusable components that can be easily applied to multiple elements throughout your application, enhancing maintainability and code organization.

In summary, built-in directives are provided by Angular out of the box and offer common functionalities, while custom directives are developed by programmers to add specific behavior and features tailored to their application's requirements. Both types of directives play a significant role in extending the functionality and interactivity of Angular applications.

## Types of custom directives

In Angular, there are three types of custom directives based on their function and purpose:

1. **Structural Directives:**
Structural directives modify the layout of the DOM by adding or removing elements based on conditions. They use the asterisk (*) syntax in the template to indicate that they are structural directives. The most common structural directive is `ngIf`, which conditionally adds or removes elements from the DOM.

Example of a structural directive:

```html
<div *ngIf="showElement">This div will only be displayed if showElement is true.</div>
```

2. **Attribute Directives:**
Attribute directives change the appearance or behavior of an element by modifying its attributes. They are used as attributes in the template. Attribute directives can manipulate the element they are applied to, change its style, or add specific behavior.

Example of an attribute directive:

```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

In this example, `appHighlight` is an attribute directive that changes the background color of an element when the mouse enters and removes the highlight when the mouse leaves.

To use the attribute directive:

```html
<p appHighlight>
  Hover over this paragraph to see the custom directive in action!
</p>
```

3. **Component Directives:**
Component directives are a type of custom directive that acts as standalone components with their own template and logic. They are used to encapsulate complex functionalities or UI elements and are reusable throughout the application. Component directives are created using the `@Component` decorator, similar to regular components, but they do not have their own selector.

Example of a component directive:

```typescript
import { Component } from '@angular/core';

@Component({
  template: '<p> This is a component directive.</p>'
})
export class MyComponentDirective { }
```

To use the component directive:

```html
<app-my-component-directive></app-my-component-directive>
```

Component directives provide a way to create reusable components that can be used as a single unit, containing both the template and the logic.

In summary, the three types of custom directives in Angular are:

1. Structural Directives: Modify the layout of the DOM based on conditions (e.g., `ngIf`, `ngFor`).
2. Attribute Directives: Change the appearance or behavior of elements by modifying their attributes (e.g., `appHighlight`).
3. Component Directives: Act as standalone components with their own template and logic, encapsulating complex functionalities (e.g., `MyComponentDirective`).

## Example of creating a custom attribute directive

Let's create a simple example to generate a new directive in Angular and use it in a template.

Step 1: Generate the Directive
Run the following Angular CLI command to generate a new directive named `myDirective`:
```bash
ng generate directive myDirective
```

This command will create a new directory called `my-directive` with the necessary files for the directive.

Step 2: Implement the Directive
Edit the `my-directive.directive.ts` file to define the behavior of the directive.

```typescript
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appMyDirective]'
})
export class MyDirectiveDirective {

  constructor(private el: ElementRef) {
    // Directives can access the element they are applied to using ElementRef
    this.el.nativeElement.style.backgroundColor = 'yellow';
    this.el.nativeElement.style.fontWeight = 'bold';
  }

}
```

In this example, we have created a simple directive that sets the background color to yellow and the font weight to bold for any element it's applied to.

Step 3: Use the Directive in a Template
To use the `appMyDirective` directive in a template, add it as an attribute to an HTML element.

app.component.html:
```html
<div appMyDirective>
  This text will have a yellow background and bold font weight.
</div>

<p appMyDirective>
  This paragraph will also have a yellow background and bold font weight.
</p>
```

Step 4: Add the Directive to the Module
Don't forget to add the `MyDirectiveDirective` to the declarations array of the module where you want to use the directive.

app.module.ts:
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { MyDirectiveDirective } from './my-directive/my-directive.directive';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent,
    MyDirectiveDirective // Add the directive to the declarations
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Step 5: Run the Application
Now, when you run the Angular application, the elements with the `appMyDirective` attribute will have a yellow background and bold font weight, as defined in the directive.

That's it! You have successfully generated a new directive in Angular and used it in a template. Directives are powerful tools in Angular that allow you to add custom behavior and styles to HTML elements, making your templates more dynamic and reusable.

## Components instantiated as attributes

In Angular, components can be instantiated using attributes in other components' templates by using square brackets `[ ]` in the component selector. This technique allows you to use a component as an attribute within another component's template. By doing so, you can apply the functionality and template of the child component to a specific element within the parent component's template.

Let's create a simple example to demonstrate how to use a component as an attribute in another component's template.

Step 1: Generate the Component
Run the following Angular CLI command to generate the component:

```bash
ng generate component shared-component
```

This will generate the `shared-component` component with its template, styles, and TypeScript file.

Step 2: Implement the Shared Component
Edit the `shared-component.component.ts` file to define the content of the shared component.

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: '[app-shared-component]', // Use square brackets in the selector
  template: `
    <div>
      <p>This is the Shared Component.</p>
      <p>Received message: {{ message }}</p>
    </div>
  `,
  styles: []
})
export class SharedComponentComponent {
  @Input() message: string;
}
```

In this example, we've set the selector of the `shared-component` as `[app-shared-component]`, which means this component can be used as an attribute in another component's template.

Step 3: Use the Shared Component as an Attribute
Now, let's use the `shared-component` as an attribute in another component's template.

Edit the `app.component.html` file to include the `shared-component` component as an attribute.

```html
<div>
  <h1>Welcome to My Angular App</h1>
  <div [app-shared-component] message="Hello from the Parent Component"></div>
</div>
```

Step 4: Add the Shared Component to the Module
Don't forget to add the `SharedComponentComponent` to the declarations array of the module where you want to use the `shared-component` component.

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { SharedComponentComponent } from './shared-component/shared-component.component'; // Import the SharedComponentComponent

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent,
    SharedComponentComponent // Add the SharedComponentComponent to the declarations
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Step 5: Run the Application
Now, when you run the Angular application, the `shared-component` will be used as an attribute in the `app.component.html` template, and its content will be displayed within the div element.

That's it! You have successfully instantiated a component using an attribute in another component's template by using the square brackets `[ ]` in the selector. This technique allows you to reuse the content and functionality of the shared component within different parts of your Angular application.
