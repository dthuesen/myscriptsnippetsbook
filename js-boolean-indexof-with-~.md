# JavaScript Boolean .indexOf\(\) with ~

Normally ~ is a bitwise operator and is the same as `-(x+1)`. That means:

```
~1    // = -2 because -(1+1) = -2
~2    // = -3 because -(2+1) = -3 
and so on...
```

But one can take to get a boolean return with the use of `.indexOf()`

**.indexOf\(\)**

Assume we have a **list**

`let list = ['apple', 'flower', 'car', 23, {'name': 'Detlef', 'age': '51'}, 'bus'];`

Then one is able to find out if an **apple** is in the list with the use of `~` and `.indeOf()`

Here's the example:

```
let list = ['apple', 'flower', 'car', 23, {'name': 'Detlef', 'age': '51'}, 'bus'];

~list.indexOf('apple')
$: -1         // the return value is -1 because it is on index 0 and the ~ adds -1 what makes -1

!~list.indexOf('apple')
$: false      // now the return value is false because it was converted into a boolean operator (~! together)
!!~list.indexOf('apple')
$: true       // it's clear, that this returns true
!!~list.indexOf('toast')
$: false      // for the use case of finding something in an Array !!~ is a better return
```

But one have to keep in mind, that an expression like this is not clear, it is not readable a an english phrase

**The use case:**

```
if (~list.indexOf(something)) {
}
```

is in plain 'english':

```
if (list.indexOf(something) >= 0) {
}

or like this

if (~list.indexOf(item)) {
   // item in list
 } else {
   // item *not* in list
 }
```

**Further tests for the **`!~`** and **`!!~`** combination:**

```
let list = ['apple', 'flower', 'car']

(!~list.indexOf('apple')) == false      // true
(!~list.indexOf('apple')) === false     // true
(~list.indexOf('apple')) === true       // false
(~list.indexOf('apple')) == true        // false
(~list.indexOf('apple')) == false       // false

let itemA = 'henry';
let itemB = ~~itemA;
itemB                                   // 0
let itemC = ~itemA
itemC                                   // -1

let itemStringNumber = '23' 
let itemNum = ~~itemStringNumber
itemNum                                 // 23  -> converts a string number into a number
let itemNumWhatever = ~itemStringNumber
itemNumWhatever                         // -24 -> converts into number and adds -1
```

`!~`** and **`!!~`**tests with a String:**

```
    let town = 'Hamburg'
    town.indexOf('mb');
$: 2

    ~town.indexOf('mb');
$: -3

    ~~town.indexOf('mb');
$: 2

    ~~town.indexOf('xz');
$: -1

    ~town.indexOf('xz');
$: 0

    town.indexOf('xz');
$: -1


My opinion: better only take .indexOf for these use cases:

    let town = 'Hamburg'

    if(town.indexOf('mb') > 0) {     // returns true, it is on index 2
        // do something
    };

or

    if(town.indexOf('xy') > 0) {     // returns false, it is on index -1
        // do something
    };

same as

    if(town.indexOf('xy') === -1) {   // returns false, it is on index -1
        // do something
    };


By the way
    ~town.indexOf('xz');          // returns 0
```



