# Authentication, authorization

- Authentication is the act of verifying a user's identity
- Authorization is the act of determining a user's permissions based on their identity (admin, mod, normal user, etc.)

### Cryptographic hashing functions
- passwords should be stored as hashed version 
- hashing functions take a string and return a hashed version of that string
- that hashed version is stored in DB (mongo, etc.)
- they are "_one-way functions_"; they cannot be definitively reversed (feasibly)
- a small change in input should yield a large change in output
- They are _deterministic_: same input yields same output
- Very unlikely to find 2 outputs with the same value
- password hash functions are deliberately _slow_

### Salts
- an extra safeguard
- theoretically, anyone who uses the same password, like 'monkey', their output hash would be exactly the same if the same hash function is used
- so, we can use salts (a random value added to the password before hashing it)
    - this helps ensure unique hashes and mitigate common attacks

### brcypt
- common password hashing function
- implemenations of this algorithm in many languages (js, python, etc.)

## Authentication using Passport
- an easy method of implementing authentication, _not_ from scratch