# JavaScript - Closure

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



