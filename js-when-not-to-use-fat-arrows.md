# JavaScript - when not to use Fat Arrow functions

\(Picked from: ["When 'not' to use arrow functions" by Dmitri Pavlutin \| 06 Jun 2016](https://rainsoft.io/when-not-to-use-arrow-functions-in-javascript/)\)



* [1. Defining methods on an object](#1-defining-methods-on-an-object)
* [1a. Object literal](#1a-object-literal)
* [1b. Object prototype](#1b-object-prototype)
* [2. Callback functions with dynamic context](#2-callback-functions-with-dynamic-context)
* [3. Invoking constructors](#3-invoking-constructors)
* [4. Too short syntax](#4-too-short-syntax)



### 1. Defining methods on an object

In JavaScript the method is a function stored in a property of an object. When calling the method, this becomes the object that method belongs to.



#### 1a. Object literal

```
var calculate = {  
  array: [1, 2, 3],
  sum: () => {
    console.log(this === window); // => true
    return this.array.reduce((result, item) => result + item);
  }
};
console.log(this === window); // => true  
// Throws "TypeError: Cannot read property 'reduce' of undefined"
calculate.sum();  
```

**`calculate.sum`** method is defined with an arrow function. But on invocation **`calculate.sum()`**throws a TypeError, because **`this.array`** is evaluated to **`undefined`**. 

When invoking the method **`sum()`** on the **`calculate`** object, the context still remains **`window`**. It happens because the arrow function binds the context lexically with the **`window`** object. 

Executing **`this.array`** is equivalent to **`window.array`**, which is **`undefined`**.

**The fixed version:**

```
var calculate = {  
  array: [1, 2, 3],
  sum() {
    console.log(this === calculate); // => true
    return this.array.reduce((result, item) => result + item);
  }
};
calculate.sum(); // => 6 
```

#### 1b. Object prototype

The same rule applies when defining methods on a **`prototype`** object. 

Instead of using an arrow function for defining **`sayCatName`** method, which brings an incorrect context **`window`**:

```
function MyCat(name) {  
  this.catName = name;
}
MyCat.prototype.sayCatName = () => {  
  console.log(this === window); // => true
  return this.catName;
};
var cat = new MyCat('Mew');  
cat.sayCatName(); // => undefined 
```

**Use the old school function expression:**

```
function MyCat(name) {  
  this.catName = name;
}
MyCat.prototype.sayCatName = function() {  
  console.log(this === cat); // => true
  return this.catName;
};
var cat = new MyCat('Mew');  
cat.sayCatName(); // => 'Mew'  
```

**`sayCatName`** regular function is changing the context to **`cat`** object when called as a method: **`cat.sayCatName()`**.



#### 2. Callback functions with dynamic context

**`this`** in JavaScript is a powerful feature. It allows to change the context depending on the way a function is called. Frequently the context is the target object on which invocation happens, making the code more natural. It says like "something is happening with this object".

However the arrow function binds the context statically on declaration and is not possible to make it dynamic. It's the other side of the medal in a situation when lexical **`this`** is not necessary.

Attaching event listeners to DOM elements is a common task in client side programming. An event triggers the handler function with **`this`** as the target element. Handy usage of the dynamic context.

The following example is trying to use an arrow function for such a handler:

```
var button = document.getElementById('myButton');  
button.addEventListener('click', () => {  
  console.log(this === window); // => true
  this.innerHTML = 'Clicked button';
});
```

**`this`** is window in an arrow function that is defined in the global context. When a click event happens, browser tries to invoke the handler function with button context, but arrow function does not change its pre-defined context. 

**`this.innerHTML`** is equivalent to **`window.innerHTML`** and has no sense.

You have to apply a function expression, which allows to change **`this`** depending on the target element:

```
var button = document.getElementById('myButton');  
button.addEventListener('click', function() {  
  console.log(this === button); // => true
  this.innerHTML = 'Clicked button';
});
```

When user clicks the button, **`this`** in the handler function is **`button`**. Thus **`this.innerHTML = 'Clicked button'`** modifies correctly the button text to reflect clicked status.

#### 3. Invoking constructors

**`this`** in a construction invocation is the newly created object. When executing **`new MyFunction()`**, the context of the constructor **`MyFunction`** is a new object: **`this instanceof MyFunction === true`**.

**Notice that an arrow function cannot be used as a constructor. **JavaScript implicitly prevents from doing that by throwing an exception. 

Anyway **`this`** is setup from the enclosing context and is not the newly created object. In other words, an arrow function constructor invocation doesn't make much sense and is ambiguous. 

Let's see what happens if however trying to:

```
var Message = (text) => {  
  this.text = text;
};
// Throws "TypeError: Message is not a constructor"
var helloMessage = new Message('Hello World!');  
```

Executing **`new Message('Hello World!')`**, where **`Message`** is an arrow function, JavaScript throws a **`TypeError`** that **Message** cannot be used as a constructor.

I consider an efficient practice that ECMAScript 6 fails with verbose error messages in such situations. Contrary to fail silently specific to previous JavaScript versions.

The above example is fixed using a **`function expression`**, which is the correct way \(including the **`function declaration`**\) to create constructors:

```
var Message = function(text) {  
  this.text = text;
};
var helloMessage = new Message('Hello World!');  
console.log(helloMessage.text); // => 'Hello World!' 
```

#### 4. Too short syntax

At some level the compressed function becomes difficult to read, so try not to get into passion. Let's see an example:

```
let multiply = (a, b) => b === undefined ? b => a * b : a * b;  
let double = multiply(2);  
double(3);      // => 6  
multiply(2, 3); // => 6  
```

**`multiply`** returns the multiplication result of two numbers or a closure tied with first parameter for later multiplication. 

The function works nice and looks short. But it may be tough to understand what it does from a first look.



To make it more readable, it is possible to restore the optional curly braces and **`return`** statement from the arrow function or use a regular function:

```
function multiply(a, b) {  
  if (b === undefined) {
    return function(b) {
      return a * b;
    }
  }
  return a * b;
}
let double = multiply(2);  
double(3);      // => 6  
multiply(2, 3); // => 6  
```





