# Angular - Custom Validators

* [**First explanation**](#first-explanation)
* [**Build a custom validator**](#build-a-custom-validator)
* [**Custom Validator with Parameters**](#custom-validator-with-parameters)
* [**Cross-Field Validation**](#cross-field-validation)

### First explanation

In it's simplest form a custom validator is a function. The validator function always takes one parameter. The parameter is of type **AbstractControl** to allow passing in either FormGroup or FormControl.

```js
function myCustomValidator(control: AbstractControl) {    // <-- the parameter of type AbstractControl

}
```

The function returns an Object where the key is a string and the value is a boolean \(defining the broken rule\) or null if it is valid:

```js
function myCustomValidator(control: AbstractControl): {[key: string]: boolean} | null {

}
```

In the body of the validator function it will be checked if 'something is wrong' or null will be returned:

```js
function myCustomValidator(control: AbstractControl): {[key: string]: boolean} | null {
    if (somethingIsWrong) {
        return { 'myValidator': true };
    }
    return null;
}
```

The broken validation rule is then added to the passed in FormControl or FormGroup error collection.

### Build a custom validator

Assume there's a form in the template with an **input element** for that a custom validator needed, like here:

```js
...

<div [ngClass]="{'has-error': ( customerForm.get('rating').touched ||
                                customerForm.get('rating').dirty ) &&
                               !customerForm.get('rating').valid }">

  <label for="ratingId">Rating</label>
  <div>
    <input id="ratingId"
           type="number"
           formControlName="rating" />                     // <-- formControlName directive set to 'rating'
  </div>
</div>
```

Now **add that new FormControl to the form model** in the component class, like so:

```js
ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
        phone: '',
        notification: 'email',
        rating: '',                // <-- the new added FormControl 'rating'
        sendCatalog: true
    });
}
```

And then **create a custom numeric range validator** by adding the validator function above the component class \(if it only will be used by this component\) or in an **external file** \(e.g. called 'custom.validators.ts' with export of each validator\)':

```js
...
import {FormBuilder, FormGroup, Validators, AbstractControl} from '@angular/forms'
...

// the custom validator function
function ratingRange (control: AbstractControl): {[key: string]: boolean} | null {
  if (control.value != undefined && (isNaN(control.value) || control.value < 1 || control.value > 5)) {
    return { 'range': true };
  };
  return null
}

@Component({
...
```

The validator function in this case is placed above component class. It takes one parameter of type AbstractControl \(to allow either validation of a **FormControl** or a **FormGroup**\) and returns an **Object with a boolean value for 'range' of true.** **What means the the validation rule broke. **To do that it checks if the input

* is not equals undefined AND not a number 
* OR if it is less then 1 
* OR higher than 5 

If one of these tests pass the validation rule broke and it returns the Object `{ 'range': true }`.  With this the name of the validation rule name is then added to the validators error collection for the passed in FormControl. Otherwise **it returns null if it is valid.**

After the creation of the the custom validator it can be added to the FormControl in the form model in the component class. **Change the value of the FormControl from a single value to an array and pass in the name of the validator function as the second parameter**:

```js
ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
        phone: '',
        notification: 'email',
        rating: ['', ratingRange],                // <-- the new added FormControl 'rating'
        sendCatalog: true
    });
}
```

If there should be an error message in the template, one could do it like so:

```js
...

<div [ngClass]="{'has-error': ( customerForm.get('rating').touched ||
                                customerForm.get('rating').dirty ) &&
                               !customerForm.get('rating').valid }">

  <label for="ratingId">Rating</label>
  <div>
    <input id="ratingId"
           type="number"
           formControlName="rating" />                     
    <span *ngIf="( customerForm.get('rating').touched ||      // <-- implementation of the validation rule
                   customerForm.get('rating').dirty ) &&      // <-- with the validation rule name
                   customerForm.get('rating').errors">    
      <span *ngIf="customerForm.get('rating').errors.range">  // <-- here the validation rule name is used
        Please rate your experience grom 1 to 5.              // <-- and only displays if it breakes
      </span>
    </span>
  </div>
</div>
```

**Keep in mind:** A custom validator is a function. The hard part of writing a custom validation rule is to ensure the appropriate return value. `null` if a FormControl is valid and a key and value pair if it is invalid. Where the key is the name of the validation rule and the value is true to add it to the validation errors.

### Custom Validator with Parameters

Just adding more parameters to a custom validator is not possible because it takes only one. Thus it is a good way to write a more complex function, a factory function \(which can take any numbers and type of parameters\) that then returns a custom validator function.

```js
function myCustomValidator(param: any): ValidatorFn {
    return (control: AbstractControl): {[key: string]: boolean} | null => {
        if(somethingIsWrong) {
            return { 'myValidator': true };
        }
        return null;
    }
}
```

The above code shows the **returned validator function** is an arrow function which looks w/o types like this:

```js
(control) => {
    if(somethingIsWrong) {
        return { 'myValidator': true };
    }
    return null;
}
```

#### Now create the custom validation which takes parameters

First take the custom validator functin from the example above:

```js
...
import {FormBuilder, FormGroup, Validators, AbstractControl} from '@angular/forms'
...

// the custom validator function
function ratingRange (control: AbstractControl): {[key: string]: boolean} | null {
  if (control.value != undefined && (isNaN(control.value) || control.value < 1 || control.value > 5)) {
    return { 'range': true };
  };
  return null
}

@Component({
...
```

And wrap it in a factory function like this and add **ValidatorFn** to the import statement for the return type of the factory function wrapper. **Change the validator function into a returned arrow function** by removing the function keyword and the function name and adding the arrow just before the function body:

```js
...
import {FormBuilder, 
        FormGroup, 
        Validators, 
        AbstractControl, 
        ValidatorFn                 // <-- ValidatorFn needs to be imported for the right return type
        } from '@angular/forms'
...

// the factory function which takes two parameters (min and max acceptable value)
function ratingRange(min: number, max: number): ValidatorFn {

  // the custom validator function
  return (control: AbstractControl): {[key: string]: boolean} | null => {
    if (control.value != undefined && (isNaN(control.value) || control.value < 1 || control.value > 5)) {
      return { 'range': true };
    };
    return null
  },

}

@Component({
...
```

Now the logic of the validator function has to be modified to use the passed in parameters, like so:

```js
...
import {FormBuilder, 
        FormGroup, 
        Validators, 
        AbstractControl, 
        ValidatorFn                 
        } from '@angular/forms'
...

// the factory function which takes two parameters (min and max acceptable value)
function ratingRange(min: number, max: number): ValidatorFn {

  // the custom validator function
  return (control: AbstractControl): {[key: string]: boolean} | null => {
    if (control.value != undefined && 
                      (isNaN(control.value) || 
                      control.value < min ||      // <-- the former numbers are changed to the 
                      control.value > max)) {     // <-- passed in parameters (min and max)
      return { 'range': true };
    };
    return null
  },

}

@Component({
...
```

At least change the **FormControl validation to pass in the minimum and maximum values**:

```js
ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
        phone: '',
        notification: 'email',
        rating: ['', ratingRange(1, 5)],                // <-- passing in the min and max values
        sendCatalog: true
    });
}
```

### Cross-Field Validation

Comparison **across one or more FormControls** like e.g. start and end date in a form or comparing an **email address** with an **confirm email address** field. The trick to cross-field validation is to define a **nested FormGroup**, like so:

```js
this.customerForm = this.fb.group({
    firstName: ['', [Validators.required, Validators.minLength(3)]],
    lastName: ['', [Validators.required, Validators.maxLength(50)]],
    availability: this.fb.group({                                            // <-- the 
        start: ['', Validators.required],                                   // <-- nested
        end: ['', Validators.required]                                     // <-- FormGroup
    })
});
```

The will be defined in the component class an in the template then they will be validated together:

```
<div formGroupName="availability">
    ...
    <input formControlName="start" />
    ...
    <input formControlName="end" />
</div> 
```



