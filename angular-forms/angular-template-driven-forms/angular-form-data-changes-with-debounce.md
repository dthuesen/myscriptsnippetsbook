# Angular watch form data changes with debounce

Watching for changes in the template driven form and debouncing the event so that not every single letter forces an api call: 

**In the template:** 

```js
<form #customerForm="ngForm">
  <div class="row">
    <div class="col-md-4 col-xs-12">
      <nt-textinput [(ngModel)]="customer.companyName"
                    name="company">
    </div>
    <div class="col-md-4 col-xs-12">
      <nt-textinput [(ngModel)]="customer.lastName"
                    name="lastName"
                    required></nt-textinput>
    </div>
    <div class="col-md-4 col-xs-12">
      <nt-textinput [(ngModel)]="customer.firstName"
                    name="firstName"></nt-textinput>
    </div>
      ...
  </div>
</form>
```

In the class:

```js
import { AfterViewInit } from '@angular/core';
...

export class CustomerComponent {
    
    @ViewChild('customerForm') form;
    @Output() customerUpdated = new EventEmitter();
    ...
    
    constructor(
        ...
    ) {
        ...
    }
    
    ...
    
    ngAfterViewInit() {
        this.form.valueChanges
          .debounceTime(500)                             // <--
          .subscribe( (data) => {                        // <--
            this.customerUpdated.emit(data);
            console.log('data: ', data);
          });
      }

}
```



