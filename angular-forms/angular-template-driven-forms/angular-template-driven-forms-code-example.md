# **Angular Template-Driven Forms Code Example**

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

Transformed into a **template-driven form** in the template it would look like this:

```js
<form>
    <fieldset>
        <div [ngClass]="{'has-error' : firstNameVar.touched && !firstNameVar.valid}">
            <label for="firstNameId">First Name</label>
            <input id="firstNameId" type="text"
                   placeholder="First Name (required)"
                   required
                   minLength="3" 
                   [(ngModel)]="customer.firstName"   // <-- binding the value of this input to a property
                   name="firstName"                   // <-- this attribute is required for associating
                                                      //     the FormControl with the FormGroup
                   #firstNameVar="ngModel" />         // <-- a reference variable to access the FormControl
                                                      //     instance
            <span *ngIf="firstNameVar.touched && firstNameVat.errors">
                Please enter your first name.
            </span>
        </div>
        ...
        <button type="submit">Submit</button>
    </fieldset>
</form>
```



