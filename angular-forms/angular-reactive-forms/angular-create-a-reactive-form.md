# Angular - Create A Reactive Form

1\) **Import FormGroup into the component**

```js
import { FormGroup } from '@angular/forms';
```

2\) **Declare the root form**

```js
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;                      // <-- this property holds the reference to the form model
}
```

3\) **Define the data model**

```js
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent implements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         // <-- this property defines the data model
}
```

4\) Assign the customerForm property to an **new instance of the FormGroup**. Do this in the ngOnInit\(\) live cycle hook to ensure the component and template are initalized before building the form model.

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

5\) **Import** **FormControl** to the component

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

6\) **Add the first FormControls \(in an object with key and value pairs\) to the FormGroup**

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

7\) To be able to use the form in the entire part of the app module the **ReactiveFormsModule has to be imported into the app module**...

```js
/* app.module.ts */

...
import { FormsModule, ReactiveFormsModule } from '@angular/forms'; // only ReactiveFormsModule!!!!!
                                                                   // FormsModule was for 
                                                                   // template-driven forms
...
```

8\) ... **and added to the imports array**

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

9\) Now in the template the Reactive Forms Directives can be used to bind the form to the form model.  
The Reactive Forms Directives are:

* formGroup  
* formControl  
* formControlName  
* formGroupName  
* formArrayName

9a\) Use the **formGroup directive** to bind the `form` element to the **FormGroup instance property** defined in the component class

```js
<form (ngSubmit)="save()" formGroup>
      ...                                             
</form>
```

9b\) Use square brackets **to define property binding and bind to the customerForm property**

```js
<form (ngSubmit)="save()" [formGroup]="customerForm"> // customerForm is the form model property from the
      ...                                             // component class
</form>
```

9c\) Use the formControlName directive to bind each input element to its associated formControl. The will be bound to the name of the formControl instance as defined in the form model

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

9c\) The whole form could look like this

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
               [disabled]="!customerForm.valid">
           Save
       </button>
   </span>


</form>
```

10-a\) To access the Form Model Poperties one option is to navigate through the form model hierarchy like this

```
customerForm.controls.firstName.valid
```

10-b\) Or alternatively with the get\(\) method of the FormGroup - this is often shorter, especially with nested FormGroups

```
customerForm.get('firstName').valid
```



