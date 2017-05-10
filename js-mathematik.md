# JavaScript - Math

* [Zahlen potenzieren](#zahlen-potenzieren)
* [Math.pow\(\)gibt die Potenz der Basis mit dem Exponentenan \(Basis^Exponent\)](#mathpowgibt-die-potenz-der-basis-mit-dem-exponentenan-basisexponent)
* [Math.sqrt\(\)](#mathsqrt)
* [Math.random\(\)](#mathrandom)
* [Math.ceil\(\)](#mathceil)
* [Math.trunc\(\)](#mathtrunc)
* [Math.abs\(\)](#mathabs)
* #### Zahlen potenzieren

2 \*\* 2   // 4

2 \*\* 3   // 8

9 \*\* 2   // 81

#### `Math.pow()`gibt die Potenz der `Basis` mit dem `Exponenten`an \(Basis^Exponent\)

Parameter:

* **Basis** \(auch: die Grundzahl\)
* **Exponent** \(auch: die Hochzahl\)

Rückgabewert

* Eine Zahl, die die Basis potenziert mit dem Exponenten repräsentiert.

Weil `pow()`eine statische Funktion von `Math`ist, wird es immer als `Math.pow()`eingesetzt, jedoch nicht als Methode eines erzeugten`Math`Objektes \(`Math`ist kein Konstruktor\).

```
Math.pow(7, 2);    // 49
Math.pow(4, 0.5);  // 2 (Wurzel aus 4)
Math.pow(8, 1/3);  // 2 (Kubikwurzel aus 8)
Math.pow(27, 1/3); // 3 (Kubikwurzel aus 9)
```

#### `Math.sqrt()`

```
Math.sqrt(9); // 3
Math.sqrt(2); // 1.414213562373095

Math.sqrt(1);  // 1
Math.sqrt(0);  // 0
Math.sqrt(-1); // NaN
```

#### `Math.random()`

```
Math.random();  // Gibt eine Zufallszahl zwischen 0 (inklusive) und 1 (exklusive) zurück

// Math.random() * (max - min) + min;
Math.random() * (8 - 2) + 2; // 7.996127438621551 - Gibt eine Zufallszahl zwischen min (inklusive) 
                                                     und max (exklusive) zurück

// Math.floor(Math.random() * (max - min)) + min;  eine Zufallszahl zwischen 
// 'min' (inklusive) und max (exklusive)
Math.floor(Math.random() * (8 - 2)) + 2;  // 5 - Die Verwendung von Math.random() erzeugt ganze Zahlen

// Math.floor(Math.random() * (max - min +1)) + min; eine Zufallszahl zwischen 
// 'min' (inklusive) und max (inklusive)
Math.floor(Math.random() * (8 - 2 +1)) + 2;
```

#### `Math.ceil()`

\(Immer aufrunden\)

```
Math.ceil(.95);    // 1
Math.ceil(4);      // 4
Math.ceil(7.004);  // 8
Math.ceil(-0.95);  // -0
Math.ceil(-4);     // -4
Math.ceil(-7.004); // -7
```

#### `Math.trunc()`

\(Nachkommastellen unter den wegfallen lassen\)

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

#### `Math.abs()`

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

#### `Math.min()` / `Math.max()`

Syntax: `Math.min([value1[, value2[, ...]]])` bzw. `Math.max([value1[, value2[, ...]]])`

**Beispiele Math.min\(\):**

```
var x = 10, y = -20;
var z = Math.min(x, y);

bzw.

Math.max(10, 20);   //  20
Math.max(-10, -20); // -10
Math.max(-10, 20);  //  20
```

Um den **Maximalwert eines Arrays** zu ermitteln, gibt es zwei mögliche Ansätze:

```
// Vor ES6

var numbers = [1,2,3,4,5,6,7]

console.log(Math.max.apply(null, numbers));


// Ab ES6

let numbers = [1,2,3,4,5,6,7]

console.log(Math.max(...numbers));  // 7
```

Gleiches gilt für Math.min\(\).

#### Math.min & Math.max & Math.random & Math.floor

Angenommen ein Zahlenliste wird als Array zurück gegeben und man möchte mit dem **Maximal-** und dem **Minimalwert** **Limitierung** für das Auffinden einer **Zufallszahl** festlegen. Dann sähe das so aus:

Zur Erinnerung, die Syntax dafür ist folgende `Math.random() * (max - min) + min`:

```
let array = [9, 55, 36, 3, 89, 107, 88, 15, 12, 43, 201, 8, 19]
console.log( Math.random() * (Math.max(...array) - Math.min(...array)) + Math.min(...array) );

// ergibt z.B: 7.302932419446655 oder 175.2559285298826
```

Das ganze jetzt noch **mit ganzen Zahlen**:

```
let array = [9, 55, 36, 3, 89, 107, 88, 15, 12, 43, 201, 8, 19]
console.log( Math.floor(Math.random() * (Math.max(...array) - Math.min(...array)) + Math.min(...array)) );
```

**Aber ACHTUNG: **Auf diese Weise wird nie die Höchste Zahl einbezogen. Wenn man also aus einem Array von \[1, 2, 3\] eine Zufallszahl zieht, werden immer nur entweder 1 oder 2 wiedergegeben \(s. oben Math.random\(\) \). Daher muss der Maximalwert plus 1 angegeben werden. Also so wie in diesem Beispiel - **inklusive min und max**:

```
console.log( Math.floor(Math.random() * ((Math.max(...array) +1) - Math.min(...array)) + Math.min(...array)) );
```



