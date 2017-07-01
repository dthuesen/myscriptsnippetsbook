# Angular Forms - Overview

Angular has two technical ways to handle forms: [template driven forms](/angular-forms/angular-template-driven-forms.md) with `FormsModule` and [reactive forms](/angular-forms/angular-reactive-forms.md) with `ReactiveFormsModule`**. **Both are included in the `@angular/forms`library. They share a common set of **form control** classes.

They differ fundamentally by their programmatic approach. Each of them even has its own module: `FormsModule` and `ReactiveFormsModule`.

* **Template-driven forms are asyncronous.**

* **Reactive forms are syncronous**

This difference matters. Writing about this later.

### Building Block of forms in Angular

##### FormControl

There are input elements in a form and the elements have

* Value
* Validation Status
* User Interactions \(changed the value, touched the element, etc.\)
* Events

Code example:

```
const control = new FormControl(),   // create new instance
control.value                        // null
control.status                       // VALID
control.valid                        // true
control.pristine                     // true
control.untouched                    // true

control.setValue('Tom');
control.value                        // 'Tom'
control.reset(); 
control.value                        // null                    
control.disable();
control.status                       // DISABLED
```

##### FormGroup

A FormGroup can contain several FormControls e.g.

![](/assets/formgoup_wit_formcontrols.png)

A form in itself is a FormGroup and FormGroup is a class.

Code example:

```
const form = new FormGroup({
    street: new FormControl('', Validators.required),
    city: new FormControl('')
});

form.value                    // {street: '', city: ''}
form.status                   // INVALID

form.setValue({
    street: '123 Pine',
    city: 'San Francisco'
})
form.status                   // VALID
```

##### FormArray

### Async vs. Sync

[Reactive forms](/angular-forms/angular-reactive-forms.md) are synchronous. [Template-driven forms](/angular-forms/angular-template-driven-forms.md) are asynchronous.  
In [**reactive forms**](/angular-forms/angular-reactive-forms.md) the whole form control tree will be generated in code \(in the component class\). That is fine because it makes it easy to immediately update a value or drill down through the descendants of the parent form because all controls are present in the class.

Whereas [**template-driven** forms](/angular-forms/angular-template-driven-forms.md) delegate the creation of their form controls to directives. But these directives take more than one cycle to build the control tree to avoid _"changed after checked"_ errors. The result is it takes a short moment until before any of these controls can be manipulated from within the component class.

One example to play through for testing this behavior: Inject the form control with a @ViewChild\(NgForm\) query and see what it logs out in the `ngAfterViewInit` lifecycle hook. It has no children. Waiting a tick \(maybe using `setTimeout`\) then it is possible to extract a value from a control, test its validity or set a new value to it.

For unit testing the asyncronous behavior of template-driven form makes it more complicated. To solve it it is necessary to wrap the test block in `async()` or `fakeAsync()` to avoid expecting values that aren't there yet. That differs from reactive forms - everything is available as expected.

### Template-driven forms

With [template driven forms](#template-driven-forms) **HTML form controls** \(like `<input>` or `<select>`\) will be bound to data model properties in the component class through directive like `ngModel`.

It is not necessary to create Angualr form control object. They will be created by Angular automatically in the background with the use of the information of the data bindings. Agular handles the push and pull of data values with ngModel. Angualr updates the data model with user input as soon as it happens. That's the reason why **ngModel is not part of the ReactiveFormsModule**.

[Template driven forms](/angular-forms/angular-template-driven-forms.md) means less code in the component class but more code in the template. [**Template driven forms**](/angular-forms/angular-template-driven-forms.md)** are asyncronous.**

### Reactive forms

[Angular's reactive forms](/angular-forms/angular-reactive-forms.md) makes the way to reactive programming style easier for management of the **data flow **between **non-UI data model** \(typically retrieved from a server\) and **UI-oriented form model** that keeps the states and values of the **HTML form controls** from the view.

[Reactive forms ](/angular-forms/angular-reactive-forms.md)provide a ease of use for ractive patterns, testing and validation.

[Angular reactive forms](/angular-forms/angular-reactive-forms.md) create a tree of form control objects in the component class and binds them to native form control elements in the template. The form control objects will be manipulated directly in the component class.

Because the component class is able to access both data model and the form control structure directly, it can push data model values into the form controls and pull user input value back out of them. The component class observes changes in the form control state and reacts to them.

The advantage of working with form control objects directly is that their values and validity are updating synchronously and therefore are always under control. There are no timing issues which could encounter with template-driven forms \(they are async\). And, [reactive forms](/angular-forms/angular-reactive-forms.md) are easier to [unit test](/angular-testing/tdd-test-setup.md).

