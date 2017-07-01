# Angular Reactive Forms

First add a FormGroup in the template:

```
<form [formGroup]="postForm" (ngSubmit)="addPost(postForm.value)">
</form>
```

In reactive forms the FormGroup is bound to a property in the component class \(in this case "postForm"\) and the value of the FormGroup will be emitted via event binding using `(ngSubmit)` which calls the method `addPost()` of the component class.

In the component class add the property posts and the method addPost:

```
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms'

@Component({
  selector: 'app-post-form',
  templateUrl: './post-form.component.html',
  styleUrls: ['./post-form.component.css']
})
export class PostFormComponent {

  postForm: FormGroup;          // <----- the property of the form

  constructor(
    private fb: FormBuilder
  ) {

    this.postForm = fb.group({
      // the properties of the FormControls go here...
    })

  }

  addPost(postFormValue) {      // <----- the method with the passed in value of the FormGroup
    // do something with the emitted value of the form...
  }
}
```

Them add some FormControls to the FormGroup:

```
<form [formGroup]="postForm" (ngSubmit)="addPost(postForm.value)">
    <label>Name
        <input type="text" formControlName="name">
    </label>
    <label>Description
        <textarea type="text" formControlName="description"></textarea>
    </label>

</form>
```

These FromGroupNames have equivalent properties in the component class, like so:

```

```



