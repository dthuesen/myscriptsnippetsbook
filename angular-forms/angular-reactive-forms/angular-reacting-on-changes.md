# Angular - Reacting on Changes

Forms reacting on user changes dynamically.

The property **valueChanges** of FormControl and FormGroup **emits events on value changes**. It's an `Observable<any> `and collects the events asynchronously. The  property **statusChanges** emits events on validation changes.

To watch the **events for a single FormControl,** we subscribe to its valueChanges observable, like so:

```js
this.myFormControl.valueChanges.subscribe( value => console.log(value) );
```



