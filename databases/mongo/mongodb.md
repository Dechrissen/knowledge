# Mongo DB
- commonly used with Node.js and Express (MEAN & MERN stacks)
    - MEAN: mongo express and node
    - MERN: mongo express react and node
- document-based distributed database
- plays well with JavaScript

## Installation
- need to install the MongoDB server first which lets the service run in the background
    - MongoDB Compass is a GUI tool
- then need to install MongoDB shell to run commands

## Basics of mongo shell
- run the command `mongosh` for Mongo shell
### Make a database
```
use someDatabase
```

### BSON (Binary JSON)
- Mongo stores data in BSON (Binary JSON)
- takes up less space

## CRUD operations

### Create (insert)
- data needs to be inserted into Collections
- if a Collection doesn't exist, it will be created automatically
```
show collections
```
- the above should print none, because none exist at first
to insert one item into a collection `dogs`:
```
db.dogs.insert({name: "Charlie", age: 3, breed: "Corgi", catFriendly: true})
```
- the above will create `dogs` collection
- the `_id` Primary Key property will be created automatically for each entry in a collection
```
db.dogs.insert({name: "Wyatt", age: 14, breed: "Golden Retriever", catFriendly: false}, {name: "Tanya", age: 17, breed: "Chihuahua", catFriendly: true})
```

### Read (`Find()`)
finds all documents in the collection `dogs`
```
db.dogs.find()
```
if you pass in an argument, you can find all based on some parameter (or multiple parameters)  
this returns a "Cursor" (a reference to the results)
```
db.dogs.find({breed: "Corgi"})
```
we can also find only one with findOne() and this will return the actual document, not a Cursor
```
db.dogs.findOne({breed: "Corgi"})
```

### Update

`updateOne()` takes two arguments: a Selector and an Operator (like `{$set: {}`})  
you can pass in one or multiple new key/value pairs
```
db.dogs.updateOne({name: "Charlie"}, {$set: {age: 4}})
db.dogs.updateOne({breed: "Dachshund"}, {$set: {age: 5, breed: "Lab"}})
```
you can also pass in a non-existent key/value pair and it will add it to the document
```
db.dogs.updateOne({name: "Charlie"}, {$set: {color: 'orange'}})
```

`updateMany()` can match on multiple documents, and apply the update to many documents at once
```
db.dogs.updateMany({catFriendly: true}, {$set: {isAdopted: false}})
```

### Delete
`deleteOne()`
```
db.dogs.deleteOne({name: "Tanya"})
```
`deleteMany()`
```
db.dogs.deleteMany({isAvailable: false})
```
to delete all the documents in a collection at once, you can use `db.collection.deleteMany({})`


## Operators
- `$set` to set a new value for some parameter(s)
    - `{$set: {age: 4}}`
- `$currentDate` to set the value of some parameter to the current date
    - e.g. `{ $currentDate : {lastModified: true} }` will set the value of lastModified to the current date. kind of confusing because you need to set it to `true`

### Accessing nested parameters
if you have some object with nested objects:
```
{
    "_id": ObjectId("fija39fjaf"),
    "name" : "Matt",
    "age" : 4,
    "personality": {
        "catFriendly" : true,
        "childFriendly" : false
    }
}
```
to access the nested parameters, the syntax uses quotes and dot operator:
```
db.dogs.find({'personality.catFriendly': true})
```

### Query operators
- greater than `$gt`
- less than `$lt`
- or `$or: [<expressions>]`
- and `$and`
- in (some Array) `$in`
- not in (some Array) `$nin`
- etc.

example, find some documents where `qty` is **greater than** 4:
```
db.stuff.find({ qty: {$gt: 4} })
```

example, find some documents where the breed is **in** some array of breeds:
```
db.dogs.find({ breed: { $in: ['Dachshund', 'Corgi', 'Chihuahua'] } })
```

## Mongo data/model relationships
[Official MongoDB blog psot about this](https://www.mongodb.com/blog/post/6-rules-of-thumb-for-mongodb-schema-design)  

- many entities are commonly stored in a database
- they all are connected to each other via different relationships
- in NoSQL databases like mongo, we have more freedom in defining the relationships between entities (whereas with relational DBs liek SQL there is only one approach: using multiple tables to link fields together)

### One-to-few relationship
- embed the data directly in the document
```
{
    name: "John",
    addresses: [
        {street: "fafefwef", city: "fkuahf"},
        {street: "fafefeasfefwef", city: "fkufseriogfhasef"}
    ]
}
```
Mongoose will automatically treat embedded objects as their own documents, and assign them an `_id`. You can disable this in the schema by doing:
```javascript
const userSchema = new mongoose.Schema({
    first: String,
    last: String,
    addresses: [
        {
            _id: { _id: false }, //this
            street: String,
            city: String,
            state: String,
            country: String

        }
    ]
})
```

### One-to-many relationships
- Store your data separately, but then store references to document IDs somewhere inside the parent
- similar to a SQL-ish approach
```
{
    name: "John",
    addresses: [
        ObjectID('4892798274'),
        ObjectID('4877798274'),
        ObjectID('4894345534')
    ]
}
```

Example with Farm / Product schemas
```javascript
// first make the Product schema and model
const productSchema = new Schema({
    name: String,
    price: Number,
    season: {
        type: String,
        enum: ["Spring", "Summer", "Fall", "Winter"]
    }
})

const Product = mongoose.model("Product", productSchema);

// then make the Farm schema and model
const farmSchema = new Schema({
    name: String,
    city: String,
    products: [{ type: Schema.Types.ObjectId, ref: 'Product' }] // here is where we have a reference to our Product schema
                                                                // this tells mongoose that this is an array of ObjectIds
                                                                // and each one is connected to our product model
})

const Farm = mongoose.model("Farm", farmSchema);
```
then, we can make a farm, then push a product to that farm:
```javascript
const farm = new Farm({ name: 'Potbelly Farms', city: "Coram, NY" });
const melon = await Product.findOne({name: 'melon'});
farm.products.push(melon); // since we did the ref: 'Product' in the farmSchema, this will push the ObjectId to the products array in the farm model
await farm.save();
```

#### Populating the reference IDs
After we have an array of ObjectIds, we can use `.populate()` to actually populate the array with the actual documents those IDs represent
```javascript
Farm.findOne({name: "Potbelly Farms"})
    .populate('products') // the name of the `products` field on the farmSchema
    .then(farm => console.log(farm));
```

### One-to-bajillions relationship
- With thousands or more documents, it's efficient to store a reference to the parent on the child document

```javascript
// one
const userSchema = new Schema({
    username: String,
    age: Number
})

// to bajillions
const tweetSchema = new Schema({
    text: String,
    likes: Number,
    user: {type: Schema.Types.ObjectId, ref: 'User'}
})

const User = mongoose.model('User', userSchema);
const Tweet = mongoose.model('Tweet', tweetSchema);
```

