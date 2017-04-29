# JavaScript - Closure and Modules

##### Closure

```
function makeAdder(x) {

    function add(y) {
        return y + x;             // closure over inner variable 'x'
    }

    return add;
}

let plusOne = makeAdder(1);       // closure with inner add() function over parameter 'x'

let plusTen = makeAdder(10);


plusOne(3);                        // = 4   <-- 3 + 1 
plusOne(41);                       // = 42  <-- 41 + 1 

plusTen(13)                        // = 23  <-- 13 + 10
```

With each call the reference to the inner function add\(\) gets returned and the outer function makeAdder remembers what x value was. With a call plusTen\(13\), it adds 13 \(its inner y\) to the 10 \(remembered by x\) returns 23 as a result.

##### Module

The most common use of a closure in JS is the module pattern. Modules lets define private implementation details \(variables, functions\) as well as public API for the outside access.

```
function User() {
    let userName, password;

    function doLogin(user, password) {
        userName = user;
        password = pw;

        // the other login logic
    }

    let publicAPI = {
        login: doLogin
    }

    return publicAPI;
}

// create a 'User' module instance
let Fred = User();

Fred.login('fred', '!43TrippleX1234');
```

The `User` function serves an outer scope that holds the variables `userName` and `password`, as well as the inner `doLogin()` function. The variables `userName` and `password` are as well private details as the `doLogin()` function. Th

