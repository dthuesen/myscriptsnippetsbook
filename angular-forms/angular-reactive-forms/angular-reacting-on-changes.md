# Angular - Reacting on Changes

* [**Implementation of valueChanges for a FormControl**](#implementation-of-valuechanges-for-a-formcontrol)\#
* [**Reacting to changes**](#reacting-to-changes)
  1. [**Currently the changes events in the form template are hard coded** as click event handlers, like here](#1-currently-the-changes-events-in-the-form-template-are-hard-coded-as-click-event-handlers-like-here)
  2. [Now **remove the click handlers in the template**](#2-now-remove-the-click-handlers-in-the-template)
  3. [**Set up a watcher **on the notification in the component class' `ngOnOnit()` method like above and in the call back function call the already existing `setNotification()` method and pass in the value](#3-set-up-a-watcher-on-the-notification-in-the-component-class-ngononit-method-like-above-and-in-the-call-back-function-call-the-already-existing-setnotification-method-and-pass-in-the-value)
  4. [Move the **validation messages from the HTML into the component class** and react by setting the appropriate message to display.](#4-move-the-validation-messages-from-the-html-into-the-component-class-and-react-by-setting-the-appropriate-message-to-display)
     * [a\) Set up a data structure for keeping the validation messages](#a-set-up-a-data-structure-for-keeping-the-validation-messages)
     * [b\) Define a property that will contain the validation message to display.](#b-define-a-property-that-will-contain-the-validation-message-to-display)
     * [c\) Add a watcher on the FormControl](#c-add-a-watcher-on-the-formcontrol)
     * [d\) Add the new `setMessage()`method to the class](#d-add-the-new-setmessage-method-to-the-class)
     * [e\) Remove the validation error messages from the template by Reactive Transformations](#e-remove-the-validation-error-messages-from-the-template-by-reactive-transformations) 
     * [f\) Implement `debounceTime()` Reactive extensions operator for better user experience](#f-implement-debouncetime-reactive-extensions-operator-for-better-user-experience)

Forms reacting on user changes dynamically.

The property **valueChanges** of FormControl and FormGroup **emits events on value changes**. It's an `Observable<any>`and collects the events asynchronously. The  property **statusChanges** emits events on validation changes.

To watch the **events for a single FormControl,** we subscribe to its valueChanges observable, like so:

```js
this.myFormControl.valueChanges.subscribe( value => console.log(value) );
```

Watching for events from **any FormControl within a nested FormGroup** is possible **by subscribing to the FormGroups valueChanges observable** - each time any of the FormControls or other nested FormGroups within this FromGoup are changing, an event is emitted and the FormGroups value is provided, like here:

```js
this.myFormGroup.valueChanges.subscribe( value => console.log(JSON.stringify(value)) );
```

The value of the above emitted observable is **a set of key and value pairs for all of the FormControls and nested FormGroups. **Therefore JSON.stringify\(\) for displaying the value.

Watching for **any change** over the **entire form and any FormControl on that form** can be made by subscribing to the valueChanges property for the **form's root FormGroup.**

```js
this.customerForm.valueChanges.subscribe( value => console.log(JSON.stringify(value)) );
```

### Implementation of valueChanges for a FormControl

To start watching as soon as the component is initialized the code has to be placed in the ngOnInit\(\) method. First subscribe to the FormControl which shoud be watched by adding the .get\(\) method to the root FormGroup and call it with the name of the FormControl \(in this case 'notification'\). The valueChanges property then will be subscribed to getting its observable events submitted \(here every time either radio buttons changes \). When a change occurs the value of the notification FormControl there's a value available that can be logged out or whatever.:

```js
ngOnInit(): void {

    ... // form model

    this.customerForm.get('notification').valueChanges.subscribe( value => console.log(value) );

}
```

Keep in mind this code must be after the definition fo the root FormGroup, otherwise this reference is null.

### Reacting to changes

As the user makes changes to the form, one can react to those changes to provide a more dynamic an customized experience - e.g.

* adjust the validation rules, 
* handle validation messages in the component class \(instead of  hardcoded in the template\), 
* adjust the user interface elements, 
* add or remove content as needed, 
* provide automatic suggestions as the user types
* or anything the imagination allows or the customers requirements.

#### 1\) Currently the changes **events in the form template are hard coded as click event handlers**, like here:

```js
<div>
  <label>Send Notifications</label>
  <div>
    <label class="radio-inline">
      <input type="radio"
             value="email"
             formControlName="notification"
             (click)="setNotification('email')" >Email   //<-- currently with click event handlers
    </label>
    <label class="radio-inline">
      <input type="radio"
             value="text"
             formControlName="notification"
             (click)="setNotification('text')" >Text     //<-- here too
    </label>
  </div>
</div>
```

#### 2\) Now remove the click handlers **in the template:**

```js
<div>
  <label>Send Notifications</label>
  <div>
    <label class="radio-inline">
      <input type="radio"
             value="email"
             formControlName="notification">Email
    </label>
    <label class="radio-inline">
      <input type="radio"
             value="text"
             formControlName="notification">Text
    </label>
  </div>
</div>
```

#### 3\) **Set up a watcher** on the notification in the component class' `ngOnOnit()` method like above and **in the call back function **call the already existing `setNotification()` method and pass in the value:

```js
ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        emailGroup: this.fb.group({ // <-- the nested FormGroup
          email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
          confirmEmail: ['', [Validators.required]], // <-- here - no pattern is needed for comparison
        }, { validator: emailMatcher }),
        phone: '',
        notification: 'email',
        rating: ['', ratingRange(1, 5)],
        sendCatalog: true
    });

    //////
    // the watcher:
    //////
    this.customerForm.get('notification').valueChanges.subscribe( value => this.setNotification(value) );
}
```

The already existing setNotification method:

```js
setNotification(notifyVia: string): void {
  const phoneControl = this.customerForm.get('phone');
  if (notifyVia === 'text') {
    phoneControl.setValidators(Validators.required);
  } else {
    phoneControl.clearValidators();
  }
  phoneControl.updateValueAndValidity();
}
```

Now if the user clicks in the form on the radio 'phone', the watcher calls the setNotification method with the value "phone" and if he clicks on 'text', the submitted value is of cause "text".

#### 4\) Move the validation messages from the HTML into the component class and react by setting the appropriate message to display.

The currently hard coded error / validation messages in the HTML:

```js
<div formGroupName="emailGroup"
[ngClass]="{ 'has-error': customerForm.get('emailGroup').errors }">

    <!-- The email input -->
    <div [ngClass]="{ 'has-error': (customerForm.get('emailGroup.email').touched ||
                                    customerForm.get('emailGroup.email').dirty) &&
                                   !customerForm.get('emailGroup.email').valid }">
        <label for="emailId">Email</label>

        <div>
            <input id="emailId"
                   type="email"
                   placeholder="Email (required)"
                   formControlName="email" />

           <!-- the validation messages block -->
            <span *ngIf="( customerForm.get('emailGroup.email').touched ||           <---- move 
                           customerForm.get('emailGroup.email').dirty ) &&           <---- these
                           customerForm.get('emailGroup.email').errors">             <---- into

                <span *ngIf="customerForm.get('emailGroup.email').errors.required">  <---- the 
                    Please enter your email address.
                </span>
                <span *ngIf="customerForm.get('emailGroup.email').errors.pattern">   <---- component classs
                    Please enter a valid email address.
                </span>
            </span>
        </div>
</div>
```

##### a\) Set up a data structure for keeping the validation messages

In the component class set up a data structure for keeping the **validation messages** as key and value pair, **where the key is the validation rule and the value is the message to display.**

##### **b\) Define a property that will contain the validation message to display.**

```js
@Component({
  // tslint:disable-next-line:component-selector
  selector: 'my-signup',
  templateUrl: './customers.component.html',
})
export class CustomersComponent implements OnInit {

  customerForm: FormGroup;                  
  customer: Customer= new Customer();
  emailMessage: string;                              // <-- b) property containing the message to display

  private validationMessages = {                     // <-- a) the new data structure for the validation 
    required: 'Please enter your email address.',    // <-- messages set up as an object
    pattern: 'Please enter a valid email address.'   // <-- 
  }

  constructor(private fb: FormBuilder) { }

  ...
```

Here the messages are also hard coded but they can also be populated by an Angular service that retrieves them from a file or back-end server.

##### c\) Add a watcher on the FormControl

To let this watcher start watching right away, add the code to the ngOnInit\(\) method

```js
@Component({
  // tslint:disable-next-line:component-selector
  selector: 'my-signup',
  templateUrl: './customers.component.html',
})
export class CustomersComponent implements OnInit {

  customerForm: FormGroup;                  
  customer: Customer= new Customer();
  emailMessage: string;                              

  private validationMessages = {                      
    required: 'Please enter your email address.',    
    pattern: 'Please enter a valid email address.'    
  }

  constructor(private fb: FormBuilder) { }

  ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        emailGroup: this.fb.group({ 
          email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
          confirmEmail: ['', [Validators.required]],
        }, { validator: emailMatcher }),
        phone: '',
        notification: 'email',
        rating: ['', ratingRange(1, 5)],
        sendCatalog: true
    });

    this.customerForm.get('notification').valueChanges.subscribe( value => this.setNotification(value) );

    ////
    // The watcher for the FormControl 'emailGroup.email'
    ////
    const emailControl = this.customerForm.get('emailGroup.email');                 // <---
    emailControl.valueChanges.subscribe( value => this.setMessage(emailControl) );  // <---
  }

  ...
```

First the FormControl is stored in a constant 'emailControl' for minimizing repeated code. Then the watcher is subscribed to it. In the callback function the `setMessage()` method is called and the FormControl passed in. And so every time the value is changed, the validation messages get reevaluated. The `setMessage()` method will determine the appropriate validation message to display, if any.

##### d\) Add the new `setMessage()` method to the class

```js
@Component({
  // tslint:disable-next-line:component-selector
  selector: 'my-signup',
  templateUrl: './customers.component.html',
})
export class CustomersComponent implements OnInit {

  customerForm: FormGroup;                  
  customer: Customer= new Customer();
  emailMessage: string;                              

  private validationMessages = {                      
    required: 'Please enter your email address.',    
    pattern: 'Please enter a valid email address.'    
  }

  constructor(private fb: FormBuilder) { }

  ngOnInit(): void {
    this.customerForm = this.fb.group({
      firstName: ['', [Validators.required, Validators.minLength(3)]],
      lastName: ['', [Validators.required, Validators.maxLength(50)]],
      emailGroup: this.fb.group({ 
        email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
        confirmEmail: ['', [Validators.required]], 
      }, { validator: emailMatcher }),
      phone: '',
      notification: 'email',
      rating: ['', ratingRange(1, 5)],
      sendCatalog: true
    });

    this.customerForm.get('notification').valueChanges.subscribe( value => this.setNotification(value) );

    const emailControl = this.customerForm.get('emailGroup.email'); 
    emailControl.valueChanges.subscribe( value => this.setMessage(emailControl) );
  }
```

```js
  populateTestData(): void {
    this.customerForm.patchValue({
        firstName: 'Jack',
        lastName: 'Harkness',
        sendCatalog: false
    });
  }

  save() {
    console.log(this.customerForm);
    console.log('Saved: ' + JSON.stringify(this.customerForm.value));
  }

  ////
  // the new method for setting the validation messages
  ////
  setMessage(control: AbstractControl): void {                        // <-- from here
    this.emailMessage = '';

    if ( (control.touched || control.dirty ) && control.errors ) {
        this.emailMessage = Object.keys(control.errors).map( key => this.validationMessages[key] ).join(' ');
    }
  }                                                                  // <-- to here

  setNotification(notifyVia: string): void {
    const phoneControl = this.customerForm.get('phone');
    if (notifyVia === 'text') {
      phoneControl.setValidators(Validators.required);
    } else {
      phoneControl.clearValidators();
    }
    phoneControl.updateValueAndValidity();
  }

}
```

The method `setMessage(control: AbdtractControl): void` takes in a **FormControl** or **FormGroup** so it is set to type if **AbstractControl**. In the method body any current message will be cleared by an empty string. It has to be passed because the most recent change to that element could could cause all validation rules pass and that would show any left over messages. The next thing is the if statement to determine wheter a validation message should be displayed or not. **All of the dirty, touched or valid checking out of the tempate moves here into the component class.**

In the method above in the body of the if statement the JavaScript `Object.keys()` is used to return **an array of of the validation errors collection keys.** The errors collection uses the validation rule name as the key. Since the validation messages data structure of this app also uses the **validation rule as the key**, it can directly map into the app's `validationMessages[key]` data structure. That way this can handle display of multiple messages, if necessary.

##### e\) Remove the validation error messages from the template by Reactive Transformations

```js
<div class="form-group"
   [ngClass]="{ 'has-error': // REMOVE --> (customerForm.get('emailGroup.email').touched  || //<-- REMOVE
                             // REMOVE --> customerForm.get('emailGroup.email').dirty) &&    //<-- REMOVE
                             // REMOVE --> !customerForm.get('emailGroup.email').valid }">   //<-- REMOVE

<label class="col-md-2 control-label"
      for="emailId">Email</label>

  <div class="col-md-8">
    <input class="form-control"
            id="emailId"
            type="email"
            placeholder="Email (required)"
            formControlName="email" />
      <div *ngIf="emailMessage" class="help-block">
        {{emailMessage}}
      </div>
      <!-- extract validatin messages to the component class -->
      <!--
      <span class="help-block" *ngIf="( // REMOVE --> customerForm.get('emailGroup.email').touched || //<--RE
                                        // REMOVE --> customerForm.get('emailGroup.email').dirty ) && //<--MO
                                        // REMOVE --> customerForm.get('emailGroup.email').errors">   //<--VE

       // REMOVE -->  <span *ngIf="customerForm.get('emailGroup.email').errors.required">  //<--
       // REMOVE -->      Please enter your email address.                                 //<--
       // REMOVE -->  </span>                                                              //<-- REMOVE
       // REMOVE -->  <span *ngIf="customerForm.get('emailGroup.email').errors.pattern">   //<--
       // REMOVE -->      Please enter a valid email address.                              //<--
       // REMOVE -->  </span>
    </span>
      -->
  </div>
</div>
```

And change it so, that it look like this:

```
<div class="form-group"
     [ngClass]="{ 'has-error': emailMessage }">
  <label class="col-md-2 control-label"
        for="emailId">Email</label>

    <div class="col-md-8">
      <input class="form-control"
              id="emailId"
              type="email"
              placeholder="Email (required)"
              formControlName="email" />
        <span class="help-block" *ngIf="emailMessage">
          {{ emailMessage }}
        </span>
    </div>
</div>
```

##### f\) Implement the `debounceTime()` Reactive extensions operator for better user experience

Observables provide operators that allow to transform how emitted events will be seen. By the way there are many observable operators that di everything from filtering, to mapping, to throttling, etc. One operator is **debounceTime**.

**DebounceTime** ignores all events until a specific time has passed without another event. For example, `debounceTime(1000)` **waits for 1 second with no events before emitting another event.** This is very useful for validation, **especially if one don't want to show the validation messages until the user has stopped typing. DebounceTime** is one of the most commsuonly used reactive trasformation operators when working with validation, but there are many others.

**ThrottleTime** is another one. It emits a value, then ignores subsequent values for a specific amount of time. Thisci is useful when receiving way too many events, as when tracking mouse movements.

**DistinctUntilChange** suppresses duplicate consecutive \(repeating\) items. This is useful when tracking key events to prevent getting events when only the Ctrl or Shift keys are changed.

Now start implementing: First import the **debounceTime Reactive extensions operator** into the component class:

```js
...

import 'rxjs/add/operator/debounceTime';

...
```

Then call the `debounceTime()`** operator** on the observable and specify the desired wait time:

```js
ngOnInit(): void {
    this.customerForm = this.fb.group({
        firstName: ['', [Validators.required, Validators.minLength(3)]],
        lastName: ['', [Validators.required, Validators.maxLength(50)]],
        emailGroup: this.fb.group({
            email: ['', [Validators.required, Validators.pattern('[a-z0-9._%+-]+@[a-z0-9.-]+.[a-z]+')]],
            confirmEmail: ['', [Validators.required]],
        }, { validator: emailMatcher }),
        phone: '',
        notification: 'email',
        rating: ['', ratingRange(1, 5)],
        sendCatalog: true
    });

    this.customerForm.get('notification').valueChanges           // <-- the observable
            .debounceTime(1000)                                  // <-- call the debounceTime operator
            .subscribe( value => this.setNotification(value) );  // <-- subscribing to the observable

    const emailControl = this.customerForm.get('emailGroup.email');
    emailControl.valueChanges.subscribe( value => this.setMessage(emailControl) );
}
```

That's it. Now the email validation should not display until the user had a chance to enter a value. Maybe the time value for the `debounceTime()` **Reactive extensions operator** could be a little longer for people not typing so fast.

#### Recap:

* Use the valueChanges\(\) Observable property
* Subscribe to the Observable by calling the subscribe method, like so:
  ```js
  this.myFormControl.valueChanges.subscribe( value => console.log(value));
  ```

  _This provides notifications each time the value of myFormControl changes_
* Write the code to react on the user changes in the subscribe function
  ```js
  this.myFormControl.valueChanges.subscribe( value => this.setNotification(value) );
  ```







