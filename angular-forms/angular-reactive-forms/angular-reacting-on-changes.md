# Angular - Reacting on Changes

Forms reacting on user changes dynamically.

The property **valueChanges** of FormControl and FormGroup **emits events on value changes**. It's an `Observable<any>`and collects the events asynchronously. The  property **statusChanges** emits events on validation changes.

To watch the **events for a single FormControl,** we subscribe to its valueChanges observable, like so:

```js
this.myFormControl.valueChanges.subscribe( value => console.log(value) );
```

Watching for events from **any FormControl within a nested FormGroup** is possible **by subscribing to the FormGroups valueChanges observable** - each time any of the FormControls or other nested FormGroups within this FromGoup are changing, an event is emitted and the FormGroups value is provided, like here:

```js
this.myFormGroup.valueChanges.subscribe( value => console.log(JSON.stringify(value)) );
```

The value of the above emitted observable is **a set of key and value pairs for all of the FormControls and nested FormGroups. **Therefore JSON.stringify\(\) for displaying the value.

Watching for **any change** over the **entire form and any FormControl on that form** can be made by subscribing to the valueChanges property for the **form's root FormGroup.**

```js
this.customerForm.valueChanges.subscribe( value => console.log(JSON.stringify(value)) );
```

### Implementation 

To start watching as soon as the component is initialized the code has to be placed in the ngOnInit\(\) method. First subscribe to the FormControl which shoud be watched by adding the .get\(\) method to the root FormGroup and call it with the name of the FormControl \(in this case 'notification'\). The valueChanges property then will be subscribed to getting its observable events submitted \(here every time either radio buttons changes \). When a change occurs the value of the notification FormControl there's a value available that can be logged out or whatever.:

```js
ngOnInit(): void {
    
    ... // form model
    
    this.customerForm.get('notification').valueChanges.subscribe( value => console.log(value) );
    
}
```

Keep in mind this code must be after the definition fo the root FormGroup, otherwise this reference is null.

