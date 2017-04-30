# JavaScript - methods

* [concat\(\)](#concat)
* [endsWith\(\) / startsWith\(\)](#endswith--startswith)
* [includes\(\)](#includes)
* [localeCompare\(\)](#localecompare)
* [match\(\)](#match)
* [padStart\(\) / padEnd ](#padstart--padend-) \(ECMAScript 2017\)
* [repeat\(\)](#repeat)

#### **concat\(\)** {#concat}

```
var hello = 'Hello, ';
console.log(hello.concat('Kevin', ' have a nice day.'));
/* Hello, Kevin have a nice day. */

var greetList = ['Hello', ' ', 'Venkat', '!'];
"".concat(...greetList); // "Hello Venkat!"

"".concat({}); // [object Object]
"".concat([]); /// ""
"".concat(null); // "null"
"".concat(true); // "true"
"".concat(4, 5); // "45"
"".concat({}); // [object Object]
```

#### endsWith\(\) / startsWith\(\)

```
var str = 'To be, or not to be, that is the question.';

console.log(str.endsWith('question.')); // true
console.log(str.endsWith('to be'));     // false
console.log(str.endsWith('to be', 19)); // true

//startsWith
var str = 'To be, or not to be, that is the question.';

console.log(str.startsWith('To be'));         // true
console.log(str.startsWith('not to be'));     // false
console.log(str.startsWith('not to be', 10)); // true
```

#### **includes\(\)**

```
var str = 'To be, or not to be, that is the question.';

console.log(str.includes('To be'));       // true
console.log(str.includes('question'));    // true
console.log(str.includes('nonexistent')); // false
console.log(str.includes('To be', 1));    // false
console.log(str.includes('TO BE'));       // false
```

#### localeCompare\(\)

The localeCompare\(\) method returns a number indicating whether a reference string comes before or after or is the same as the given string in sort order.

```
// The letter "a" is before "c" yielding a negative value
'a'.localeCompare('c'); // -2 or -1 (or some other negative value)

// Alphabetically the word "check" comes after "against" yielding a positive value
'check'.localeCompare('against'); // 2 or 1 (or some other positive value)

// "a" and "a" are equivalent yielding a neutral value of zero
'a'.localeCompare('a'); // 0
```

Check browser support for extended arguments

```
function localeCompareSupportsLocales() {
  try {
    'foo'.localeCompare('bar', 'i');
  } catch (e) {
    return e.name === 'RangeError';
  }
  return false;
}
```

**Using locales**

The results provided by localeCompare\(\) vary between languages. In order to get the sort order of the language used in the user interface of your application, make sure to specify that language \(and possibly some fallback languages\) using the locales argument:

```
console.log('ä'.localeCompare('z', 'de')); // a negative value: in German, ä sorts before z
console.log('ä'.localeCompare('z', 'sv')); // a positive value: in Swedish, ä sorts after z
```

**Using options**

The results provided by localeCompare\(\) can be customized using the options argument:

```
// in German, ä has a as the base letter
console.log('ä'.localeCompare('a', 'de', { sensitivity: 'base' })); // 0

// in Swedish, ä and a are separate base letters
console.log('ä'.localeCompare('a', 'sv', { sensitivity: 'base' })); // a positive value
```

#### match\(\)

```
var str = 'For more information, see Chapter 3.4.5.1';
var re = /see (chapter \d+(\.\d)*)/i;
var found = str.match(re);

console.log(found);

// logs [ 'see Chapter 3.4.5.1',
//        'Chapter 3.4.5.1',
//        '.1',
//        index: 22,
//        input: 'For more information, see Chapter 3.4.5.1' ]

// 'see Chapter 3.4.5.1' is the whole match.
// 'Chapter 3.4.5.1' was captured by '(chapter \d+(\.\d)*)'.
// '.1' was the last value captured by '(\.\d)'.
// The 'index' property (22) is the zero-based index of the whole match.
// The 'input' property is the original string that was parsed.
```

#### padStart\(\) / padEnd

\(MDN: experimental technology - ECMAScript 2017\)

The `padStart()` method pads the current string with another string \(repeated, if needed\) so that the resulting string reaches the given length. The padding is applied from the start \(left\) of the current string.

```
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
'abc'.padStart(8, "0");     // "00000abc"
'abc'.padStart(1);          // "abc"
```

The `padEnd()` method pads the current string with a given string \(repeated, if needed\) so that the resulting string reaches a given length. The padding is applied from the end \(right\) of the current string.

```
'abc'.padEnd(10);          // "abc       "
'abc'.padEnd(10, "foo");   // "abcfoofoof"
'abc'.padEnd(6, "123456"); // "abc123"
'abc'.padEnd(1);           // "abc"
```

#### repeat\(\)

```
'abc'.repeat(-1);   // RangeError
'abc'.repeat(0);    // ''
'abc'.repeat(1);    // 'abc'
'abc'.repeat(2);    // 'abcabc'
'abc'.repeat(3.5);  // 'abcabcabc' (count will be converted to integer)
'abc'.repeat(1/0);  // RangeError

({ toString: () => 'abc', repeat: String.prototype.repeat }).repeat(2);
// 'abcabc' (repeat() is a generic method)
```

search\(\)

The search\(\) method executes a search for a match between a regular expression and this String object.

Syntax: `str.search(regexp)`

The index of the first match between the regular expression and the given string; if not found, -1.  if \(str.search\(re\) != -1\) {let

```
let re = /Hamburg/i
let str = 'Heute scheint in Hamburg den ganzen Tag die Sonne.'

function testinput(re, str) {
  var midstring;
  if (str.search(re) != -1) {
    midstring = ' contains ';
  } else {
    midstring = ' does not contain ';
  }
  console.log(str + midstring + re);
}

testinput(re, str);
```



