# Second Code Example - Angular Template-Driven Forms

An example of a large signup form which expands after the first field are filled out valid.

* [**The Template**](#the-template)
* [**The component class**](#the-component-class)
* [**The data model class:**](#and-the-data-model-class)

Tipp about the usage of **NgForm** and **NgModel** directives: [https://angular.io/api/forms/NgForm](https://angular.io/api/forms/NgForm)

### The Template:

```js
    <div>
    <div>
        Sign Up!
    </div>

    <div>
        <form novalidate
              (ngSubmit)="save(signupForm)"  // <-- 2.) using the exported data
              #signupForm="ngForm">          // <-- 1.) exporting the ngForm directive to access its data
            <fieldset>
                <div [ngClass]="{'has-error': (firstNameVar.touched || 
                                 firstNameVar.dirty) && !firstNameVar.valid }">
                    <label for="firstNameId">First Name</label>
```

```js
                    <div>
                        <input id="firstNameId" 
                               type="text" 
                               placeholder="First Name (required)" 
                               required 
                               minlength="3"
                               [(ngModel)]=customer.firstName
                               name="firstName"
                               #firstNameVar="ngModel" />
                        <span *ngIf="(firstNameVar.touched || firstNameVar.dirty) && firstNameVar.errors">
                            <span *ngIf="firstNameVar.errors.required">
                                Please enter your first name.
                            </span>
                            <span *ngIf="firstNameVar.errors.minlength">
                                The first name must be longer than 3 characters.
                            </span>
                        </span>
                    </div>
                </div>
```

```js
            <div [ngClass]="{'has-error': (lastNameVar.touched || 
                 lastNameVar.dirty) && !lastNameVar.valid }">
                    <label for="lastNameId">Last Name</label>

                    <div>
                        <input id="lastNameId" 
                               type="text" 
                               placeholder="Last Name (required)" 
                               required 
                               maxlength="50"
                               [(ngModel)]="customer.lastName"
                               name="lastName"
                               #lastNameVar="ngModel" />
                        <span *ngIf="(lastNameVar.touched || lastNameVar.dirty) && lastNameVar.errors">
                            <span *ngIf="lastNameVar.errors.required">
                                Please enter your last name.
                            </span>
                        </span>
                    </div>
                </div>
```

```js
                <div [ngClass]="{'has-error': (emailVar.touched || emailVar.dirty) && !emailVar.valid }">
                    <label for="emailId">Email</label>

                    <div>
                        <input id="emailId" 
                               type="email" 
                               placeholder="Email (required)" 
                               required
                               pattern="[a-z0-9._%+-]+@[a-z0-9.-]+"
                               [(ngModel)]="customer.email"
                               name="email"
                               #emailVar="ngModel" />
                        <span *ngIf="(emailVar.touched || emailVar.dirty) && emailVar.errors">
                            <span *ngIf="emailVar.errors.required">
                                Please enter your email address.
                            </span>
                            <span *ngIf="emailVar.errors.pattern">
                                Please enter a valid email address.
                            </span>

                            <!-- This one does not work (because Angular Forms does not yet support -->
                            <!-- the validators for HTML input types such as email, phone and date) -->
                            <span *ngIf="emailVar.errors.email">
                                Please enter a valid email address.
                            </span>
                        </span>
                    </div>
                </div>

                <div>
                    <label>
                        <input id="sendCatalogId"
                               type="checkbox"
                               [(ngModel)]="customer.sendCatalog"
                               name="sendCatalog" >
                        Send me your catalog
                    </label>
                </div>
```

```js
             <div *ngIf="customer.sendCatalog">
                    <div>
                        <label>Address Type</label>
                        <div>
                            <label>
                                <input type="radio" id="addressType1Id" value="home"
                                       [(ngModel)]="customer.addressType"
                                       name="addressType">Home
                            </label>
                            <label>
                                <input type="radio" id="addressType1Id" value="work"
                                       [(ngModel)]="customer.addressType"
                                       name="addressType">Work
                            </label>
                            <label>
                                <input type="radio" id="addressType1Id" value="other"
                                       [(ngModel)]="customer.addressType"
                                       name="addressType">Other
                            </label>
                        </div>
                    </div>

                    <div>
                        <label for="street1Id">Street Address 1</label>
                        <div>
                            <input type="text" 
                                   id="street1Id" 
                                   placeholder="Street address"
                                   [(ngModel)]="customer.street1"
                                   name="street1">
                        </div>
                    </div>

                    <div>
                        <label for="street2Id">Street Address 2</label>
                        <div>
                            <input type="text"  
                                   id="street2Id"
                                   placeholder="Street address (second line)"
                                   [(ngModel)]="customer.street2"
                                   name="street2">
                        </div>
                    </div>
```

```js
                    <div>
                        <label for="cityId">City, State, Zip Code</label>
                        <div>
                            <input type="text" 
                                   id="cityId" 
                                   placeholder="City"
                                   [(ngModel)]="customer.city"
                                   name="city">
                        </div>
                        <div>
                            <select id="stateId"
                                    [(ngModel)]="customer.state"
                                    name="state">
                                <option value="" disabled selected hidden>Select a State...</option>
                                <option value="AL">Alabama</option>
                                <option value="AK">Alaska</option>
                                <option value="AZ">Arizona</option>
                                <option value="AR">Arkansas</option>
                                <option value="CA">California</option>
                                <option value="CO">Colorado</option>
                                <option value="WI">Wisconsin</option>
                                <option value="WY">Wyoming</option>
                            </select>
                        </div>
                        <div>
                        <input type="number"
                                   id="zipId" 
                                   placeholder="Zip Code"
                                   [(ngModel)]="customer.zip"
                                   name="zip">
                        </div>
                    </div>
                </div>

                <div>
                    <div>
                        <span>
                            <button type="submit"
                                    [disabled]="!signupForm.valid">
                                Save
                            </button>
                        </span>
                    </div>
                </div>
            </fieldset>
        </form>
    </div>
</div>
<br>Dirty: {{ signupForm.dirty }} 
<br>Touched: {{ signupForm.touched }}
<br>Valid: {{ signupForm.valid }}
<br>Value: {{ signupForm.value | json }}
```

### The component class:

```js
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

import { Customer } from './customer';

@Component({
    selector: 'my-signup',
    templateUrl: './app/customers/customer.component.html'
})
export class CustomerComponent  {
    customer: Customer= new Customer();

    save(customerForm: NgForm) {
        console.log(customerForm.form);
        console.log('Saved: ' + JSON.stringify(customerForm.value));
    }
 }
```

### And the data model class \(in this case not an interface\):

```js
export class Customer {

    constructor(public firstName = '',
        public lastName = '',
        public email = '',
        public sendCatalog = false,
        public addressType = 'home',
        public street1?: string,
        public street2?: string,
        public city?: string,
        public state = '',
        public zip?: string) { }
}
```

Taking a class instead of an interface is because on can create a new instance of it.

