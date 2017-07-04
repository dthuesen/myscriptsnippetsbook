# Angular - Create A Reactive Form

1\) **Import FormGroup into the component**

```
import { FormGroup } from '@angular/forms';
```

2\) **Declare the root form**

```
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;                      // <-- this property holds the reference to the form model
}
```

3\) **Define the data model**

```
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         // <-- this property defines the data model
}
```

4\) Assign the customerForm property to an **new instance of the FormGroup**. Do this in the ngOnInit\(\) live cycle hook to ensure the component and template are initalized before building the form model.

```
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         

    ngOnInit(): void {
        this.customerForm = new FormGroup({ });  // <-- creates the root FormGroup for the form model
    }
}
```

5\) **Import** **FormControl** to the component

```
...
import { FormGroup, FormControl } from '@angular/forms';  // <-- 

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer(); 

    ngOnInit(): void {
        this.customerForm = new FormGroup({ });  
    }        
}
```

6\) **Add the first FormControls \(in an object with key and value pairs\) to the FormGroup**

```
...
import { FormGroup, FormControl } from '@angular/forms';  

... 
export class CustomerComponent impements OnInit {
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



