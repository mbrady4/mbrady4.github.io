+++
title = "DBMS"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Software for creating and managing a database. The interface between the programmer and the database itself. Often abbreviated as DBMS or RDBMS (Relational Database Management Systems).

SQLite stores the database within a single file. 

Database schemas

A blueprint of the database. Which tables are present and what columns they have. 

## Knex migrations

Files representing a change to the database. Assures the schema is consistent across an entire team. Helps prevent bugs on database changes. Like git commits for your database.