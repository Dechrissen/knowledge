# JavaScript Prototypes, Classes, OOP

## Prototypes
- mechanism by which objects inherit methods and stuff from each other
- it's a "template object"
- `__proto__` -- a reference to the blueprint object "prototype" for whatever type it is, e.g. Array, String
- `Array.prototype` will have all the methods available on that prototype template object
- you can add new methods to thr prototypes, e.g.
```js
String.prototype.nonsense = () => {
    console.log('Some nonsense');
}

const cat ="cat";
cat.nonsense()
> "Some nonsense"
```

## Objects Oriented Programming
- helps to organize code
- break things into patterns of objects
- the `new` keyword makes it refer to a new Object, instead of the window object

### Constructor functions
 - old way, without the Class keyword
```js
// constructor function, style with capital letter
function Color(r,g,b) {
    this.r = r;
    this.g = g;
    this.b = b;
}

// this is how to define a method on a class
Color.prototype.rgb = function () {
    const { r,g,b } = this;
    return `rgb(${r}, ${g}, ${b})`;
}

Color.prototype.rgba = function (a = 1.0) {
    const { r,g,b } = this;
    return `rgba(${r}, ${g}, ${b}, ${a})`;
}

const c1 = new Color(255,40,100);
const c2 = new Color(265,40,10);
```

### Class keyword
- new way of making classes, instead of constructor functions
- more compact

```js
class Color {
    constructor(r, g, b, name) {
        this.r = r;
        this.g = g;
        this.b = b;
        this.name = name;
    }
    rgb () {
        // can also call other methods from within methods
        console.log(this.greet());

        const { r,g,b } = this;
        return `rgb(${r}, ${g}, ${b})`;
    }
    greet () {
        return `Greetings from ${this.name}!`;
    }
}

const c1 = new Color(255,40,100,'tomato');
const c2 = new Color(265,40,10,'dandelion');
```
### Extends and Super keywords
- has to do with inheritance in Classes

```js
class Pet {
    constructor (name, age) {
        this.name = name;
        this.age = age;
    }
    eat () {
        return `${this.name} is eating`
    }
}

class Cat extends Pet {
    constructor (livesLeft = 9) {
        // the super keyword is a reference to the super class (the one we're extending from)
        super(name, age);
        this.livesLeft = livesLeft;
    }
    meow () {
        return "Meow";
    }
}

class Dog extends Pet {
    bark () {
        return "Bark";
    }

    // the eat() function in here is the one that this class will have, since it's called the same in the super class
    eat () {
        return `${this.name} is scarfing down his food`
    }
}
```