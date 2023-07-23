# Directives

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

## Example of creating a custom directive

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
