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



