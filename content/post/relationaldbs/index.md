+++
title = "Realtional DBs"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
A database is a collection of data organized for easy retrieval and manipulation. A place to store data for persistence. 

### Data Persistence

Infrequently accessed and not likely to be modified. We can trust the data to stay the same unless we explicitly change or remove it. 

## Relational Databases

In relational databases, the data is stored in tabular format grouped into rows and columns (similar to spreadsheets). A collection of rows called a table. Each row represents a single record in the table and is made up of one or more columns. 

These kinds of databases are called relational because a relation is a mathematical idea that is equivalent to a table. So relational databases are databases that store their data in tables.

## Tables

- Tables organize data in rows and columns.
- Each row on a table represents one distinct record.
- Each column represents a field or attribute that is common to all records.
- Fields should have a descriptive name and a data type appropriate for the attribute it represents.
- Tables usually have more rows than columns
- Tables have primary keys that uniquely identify each row.
- Foreign keys represent the relationships with other tables.

## SQL

SQL is a standard language, which means that it almost certainly be supported, no matter how your database is managed.

- No branching, loops, variables, etc.
- Executes one specific purpose: queries
- Efficient for data retrieval and allows scalability