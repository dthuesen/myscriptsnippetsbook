# Angular - Reacting on Changes

* [**Implementation of valueChanges for a FormControl**](#implementation-of-valuechanges-for-a-formcontrol)\#
* [**Reacting to changes**](#reacting-to-changes)
  1. [**Currently the changes events in the form template are hard coded** as click event handlers, like here](#1-currently-the-changes-events-in-the-form-template-are-hard-coded-as-click-event-handlers-like-here)
  2. [Now **remove the click handlers in the template**](#2-now-remove-the-click-handlers-in-the-template)
  3. [**Set up a watcher **on the notification in the component class' ngOnOnit\(\) method like above and in the call back function call the already existing setNotification\(\) method and pass in the value](#3-set-up-a-watcher-on-the-notification-in-the-component-class-ngononit-method-like-above-and-in-the-call-back-function-call-the-already-existing-setnotification-method-and-pass-in-the-value)
  4. [Move the **validation messages from the HTML into the component class** and react by setting the appropriate message to display.](#4-move-the-validation-messages-from-the-html-into-the-component-class-and-react-by-setting-the-appropriate-message-to-display)

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

```
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
            <span *ngIf="( customerForm.get('emailGroup.email').touched ||           <---- moving 
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



