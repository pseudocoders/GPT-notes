# Forms and validation

## Template-driven forms and reactive forms 

Template-driven forms and reactive forms are two approaches to form handling in Angular. Here's a comparison of the two:

Template-driven Forms:
- Template-driven forms rely heavily on the template (HTML) to handle form validation and data binding.
- Form controls are automatically created and managed by Angular based on directives such as `ngModel`.
- Form validation is declarative and defined in the template using directives like `required`, `min`, `max`, etc.
- The form logic is handled within the template itself, making it simpler and quicker to implement for small to medium-sized forms.
- Template-driven forms are suitable for scenarios where the form logic is straightforward and tightly coupled to the UI.

Reactive Forms:
- Reactive forms focus on handling form validation and data binding programmatically within the component class.
- Form controls and their state are explicitly defined and managed by the developer in the component class.
- Form validation is implemented using validators provided by Angular, or custom validators can be created.
- The form logic is handled in a separate component class, providing more flexibility and control over form handling.
- Reactive forms are suitable for complex forms with dynamic validation requirements and scenarios where form logic needs to be reused across multiple components.

Key Differences:
- Control: Template-driven forms are more template-centric, while reactive forms are more code-centric.
- Control Over Validation: Reactive forms provide more control and flexibility over form validation by allowing custom validators and dynamic validation logic.
- Testing: Reactive forms are easier to test as the form logic is decoupled from the template.
- Data Model: Reactive forms provide a more explicit and centralized data model that reflects the form structure.
- Flexibility: Reactive forms provide more flexibility for implementing advanced features like conditional validation, dynamic form controls, and form arrays.

The choice between template-driven forms and reactive forms depends on the complexity and requirements of your form. Template-driven forms offer simplicity and ease of implementation for simple forms, while reactive forms provide more control and flexibility for complex forms with dynamic requirements.

## Reactive forms

### Built-in validators

In Angular's reactive forms, built-in validators are pre-defined functions that allow you to perform common form validations without having to write custom validation logic. These validators are provided by the `Validators` class from `@angular/forms` module. They can be used to validate form controls and apply validation rules to fields such as required, min length, max length, pattern matching, and more. Here are some examples of built-in validators:

1. `required`: Ensures that the form control has a non-empty value.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  name: ['', Validators.required]
});
```

2. `minLength`: Specifies the minimum length of the input value.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  password: ['', [Validators.minLength(6)]]
});
```

3. `maxLength`: Specifies the maximum length of the input value.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  name: ['', [Validators.maxLength(20)]]
});
```

4. `pattern`: Validates the input value against a regular expression pattern.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  email: ['', [Validators.pattern('^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}$')]]
});
```

5. `email`: Validates that the input value is a valid email address.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  email: ['', [Validators.email]]
});
```

You can combine multiple validators by passing an array of validator functions to the form control or form group.

```typescript
import { Validators } from '@angular/forms';

// Example usage with multiple validators
this.myForm = this.formBuilder.group({
  password: ['', [Validators.required, Validators.minLength(6)]],
  confirmPassword: ['', Validators.required]
});
```

Using these built-in validators simplifies the process of adding basic form validation rules to your reactive forms. However, if you have custom validation requirements, you can create your own custom validators by implementing the `ValidatorFn` interface. Custom validators give you full control over the validation logic for your specific use cases.

Here is a complete list of built-in validators available in Angular's reactive forms:

- `required`: Validates that the control has a non-empty value.
- `requiredTrue`: Validates that the control has a value of `true`. Typically used for checkbox inputs.
- `minLength`: Validates that the control value has a minimum length specified.
- `maxLength`: Validates that the control value has a maximum length specified.
- `pattern`: Validates that the control value matches a specific regular expression pattern.
- `email`: Validates that the control value is a valid email address.
- `min`: Validates that the control value is greater than or equal to a specified minimum value.
- `max`: Validates that the control value is less than or equal to a specified maximum value.
- `nullValidator`: Validates that the control value is `null`.
- `compose`: Combines multiple validators into a single validator function.

These validators are part of the `Validators` class provided by the `@angular/forms` module. They can be used individually or in combination to enforce specific validation rules on form controls.

By using these built-in validators, you can easily add common validation rules to your reactive forms without writing custom validation logic.


### Custom validators

In Angular's reactive forms, you can create custom validators to implement specific validation logic beyond the built-in validators. Custom validators allow you to define your own validation rules and apply them to form controls. Here's how you can create a custom validator:

1. Create a function that implements the `ValidatorFn` interface. This function will be responsible for performing the validation logic.

```typescript
import { AbstractControl, ValidatorFn } from '@angular/forms';

export function customValidator(): ValidatorFn {
  return (control: AbstractControl): { [key: string]: any } | null => {
    // Validation logic goes here
    const isValid = /* perform validation based on control value */;
    
    // Return validation error if invalid
    return isValid ? null : { customValidation: true };
  };
}
```

2. Use the custom validator function in your form controls by passing it as an argument to the `Validators` array.

```typescript
import { Validators } from '@angular/forms';
import { customValidator } from './custom-validator';

this.myForm = this.formBuilder.group({
  name: ['', [Validators.required, customValidator()]]
});
```

In the example above, the `customValidator` function is used to create a custom validator that checks whether the control value meets a specific validation criteria. If the validation fails, the function returns an error object with a key (`customValidation`) to indicate the validation failure.

You can also pass parameters to your custom validator function if needed. For example, if your validation logic requires a specific value or additional data, you can modify the `customValidator` function to accept parameters.

```typescript
export function customValidator(param: any): ValidatorFn {
  return (control: AbstractControl): { [key: string]: any } | null => {
    // Validation logic using the parameter
    const isValid = /* perform validation based on control value and param */;
    
    // Return validation error if invalid
    return isValid ? null : { customValidation: true };
  };
}
```

Then, when using the custom validator, you can pass the required parameter.

```typescript
this.myForm = this.formBuilder.group({
  name: ['', [Validators.required, customValidator('parameter')]]
});
```

Custom validators provide you with the flexibility to define and apply your own validation rules in reactive forms, allowing you to meet specific validation requirements for your application.

### Form condition checks

In Angular's reactive forms, you can perform condition checks on form controls and form groups to determine their state and validity. These condition checks allow you to display or hide specific elements or apply certain styles based on the form's state. Here are some common condition checks you can use:

1. `formControlName.invalid`: Checks if a specific form control is invalid. For example:

```html
<div *ngIf="myForm.get('name').invalid">
  <!-- Display error message or apply styles -->
</div>
```

2. `formControlName.valid`: Checks if a specific form control is valid.

```html
<div *ngIf="myForm.get('name').valid">
  <!-- Display success message or apply styles -->
</div>
```

3. `formControlName.dirty`: Checks if a specific form control has been modified (dirty).

```html
<div *ngIf="myForm.get('name').dirty">
  <!-- Apply styles or show an indicator -->
</div>
```

4. `formControlName.pristine`: Checks if a specific form control has not been modified (pristine).

```html
<div *ngIf="myForm.get('name').pristine">
  <!-- Apply styles or show an indicator -->
</div>
```

5. `formControlName.touched`: Checks if a specific form control has been touched (interacted with by the user).

```html
<div *ngIf="myForm.get('name').touched">
  <!-- Apply styles or show an indicator -->
</div>
```

6. `formControlName.untouched`: Checks if a specific form control has not been touched.

```html
<div *ngIf="myForm.get('name').untouched">
  <!-- Apply styles or show an indicator -->
</div>
```

These condition checks can be used within Angular's template syntax, such as `*ngIf` directives, to conditionally display elements or apply specific styles based on the form's state. By leveraging these condition checks, you can create dynamic and interactive forms that respond to user input and provide visual feedback based on the form's validity and state.


### Displaying validation errors dynamically


To dynamically display validation errors, you can leverage Angular's form control state and the `errors` property. Here's an example of how you can achieve this:

1. Define your form controls and apply the desired validators. For example, let's assume you have a form control for the name field:

```typescript
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

// Inside your component class
myForm: FormGroup;

constructor(private formBuilder: FormBuilder) {
  this.myForm = this.formBuilder.group({
    name: ['', Validators.required]
  });
}
```

2. In your component's template, bind the form control to an input field and display validation errors dynamically. You can use the `errors` property of the form control to access the validation errors.

```html
<form [formGroup]="myForm">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" formControlName="name">

    <div *ngIf="myForm.get('name').invalid && (myForm.get('name').dirty || myForm.get('name').touched)">
      <div *ngIf="myForm.get('name').errors.required">Name is required.</div>
    </div>
  </div>
</form>
```

In the above code, the `*ngIf` directives are used to conditionally display the error messages. The `myForm.get('name').invalid` condition checks if the form control is invalid, while `(myForm.get('name').dirty || myForm.get('name').touched)` ensures that the error is displayed only when the user has interacted with the field.

The nested `*ngIf` checks for the specific validation error. In this case, it checks for the `required` error and displays the corresponding error message.

As the user inputs data and triggers the form control's validation, Angular automatically updates the `errors` property, and the template reacts to those changes by displaying or hiding the validation error message dynamically.

By following this approach, you can display validation errors in real-time as the user interacts with the form, providing immediate feedback on the input validity.

