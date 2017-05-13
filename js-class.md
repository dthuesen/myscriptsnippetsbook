#### JavaScript - Class \(ES6\)

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
    super.greet();
    this.greet();
  }

}

let ich = new Ich('Detlef', 51)

ich.greetTwice();

```



