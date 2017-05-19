# JavaScript Array

* [.length](#length)
* [.concat\(\)](#concat)
* [.filter\(\)  ](#filter--)
* [.forEach\(\)](#foreach)
* [.includes\(\)](#includes)
* [.join\(\)](#join)
* [.pop\(\)](#pop)
* [.map\(\)  ](#map--)
* [.reduce\(\) ](#reduce-)
* [.some\(\)](#some)
* [.find\(\)](#find)
* [Loop over an Array](#loop-over-an-array)
* [for in loop - iterate over Array index](#for-in-loop---iterate-over-array-index)
* [Destructuring](#destructuring)
* [Destructuring with the rest parameter](#destructuring-with-the-rest-parameter)
* [Destructuring with default parameter](#destructuring-with-default-parameter)
* [Destructuring for swapping variables](#destructuring-for-swapping-variables)

#### .**length**

```
let numbers = [1, 2, 3, 4, 5];
numbers.length                            // 5
numbers.length = 3;
numbers                                   // [1, 2, 3]
numbers.length                            // 3
```

#### **.concat\(\)**

```
var arr1 = ['a', 'b', 'c'];
var arr2 = ['d', 'e', 'f'];

var arr3 = arr1.concat(arr2);

// arr3 is a new array [ "a", "b", "c", "d", "e", "f" ]
```

#### .**filter\(\)  **

\(creates a new array\)

```
var words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];

var longWords = words.filter(function(word){
  return word.length > 6;
})

// Filtered array longWords is ["exuberant", "destruction", "present"]
```

#### **.forEach\(\)**

```
var a = ['a', 'b', 'c'];

a.forEach(function(element) {
    console.log(element);
});

// a
// b
// c
```

#### **.includes\(\)**

```
var a = [1, 2, 3];
a.includes(2); // true 
a.includes(4); // false
```

#### **.join\(\)**

```
var a = ['Wind', 'Rain', 'Fire'];
a.join();    // 'Wind,Rain,Fire'
a.join('-'); // 'Wind-Rain-Fire'
```

#### **.pop\(\)**

```
var a = [1, 2, 3];
a.pop();

console.log(a); // [1, 2]
```

#### **.map\(\)**

\(creates a new array with the results - more information:  [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map "Array on MDN")\)

```
var numbers = [1, 5, 10, 15];
var roots = numbers.map(function(x) {
   return x * 2;
});
// roots is now [2, 10, 20, 30]
// numbers is still [1, 5, 10, 15]

var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt);
// roots is now [1, 2, 3]
// numbers is still [1, 4, 9]

Syntax:
var new_array = arr.map(callback[, thisArg])

callback             ->Function that produces an element of the new Array, taking three arguments:
      currentValue     -> The current element being processed in the array.
      index            -> The index of the current element being processed in the array.
      array            -> The array map was called upon.
thisArg              -> Optional. Value to use as this when executing callback.
```

#### **.reduce\(\) **

\(more information:** **[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/Reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)\)

```
var sum = [0, 1, 2, 3].reduce(function(acc, val) {
  return acc + val;
}, 0);
// sum is 6

var list1 = [[0, 1], [2, 3], [4, 5]];
var list2 = [0, [1, [2, [3, [4, [5]]]]]];

const flatten = arr => arr.reduce(
  (acc, val) => acc.concat(
    Array.isArray(val) ? flatten(val) : val
  ),
  []
);
flatten(list1); // returns [0, 1, 2, 3, 4, 5]
flatten(list2); // returns [0, 1, 2, 3, 4, 5]

Side Note Explanation of recursion:
1.flatten[] --> []
2.flatten[0] --> [] + 0 --> [0]
3.flatten[0,1] --> [] + 0 --> [0] + 1 --> [0,1]
4.flatten[0,[1]] --> [] + 0 --> [0] + flatten[1] --> [0] + ([]+1) --> [0] + ([1]) --> [0,1]
```

**Converting Array of Objects with .reduce\(\)**

```
const peopleArray = [
    { id: 123, name: "dave", age: 23 },
    { id: 456, name: "chris", age: 21 },
    { id: 789, name: "bob", age: 22 },
    { id: 101, name: "paul", age: 24 },
    { id: 102, name: "mary", age: 20 },
]

// normal selection of a person by id would look like this:

let idToSelect = 789;
let selectedPerson;

for (let person of peopleArray) {
    if (person.id === idToSelect) {
        selectedPerson = person;
        break;
    }
}


```

#### .some\(\)

Evaluates whether some element passes a test provided by a callback function.

**Syntax:** `arr.some(callback[, thisArg])`

```
function isBiggerThan10(element, index, array) {
  return element > 10;
}

[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
[12, 5, 8, 1, 4].some(isBiggerThan10); // true


or with arrow functions:

[2, 5, 8, 1, 4].some(x => x > 10);  // false
[12, 5, 8, 1, 4].some(x => x > 10); // true
```

**Checking whether a value exists in an array**

```
var fruits = ['apple', 'banana', 'mango', 'guava'];

function checkAvailability(arr, val) {
  return arr.some(function(arrVal) {
    return val === arrVal;
  });
}

checkAvailability(fruits, 'kela');   // false
checkAvailability(fruits, 'banana'); // true


... or with arrow functions:

var fruits = ['apple', 'banana', 'mango', 'guava'];

function checkAvailability(arr, val) {
  return arr.some(arrVal => val === arrVal);
}

checkAvailability(fruits, 'kela');   // false
checkAvailability(fruits, 'banana'); // true
```

**Converting any value to Boolean**

```
var TRUTHY_VALUES = [true, 'true', 1];

function getBoolean(a) {
  'use strict';

  var value = a;

  if (typeof value === 'string') { 
    value = value.toLowerCase().trim();
  }

  return TRUTHY_VALUES.some(function(t) {
    return t === value;
  });
}

getBoolean(false);   // false
getBoolean('false'); // false
getBoolean(1);       // true
getBoolean('true');  // true
```

#### .find\(\)

Returns the value of the first element inan array that fits the testing function.

**Syntax:** `arr.find(callback[, thisArg])`

```
function isBigEnough(element) {
  return element >= 15;
}

[12, 5, 8, 130, 44].find(isBigEnough); // 130
```

**Find an object in an array by one of its properties:**

```
var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function findCherries(fruit) { 
    return fruit.name === 'cherries';
}

console.log(inventory.find(findCherries)); 
// { name: 'cherries', quantity: 5 }
```

**Find a prime number in an array:**

```
function isPrime(element, index, array) {
  var start = 2;
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false;
    }
  }
  return element > 1;
}

console.log([4, 6, 8, 12].find(isPrime)); // undefined, not found
console.log([4, 5, 8, 12].find(isPrime)); // 5
```

#### Loop over an Array

**for of loop - iterate over Array values**

```
let stars = ["James Bond", "Mrs. Moneypenny", "Q", "M", "Octopussy", "Dr. No"]

for (let star of stars) {
  console.log("I'm famous, my name is " + star);
}

// prints out:

"I'm famous, my name is James Bond"
"I'm famous, my name is Mrs. Moneypenny"
"I'm famous, my name is Q"
"I'm famous, my name is M"
"I'm famous, my name is Octopussy"
"I'm famous, my name is Dr. No"
```

But be aware of the `for in loop` which has more value for object iterations:

#### **for in loop - iterate over Array index**

```
let stars = ["James Bond", "Mrs. Moneypenny", "Q", "M", "Octopussy", "Dr. No"];

for (let star in stars) {
  console.log("I'm famous, my name is " + star);
}

// prints out:

"I'm famous, my name is 0"
"I'm famous, my name is 1"
"I'm famous, my name is 2"
"I'm famous, my name is 3"
"I'm famous, my name is 4"
"I'm famous, my name is 5"
```

#### Destructuring

```
let stars = ["James Bond", "Mrs. Moneypenny", "Q", "M", "Octopussy", "Dr. No"];

// assigning three of the values to three variables
let [nameOne, nameTwo, nameThree] = stars;

// printing out the variables...
console.log('nameOne: ...... ' + nameOne);
console.log('nameTwo: ...... ' + nameTwo);
console.log('nameThree: .... ' + nameThree);

// results like this (this does not touch the original array):
"nameOne: ...... James Bond"
"nameTwo: ...... Mrs. Moneypenny"
"nameThree: .... Q"

// when assigning more variables than the array contains will return undefined:

let [nameOne, nameTwo, nameThree, nameFour, nameFive, nameSix, nameSeven] = stars;

console.log(nameSeven);

// undefined
```

Immediately destructuring is a more shorter form of the example above:

```
let [a, b] = ["James Bond", "Dr. No", "Mrs Moneypenny"];

console.log(a);
console.log(b);

// prints ou:
"James Bond"
"Dr. No"
```

#### Destructuring with the rest parameter

```
let stars = ["James Bond", "Mrs. Moneypenny", "Q", "M", "Octopussy", "Dr. No"];

let [name1, ...name2] = stars;

console.log('name1:');
console.log(name1);
console.log('name2:');
console.log(name2);

// prints out:
"name1:"
"James Bond"
"name2:"
["Mrs. Moneypenny", "Q", "M", "Octopussy", "Dr. No"]
```

#### Destructuring with default parameter

```
let array = [1, 2, "three", "x", 5];

let [a = 'default', b, c, d, e, f = 'default'] = array;

console.log(a);
console.log(b);
console.log(c);
console.log(d);
console.log(e);
console.log(f);

// prints out:
1            // if a value exists it will overwrite the default value
2
"three"
"x"
5
"default"    // this value didn't exist
```

#### Destructuring for swapping variables

```
let a = 15;
let b = "fifteen";

[a, b] = [b, a]           // here an array is actually destructured :-)

console.log("a: " + a);
console.log("b: " + b);

// prints out:
"a: fifteen"
"b: 15"
```

#### Destructuring with leaving out one or some values

```
let array = [1, 2, "three", "x", 5];

let [a, , c] = array;

console.log("a: " + a);
console.log("c: " + c);

// prints out:
"a: 1"
"c: three"
```



