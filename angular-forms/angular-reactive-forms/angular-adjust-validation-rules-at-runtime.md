# Angular - Adjust Validation Rules at Runtime

E.g. depending on a selection of the user the form changes its input fields, like so: the user can choose if he wants to recieve information via email or text. If he choses text he has to enter his phone number wheras if he decides to get it via email he has to enter his email address and for the two paths the form validation has to change during the input by the user.

For changing the validation at runtime the **setValidatiors\(\) method** will be called with one passed in **validator** or an **array of validators** passed in:

```js
myControl.setValidators((Validators.required);

// or

myControl.setValidators([Validators.required, Validators.maxLength(30)]);
```

All validators can also be removed with a call of **clearValidators\(\)**

```js
myControl.clearValidators();
```

**clearValidators\(\)** comes in handy if in some circumstances one want to add validators roules and under other conditions one want to remove them.

Updating the validation rules doesn't cause the validation status of the control to be re-evaluated. So if they get changed and one want **to re-evaluate** the control evaluation based on the new evaluation rules one calls the **updateValueAndValidity\(\) method **on that form as well.

```
myControl.updateValueAndValidity();
```

### Steps for implementation:

1\) Add the desired input elements to the template. In this case the behavior of the validation depends on a selection between two radio button input elements:

```
  <div>
      <input type="radio"
             value="email"
             formControlName="notification"
             (click)="setNotification('email')" >Email

      <input type="radio"
             value="text"
             formControlName="notification"
             (click)="setNotification('text')" >Text
  </div>
```

These form controls have the same name so the selection sets the value to this form control \("notification"\). The event binding with the click event calls a method and passes the value of what is clicked to the called method.

2\) In the component class add the method for setting the notification value:

```
setNotification(notifyVia: string): void {
    
}
```



