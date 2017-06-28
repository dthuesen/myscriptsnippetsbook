# Angular Forms - Overview

Angular has two technical ways to handle forms: template driven forms with **`FormsModule`** and reactive forms with **`ReactiveFormsModule`. **Both are included in the `@angular/forms`library. They share a common set of **form control** classes.

They differ fundamentally by their programmatic approach. Each of them even has its own module: **`FormsModule`** and **`ReactiveFormsModule`**.

* **Template driven forms are asyncronous.**

* **Reactive forms are syncronous**

This difference matters.

#### Async vs. Sync

Reactive forms are synchronous. Template-driven forms are asynchronous.   
In **reactive forms** the whole form control tree will be generated in code \(in the component class\). That is fine because it makes it easy to immediately update a value or drill down through the descendants of the parent form because all controls are present in the class.

Whereas template-driven forms delegate the creation of their form controls to directives. But these directives take more than one cycle to build the control tree to avoid _"changed after checked"_ errors. The result is it takes a short moment until before any of these controls can be manipulated from within the component class.

One example to play through for testing this behavior: Inject the form control with a @ViewChild\(NgForm\) query and see what it logs out in the `ngAfterViewInit` lifecycle hook. It has no children. Waiting a tick \(maybe using `setTimeout`\) then it is possible to extract a value from a control, test its validity or set a new value to it.

For unit testing the asyncronous behavior of template driven form makes it more complicated. To solve it it is necessary to wrap the test block in `async()` or `fakeAsync()` to avoid expecting values that aren't there yet. That differs from reactive forms - everything is available as expected.



#### Template driven forms

With template driven forms HTML form controls \(like `<input>` or `<select>`\) will be bound to data model properties in the component class through directive like `ngModel`.

It is not necessary to create Angualr form control object. They will be created by Angular automatically in the background with the use of the information of the data bindings. Agular handles the push and pull of data values with ngModel. Angualr updates the data model with user input as soon as it happens. That's the reason why ngModel is not part of the ReactiveFormsModule.

Template driven forms means less code in the component class but more code in the template. **Template driven forms are asyncronous.**



