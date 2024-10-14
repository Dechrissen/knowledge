# Express

- node package
- web dev framework
- fast, minimal
- helps get servers up and running with node

## What is a framework
- the framework is in control of the flow of the code (vs. a library)
- provides the structure for making full applications
- _Ruby on Rails_ is another one (very opinionated/controlling)

## The basic setup
- the `req` object created by express represents the client's request
    - based on the incoming HTTP request
- the `res` object created by express represents our reponse
- both get passed into the callback function for some `app.use` or `app.get` to route requests
    - e.g. `app.get('/cats', (req,res) => {res.send("Cat request")})`
- `res` has methods on it, like `res.send()` for example
    - the `res.send()` method can be used to send text, HTML, or JSON (it's versatile; it will send with the appropriate headers)
    - any time we use `res.send()`, we're done for that particular request. So you can't have 2 `res.send()`s for one response
```js
const express = require('express');

const app = express();
const port = 3000;

// this will run when ANY request is received
app.use((req, res) => {
    // req and res are created by express (the request and response objects, which we can access in here)
    console.log("Got a new request")
})

app.listen(port, () => {
    console.log("Listening on port", port)
})
```

### Routing requests
- how to route endpoints requested, like `/dogs`, `/home`
- routes are match IN ORDER

```js
const express = require('express');

const app = express();
const port = 3000;

// GET
// this is a route
app.get('/cats', (req,res) => {
    console.log("Cat GET request")
})

// this is a route
app.post('/dogs', (req,res) => {
    console.log("Dog POST request")
    res.send('Woof')
})

// generic wildcard route (needs to come at the end of the routes)
app.get(/(.*)/, (req, res) => {

})

app.listen(port, () => {
    console.log("Listening on port", port)
})
```

### Path parameters
- Using colon to match patterns

```js
app.get('/r/:subreddit', (req,res) => {
    const { subreddit } = req.params; // params is a special property on `req` that we have access to
    res.send(`Browsing the ${subreddit} subreddit.`)
})
```
- You can have more than one colon parameter
```js
app.get('/r/:subreddit/:postId', (req,res) => {
    const { subreddit, postId } = req.params; // params is a special property on `req` that we have access to
    res.send(`Viewing Post ID ${postId} on the ${subreddit} subreddit.`)
})
```

### Working with query strings
- we can use the `req` object that express makes for us
- e.g. if we had the url `website.com/search?q=dogs&color=red` then the `req` object would automatically have a propery `query`:
    - `{q: 'dogs', color: 'red'}`
```js
app.get('/search', (req,res) => {
    const { q } = req.query; // params is a special property on `req` that we have access to
    if (!q) {
        res.send("Nothing found if nothing searched") // if there is no q param in the url
    }
    res.send(`Search results for ${q}`)
})

```

### Using `nodemon` to automatically restart server
- QoL for restarting server whenever the code is changed

install it globally, then you can use `nodemon` instead of `node` to run apps
```bash
npm i -g nodemon
```

## Importing `json` data files to work with
In `index.js` ...

```js
// the data file is in the same directory as index.js
const redditData = require('./data.json');

app.get('/data', (req,res) => {
    const { subreddit } = req.params;
    const data = redditData[subreddit];
    res.render('subreddit.ejs', { ...data }); // ... is the spread operator so we have access to all the properties of the data without using dot operator
})
```

## Serving static assets
- we can using the `express.static` built-in middleware
- this way, we don't need to reference relative paths in the html head, since the entire contents of some static files are being served
- we can group all our static files together in a directory `public`
    - `public/style.css`
    - `public/app.js`
    - `public/imgs`

```js
// simply add this to the index.js file for the project
app.use(express.static(path.join(__dirname, '/public')));
```

then in the html head of some `ejs` template ...
```html
<link rel="stylesheet" href="/style.css">
```

## Quick html form review
- you can specify the `method` (http verb) in the form
```html
<form action="/tacos" method="get">
    <input type="text" name="meat">
    <input type="number" name="qty">
    <button>Submit</button>
</form>

<form action="/tacos" method="post">
    <input type="text" name="meat">
    <input type="number" name="qty">
    <button>Submit</button>
</form>
```

## Parsing the Request body
- the `req` object that express creates in the callback function for some get/post request needs to be parsed
- we can use middleware built-in: `app.use(express.urlencoded({extended: true}))` (this is for parsing incoming requests with URL encoded payloads)
    - for parsing incoming requests with JSON encoded payloads, we would use `app.use(express.json())`
- then we can access `req.body`

in `index.js`...
```js
app.use(express.urlencoded({extended: true}));

app.POST('/tacos', (req,res) => {
    console.log(req.body) // here we can parse and log the request body, which will contain an object
    res.send("POST /tacos request");
});

```
in the console ...
```
{ meat: 'pork', qty: '1' }
```
because this is what we passed into our POST request html form.

### Doing stuff with the req.body
```js
app.POST('/tacos', (req,res) => {
    const { meat, qty } = req.body; 
    res.send(`You ordered ${qty} ${meat}s`);
});
```

## Redirecting the user after form submission
- by specifying a path in `res.redirect('/homepage')`, the `res` generated by express will include a redirect status code (`302` by default)
- the `res` also includes the path `/homepage` under the location header
- your browser will automatically make a new request with this path `/homepage`
```js
app.POST('/tacos', (req,res) => {
    const { meat, qty } = req.body;
    // do something with meat and qty
    res.redirect('/tacoshomepage'); // then redirect the user
});
```

## The `uuid` package
- This package helps us to create unique IDs for things like comments, or some other resource
- It's an `npm` package for Node.js
- `npm i uuid`
- Then require it in `index.js`
- then we can call it whenever we need a new unique ID

```js
const {v4 : uuid } = require('uuid'); // this is destructuring where we can rename something (naming it uuid)
uuid(); // and call it whenever we need a new unique ID

```

## Method override (`method-override` npm package)
- allows us to use HTTP verbs that the client doesn't support (PATCH, PUT etc)
- `npm i method-override`
```javascript
const methodOverride = require('method-override');
app.use(methodOverride('_method')); // this is for using more HTTP verbs, like PUT PATCH DELETE. the 'method' in an HTML form will be overridden
```
then, in HTML `<form>`s we can use the `_method` query string:
```html
<form action="/campgrounds/<%= campground._id %>?_method=DELETE" method="POST">
```
and back in `app.js` we can use these HTTP verbs:
```javascript
// DELETE for deleting a campground from the show page
app.delete('/campgrounds/:id', async (req,res) => {
    const { id } = req.params;
    await Campground.findByIdAndDelete(id);
    res.redirect('/campgrounds');
})
```

## Express middleware
Used for:
- authentication
- keeping people logged in
- cookies
- etc.

Middleware are functions that run during the request/response lifecycle. In fact, Express as a framework is just a series of middleware calls.

`request --> MIDDLEWARE --> response`

- each middleware function has access to the `request` and `response` objects
- middleware can end the HTTP request by sending back a response with methods like `res.send()`
- OR middleware can be chained together, one after another by calling `next()`

### `morgan` (an external request logging middleware for node.js)
- `npm i morgan`
```javascript
const morgan = require('morgan');

app.use(morgan('tiny'));

// this will log the 'tiny' format of the req object, for every request made, in the console
```

### Defining our own middleware
- whatever function you put in an `app.use()` will be called for every single request
- if you don't call `next()`, then it will hang there and never move on
- you need the third parameter (`next`) there in the callback in order to access `next`

```javascript
app.get('/', function (req, res, next) {
    console.log('Here is some middleware!');
    next(); // this moves to the next middleware in the path (could be another middleware, could be a route handler like app.get(), etc.)
})
```

### More middleware stuff
- you can do more stuff with the `req` object in middleware

```javascript
app.use((req,res,next) => {
    console.log(req.method.toUpperCase()); // this will log GET in the console for example
    req.method = 'GET'; // or in this wacky example, you can change every request to be a GET request (in the express `req` object, at least)
    next();
})
```

### 404 handler
```javascript
// if this is at the end of all the routes in the file, this will catch everything
app.use((req,res) => {
    res.status(404).send('not found');
})
```

### Passing functions into regular routes as middleware
```javascript
function verifyPassword (req, res, next) {
    const { password } = req.query; // we're destructuring and taking the `password` arg from the query string in the url
    if (password === 'abc123') {
        next();
    } else {
        res.send('This page is password-protected!');
    }
}

// the 'next' callback (what's inside the req,res callback here) will be run if the `verifyPassword` function ever calls `next()`
// otherwise, it won't ever run the callback in this route
app.get('/secret', verifyPassword, (req,res) => {
    res.send('Secret');
})
```

## Error handling in Express
- handling APIs being down
- handling incomplete forms
- etc.

Error handling middleware callbacks take 4 parameters (err, req, res, next).  
They also need to be after all the other middleware. This will trigger when any errors occur.
```javascript
app.use((err, req, res, next) => {
    console.log('Error');
    next(err); 
})
```
When `err` is passed into `next()`, Express _knows_ it's an error. It then passes `err` on to the next _error handling_ middleware. If nothing is passed into `next()`, then it will go to the next _non-error handling_ middleware.

### Custom Error classes in Express
in some new file `AppError.js` ...  
we can define a new custom Error class:

```javascript
class AppError extends Error {
    constructor(status, message) {
        super();
        this.status = status;
        this.messsage = message;
    }
}

module.exports = AppError;
```

in `app.js`...
```javascript
const AppError = require('./AppError');

throw new AppError(401, 'authentication required');
```

Express is looking for `status` property on Errors in the built-in Error handler `app.use((err, req, res, next) => {})`  
Normal JS Errors do not have this property, so if we make our own custom Error class, we need to add the `status` property.  

Here's an example custom Error handler:
```javascript
app.use((err, req, res, next) => {
    // desctructuring the `err` object and assigning default status and message if they aren't present on the `err` object
    const { status = 500, message = 'Something went wrong!' } = err;
    
    // sending a response with those values
    res.status(status).send(message);
})
```

### Async Error handling
_Note: Apparently we don't need to worry about any of this in Express 5._  

A tedious way:
- to wrap code within our `async` callbacks in our route handlers in _try/catch blocks_ and then pass the `e` from `catch(e)` to `next(e)` in the catch block

A better way:
- to define a function that we pass our entire route handler callback into

```javascript
// here, `fn` refers to the entire callback function normally passed into a route handler in Express
function wrapAsync(fn) {
    return function(req, res, next ) {
        fn(req, res, next).catch(e => next(e));
    }
}

// then we can use it in our routes ... (async errors will be handled)
app.post('/products', wrapAsync(async (req,res,next) => {
    const newProduct = new Product(req.body);
    await newProduct.save();
    res.redirect(`/products/${newProduct._id}`);    
}))
```