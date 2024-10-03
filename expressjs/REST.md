# REST (Representational State Transfer)
- architectural style for distributed hypermedia systems
- **guidelines** for how **client and server** should communicate with each other
    - how they should perform **CRUD** (create read update delete) operations on a given resource
- a "RESTful" system complies with these guidelines
- most common way of approaching REST is in **formatting the URLs and HTTP verbs in your applications**

## Common RESTful route types
Using some social media comment system as an example (Comments as a resource):

Name | Path | Verb | Purpose
--- | --- | --- | ---
Index | `/comments` | GET | Display all comments
New | `/comments/new` | GET | Form to create a new comment
Create | `/comments` | POST | Creates new comment on server
Show | `/comments/:id` | GET | Details for one specific comment
Edit | `/comments/:id/edit` | GET | Form to edit a specific comment
Update | `/comments/:id` | PATCH | Updates a specific comment on server
Destroy | `/comments/:id` | DELETE | Deletes specific comment on server

