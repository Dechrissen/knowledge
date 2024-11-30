# Common vulnerabilities in web applications

## SQL / NoSQL injection
- the act of injecting some SQL or NoSQL code into a query that will run and access/manipulate a database
- in the case of Mongo in Express, we can use the package `express-mongo-sanitize` to "sanitize" the queries before being sent to the DB
    - e.g., remove `$`, or replace them, etc.

## XSS (cross-site scripting)
- the act of injecting JavaScript `script`s into an HTML document to run exploitative JS code