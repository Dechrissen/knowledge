# React
Documentation: react.dev

- front end library for building User Interfaces
- uses "components"
- components combine HTML and logic into a single reusable function

## JSX
- React makes use of JSX
- it's a syntax extension for JavaScript
- allows you to write HTML in JavaScript
- a library like `babel` will interpret JSX and translate it into regular JavaScript
- you must explicitly close self-closing tags like `<br/>` or `<input/>`
- components can only return a single JSX element (so multiple elements must be enclosed in one top-level element, like a `div`)
    - or if we don't want to return an outer `div`, we can utilize React _fragments_ (`<>` and `</>`):
    ```javascript
    return (
        <>
            <p>blah blah</p>
        </>
    )
    ```

### Evaluating JS expressions within JSX
- we can use `{}` for this
```javascript
function blah() {
    const hello = "hello";
    return (
        <>
            <p>{hello} blah {1 + 4 + 7} blah</p>
        </>
    )
}
```


## Structure of an app
- `App()` is typically the top level component in the `App.js` file
- `index.js` loads in the stuff in `App.js` (usually leave `index.js` alone)

## Writing our own components
In `App.js` ...
```javascript
function ExampleComponent() {
    return <h1>Hello World</h1>;
}

export default function App() {
    return (
        <div className = "App">
            <ExampleComponent/> 
        </div>
    );
}
```
- we typically put components in their own dedicated (same-named) `.js` files, like `ExampleComponent.js` here, and then we export them from that file (`export default ExampleComponent`) and import them in `App.js` using ES6 syntax (`import ExampleComponent from "./ExampleComponent";`)

### Configurable components (using Props)
- using Props, we can create configurable components (ones that load different content, like different images for various Airbnb entries)
- Props are like arguments that we can provide to our components
- They are similar to HTML attributes (like `<div class="some-attribute">`)

```javascript
function App() {
    return (
        <>
            <ExampleComponent person='blah'/>
        </>
    )
}

export default App;
```

Then in the `ExampleComponent` function (usually in its own file), we have access to that `props` as an argument, and `person` is one of its properties:
```javascript
function ExampleComponent(props) {
    return <h1>Hello World, {props.person}</h1>;
}
```

#### Props of non-string types
You can also pass props of different types, like numbers, if you use `{}` (it will interpret it as pure JavaScript), e.g.
```javascript
function App() {
    return (
        <>
            <ExampleComponent num={25} things={[2, 3, 4, 'a', 'b']}/>
        </>
    )
}

export default App;
```

And you can set default values if none are specified (here we are destructuring, so we can reference `num` directly):
```javascript
function ExampleComponent({num = 6}) {
    return <h1>Hello World, {num}</h1>;
}
```

### rendering HTML elements of an array
- You can add JSX elements in an array, and that's what will be rendered on the page when it's returned. They'll all be rendered on the page in order
```javascript
function ExampleComponent({num = 6}) {
    return [<p>hi</p>, <div>blabla</div>, <li></li>];
}
```

## Vite - Helpful starter tool
- run `npm create vite@latest` and this will create a starter React app