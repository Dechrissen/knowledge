# AJAX and APIs

## AJAX (AJAJ)
Asynchronous  
JavaScript  
And  
XML (or JSON which is more likely)

## APIs
Application Programming Interface.

- usually refers to Web APIs (HTTP-based)
- an "interface, not for humans, but for some application to communicate with"
- contains raw data that some application can do something with
- endpoints (URLs) are exposed which respond with information for software to consume
- interface that occurs over HTTP

### XML
- not as common as it used to be
- kind of like HTML but uses custom tags

### JSON (what APIs respond with)
JavaScript Object Notation.
- key/value pairs
- every key has to be a double-quoted string

```js
// parsing JSON
const info = `{"price": 12.54, "name": "Derek"}` 
const parsedJson = JSON.parse(info)
parsedJson.price
> 12.54
```

## HTTP verbs
`GET` - for getting data/information  
`POST` - for posting data to a db  
`DELETE` - for deleting data in an API  

### HTTP status codes
Starts with 2, means it's a good response
- `200` - success/OK
- `201` - created
- `202` - accepted

Starts with 3, means it's related to redirection of some sort

Starts with 4, means it's a bad response
- `404` not found
- `405` method not allowed (doesn't support specific HTTP verb)

### Queries
- key/value pairs
- starts with `?` separated by `&`
    - `someapi.com/api/search/tvshows?q=term&year=2024`

### Headers
- key/value pairs
- metadata that will add details as part of your request
- APIs can accept certain headers, e.g. for type of response
    - Accept : application/json
    - Accept : text/html

## Using the Fetch API
- newer way of making requests in JS (instead of XHR)
```js
// fetch returns a Promise
fetch("some/url")
    .then((res) => {
        console.log("Resolved")
        console.log(res)
        return res.json() // to read the ReadableStream in the Promise returned from `fetch`
    })
    .then((data) => {
        console.log(data) // the data that the res.json() returned
    })
    .catch((err) => {
        console.log(err)
    })

// another example with async functions
const someFunc = async () => {
    const res = await fetch('some/url');
    const data = await res.json();
    console.log(data);
    const res2 = await fetch('some/url/2');
    const data2 = await res2.json();
    console.log(data2);
}
```

### Axios
A library for making HTTP requests (uses `fetch` under the hood).

HTML:
```html
<script src="https://unpkg.com/axios@1.6.7/dist/axios.min.js"></script>
```
JavaScript:
```js
// don't need to parse the ReadableStream with axios, it instantly parses it
axios.get('some/url')
    .then((res) => {
        console.log(res)
    })
    .catch((err) => {
        console.log(err)
    })
```

#### Headers in Axios
```js
const getDadJoke = async () => {
    const headers = { headers: {Accept: 'application/json'} }
    const res = await axios.get('https://icanhazdadjoke.com/', headers);
    console.log(res.data.joke); // since it's JSON, can use dot operator
}
```