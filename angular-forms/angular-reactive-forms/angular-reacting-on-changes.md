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

Watching for **any change** over the **entire form** can be made by subscribing to the valueChanges property for the **form's root FormGroup.**

