# JavaScript

## General notes about JavaScript
- you _can_ have a variable that changes type, e.g.
```
let num = 6;
num = true; // ok
num = "dog"; // ok
```
_note: TypeScript enforces type restrictions_
- do not use `var` (outdated), use `let` and `const`
- camelCase is typical for JS

## Primitive types
main ones:
- number
- string
- boolean
- null
- undefined
two other primitives:
- Symbol
- BigInt

### Comments
```
// written like this
```

### Numbers
- JS has one number "number" type, used for:
    - positive
    - negative
    - ints
    - decimal numbers (floats)

### Math operators
- addition / subtraction
```
1 + 2;
1 - 2;
```
- multiplication
```
1 * 2;
```
- division
```
1 / 2;
```
- modulo
```
3 % 2;
> 1 // remainder
```
- exponent
```
2 ** 4; // two to the fourth power
```

#### the Math built-in object
- collection of variables, for PI (`Math.PT`), E (`Math.E`), etc.
- and methods
```
// round down (chop off the decimal)
Math.floor(23.90)
> 90

// round up
Math.ceil(34.99)
> 35

// random number between 0 and 1
Math.random()
```

### Other operators
- typeof
```
typeof 6;
> "number"
```
- increment operator
```
var += 2;
var++; // shorthand for increment by 1
```
- decrement operator
```
var -= 2;
var--; // shorthand for decrement by 1
```
- mult-crement (?) and divi-crement (?)
```
var *= 2;
var /= 2;
```

#### Note about prefix/postfix ++ operator
```
// this will return the value i and then increment i afterward
i++;
// so if you assign it to a variable, it will assign the non-incremented value, e.g.
let i = 1;
let num = i++;
num
> 1
i
> 2

// but if you reverse this, and use ++i
// it will increment the variable before assigning it to the new variable
// so num and i would be equal when printing them, e.g.
let i = 1;
let num = ++i;
num
> 2
i
> 2

```

### NaN
A numeric value. (Considered a number by JS). Represents something that is not a number.

e.g.
```
0 / 0;
> NaN
```

### Variables
```
let numItems = 45;
```
#### const vs. let
- use const for variables that will be immutable
```
const num = 3;
num++;
> ERROR (constant variable cannot be reassigned)
num = 4;
> ERROR (constant variable cannot be reassigned)
```
#### var
- the old way of defining variables (slightly different from `let`)

### Booleans
```
// lowercase
true
false
```

### Strings
```
let word = "chicken";

// indexing a string
word[0]
> c

// checking string length
word.length
> 7

// concatenation
"chi" + "cken"
> "chicken"
```
#### String properties / methods
```
// length property
word.length
> 7

// toUpperCase() method
let newWord = word.toUpperCase();
newWord
> "CHICKEN"

// trim() method
let userInput = "   hello my name is derek   ";
userInput.trim()
> "hello my name is derek"

// indexOf() method
"hello".indexOf('e')
> 1

// slice() method
"hello".slice(1)
> "ello"
"hello".slice(1, 4) // non-inclusive
> "ell"
```

#### String template literals

```
// need to use backtick characters
// can put expressions to be evaluated in the curly braces
`I counted ${3 + 4} sheep`
> "I counted 7 sheep"
```

### Null & Undefined
- Null: intentional absence of value (must be assigned)
- Undefined: something that has not been assigned yet


### Comparison operators
```
>
<
>=
<=
== // equality (does not check for equality of type)
!= // non-equality
=== // strict equality (plus equality of type)
!== // strict non-equality (cares about quality of type)

// equality examples
null == undefined;
> true
1 == true;
> true
0 == false;
> true
1 == '1';
> true

// non-equality examples
1 != 2;
> true
1 != '2';
> true
1 != '1';
> false // coerces them to be same type first
1 !== '1';
> true // cares about type
```

### Truthy and falsy values
all values have an inherent truthyness or falsyness  

all values in js are truthy EXCEPT:
- `false`
- `0`
- `""` (empty string)
- `null`
- `undefined`
- `NaN`


### Conditionals
```
// traditional if/else
if (num > 3) {
    console.log("num is greater than 3")
}
else if (num === 3) {
    console.log("num is 3")
}
else {
    console.log("something else");
}

// switch statements
// default behavior will execeute each case after a matched case
// so we need a `break` in each case to prevent that
// can stack 2 cases one after another if they should run the same code
// `default` is used as the default catch-all case at the end
const day = 1;
switch (day) {
    case 1:
        console.log('Monday');
        break;
    case 2:
        console.log('Tuesday');
        break;
    case 3:
        console.log('Wednesday');
        break;
    case 4:
        console.log('Thursday');
        break;
    case 5:
        console.log('Friday');
        break;
    case 6:
    case 7:
        console.log('WEEKEND');
        break;
    default:
        console.log('not a day');
}
```

### Logical operators
```
&& //and
|| //or
!  //not
```

### Printing stuff
```
// prints to console
console.log("hello")

// alert popup in browser
alert("hello")

// prompt popup in browser
let userInput = prompt("enter a number")
```

#### Parsing user input
```
let userInput = parseInt(prompt("enter a number"))
```

## The `<script>` element
- connect a `.js` file to an html document with the `<script>` tag like so:
    - `<script src="app.js"></script>` in the html head or at the end of the file, or wherever
    - it would need to be at the end of the file if it's supposed to interact with html elements on the page, so those can be loaded first

## Arrays
```{javascript}
let colors = ['red', 'blue', 'green'];

// properties
colors.length
> 3

// indexing
colors[1]
> 'blue'

// updating an array
colors[1] = 'yellow'
colors
> ['red', 'yellow', 'green']

// you can assign a value at a later index even if the array is not that long
colors[6] = 'purple'
colors
> ['red', 'yellow', 'green', empty x 3, 'purple']
colors[4]
> undefined

// methods
colors.push('black') // add to end. you can add multiple args at once
colors.pop() // remove from end
colors.shift() // remove from start (index 0)
colors.unshift('cyan') // add to start (at index 0)
// some more methods
first_array.concat(second_array) // makes a new 3rd array, consisting of the first with the second appended to the end
colors.includes("red") // returns bool
> true
colors.indexOf("orange") // gets the index of item, returns -1 if not found
> -1

colors.reverse() // reverse the order of the array. it DOES change the original array! (destructive)

colors.slice() // with no args: makes a copy of the list
colors.slice(2) // makes a list from index of 2 to the end
colors.slice(2, 5) // from index 2 to 5 (non-inclusive)

// Reference Types and Equality Testing
[1,2,3] === [1,2,3] // two seemingly identical arrays
> false

// Pointers to arrays in memory
nums = [1,2,3]
numsCopy = nums
// both point to the same array
// e.g.
numsCopy.push(4)
numsCopy
> [1,2,3,4]
nums
> [1,2,3,4]
// both are updated, because they point to the same array!

// Nested Arrays
const gameBoard = [['X', 'O', 'X'],['O', null, 'X'],['O', 'O', 'X']]
gameBoard[1][1]
> null
```

## Objects (object literals)
- Another data structure.
- Collections of **properties**
- Properties are key-value pairs

```{javascript}
const fitnessData = {
    steps       : 308727,
    calories    : 1200,
    name        : 'derek'
}

// accessing data in an object
fitnessData['steps'] // with square brackets
> 308727

fitnessData.calories // with a dot
> 1200

// All keys are converted to strings (if it's a string, number, bool, etc.)

// Modifying objects
const grades = {maxwell:96, john:60}
grades.john = 100
```

## Loops
### For loops
```{javascript}
// for (initial expression (variable); condition (boolean expression); increment expression)
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
> 1
> 2
> 3
> 4
> 5

// iterating over arrays
const animals = ['cat', 'dog', 'horse']

for (let i = 0; i < animals.length; i++) {
    console.log(animals[i]);
}

// iterating over arrays in reverse
for (let i = animals.length - 1; i >= 0; i--) {
    console.log(animals[i]);
}
```

### While loops
```{javascript}

// while (condition)

let count = 0;
while (count < 10) {
    console.log(count);
    count++;
}
```

### Break keyword
```
let input = prompt('enter something');
while (true) {
    input = prompt(input);
    if (input === "magic word") {
        break;
    }
}
```

### For...of loops
```
// for (let element of iterableThing)
const colors = ['red','blue','green']

for (let color of colors) {
    console.log(`The color is ${color}`)
}

// can be used with strings, because they're iterable
for (let char of someString) {
    console.log(char)
}
```