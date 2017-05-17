# JavaScript - Object

* [assign\(\)](#assign)
* [create\(\)](#create)
* [keys\(\)](#keys)
* [hasOwnProperty\(\)](#hasownproperty)
* [Loop over an object](#loop-over-an-object)
* [.is\(\)](#is)
* [.setPrototypeOf\(\)](#setprototypeof)

#### .assign\(\)

**Syntax:** `Object.assign(target, ...sources)`

**Return value:** The target object.

**Cloning an object**

```
var obj = { a: 1 };
var copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```

**Merging objects**

```
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.
```

**Merging objects with same properties**

```
var o1 = { a: 1, b: 1, c: 1 };
var o2 = { b: 2, c: 2 };
var o3 = { c: 3 };

var obj = Object.assign({}, o1, o2, o3);

console.log(obj); // { a: 1, b: 2, c: 3 }
```

**Copying symbol-typed properties**

```
var o1 = { a: 1 };
var o2 = { [Symbol('foo')]: 2 };

var obj = Object.assign({}, o1, o2);
console.log(obj); // { a : 1, [Symbol("foo")]: 2 } (cf. bug 1207182 on Firefox)

Object.getOwnPropertySymbols(obj); // [Symbol(foo)]
```

**Primitives will be wrapped to objects**

```
var v1 = 'abc';
var v2 = true;
var v3 = 10;
var v4 = Symbol('foo');

var obj = Object.assign({}, v1, null, v2, undefined, v3, v4); 
// Primitives will be wrapped, null and undefined will be ignored.
// Note, only string wrappers can have own enumerable properties.

console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

#### create\(\)

**Syntax: **`Object.create(proto[, propertiesObject])`

**Return value: **A new object with the specified prototype object and properties.

```
// Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

// superclass method
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// subclass extends superclass
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?',
  rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?',
  rect instanceof Shape); // true
rect.move(1, 1); // Outputs, 'Shape moved.'
```

#### keys\(\)

**Syntax: **`Object.keys(obj)`

**Return value: **An array of strings that represent all the enumerable properties of the given object.

```
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // ['2', '7', '100']

// getFoo is property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  } 
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```

**Notes**

```
Object.keys('foo');
// TypeError: "foo" is not an object (ES5 code)

Object.keys('foo');
// ["0", "1", "2"]                   (ES2015 code)
```

#### hasOwnProperty\(\)

**Syntax: **`obj.hasOwnProperty(prop)`

**Return value: **A Boolean indicating whether or not the object has the specified property as own property.

```
o = new Object();
o.prop = 'exists';

function changeO() {
  o.newprop = o.prop;
  delete o.prop;
}

o.hasOwnProperty('prop');   // returns true
changeO();
o.hasOwnProperty('prop');   // returns false
```

**Iterating over the properties of an object**

```
var buz = {
  fog: 'stack'
};

for (var name in buz) {
  if (buz.hasOwnProperty(name)) {
    console.log('this is fog (' + 
      name + ') for sure. Value: ' + buz[name]);
  }
  else {
    console.log(name); // toString or something else
  }
}
```

**Using hasOwnProperty as a property name**

```
var foo = {
  hasOwnProperty: function() {
    return false;
  },
  bar: 'Here be dragons'
};

foo.hasOwnProperty('bar'); // always returns false

// Use another Object's hasOwnProperty
// and call it with 'this' set to foo
({}).hasOwnProperty.call(foo, 'bar'); // true

// It's also possible to use the hasOwnProperty property
// from the Object prototype for this purpose
Object.prototype.hasOwnProperty.call(foo, 'bar'); // true
```

#### Loop over an object

**for in loop - iteration over props** \(or keys\)

```
let obj = {
  name: 'James',
  lastname: 'Bond',
  age: 108,
  city: 'London',
  street: 'bondstreet'
}

for (let key in obj) {
  console.log(obj);
}

// prints out:
"name"
"lastname"
"age"
"city"
"street"
```

**for in loop - iteration over values**

```
let obj = {
  name: 'James',
  lastname: 'Bond',
  age: 108,
  city: 'London',
  street: 'bondstreet'
}

for (let key in obj) {
  console.log(obj[key]);
}

// prints out:

"James"
"Bond"
108
"London"
"bondstreet"
```

#### .is\(\)

Comparing two objects, like == and === but better.

Syntax: Object.is\(objectA, objectB\)

Some Examples:

    console.log(`
        ${0 === 0}                    // true
        ${0 === "A"}                  // false
        ${0 === undefined}            // false
        ${0 === null}                 // false
        ${0 === true}                 // false
        ${0 === false}                // false
        ${0 === NaN}                  // false
        ${0 === ''}                   // false
        ${"A" === NaN}                // false
        ${undefined === ''}           // false
        ${NaN === NaN}                // false
        ${undefined === null}         // false
        ${null === ''}                // false
        ${''=== ''}                   // true
        ${null === undefined}         // false

        ${Object.is(0, 0)}            // true
        ${Object.is(0, "A")}          // false
        ${Object.is(0, undefined)}    // false
        ${Object.is(0, null)}         // false
        ${Object.is(0, true)}         // false
        ${Object.is(0, false)}        // false
        ${Object.is(0, NaN)}          // false
        ${Object.is(0, '')}           // false
        ${Object.is("A", NaN)}        // false
        ${Object.is(undefined, '')}   // false
        ${Object.is(NaN, NaN)}        // true    <---
        ${Object.is(undefined, null)} // false
        ${Object.is(null, '')}        // false
        ${Object.is('', '')}          // true
        ${Object.is(null, undefined)} // false
    `);

More about that topic and why better to use Obejct.is\(\) here: [Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)

#### .setPrototypeOf\(\)

For setting an Object as a  prototype of another Object use `Object.setPrototypeOf(childObject, parentObject)`

```
let a = {
  x: 1
}

let b = {
  y: 2
}

Object.setPrototypeOf(a, b);
console.log(a.y);

// prints out:

$ 2
```



