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

## Built-in validators

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


