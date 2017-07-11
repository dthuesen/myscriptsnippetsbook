# Angular - Reactive Forms Validations

### Creating the FormControls

```js
this.customerForm = this.fb.group({
    firstName: '',
    sendCatalog: true
});
```

or like this with the value of the keys as an object:

```js
this.customerForm = this.fb.group({
    firstName: {value: 'n/a', disabled: true},
    sendCatalog: {value: true, disabled: false}
});
```

or the key-value pair with the **value defined as an array**:

```
this.customersForm = this.fb.group({
    firstName: [''],
    sendCatalog: [true]
});
```

The version with an array as the value is for implementing validation rules, like this:

```
this.customerForm = this.fb.group({
    firstName: ['', Validators.required],
    sendCatalog: true
})
```

For built-in validators the `Validators` class is used with one of its methods \(`.required`, `.maxLength`, `.minLength`, `.compose`, `.composeAsync`, `.email`, `.max`, `.min`, `.nullValidator`, `.pattern`, `.prototype`, `.requiredTrue`\)

When using more than one validator an array is used for the second argument in the array of the value, like this:

```
this.customerForm = this.fb.group({
    firstName: ['', 
                [Validators.required, Validators.minLength(3)]],
    sendCatalog: true
})
```



