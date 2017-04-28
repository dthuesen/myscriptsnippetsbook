# JavaScript - Switch statement

Simple switch statement:

```
switch (choice) {
  case 'rock':
    playersChoice = 1;
    countdown();
    break;
  case 'paper':
    playersChoice = 2;
    countdown();
    break;
  case 'scissors':
    playersChoice = 3;
    countdown();
    break;
}
```

Switch statement with included if...else statement:

```
switch (value) {
   case 3:
    if (player > computer) {
      reason = this.reasons[0];
    } else {
      reason = this.reasons[2];
    }
    break;
  case 5:
    if (player > computer) {
      reason = this.reasons[3];
    } else {
      reason = this.reasons[4];
    }
    break;
  case 4:
    if (player < computer) {
      reason = this.reasons[5];
    } else {
      reason = this.reasons[2];
    }
    break;
}
```



