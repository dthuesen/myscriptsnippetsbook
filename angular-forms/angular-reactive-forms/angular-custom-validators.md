# Angular - Custom Validators

* [**First explanation**](#first-explanation)
* [**Build a custom validator**](#build-a-custom-validator)
* [**Custom Validator with Parameters**](#custom-validator-with-parameters)
* [**Cross-Field Validation**](#cross-field-validation)
* [**Implementation of the Cross-Field Validation**](#implementation-of-the-cross-field-validation)
  * [1\) The starting point in the template - confirm email input field as FormGroup](#the-starting-point-in-the-template---confirm-email-input-field-as-formgroup)
  * [2\) Adding the FormControl in the forms model in the component class](#2-adding-the-formcontrol-in-the-forms-model-in-the-component-class)
  * [3\) Transforming the fields into a nested FormGroup \(needed for cross-field validation\)](#3-transforming-the-fields-into-a-nested-formgroup-needed-for-cross-field-validation)
  * [4\) Create the same nested FormGroup \(with FormGroupName and FormControlName directives\) in the template as well](#4-create-the-same-nested-formgroup-with-formgroupname-and-formcontrolname-directives-in-the-template-as-well)

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

```js
<div formGroupName="availability">        // <-- the name of the FormGroup like in the form model
    ...
    <input formControlName="start" />     // <-- the FormControl 'start'
    ...
    <input formControlName="end" />       // <-- and the FormControl 'end'
</div>
```

### Implementation of the Cross-Field Validation

#### 1\) The starting point in the template - confirm email input field as FormGroup

In this section an email and confirm email field will be added to the form. **First add the confirm email element and its FormControl \('confirmEmail'\) to the template:**

```js
<div [ngClass]="{ 'has-error': (customerForm.get('confirmEmail').touched ||
                                customerForm.get('confirmEmail').dirty) &&
                                !customerForm.get('confirmEmail').valid }">
  <label for="confirmEmailId">Confirm Email</label>

    <div>
      <input id="confirmEmailId"
              type="email"
              placeholder="Confirm Email (required)"
              formControlName="confirmEmail" />
        <span *ngIf="(customerForm.get('confirmEmail').touched ||
                      customerForm.get('confirmEmail').dirty) &&
                      customerForm.get('confirmEmail').errors">
          <span *ngIf="customerForm.get('confirmEmail').errors.required">
              Please confirm your email address.
          </span>
      </span>
    </div>
</div>
```

#### 2\) Adding the FormControl in the forms model in the component class

**Now add the FormControl for this input element to the form model:**

```js
ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
        confirmEmail: ['', [Validators.required]],   // <-- here - no pattern is needed for comparison
        phone: '',
        notification: 'email',
        rating: ['', ratingRange(1, 5)],
        sendCatalog: true
    });
}
```

The FormControl doesn't need a patter validation because it will be compared against the email FormControl and that already has a pattern validation.

#### 3\) Transforming the fields into a nested FormGroup \(needed for cross-field validation\)

**For making the cross-field validation create a nested FormGroup in the form model and name it 'emailGroup':**

```js
 ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        emailGroup: this.fb.group({                                           // <-- the nested FormGroup
            email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
            confirmEmail: ['', [Validators.required]],   // <-- here - no pattern is needed for comparison
        }),     
        phone: '',
        notification: 'email',
        rating: ['', ratingRange(1, 5)],
        sendCatalog: true
    });
}
```

#### 4\) Create the same nested FormGroup \(with FormGroupName and FormControlName directives\) in the template as well

Now group the template as well. First surround both elements with a `<div></div>` element and indent them. Then place the `formGroupName` directive and **set it equal to the name of the nested FormGroup and change each reference of the FormControls for **`email`** and **`confirmEmail`:

```js
<div formGroupName="emailGroup">                       // <-- the surrounded div for the nested FormGroup

    <!-- The email input -->
    <div [ngClass]="{ 'has-error': (customerForm.get('emailGroup.email').touched || // <-- 'email' becomes
                                    customerForm.get('emailGroup.email').dirty) &&  //<-- 'emailGroup.email'
                                    !customerForm.get('emailGroup.email').valid }"> // <-- like here
      <label for="emailId">Email</label>

        <div>
          <input id="emailId"
                 type="email"
                 placeholder="Email (required)"
                 formControlName="email" />
            <span *ngIf="(customerForm.get('emailGroup.email').touched ||    // <-- and here 
                          customerForm.get('emailGroup.email').dirty) &&       // <-- and here
                          customerForm.get('emailGroup.email').errors">          // <-- and here
              <span *ngIf="customerForm.get('emailGroup.email').errors.required">  // <-- and here
                  Please enter your email address.
              </span>
              <span *ngIf="customerForm.get('emailGroup.email').errors.pattern">   // <-- and here
                  Please enter a valid email address.
              </span>
          </span>
        </div>
    </div>
```

```js
    <!-- The confirm email input -->
    <div [ngClass]="{ 'has-error': 
                        (customerForm.get('emailGroup.confirmEmail').touched || // <-- and 
                        customerForm.get('emailGroup.confirmEmail').dirty) && //<-- confirmEmail
                        !customerForm.get('emailGroup.confirmEmail').valid }"> // <-- becomes
      <label for="confirmEmailId">Confirm Email</label>

        <div>
          <input id="confirmEmailId"
                  type="email"
                  placeholder="Confirm Email (required)"
                  formControlName="emailGroup.confirmEmail" />             // <-- emailGroup.confirmEmail
            <span *ngIf="(customerForm.get('emailGroup.confirmEmail').touched ||       // <-- like here... 
                          customerForm.get('emailGroup.confirmEmail').dirty) &&        // <-- and here... 
                          customerForm.get('emailGroup.confirmEmail').errors">         // <-- and here...
            <span *ngIf="customerForm.get('emailGroup.confirmEmail').errors.required"> // <-- and here.
                  Please confirm your email address.
              </span>
          </span>
        </div>
    </div>
</div>
```

#### 5\) Build the cross-field validator function

If the validtion does not require parameters there's no need for writing a factory function that returns a validator function - but now it's ok to build a simple validator function. Because the FormGroup gets validated it is passed into the validator function.

Just to keep it in memory a validator function is build like this:

```js
function dateCompare(control: AbstractControl): {[key: string]: boolean} | null {
    let startControl = control.get('start');
    let endControl = control.get('end');

    if (startControl.value !== endControl.value) {
        return { 'match': true };
    }
    return null
}
```

In this example first the **FormControls** \('start' and 'end'\) from the passed in **FormGroup** get accessed. Then the function compares the start date and the end date. If the values aren't equal it will return an error object with the key `'match'` and the value `true` which means the rule is broken an it will go into the **error collection for FormGroup** not the individual FormControls. If the validation rule passed `null` will be returned. This validator has to be added to the FormGroup of the form model, like here:

```js
/// 
//  the email comparison function (custom cross-field validator) above the component class
///
function emailMatcher(control: AbstractControl): {[key: string]: boolean} | null {
  const emailControl = control.get('email');
  const confirmControl = control.get('confirmEmail');

  if (emailControl.pristine || confirmControl.pristine) {
    return null;
  }

  if (emailControl.value === confirmControl.value) {
    return null
  }
  return { 'match': true };
}
...
.....

/// 
//  The component class
///
@Component({
......
...

/// 
//  The form model in the ngOnInit live cycle hook
///
ngOnInit(): void {
  this.customerForm = this.fb.group({
      firstName: ['', [Validators.required, Validators.minLength(3)]],
      lastName: ['', [Validators.required, Validators.maxLength(50)]],
      emailGroup: this.fb.group({                  // <-- the nested FormGroup
        email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
        confirmEmail: ['', [Validators.required]], // <-- here - no pattern is needed for comparison
      }, { validator: emailMatcher }),             // <-- implementation of the custom validator function
      phone: '',
      notification: 'email',
      rating: ['', ratingRange(1, 5)],
      sendCatalog: true
  });
}
```

Note that here it is not possible to add the validator function it self. The **FormGroup requires the provision of an object with a validator key and the function as the value **`{ validator: emailMatcher }`**.**

#### 6\) Correcting the display of the error messages of the cross-field validation

**Because the error if the cross-field validation rule breaks will be collected in the FormGroup error collection** this circumstance has to be taken in concern when writing the code to display error messages.

As shown above \(in step 4\) there are three `<span>` elements displaying the error messages \(they also may be `<div>` s\). One surounds the nested two and makes them showing up if certain conditions are true - if the element `confimEmail` is touched or dirty and if it has errors. The inner two of them are for showing either if there's an error because the email is `required` or if the entered emails don't `match`. Now there's need for checking the **nested FormGoup** named '`emailGroup`' with it's contained **FormControl** '`confirmEmail`', too. Because **nested FormGroups have their own error collection** the errors of their FormControls will be collected in the error collection of that group. So this has to be taken in concern when writing the code for that, like here:

```js
<div>
    <input id="confirmEmailId"
        type="email"
        placeholder="Confirm Email (required)"
        formControlName="emailGroup.confirmEmail" /> 

    <span *ngIf="(customerForm.get('emailGroup.confirmEmail').touched ||        
                  customerForm.get('emailGroup.confirmEmail').dirty) &&         
                  (customerForm.get('emailGroup.confirmEmail').errors ||
                  customerForm.get('emailGroup').errors) ">        //<-- checking nested FormGroup for errors

        <span *ngIf="customerForm.get('emailGroup.confirmEmail').errors?.required"> //<-- save navigation
            Please confirm your email address.                                      //    operator
        </span>

        <span *ngIf="customerForm.get('emailGroup').errors?.match">                 //<-- save navigation
            The confirmation does not match the email address.                      //    operator
        </span>

    </span>
</div>
```

When there's no error in one of these error types above it would return "`cannot read porperty required of null type of error`" for that empty property in the error collection. To prevent this it is necessary to use the **Save Navigation Operator** \(or Elvis Operator\) for both properties \(`.required` and `.match`\).

