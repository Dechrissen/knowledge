# Mongoose ODM with Node.js
- Mongoose is an Object Document/Data Mapper
- it maps documents coming from a database into usable JavaScript objects
- provides ways fo rus to model out our application data and define a schema
- it offers easy ways to validate and build complex queries from JavaScript

### installing
```
npm install mongoose
```
set up in a project:
```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/movieApp')
.then(() => {
    console.log('CONNECTION OPEN');
})
.catch(err => {
    console.log(err);
});
```

### a `Model` in mongoose is a representation of the database coming from Mongo
- first you make a Schema:
    - e.g. movie Schema for defining types for some movie document
```javascript
const movieSchema = new mongoose.Schema({
    title: String,
    year: Number,
    score: Number,
    rating: String
})
```
- then you take the Schema, and tell mongoose that you want to make a Model using that Schema
```javascript
const Movie = mongoose.model('Movie', movieSchema)
// from this, mongoose will make a collection 'movies' lowercased and pluralized
```
- now you can make new instances of the Movie class (Create in CRUD acronym)
```javascript
// this is a new instance of Movie
const amadeus = new Movie({title: 'Amadeus', year: 1986, score: 9.2, rating: 'R'});
amadeus.save();
```

#### Creating
you can also use `Model.insertMany()`
```javascript
// this way, you dont need to create a new instance of Movie, and dont need to run movie.save() on each. It does this automatically.
Movie.insertMany([
    {title: 'Amadeus', year: 1986, score: 9.2, rating: 'R'},
    {title: 'Amadeus2', year: 1986, score: 9.2, rating: 'R'},
    {title: 'Amadeus3', year: 1986, score: 9.2, rating: 'R'}
])
    .then(data => {
        console.log('Successful add');
        console.log(data); // this will log all the added entries
    })
```

#### Finding (retrieving)
- same syntax as in mongo shell
- `Model.find()`, `Model.findOne()`, `Model.findById()`, etc.
- the result of these is _like_ a Promise, but not actually one

```javascript
// Queries are thenable, i.e. you can use .then (but they are not actual Promises)
Movie.find({rating: 'R'}).then(data => console.log(data));
```

#### Updating
- `Model.updateOne(query, updatedValue)`
- `Model.updateMany(query, updatedValue)`

```javascript
// two args: query and update value
// this updates all movies with title Amadeus to have score of 10
Movie.updateMany({title: "Amadeus"}, {score: 10})
    .then(res => {
        console.log(res)
    })
```
- `Model.findOneAndUpdate()` to update one and then return the updated document once the update is applied
    - the default behavior will **send back the old, un-updated doc**
    - you can pass in `options` object as the third arg
    - if you pass in `{new: true}` as that object, it will return the NEWLY UPDATED document instead

#### Deleting
- `Model.remove()` (just sends a remove command to mongo, no mongoose stuff involed)
- `Model.deleteOne()`
- `Model.deleteMany()`
- `Model.findOneAndDelete()` this returns the deleted document

## Schema validations
- these apply when creating new documents of some Schema
```javascript
// default is false for the `required` fields
const productSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true,
        maxlength: 20
    },
    price: {
        type: Number,
        required: true,
        min: 0
    },
    onSale: {
        type: Boolean,
        default: false
    },
    categories: {
        type: [String],
        default: ['cycling']
    },
    qty: {
        online: {
            type: Number,
            default: 0
        },
        inStore: {
            type: Number,
            default: 0
        }
    }
});
```

- in order to apply the validations when **updating** documents (since they don't run by default) you need to pass in the `options` object as the third arg when running something like `Model.findOneAndUpdate(query, newValue, options)`
    - `{ new: true, runValidators: true }`
    - this will make sure the NEW value is returned, and the Schema validators WILL run
```javascript
// this will throw an error since our validators in the Schema said prices cannot be less than 0
Product.findOneAndUpdate({name: "Bike Helment"}, price: {-9.99}, {new: true, runValidators: true})
```

- Custom validation error messages in Schema 
```javascript
const productSchema = new mongoose.Schema({
    price: {
        type: Number,
        required: true,
        min: [0, "Price must be greater than 0"] // this is the syntax: first item in Array is the value, second is the custom error message
    },
    size: {
        type: String,
        enum: ['S', 'M', 'L', 'XL'] // this is a validator that will make sure the value is in this array
    }
})
```

### Model instance methods
- method that's available on every instance of some class (vs. class (or Model) method which is on the class (or Model) itself)

```javascript
// after definiing some Schema `productSchema` ...
productSchema.methods.greet = function() {
    console.log(`Hello from ${this.name}`); // here, `this` refers to the instance of the Product model
}

const p = new Product({name: 'bag', price: 10});

p.greet(); // then we can access the method on any instance of Product
```

Better example, using some method that will toggle whether a product is "on sale" or not:
```javascript
productSchema.methods.toggleOnSale = function() {
    this.onSale = !this.onSale;
    return this.save(); // since this is thenable
}

await someProduct.toggleOnSale();
```

### Model static methods
- method available on the Model itself

example "Fire Sale" that puts every product on sale to cost $0
```javascript
productSchema.statics.fireSale = function () {
    return this.updateMany({}, {onSale: true, price: 0}); //  here, `this` refers to the Product model itself, as opposed to some instance of it
}

// then we can call this method on the model
Product.fireSale().then(res => console.log(res));
```

## Mongoose virtuals (getters and setters)
- these are things which behave as if they're regular properties of some model, but actually you can pass in a function to return some value
```javascript
const personaSchema = mongoose.Schema({
    first: String,
    last: String
})

// this is a virtual "get" function, which behaves like it's a property of the Schema
personaSchema.virtual('fullName').get(function() {
    return `${this.first} ${this.last}`
})

const Person = mongoose.model('Person', personaSchema);

const derek = new Person({first: 'Derek', last: 'Andersen'});

derek.fullName // this acts as a property, but it won't appear in the instance itself. only when called explicitly
> 'Derek Andersen'


// there are also virtual "set" functions `set()` that can update the `first` and `last` properties via changing the value of the full value fullName for example
```

## Mongoose middleware
- runs some code before or after some actions happen to the db, like CRUD operations

### types of middleware
```javascript
var schema = new Schema(..);

// pre hook
// runs before every time instance.save() is called
schema.pre('save', async function() {
    console.log('About to save ... ')
})

// post hook
// runs after every time instance.save() is called
schema.post('save', async function() {
    console.log('Just saved!')
})
```
other examples of actions for middleware:
- update
- remove
- delete
- etc.

## Error handling in Mongoose
Mongoose has several types of Errors:
- ValidationError
- CastError

We can define our own functions to handle these errors, e.g.
```javascript
// Error-handling middleware:
app.use((err, req, res, next) => {
    if (err.name === 'ValidationError' ) {
        err = handleValidationError(err);
        next(err); // so we continue to the next route/middleware in Express
    }   
})

// custom Mongoose Error handler:
function handleValidationError (err) {
    // do something with the err
    // ........
    // console.dir(err) to get the properties of it, like `name`, idk
    // then ...
    return err; // so it goes back to our middleware, and gets passed to `next()` (to go to the next middleware or whatever)
}
```

## Mongoose middleware on deletion
- if you have some linked documents/data, and you want to delete (for example) all Products in a Farm when that Farm is deleted ...
- you can use Mongoose middleware on the Schema for that thing you're deleting

```javascript
// this is post middleware, which runs after the query (which is what we want becuase we need access to the deleted document)
farmSchema.post('findOneAndDelete', async function (farm) {
    if (farm.products.length) {
        const res = await Product.deleteMany({ _id: { $in: farm.products } }); // deletes all the products that are in some farm's `products` array
        console.log(res);
    }
} )
```