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

clearValidators\(\) comes in handy if in some circumstances one want to add validators roules and under other conditions one want to remove them.



