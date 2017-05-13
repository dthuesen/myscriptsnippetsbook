#### JavaScript - Class \(ES6\)

* [Inheritance \(ancestor and descendant constructor functions\)](#inheritance-ancestor-and-descendant-constructor-functions)
* [Overwrite Methods and call super\(\) for calling the ancestors method](#overwrite-methods-and-call-super-for-calling-the-ancestors-method)
* [Classes have to be instantiated, but not always](#classes-have-to-be-instantiated-but-not-always)
* [Export a helper class](#export-a-helper-class)

#### Inheritance \(ancestor and descendant constructor functions\)

```
class Person {

  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log('Hello my name is ' + this.name + ' and I am ' + this.age);
  }

}

class Ich extends Person {

  constructor(name, age) {
    super(name);
    this.age = age;
  }

}

let ich = new Ich('Detlef', 51)

ich.greet();

// logs out:

Hello my name is Detlef and I am 51
```

The `super()` method will call the parent the `constructor` and is able to pass an `argument` to the parent `constructor` - in this case the `name`. With this call it is possible to do the `console.log()` from the parent and therein call this.age from the descendants constructor.

#### Overwrite Methods and call super\(\) for calling the ancestors method

```
class Person {

  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log('Hello my name is ' + this.name + ' and I am ' + this.age);
  }

}

class Ich extends Person {

  constructor(name, age) {
    super(name);
    this.age = age;
  }

  greet() {
    let date = new Date();
    let day = date.getDate();
    let month = date.getMonth() + 1;
    let year = date.getFullYear();
    console.log('And today is ' + day + '.'+ month + '.' + year )
  }

  greetTwice() {
    console.clear();
    super.greet();          // calling the greet() method from the ancestor/parent class
    this.greet();           // calling the greet() method from this class
  }

}

let ich = new Ich('Detlef', 51)

ich.greetTwice();
```

#### Classes have to be instantiated, but not always

Normally a class has to be instantiated before a method of a class can be called. But if there is no need for instantiation or one want to share the class around in the code \(e.g. a helper class\), it might be better not to do it. With the static keyword there's a way around that:

**Class instantiation:**

```
class Helper {
  logTwice(message) {   
    console.log(message);
    console.log(message);
  }
}

let helper = new Helper();   // <-- instantiation

helper.logTwice("Logged!");  // <-- w/o instantiation the logTwice() method can be called
```

**No instantiation and no static keyword:**

```
class Helper {
  logTwice(message) {   
    console.log(message);
    console.log(message);
  }
}

Helper.logTwice("Logged!");  // <-- This wouldn't work
^^
```

**Using the static keyword w/o instantiation**

```
class Helper {
  static logTwice(message) {   // using the static keyword
    console.log(message);
    console.log(message);
  }
}

Helper.logTwice("Logged!");  // <-- This works!!!
^^
```

#### Export a helper class

The helper class above is of cause able to be exported \(like other modules in ES6\)  and can then be shared with other modules:

```
export class Helper {
  static logTwice(message) {   // using the static keyword
    console.log(message);
    console.log(message);
  }
}

Helper.logTwice("Logged!");  // <-- This works!!!
^^
```



