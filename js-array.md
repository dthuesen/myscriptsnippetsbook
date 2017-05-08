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
* [Loop over an Array](#loop-over-an-array)
* [for in loop - iterate over Array index](#for-in-loop---iterate-over-array-index)

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
      currentValue   -> The current element being processed in the array.
      index          -> The index of the current element being processed in the array.
      array          -> The array map was called upon.
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
let stars = ["James Bond", "Mrs. Moneypenny", "Q", "M", "Octopussy", "Dr. No"]

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



