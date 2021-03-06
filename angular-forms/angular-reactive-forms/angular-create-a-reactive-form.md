# Angular - Create A Reactive Form

**1\) Import ReactiveFormsModule into the app module \(app.module.ts\)**

```js
import { ReactiveFormModule } from '@angular/forms';
```

**2\) Import FormGroup into the component**

```js
import { FormGroup } from '@angular/forms';
```

**3\)** **Declare the root form**

```js
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;                      // <-- this property holds the reference to the form model
}
```

**4\)** **Define the data model**

```js
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         // <-- this property defines the data model
}
```

**5\)** Assign the customerForm property to an **new instance of the FormGroup**. Do this in the ngOnInit\(\) live cycle hook to ensure the component and template are initalized before building the form model.

```js
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         

    ngOnInit(): void {
        this.customerForm = new FormGroup({ });  // <-- creates the root FormGroup for the form model
    }
}
```

**6\)** **Import** **FormControl** to the component

```js
...
import { FormGroup, FormControl } from '@angular/forms';  // <-- 

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer(); 

    ngOnInit(): void {
        this.customerForm = new FormGroup({ });  
    }        
}
```

**7a\)** **Add the first FormControls \(in an object with key and value pairs\) to the FormGroup**

```js
...
import { FormGroup, FormControl } from '@angular/forms';  

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer(); 

    ngOnInit(): void {
        this.customerForm = new FormGroup({
            firstName: new FormControl(),       // <--
            lastName: new FormControl(),
            email: new FormControl(),
            sendCatalog: new FormControl(true)  // here a default value is passed in (optional)
        });
    }
}
```

**7b\)** **Use FormBuilder to shorten the boilerplate** of the FormGroup code. **FormBuilder creates a form model from a configuration**, like this \(compare it to the code example above\):

```js
import { FormBuilder, FormGroup } from '@angular/forms';   // <-- 1) import FormBuilder and delete FormControl
                                                           //        from import statement it is no longer
                                                           //        needed. That's done by FormBuilder.
...
export class CustomerComponent implements OnInit {
...
constructor(private fb: FormBuilder) { }           // <-- 2) inject FormBuilder via constructor parameter
...
    ngOnInit(): void {
        this.customerForm = this.fb.group({        // <-- 3) use FormBuilder instance. 
            firstName: '',
            lastName: '',
            email: '',
            sendCatalog: true
        });
    }
}
```

The `group()` method of the FormBuilder allows to define a set of controls in an **configuration object** and also nested form groups. **Each key in the configuration object is the FormGroup name** and each value defines the specified default value for the FormControl \(like above with "`sendCatalog: true`"\). It is associated with the root form group. Alternatively the value can be defined as an object \(like this:`firstName: {value: 'n/a', disabled: true}`\) with a boolean `disabled` property. **It is also possible to define it as an array** \(like this: `firstName: ['', ..., ...]`or this: `firstName: [{value: 'n/a', disabled: true}, ..., ...]`\) . In this case the last two elements of the array are for the **definition of the validation** rules.

**8\)** To be able to use the form in the entire part of the app module the **ReactiveFormsModule has to be imported into the app module**...\(already pointed out in step1\)

```js
/* app.module.ts */

...
import { FormsModule, ReactiveFormsModule } from '@angular/forms'; // only ReactiveFormsModule!!!!!
                                                                   // FormsModule was for 
                                                                   // template-driven forms
...
```

**9\)** ... **and added to the imports array**

```js
@NgModule({
  declarations: [
    AppComponent,
    CustomersComponent,
  ],
  imports: [
    BrowserModule,
    FormsModule,               // <-- the FormsModule was for template-driven forms
    ReactiveFormsModule        // <-- this here!!!
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**10\)** **Now in the template the Reactive Forms Directives can be used to bind the form to the form model.  
The Reactive Forms Directives are:**

* formGroup  
* formControl  
* formControlName  
* formGroupName  
* formArrayName

**11a\)** Use the **formGroup directive** to bind the `form` element to the **FormGroup instance property** defined in the component class

```js
<form (ngSubmit)="save()" formGroup>
      ...                                             
</form>
```

**11b\)** Use square brackets **to define property binding and bind to the customerForm property**

```js
<form (ngSubmit)="save()" [formGroup]="customerForm"> // customerForm is the form model property from the
      ...                                             // component class
</form>
```

**11c\)** Use the **formControlName directive** to bind each input element to its associated formControl. The will be bound to the name of the formControl instance as defined in the form model

```js
<form (ngSubmit)="save()" [formGroup]="customerForm"> 
      <fieldset>
            <div ...>
                  <label for="firstNameId">First Name</label>
                  <input type="text" 
                         formControlName="firstName"    // <-- binding to the property of the form model
                         id="firstNameId" 
                         placeholder="First Name (required)">
                   <span ...>
                        ...
                   </span>
            </div>
              ...
      </fieldset>                                            
</form>
```

**11d\)** The whole form could look like this

```js
<form (ngSubmit)="save()" 
      [formGroup]="customerForm"
      novalidate 
      autocomplete="off">

    <label for="firstNameId">First Name</label>
    <input type="text" 
           formControlName="firstName" 
           id="firstNameId" 
           placeholder="First Name (required)" 
           required>

    <label for="lastNameId">Last Name</label>
    <input type="text" 
           formControlName="lastName" 
           id="lastNameId" 
           placeholder="Last Name (required)" 
           required>

    <label for="emailId">Last Name</label>
    <input type="email" 
           formControlName="email" 
           id="emailId" 
           placeholder="Email (required)"
           pattern="[a-z0-9._%+-]+@[a-z0-9.-]+"
           required>

    <label>
       <input id="sendCatalogId"
              type="checkbox"
              formControlName="sendCatalog" >
       Send me your catalog
   </label>

   <span>
       <button type="submit"
           [disabled]="!customerForm.valid"> // use the customerForm property to check if the form is valid
           Save
       </button>
   </span>


</form>
```

**12a\)** **To access the Form Model Poperties** one option is to **navigate through the form model hierarchy** like this

```
customerForm.controls.firstName.valid
```

**12b\)** Or **alternatively** with the **get\(\) method of the FormGroup** - this is often shorter, especially with nested FormGroups

```
customerForm.get('firstName').valid
```



