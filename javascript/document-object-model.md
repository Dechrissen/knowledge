# JavaScript Document Object Model (DOM)

- HTML and CSS go in, JavaScript objects come out
    - one for the body
    - one for the stuff in the body
    - one for the head
    - etc

## The Document object
- entry point into the DOM
- contains representations of all content on a page
- has useful methods and properties
- root of the "tree structure" of the DOM (with child nodes from elements inside other elements)

### Selecting elements with JavaScript

```js
// fetch the object that represents some element on the page
// doesn't fetch html, but rather the JS object that represents it
const banner = document.getElementById('banner');
```

#### HTMLCollections
```js
// you can also select multiple elements at once (by tag name here)
// returns an "htmlcollection"
const images = document.getElementsByTagName('img');

// you can use indexes with these HTMLCollections
images[2]

// and you can iterate
for (let img of images) {
    img.src = "some_new_img_path" // you can directly change the `src` attribute, this will change all images on the page
}
```

- HTMLCollections are made up of `Elements` in JS

```js
// get all elements of a class
document.getElementsByClassName('square');
```

#### querySelector (new way of selecting single elements)
- select them by writing them how you would in CSS
```js
// Finds first h1 element
document.querySelector('h1'); 

// Find class
document.querySelector('.square')

// Find id
document.querySelector('#someId')

// can use nth-of-type
document.querySelector('a:nth-of-type(2)')

// attribute selector
document.querySelector('input[type="checkbox"]');

// select all (returns a collection of matching elements)
document.querySelectorAll('img')
```

#### innerHTML, textContent, innerText
```js
const h1 = document.querySelector('h1');

// shows the element
console.dir(h1)

// shows the inner text of h1
h1.innerText

// does the same thing but shows all elements in some text, such as elements that are hidden
h1.textContent

// update the text
h1.innerText = "some new text"

// innerHTML includes all tags as well. this will add an <i> tag inside the h1
h1.innerHTML = "<i>new heading</i>"
```

#### Attributes
```js
const firstLink = document.querySelector('a')
firstLink.getAttribute('href')
```

### Changing styles with JS
```js
const h1 = document.querySelector('h1');

// using single attributes, one at a time
h1.style.color = 'green';
h1.style.fontSize = '3em';
h1.style.border = '2px solid pink';

// how to get the property value of an existing element
window.getComputedStyle(h1)
// > returns an object with all the attributes for that element's CSS 

// get font size
window.getComputedStyle(h1).fontSize
// > '32px'
```

### Manipulating classes with JS
```js
const h2 = document.querySelector('h2');

h2.getAttribute('class')
> null
// here the h2 has no class

h2.setAttribute('class', 'purpleClass')
// this will set the class of the h2 to purpleClass
// note: it overwrites its class attribute entirely!!

// Class list object

// this will add the purpleClass to its existing classes
h2.classList.add('purpleClass')

//can also remove
h2.classList.remove('purpleClass')

//check for class
h2.classList.contains('purpleClass')
> true

//toggle class
h2.classList.toggle('purpleClass')
```

### Properties and methods for relative elements
```js
const p = document.querySelector('p');

// get parent
p.parentElement
> div

// can chain them
p.parentElement.parentElement
> body

// return collection of child elements
p.children
> HTMLCollection [b, ul, a, a]

// siblings
p.nextElementSibling
> p
p.previousElementSibling
> p
```
#### Creating/inserting new elements
```js
// this creates a new element
const newimage = document.createElement('img')

// set attributes
newimage.src = 'link/to/img'

// then you need to append it to the document (last child on page)
document.body.appendChild(newimage)

// append() by itself
const p = document.querySelector('p');
p.append('some text to add') // this append to the inside of the element, at the end
// only works because it's text

// prepend
const p = document.querySelector('p');
p.prepend('text which will go at the beginning');

// add an adjacent element (not a child of some element)
p.insertAdjacentElement(position, element);
// position can be:
// 'beforebegin'
// 'afterbegin'
// 'afterend'
// 'afterbegin'
```

#### Removing elements
```js
// removes the element itself
h1.remove();
```