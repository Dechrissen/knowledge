# JavaScript Events

Responding to user inputs and actions.

- clicking
- dragging
- hovering
- form submission
- key presses
- copy/paste

## Clicks
1. can be written in html markup `<button onclick="alert('you clicked the button');">BUTTON</button>`
2. properties can be added to elements in javascript

```js
btn.onclick = () => {
    console.log('clicked');
}
```
3. event listeners
```js
btn.addEventListener("click", function() {
    console.log('clicked the button');
})

// a third option can be added to `addEventListener`, {once: true} which makes it only happen once
btn.addEventListener("click", someFunction, {once: true});
```

### This keyword in event handlers
```js
// `this` refers to the thing being clicked
heading.addEventListener('click', function() {
    this.style.color = 'red';
})
```

## The Event object
- gets passed into the event listener and we can use it to get data like:
    - which keys wre pressed
    - coordinates on screen
    - etc.
```js
input.addEventListener('keydown', function(evt) {
    // do something with `evt`
    console.log(evt.key); // this gets the key pressed (the character generated)
    console.log(evt.code); // this gets the code of the key pressed (the code, like leftshift vs rightshift)
})

// can add event listener on the whole window
window.addEventListener('keydown', function(evt) {
    console.log(evt.code);
})
```

## Form events
- default behavior of form submission changes the url of the page and sends the data to the location in `action` attribute, e.g. 
```html
<form action="/endpoint">
    <input type="text" />
    <button>Submit form</button>
</form>
```
```js
const form = document.querySelector('#someForm');
form.addEventListener('submit', function(evt) {
    evt.preventDefault(); // this line will prevent the redirect to the new location in `action` attribute
    console.log('form submitted');
})
```

### input field value attribute
- when text is typed in a text input field, we can access it with `input.value`
```js
const input = document.querySelector('#someInput');

// we can add an event listener on form submission
form.addEventListener('submit', function(evt) {
    evt.preventDefault();
    console.log(input.value);
});
```

### change and input events
```js
input.addEventListener('change', function() {
    // fires when the input field changes, but not till user leaves the field
})

input.addEventListener('input', function() {
    // fires when the input field changes (live)
})
```

### stopPropagation (event bubbling)
```js
button.addEventListener('click', function (evt) {
    console.log('something');
    evt.stopPropagation(); // prevents the event from bubbling up to the outer elements if this button is contained in others that have click events as well
})
```

### event delegation
- add events to a container (like a <ul>) for all <li>s within it
```js
ul.addEventListener('click', function(evt) {
    evt.target.remove(); //this removes the target that was clicked inside of <ul>, regardless of what element type it is
})

ul.addEventListener('click', function(evt) {
    evt.target.nodeName === 'LI' && evt.target.remove(); //this removes the target that was clicked inside of <ul> if and only if it's an <li>
})
```