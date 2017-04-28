# JavaScript Array

.**legth**

```
let numbers = [1, 2, 3, 4, 5];
numbers.length                            // 5
numbers.length = 3;
numbers                                   // [1, 2, 3]
numbers.length                            // 3
```

**.concat\(\)**

```
var arr1 = ['a', 'b', 'c'];
var arr2 = ['d', 'e', 'f'];

var arr3 = arr1.concat(arr2);

// arr3 is a new array [ "a", "b", "c", "d", "e", "f" ]
```

.**filter\(\)  - **creates a new array

```
var words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];

var longWords = words.filter(function(word){
  return word.length > 6;
})

// Filtered array longWords is ["exuberant", "destruction", "present"]
```

**.forEach\(\)**

```
var a = ['a', 'b', 'c'];

a.forEach(function(element) {
    console.log(element);
});

// a
// b
// c
```

**.includes\(\)**

```
var a = [1, 2, 3];
a.includes(2); // true 
a.includes(4); // false
```

**.join\(\)**

```
var a = ['Wind', 'Rain', 'Fire'];
a.join();    // 'Wind,Rain,Fire'
a.join('-'); // 'Wind-Rain-Fire'
```

**.pop\(\)**

```
var a = [1, 2, 3];
a.pop();

console.log(a); // [1, 2]
```

.map\(\)  - creates a new array with the results  [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map "Array on MDN")

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



