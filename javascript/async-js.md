# Asynchronous JavaScript

## Call stack
- Last in, first out (LIFO)
- Used by interpreter to keep track of where it is

### Single threaded
- JS is single threaded (one line of code at a time)

### Example with callback
```js
console.log("Something")
setTimeout(() => {
    console.log("Here is the data you requested ...")
}, 3000)
console.log("Something else")

// this will print out:
// Something
// Something else
// then 3 sec later ...
// Here is the data you requested ...
```

### Web APIs in browsers
- browsers come with Web APIs that are able to handle certain tasks in the background (like making requests)
- The JS calls tack recognizes these Web API functions and passes them off to the browser to take care of
- Once the browser finishes those tasks, they return and are pushed on the stack as a callback (for JS to execute them)

## Promises
- a Promise is an object representing the eventual completion or failure of an asynchronous operation
- it starts as Pending, and eventually gets Resolved or Rejected
- methods can be attached to Promises
```js
const fakeRequestPromise = (url) => {
    return new Promise((resolve, reject) => {
        const delay = Math.floor(Math.random() * (4500)) + 500;
        setTimeout(() => {
            if (delay > 4000) {
                reject('Connection Timeout :(') // err
            } else {
                resolve(`Here is your fake data from ${url}`) // data
            }
        }, delay)
    })
}


// now we can use this function that returns a Promise
// and we can return Promises within callbacks, to chain them
fakeRequestPromise('someUrl')
    .then((data) => {
        console.log('Success for someUrl')
        console.log(data) // this is from the fakeRequestPromise function
        return fakeRequestPromise('someOtherUrl') // returning a Promise, so we can chain another `.then`
    })
    .then((data) => {
        console.log('Success for someOtherUrl')
        console.log(data) // this is from the fakeRequestPromise function
    })
    .catch((err) => {
        console.log('Failure on some request!') // this will trigger if any of the `.then` fail
        console.log(err) // this is from the fakeRequestPromise function
    })
```

### Making Promises
```js
new Promise(function(resolve, reject) {
    resolve();
    // or
    reject();
})

// example
const fakeRequest = (url) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (5 > 3) {
                resolve('Fake data here');
            }
            reject('Uh oh');
        }, 3000)
    })
}
```

## Async and Await keywords
### Async functions
- newer and cleaner syntax for working with async code
- Async functions always return a Promise!
```js
async function hello() {
    return 'hello'; // a Promise
}

const hello = async () => {
    return 'hello'; // a Promise
}

hello()
    .then((data) => {
        console.log('Promise resolved with', data);
    })
```
- if there is an Error thrown in an async function, the returned Promise will be rejected, e.g.
```js
async function hello() {
    throw new Error('uh oh'); // this will reject the promise returned
    return; // this wont run
}
```

### Await
- can only use the `await` keyword inside async functions
- await will pause the execution of the function, waiting for a Promise to be resolved

```js
async function rainbow() {
    await changeColor('red', 1000);
    await changeColor('green', 1000);
    await changeColor('blue', 1000);
    return "DONE";
}

rainbow()
    .then(() => {
        console.log('End of rainbow');
    })
```

### Error handling in reject()
- can use try/catch blocks in async functions to handle Promises
```js
async function something () {
    try {
        await getRequest() // returns a Promise, could be resolved or rejected
    // if rejected, it will be passed into catch as `err`
    } catch (err) {
        console.log(err)
    }
}
```
