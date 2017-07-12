# Angular - Adjust Validation Rules at Runtime

E.g. depending on a selection of the user the form changes its input fields, like so: the user can choose if he wants to recieve information via email or text. If he choses text he has to enter his phone number wheras if he decides to get it via email he has to enter his email address and for the two paths the form validation has to change during the input by the user.

For changing the validation at runtime the **setValidatiors\(\) method** will be called: `myControl.setValidatiors(Validators.required);`

