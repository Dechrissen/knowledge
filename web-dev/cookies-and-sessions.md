## HTTP cookies
- used to store some piece of data in a user's browser
- they persist until the next time the user visits that site again
- in Express, we can use the additional package `cookie-parser` (npm i cookie-parser)

### cookie signing
- cryptography method
- can use a `secret` string to sign a cookie

## Sessions
- **sessions** are stored server-side, can be used to store more data than a cookie can
- a cookie is sent to the client, which acts as a key/ID to unlock that **session**
- **sessions** do not replace a database, they are something else adjacent to DBs (like for a shopping cart, which you might want a user to be able to do without creating a user account/registering)
- in Express, we can use the additional package `express-session` (npm i express-session)
- in production, we can use a non-long-term storage solution like Redis