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



