# Angular - Reactive Forms Code Example

## The HTML form

The starting point is an example with a basic form:

_**customer.component.html**_

```js
<form>
    <fieldset>
        <div>
            <label for="firstNameId">First Name</label>
            <input id="firstNameId" type="text"
                   placeholder="First Name (required)"
                   required
                   minLength="3" />
        </div>
        <button type="submit">Submit</button>
    </fieldset>
</form>
```

Transformed into a **reactive form** in the template it would look like this:

```js
<form (ngSubmit)="save()" [formGroup]="signupForm">
    <fieldset>
        <div [ngClass]="{'has-error' : formError.firstName}">
            <label for="firstNameId">First Name</label>
            <input id="firstNameId" type="text"
                   placeholder="First Name (required)"
                   formControlName="firstName" />
            <span *ngIf="formError.firstName">
                {{formError.firstName}}
            </span>
        </div>
        ...
        <button type="submit">Submit</button>
    </fieldset>
</form>
```

The form model is build in the component class, so no need for template reference variables. Reactive form do not use two way binding, so not `ngModel` or `name` attribute. And with reactive forms the validation is defined in the component class, so no validation attributes here either. The form element now binds the formGroup directive to the form model created in the class. Each form element is now bound to a form control instance in the form model \(`formControlName="firstName"`\).

