# Angular Reactive Forms

First add the `formGroup` directive in the template:

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

Them add some FormControls as `formControlName` directives to the FormGroup:

```
<form [formGroup]="postForm" (ngSubmit)="addPost(postForm.value)">
    <label>Name
        <input type="text" formControlName="name">            // <--- FormControl directive
    </label>
    <label>Description
        <textarea formControlName="description"></textarea>   // <--- FormControl directive
    </label>

</form>
```

These FromGroupNames have equivalent properties in the component class, like so:

```
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms'

@Component({
  selector: 'app-post-form',
  templateUrl: './post-form.component.html',
  styleUrls: ['./post-form.component.css']
})
export class PostFormComponent {

  postForm: FormGroup;
  name = ''                       // <----- the name property of the form
  description = ''                // <----- and the description property of the form

  constructor(
    private fb: FormBuilder
  ) {

    this.postForm = fb.group({
      'name': ['', Validator.required],          // <--- The validation of the FormControls
      'description': ['', Validator.required],

    })

  }

  addPost(postFormValue) {      // <----- the method with the passed in value of the FormGroup
    // do something with the emitted value of the form...
  }
}
```

The above example of the validation of the FormControl has more possible validator methods possible, like here:

```
'description': [null, Validators.compose([Validators.required, 
                                          Validators.minLength(30), 
                                          Validators.maxLength(500)])],
```

These validators then will be put in an array in the `Validators.compose()` method call.

