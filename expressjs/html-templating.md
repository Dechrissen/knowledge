# Creating dynamic HTML with templating
- instead of creating static HTML that is always the same
- we can create templatized HTML to create dynamic changes

## EJS
In `index.js` ...
- in express, we can specify a templating engine using
```js
app.set('view engine', 'ejs');
```
- by doing this, we don't need to `require('ejs')` in our `index.js`
- instead, express will require it behind the scenes since we set the view engine

then we need to create a `views` directory in our project folder
- in `views`, we can create template `.ejs` files for our pages
- in `.ejs` files, we can write standard html

```bash
mkdir views
touch view/home.ejs
```

then in `home.ejs` we can create a standard webpage with html

then in our `index.js`, 
```js
app.get('/', (req,res) => {
    // this render() function sends a file to the browser to be rendered
    // by default, it looks in the `views` directory
    res.render('home.ejs') 
})
```

### Setting the `views` location in `index.js`
- if we want the app to be able to run from any location (not necessarily from the location where `index.js` exists), we have to change the location of `views` dir
- by default, express looks for `views` in the location where `index.js` was run from

in `index.js` ...
```js
const path = require('path'); // in order to use path.join (built-in with node)

// then
app.set('views', path.join(__dirname, '/views')); // this takes current file index.js and joins the path where it's location with `/views`
```

### Passing variables to template files (views)
in `index.js`...

```js
app.get('/random', (req,res) => {
    const num = Math.floor(Math.random() * 10) + 1
    res.render('random.ejs', {rand: num})
    // if we pass in a second argument (an object) to render(),
    // we can have access to that variable in the `random.ejs` file
    // here, `num` is being assigned to `rand`, which will be available in random.ejs
})
```

### Templating
in `somefile.ejs` we can use EJS template tags  
#### the `<%= %>` tags are for rendering javascript
```html
<h1>Here's a number: <%= rand %></h1>
```
#### the `<% %>` tags are for NOT rendering javascript
so we can use conditionals
```html
<% if (num % 2 === 0) { %>
<h2> That number is even! </h2>
<% } else { %>
<h2> That number is odd! </h2>
<% } %>
```

or loops
```html
<h1>All the Cats</h1>
<ul>
    <% for (let cat of cats) { %>
        <li><%= cat %></li>
    <% } %>
</ul>
```
#### the `<%- %>` tags are for outputting the unescaped value into the template (for partial html templates)
```html
<%- include('partials/head.ejs') %>
```

### Partials (partial templates)
- if some pages on a website share the exact same markup, we can take advantage of partial templates
- make a directory in `views` called `partials`
    - `partials/head.ejs` (for html head)
    - `partials/navbar.ejs`
    - `partials/footer.ejs`

then in the other `ejs` pages where you want to use it
```html
<%- include('partials/head.ejs') %>
<%- include('partials/navbar.ejs') %>

blah html
blah blah html
blah html
blah blah blah html
blah html

<%- include('partials/footer.ejs') %>
```