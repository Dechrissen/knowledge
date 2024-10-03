# Node.js
- javascript runtime environment
- if you run `node` in terminal, you'll be in the Node REPL (read evaluate print loop)
- `global` is the global scope object, like `window` in the browser

### Process object and `argv` array
- `process` is a global object with a bunch of relevant methods and properties, like `process.version`, `properties.cwd()`
```js
process.argv
// property that returns an array containing the command line arguments passed when Node.js process was launched
// first element will be process.ExecPath (the location of the node executable)
```

#### Getting user input on command line
```js
// use process.argv
const input = process.argv[2]; // this gets the third element on the command line
// e.g. from `node index.js this_one_here`
```

### Node has built-in methods
- `fs` for filesystem stuff
- `http` for http requests

```js

```

### Exporting modules (`module.exports`)
in some file `math.js` you can define some variables, e.g.
```js
const PI = 3.14;
const add = (x,y) => x + y;

// export them in the file
module.exports.add = add;
module.exports.PI = PI;
```
then in some other file, `app.js`, you can require `math.js`
```js
const math = require('./math');
// OR you can destructure it to get only certain variables
const {add, PI} = require('./math'); // in this case, you can just reference `add` and `PI` without referencing `math.`

console.log(math.PI);
```
### require an entire directory (`index.js`)  
if you have a few files in `index.js`, you can reference other js files in whatever folder `index.js` is in, e.g.  
- `stuff/index.js`
- `stuff/one.js`
- `stuff/two.js`
- `stuff/three.js`

then in `index.js`,
```js
const one = require('./one');
const two = require('./two');
const three = require('./three');

const all = [one, two, three];
```
then in some other file,
```js
// reference the entire containing directory `stuff`
const stuff = require('./stuff');
```