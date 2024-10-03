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

// splice() to delete or insert at index
// syntax:
array.splice(start, deleteCount, item1)
// e.g.
colors.splice(1, 0, "magenta") // this starts at index 1, deletes 0 entries, and adds "magenta" there

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
- *NOT* iterable (can't use a normal for loop)

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

// Object methods
Object.keys(grades)
> ["maxwell", "john"]

Object.values(grades)
> [96, 60]

Object.entries(grades)
> [["maxwell", 96], ["john", 60]]
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
- usually to iterate over arrays
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

### For...in loops
- used for objects (iterate over keys)
```
const testScores = {
    john: 80,
    mike: 60,
    darla: 90
}
for (let person in testScores) {
    console.log(testScores[person])
}

> 80
> 60
> 90
```

## Functions
```
function singSong(lyric) {
    console.log(`here is a song about ${lyric}`);
    console.log(`here is a song about ${lyric}`);
    console.log(`here is a song about ${lyric}`);
}
```

### Scope
- the scope of variables are only within a function
- Block scope: this means that, in a block like an `if` statement, (const/vars) defined within that block are not available outside of it
- Lexical scope: inner (nested) functions defined in outer (parent) functions have access to variables defined in the outer function
    - this does NOT work in reverse (variables defined in inner functions are not accessible in the parent function)

### Function expressions
```
// functions can be unnamed and stored in a variable
// behave the same way, can be called in the same way
const add = function (x, y) {
    return x + y;
}
let number = add(2, 7);
```

### Higher order functions
- functions that accept functions as arguments
- functions that return other functions
```
// Functions as arguments

// this will call some function func() twice, if it's passed into callTwice() using only the function name (func) without parentheses
function callTwice(func) {
    func();
    func();
}

// assume rollDie() is some function
callTwice(rollDie)
> 2
> 5

// Returning functions
function makeMysteryFunc() {
    const rand = Math.random();
    if (rand > 0.5) {
        return function() {
            console.log("Good");
        }
    } else {
        return function() {
            console.log("Bad);
        }
    }
}

// if we call makeMysteryFunc(), it will return one of those 2 functions, but it still won't call that function
// e.g.
makeMysteryFunc()
> f () { console.log("Good"); }

// we need to save the function to a variable
const mystery = makeMysteryFunc();

mystery
> f () { console.log("Bad"); } // returns the function in mystery

mystery()
> "Good" // calls the function in mystery

// Returning functions with passed-in arguments

// this function returns a function that differs depending on the supplied min and max
// then the returned function can be stored to a variable
// then it can be used by providing a 'num'
// which will be checked if it's between min and max
function makeBetweenFunc(min, max) {
    return function (num) {
        return num >= min && num <= max;
    }
}
```

### Methods for objects
- functions can be added as a property for some object
```
const square = {
    area: function(side) {
        return side * side
    },
    perimeter: function(side) {
        return side * 4
    }
}

square.area(2);
> 4

// can also write methods in objects without the function keyword
const square = {
    area(side) {
        return side * side
    },
    perimeter(side) {
        return side * 4
    }
}
```

### This keyword
- to access other properties in the same object
```js
const cat = {
    name: "Fred",
    color: "black",
    meow: function () {
        console.log(`${this.name} says meow`)
    }
}
```
- the value of `this` depends on the invocation context of the function it is used in
```js
const meow2 = cat.meow;

meow2
> says meow    //this output lacks the this.name portion from the original cat object
```
### The js `window` object
- the window object is built-in, and sort of the default object when a different one is not invoked
- that's why the `meow2` from above does not have the "this.name" portion of the meow() method

## Try / catch error handling
```js
try {
    undefinedVariable.toUpperCase();
} catch {
    console.log("Error!");
}
```

## Callbacks and Array Methods
#### forEach
```js
const numbers = [1,2,3];

// using forEach, which needs function passed in
numbers.forEach(function (x) {
    console.log(x);
})

// same as using a for ... of
for (let x of numbers) {
    console.log(x);
}
```

#### map
- creates a new array with the results of calling a callback on every element in the array
```js
const numbers = [1,2,3];

const doubles = numbers.map(function(num) {
    return num * 2;
})

// this will make a new array which runs the function on each element in numbers
```

### Arrow functions
- newer syntax for defining functions
- syntactically compact

```js
const square = (x) => {
    return x * x;
}

const rollDie = () => {
    return Math.floor(Math.random() * 6 ) + 1;
}
```

- can also use implicit returns with arrow functions
```js
const square = x => x * x;
```

- using `this` with arrow functions
    - using `this`, it's going to refer to the scope it was created in
    - so it might annoyingly refer to the window object in the browser
    - if you use `this` in an arrow function in another normal function, it will work as intended because the scope is correct


### Other callbacks
#### setTimeout
```js
// takes 2 args, a callback function and a time
setTimeout(() => {
    console.log('hello')
}, 3000)
```
### setInterval
- works the same as `setTimeout` except the time supplied acts as a repeating interval at which to keep running the callback function (e.g. every 3 secs)
```js
setInterval(() => {
    console.log('hello')
}, 3000)
```

- `setInterval` returns an `id`. you can save this to a variable and then use `clearInterval(id)` to stop the interval from running
```js
const id = setInterval(() => {
    console.log('hello')
}, 3000)

// some amount of time ...

clearInterval(id); // this will stop the interval with that id
```

### filter
- callback needs to return a boolean
- if the callback returns true for any element, that element will be added to the filtered array
- otherwise it's ignored
```js
const numbers = [1,2,3,4,5];

numbers.filter(n => {
    return n >= 3;
})

// new array
> [3,4,5]
```

### every / some
- returns true or false
- `every` tests if every element passes some test
- `some` tests if any element passes some test
- the callback you pass in needs to return true or false as well
```js
const exams = [100, 80, 90, 65, 50];

exams.every(score => score > 70)
// returns false

exams.some(score => score > 70)
// returns true

// this example checks if all numbers in an array are even
function allEvens (nums) {
    return nums.every(num => num % 2 === 0)
}
```

### reduce
- takes an array and reduces it down to some single value
- takes 2 params (accumulator, currentElement)
```js
[3,8,20].reduce((accumulator, element) => {
    return accumulator + element; // this could be any other operation too, such as multiplication, or min or max
})

// returns the total of that list
// 31
```

## Some newer JavaScript features
### default params
```js
// old way to get a default param
function rollDie(numSides) {
    if (numSides === undefined) {
        numSides = 6;
    }
    return Math.floor(Math.random() * numSides) + 1;
}

// new way to get a default param
function rollDie(numSides = 6) {
    return Math.floor(Math.random() * numSides) + 1;
}

// order matters. 
// default params should come last in the params, otherwise the interpreter won't know if the first ones are not passed in
```

### spread
```js
Math.max(1,2,3,7,9) // this expects a bunch of args, for example, and it returns the highest

// if you want to use an array to pass in, it needs to be expanded
// we can use the spread syntax `...`

nums = [1,2,3,4,5]
Math.max(...nums)

// ... can also be used on strings
...'hello'
> ['h', 'e', 'l', 'l', 'o']

// spread can be used with objects too
const cat = {legs: 4, family: 'feline'}
const anotherCat = {...cat}

// and you can use 2 objects together to combine their keys into one
const catDog = { ...cat , ...dog }
```

### rest
`arguments` object:
- available inside every function
- it's an array-like object with a length property
- contains all the arguments passed into a function
- not available inside arrow functions

instead, we can use the rest syntax (also `...` but in params of function)
```js
// ...nums will collect all arguments passed and put it into an array
function sum(...nums) {
    return nums.reduce((accum,elem) => accum + elem);
}
```

### destructuring arrays
- a clean syntax to 'unpack'

```js
const scores = [12312, 421412, 53242, 21321, 41255]

const [gold, silver] = scores;
> gold is equal to 12312
> silver is equal to 421412

// can also use rest '...' to capture all the remaining ones into an array
const [gold, silver, ...allOthers] = scores;
> allOthers is equal to [53242, 21321, 41255]
```

### destructuring objects
```js
someObj = {
    property: 'value',
    otherProp: 'otherVal'
}

const { otherProp } = someObj;
> otherProp is equal to someObj.otherProp

// by adding a colon after the property you can rename it
const { otherProp: newNameForProp } = someObj;
> newNameForProp is equal to someObj.otherProp
```

### destructuring params
```js
function fullName(user) {
    const {firstName, lastName} = user;
    return `${firstName} ${lastName}`
}
```