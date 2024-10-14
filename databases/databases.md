# Databases

## SQL vs. NoSQL
SQL | NoSQL
--- | ---
Structured Query Language databases. Relational databases. We pre-define a schema of tables before inserting anything. | NoSQL databases do not use SQL. Instead, there are other types, like document, key-value, graph stores.
MySQL | MongoDB
Postgres | Couch DB
SQLite | Neo4j
Oracle | Cassandra
Microsoft SQL Server | Redis

## Relational (SQL)
- SQL databases are relational
- we connect different tables by referencing one another
- we refernce data from one table in another table

## Non-relational (NoSQL)
- very diverse category
- many types of databases (document, key-value, graph stores)

### Document-oriented database (document data store)
- a collection of documents, in some format like JSON, XML, YAML
- each document can be structured differently
- e.g. some document could have the "author" field, but another one in the collection might not have that same field