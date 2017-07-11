# Angular Reactive Forms - Update Values in the View

Use `setValue` and `patchValue` for updating input elements on the form from the component class. Use `setValue` for updating **all** form controls like this:

```js
this.customerForm.setValue({
    firstName: 'Jack',
    lastName: 'Harkness',
    email: 'jack@torchwood.com'
});
```

And use patchValue for updating only a **subset** of the form groups like here:

```js
this.customerForm.setValue({
    firstName: 'Jack',
    lastName: 'Harkness'
});
```

To use it like above calling a method in the class via event binding could be one way...

... event binding in the form:

```js
<button class="btn btn-default"
        type="button"
        (click)="populateTestData()">
    Test Data
</button>
```

...and then in the class the method:

```js
populateTestData(): void {
    this.customerForm.patchValue({
        firstName: 'Jack',
        lastName: 'Harkness',
        sendCatalog: false
    });
}
```



