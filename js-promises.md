# JavaScript - Promises

* [Methods](#methods)
* [Creating a Promise - example fulfilled](#creating-a-promise---example-fulfilled)
* [Creating a Promise - example rejected](#creating-a-promise---example-rejected)
* [Creating a Promise - example how a Promise resolves with another Promise](#creating-a-promise---example-how-a-promise-resolves-with-another-promise)
* [Creating a Promise - example with Promise.all\(\) and Promise.race\(\)](#creating-a-promise---example-with-promiseall-and-promiserace)

---

**Syntax:** `new Promise( /* executor */ function(resolve, reject) { ... } );`

**executor: **A function that is passed with the arguments `resolve` and `reject`. The executor function is executed immediately by the Promise implementation, passing `resolve` and `reject` functions \(the executor is called before the Promise constructor even returns the created object\). The `resolve` and `reject` functions, when called, `resolve` or `reject` the promise, respectively. The executor normally initiates some asynchronous work, and then, once that completes, either calls the `resolve` function to resolve the promise or else `rejects` it if an error occurred.

If an error is thrown in the executor function, the promise is rejected. The return value of the executor is ignored.

**A Promise is in one of these states:**

* **pending**: initial state, not fulfilled or rejected.
* **fulfilled**: meaning that the operation completed successfully.
* **rejected**: meaning that the operation failed.

### Methods

#### `Promise.all` \(iterable\)

Returns a promise that either fulfills when all of the promises in the iterable argument have fulfilled or rejects as soon as one of the promises in the iterable argument rejects. If the returned promise fulfills, it is fulfilled with an array of the values from the fulfilled promises in the same order as defined in the iterable. If the returned promise rejects, it is rejected with the reason from the first promise in the iterable that rejected. This method can be useful for aggregating results of multiple promises.

#### `Promise.race` \(iterable\)

Returns a promise that fulfills or rejects as soon as one of the promises in the iterable fulfills or rejects, with the value or reason from that promise.

#### `Promise.reject` \(reason\)

Returns a Promise object that is rejected with the given reason.

#### `Promise.resolve` \(value\)

Returns a Promise object that is resolved with the given value. If the value is a thenable \(i.e. has a then method\), the returned promise will "follow" that thenable, adopting its eventual state; otherwise the returned promise will be fulfilled with the value. Generally, if you don't know if a value is a promise or not, Promise.resolve\(value\) it instead and work with the return value as a promise.

#### Creating a Promise - example fulfilled

```
function doAsync() {
  let p = new Promise( function(resolve, reject) {              // <--- Creation of an instance of a Promise object
    console.log('In promise code, wait..');
    setTimeout( () => {
      console.log('resolving first db call....');
      resolve(console.log('And resolved!'));                    // <--- returning a Promise object that is resolved
    }, 2000);                                                   //      if everything went well
  });
  console.log('Other code while Promise is still promising');
  return p;
}

doAsync().then(function(value) {                       // <--- appending fulfillment and rejection handlers
  console.log('Fulfilled ' + value);                            //      and return another promise
}, 
function(reason) {
  console.log('Rejected! ' + reason);
});
```

#### Creating a Promise - example rejected

```
doAsync() {
  let p = new Promise( function(resolve, reject) {
    console.log('In fourth B promise code, wait..');
    setTimeout( function() {
      console.log('resolving third db call....');
      reject('Not OK!');                                  // <--- rejecting
    }, 3000);
  });
  console.log('Other code while Promise is still promising');
  return p;
}

doAsync().then(function(value) {
  console.log('fulfilled ' + value);                     // <--- does things when resolved
  return 'For sure!';
}, 
function(reason) {
  console.log('Rejected! ' + reason);                    // <--- does things when rejected
});
```

#### Creating a Promise - example how a Promise resolves with another Promise

```
doAsync() {
  let anotherPromise = new Promise(function(resolve, reject) {
    console.log('In fourth B promise code, wait..');
    setTimeout( function() {
      console.log('resolving third db call....');
      reject('Not OK! :-(');
    }, 3000);
  })

  let p = new Promise( function(resolve, reject) {
    console.log('In fourth A promise code, wait..');
    setTimeout( function() {
      console.log('resolving fourth db call after 3 seconds....');
      resolve( anotherPromise );                                      // <--- resolving with another promise
    }, 3000);
  });
  console.log('Other fourth code while Promise is still promising');
  return p;
}



doAsync().then(function() { console.log('OK!') },           // <--- does things when resolved
               function() { console.log('NOPE!!!!') });     // <--- does things when rejected
```

#### Creating a Promise - example with `Promise.all()` and `Promise.race()`

```
doAsync1() {
  let promise1 = new Promise(function(resolve, reject) {
    console.log('In fifth A promise code, wait..');
    setTimeout( function() {
      console.log('resolving fifth db call after 3 seconds....');
      resolve('#5 is OK! ');
    }, 3000);
  })
  return promise1; 
}

doAsync2() {
  let promise2 = new Promise( function(resolve, reject) {
    console.log('In sixth B promise code, wait..');
    setTimeout( function() {
      console.log('resolving sixth db call after 8 seconds....');
      resolve(' And #6 is OK too. ');                                                   // example with resolve
      // reject(' And #6 is absolutely not OK! So both will be shown as not OK....');   // example with reject
    }, 8000);
  });
  console.log('Other sixth code while Promise is still promising');
  return promise2;
}


Promise.all([doAsync1, doAsync2]).then(                  // <--- all Promises have to resolve. 
  function(value) { console.log('OK: ' + value) },       //      If one is rejected that then rejects all
  function(reason) { console.log('NOPE!: ' + reason) },
);

// This could be useful for e.g. accessing redundant stores
Promise.race([doAsync1, doAsync1]).then(                 // <--- the first resolved Promise wins the race, 
  function(value) { console.log('OK: ' + value) },       //      the next will be ignored.
  function(reason) { console.log('NOPE!: ' + reason) },
);
```



