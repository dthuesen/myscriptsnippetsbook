# Angular - Create A Reactive Form

1. Import FormGroup into the component

```
import { FormGroup } from '@angular/forms';
```

2. Declare the root form

```
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;                      // <-- this property holds the reference to the form model
}
```

3. Define the data model

```
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         // <-- this property defines the data model
}
```

Assign the custometForm property to an new instance of the FormGroup

```
...
import { FormGroup } from '@angular/forms';

... 
export class CustomerComponent impements OnInit {
    customerForm: FormGroup;             
    customer: Customer = new Customer();         // <-- this property defines the data model
}

ngOnInit(): void {
    this.customerForm = new FormGroup({ });
}
```



