# JavaScript - Try-Catch

```
try {
  let [high, low, ] = undefined;
} catch (error) {
    console.log(error.name);
}

$ TypeError
```

```
try {
  let [high, low, ] = "one";
} catch (error) {
    console.log(error.name);
}

$ undefined
```



