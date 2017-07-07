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

7\) To be able to use the form in the entire part of the app module the ReactiveFormsModule has to be imported into the app module...

```js
/* app.module.ts */

...
import { FormsModule, ReactiveFormsModule } from '@angular/forms'; // only ReactiveFormsModule!!!!!
                                                                   // FormsModule was for 
                                                                   // template-driven forms
...
```

8\) ... and added to the imports array

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



