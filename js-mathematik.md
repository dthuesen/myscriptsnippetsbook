# JavaScript - Mathematik

### Zahlen potenzieren

2 \*\* 2   // 4

2 \*\* 3   // 8

9 \*\* 2   // 81

### **`Math.pow()`**gibt die Potenz der `Basis` mit dem `Exponenten`an \(Basis^Eponent\)

Parameter: 

* **Basis** \(auch: die Grundzahl\)
* **Exponent** \(auch: die Hochzahl\)

Rückgabewert

* Eine Zahl, die die Basis potenziert mit dem Exponenten repräsentiert.

Weil **`pow()`**eine statische Funktion von **`Math `**ist, wird es immer als **`Math.pow()`**eingesetzt, jedoch nicht als Methode eines erzeugten`Math`Objektes \(**`Math`**` `ist kein Konstruktor\).

```
Math.pow(7, 2);    // 49
Math.pow(4, 0.5);  // 2 (Wurzel aus 4)
Math.pow(8, 1/3);  // 2 (Kubikwurzel aus 8)
Math.pow(27, 1/3); // 3 (Kubikwurzel aus 9)
```

### `Math.sqrt()` {#Einsatz_von_Math.sqrt()}

```
Math.sqrt(9); // 3
Math.sqrt(2); // 1.414213562373095

Math.sqrt(1);  // 1
Math.sqrt(0);  // 0
Math.sqrt(-1); // NaN
```

### `Math.random()` {#Verwenden_von_Math.random()}

```
Math.random();  // Gibt eine Zufallszahl zwischen 0 (inklusive) und 1 (exklusive) zurück

// Math.random() * (max - min) + min;
Math.random() * (8 - 2) + 2; // 7.996127438621551 - Gibt eine Zufallszahl zwischen min (inklusive) 
                                                     und max (exklusive) zurück

// Math.floor(Math.random() * (max - min)) + min;  eine Zufallszahl zwischen min (inklusive) und max (exklusive)
Math.floor(Math.random() * (8 - 2)) + 2;  // 5 - Die Verwendung von Math.random() erzeugt ganzzahlige Zahlen

// Math.floor(Math.random() * (max - min +1)) + min; eine Zufallszahl zwischen min (inklusive) und max (inklusive)
Math.floor(Math.random() * (8 - 2 +1)) + 2; 

```

### `Math.ceil()` {#Example:_Using_Math.ceil}

```
Math.ceil(.95);    // 1
Math.ceil(4);      // 4
Math.ceil(7.004);  // 8
Math.ceil(-0.95);  // -0
Math.ceil(-4);     // -4
Math.ceil(-7.004); // -7
```

### `Math.trunc()` {#Einsatz_von_Math.trunc()}

```
Math.trunc(13.37);    // 13
Math.trunc(42.84);    // 42
Math.trunc(0.123);    //  0
Math.trunc(-0.123);   // -0
Math.trunc('-1.123'); // -1
Math.trunc(NaN);      // NaN
Math.trunc('foo');    // NaN
Math.trunc();         // NaN
```

### `Math.abs()` {#Example:_Math.abs_behavior}

```
Math.abs('-1');     // 1
Math.abs(-2);       // 2
Math.abs(null);     // 0
Math.abs('');       // 0
Math.abs([]);       // 0
Math.abs([2]);      // 2
Math.abs([1,2]);    // NaN
Math.abs({});       // NaN
Math.abs('string'); // NaN
Math.abs();         // NaN
```







