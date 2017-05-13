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



