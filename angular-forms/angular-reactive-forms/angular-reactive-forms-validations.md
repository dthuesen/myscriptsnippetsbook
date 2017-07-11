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

For **built-in validators** the `Validators` class is used with one of its methods \(`.required`, `.maxLength`, `.minLength`, `.compose`, `.composeAsync`, `.email`, `.max`, `.min`, `.nullValidator`, `.pattern`, `.prototype`, `.requiredTrue`\)

When using **multiple validators** an array is used, like this:

```
this.customerForm = this.fb.group({
    firstName: ['', 
                [Validators.required, Validators.minLength(3)]],
    sendCatalog: true
})
```

The third element of the value array of the key value pairs is for any **asynchronous validators** \(e.g. calling a server side async validation method\). To minimize the async call **no async validator is called before all synchronous validators pass their validation.**

