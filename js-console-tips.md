#### JavaScript - Console tips

* [.time/timeEnd - a console timer](#timetimeend---a-console-timer)
* [.clear\(\) - clears the console](#clear---clears-the-console)
* [.count\(\)](#count)
* [.assert\(\)](#assert)
* [.warn\(\)](#warn)
* [Styling console output with CSS](#styling-console-output-with-css)
* [.table\(\)](#table)

#### .time/timeEnd - a console timer

The string in bracket will be printed out after the console.timeEnd\(\) is reached. The string for taking the time is an identifier and has to be in .time\(\) and .timeEnd\(\) the same.

```
console.time(' ...logging ends after: ');
// doing
// some 
// computation
console.timeEnd(' ...logging ends after: ');
```

#### .clear\(\) - clears the console

```
console.clear();
```

#### .count\(\)

Without label:

```
var user = "";

function greet() {
  console.count();
  return "hi " + user;
}

user = "bob";
greet();
user = "alice";
greet();
greet();
console.count();

// output:
"<no label>: 1"
"<no label>: 2"
"<no label>: 3"
"<no label>: 1"
```

With label:

```
var user = "";

function greet() {
  console.count(user);
  return "hi " + user;
}

user = "bob";
greet();
user = "alice";
greet();
greet();
console.count("alice");

// output:
"bob: 1"
"alice: 1"
"alice: 2"
"alice: 3"
```

#### .assert\(\)

Writes an error message to the console if the assertion is false. If the assertion is true, nothing happens.

**Syntax:**

`console.assert(assertion, obj1 [, obj2, ..., objN]);`

`console.assert(assertion, msg [, subst1, ..., substN]); // c-like message formatting`

**Parameters**

`assertion`

* Any boolean expression. If the assertion is false, the message is written to the console.

`obj1 ... objN`

* A list of JavaScript objects to output. The string representations of each of these objects are appended together in the order listed and output.

`msg`

* A JavaScript string containing zero or more substitution strings.

`subst1 ... substN`

* JavaScript objects with which to replace substitution strings within msg. This parameter gives you additional control over the format of the output.

```
function greaterThan(a,b) {
  console.assert(a > b, {"message":"a is not greater than b","a":a,"b":b});
}
greaterThan(5,6);

// output to console:
Assertion failed: Object {message: "a is not greater than b", a: 5, b: 6}
```

#### .warn\(\)

```
if(a.childNodes.length < 3 ) {
    console.warn('Warning! Too few nodes (%d)', a.childNodes.length);
}
```

![](/assets/import.png)

#### Styling console output with CSS

```
console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");
```

![](/assets/styling.png)

#### .table\(\)

```
console.table([{a:1, b:2, c:3}, {a:"foo", b:false, c:undefined}]);
console.table([[1,2,3], [2,3,4]]);
```

![](/assets/table.png)

or more complex:

```
function Person(firstName, lastName, age) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.age = age;
}

var family = {};
family.mother = new Person("Susan", "Doyle", 32);
family.father = new Person("John", "Doyle", 33);
family.daughter = new Person("Lily", "Doyle", 5);
family.son = new Person("Mike", "Doyle", 8);

console.table(family, ["firstName", "lastName", "age"]);
```

![](/assets/table2.png)

