# Angular Template-Driven Forms

### Creational structure of template-driven Forms:

Template-driven form keep all the data in the template on synchronisation with the properties in the component class.

**Template**

* Form element
* Input element\(s\)
* Data binding
* Validation rules \(attributes\)
* Validation error messages
* Form model automatically generated

**Component class**

* Properties for data binding \(representing the data model\)
* Method for form operations, such as submit

### Directives

* First import **FormsModule**
* Then the following directives are available:

  * **ngForm**                 --&gt; access to the form model
  * **ngModel**               --&gt; for two way binding and access to the input element  
                                       state bound in the generated form model

  * **ngModelGroup**    --&gt; for grouping input elements in the form

When a form element is added to a template...

```
<form (ngSubmit)="save()">
</form>
```

...  Angular automatically assigns the **ngForm** directive to that form. Angular creates the form model starting with the **FormGroup** instance and binds it to the form to track the form value and state. The **ngForm** directive does not have to be applied by one self.

To export the **form model** with its values and state the ngForm directive has to be exportet to a **template reference variable** \(local variable\) like this:

```
<form (ngSubmit)="save()"
    #signupForm="ngForm">      // <--- exporting the form model into a reference variable
</form>
```

This local variable references then the forms root FormGroup instance.

Anytime the form model needs to be accessed a reference of the **template refenrece variable** will be used, like her:

```
<form (ngSubmit)="save()"
    #signupForm="ngForm">      // <--- exporting the form model into a reference variable
    <button type="submit"
         [disabled]="!signupForm.valid">  // <-- using the reference variable
      Save
    </button>
</form>
```

The **ngModel** directive will be used on each input element for two way binding to keep the component class property insync with the user input value, like so:

```
<form (ngSubmit)="save()"
    #signupForm="ngForm">      // <--- exporting the form model into a reference variable

    <input id="firstNameId" type="text"
         [(ngModel)]="customer.firstName"
         name="firstName"                 // the name will be used as key in the form model
         #firstNameVar="ngModel" />       // accessing the value & state via export 
                                             into a local variable (assigning it to ngModel)     
    <button type="submit"
         [disabled]="!signupForm.valid">  // <-- using the reference variable
      Save
    </button>
</form>
```

Above Angular automatically creates a **FormControl** instance and adds it to the **form model** using the **inputs** **element name as the key**. Hence the **name attribute is needed** for the FormControl instance key. It tracks its **value** and **state**.

