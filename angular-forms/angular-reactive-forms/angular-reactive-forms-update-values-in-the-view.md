# Angular Reactive Forms - Update Values in the View

Use `setValue` and `patchValue` for updating input elements on the form from the component class. Use **`setValue`** for updating all form controls like this:

```
this.customerForm.setValue({
    firstName: 'Jack',
    lastName: 'Harkness',
    email: 'jack@torchwood.com'
});
```

And use patchValue for updating only a subset of the form groups like here:

```
this.customerForm.setValue({
    firstName: 'Jack',
    lastName: 'Harkness'
});
```



