# Angular - Custom Validators

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

Assume there's a form in the template with an input element for that desired custom validator, like this:

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
    <span *ngIf="( customerForm.get('rating').touched ||
                   customerForm.get('rating').dirty ) &&
                   customerForm.get('rating').errors">
      <span *ngIf="customerForm.get('rating').errors.range">
        Please rate your experience grom 1 to 5.
      </span>
    </span>
  </div>
</div>
```

Now add that new FormControl to the form model in the component class, like here:

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

If one of these tests pass the validation rule broke and it returns the Object `{ 'range': true }`. Otherwise **it returns null if it is valid.**












