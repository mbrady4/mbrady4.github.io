+++
title = "SQL Fundamentals"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
A *relational database* is a database that organizes information into one or more tables. Here, the relational database contains one table.

A *table* is a collection of data organized into rows and columns. Tables are sometimes referred to as *relations*. 

- `SELECT` - how we choose which columns to get
- `WHERE` - how we set conditions on the rows to be returned
- `LIMIT` - when we only want a certain number of rows
- `ORDER` - when we want to sort the output
- `JOIN` - when we need data from multiple tables combined

SELECT statements always return a new table called the result set.

A *statement* is a string of characters that the database recognizes as a valid command.

- `CREATE TABLE` creates a new table.
- `INSERT INTO` adds a new row to a table.
- `ALTER TABLE` changes an existing table.
- `UPDATE` edits a row in a table.
- `DELETE FROM` deletes rows from a table.

*Constraints* add information about how a column can be used.

- `BETWEEN` two letters *is not* inclusive of the 2nd letter.
- `BETWEEN` two numbers *is* inclusive of the 2nd number.

ORDER BY always goes after WHERE (if WHERE is present).

LIMIT is a clause that lets you specify the maximum number of rows the result set will have. This saves space on our screen and makes our queries run faster.

- `SELECT` is the clause we use every time we want to query information from a database.
- `AS` renames a column or table.
- `DISTINCT` return unique values.
- `WHERE` is a popular command that lets you filter the results of the query based on conditions that you specify.
- `LIKE` and `BETWEEN` are special operators.
- `AND` and `OR` combines multiple conditions.
- `ORDER BY` sorts the result.
- `LIMIT` specifies the maximum number of rows that the query will return.
- `CASE` creates different outputs.

Aggregate Functions

- `COUNT()`: count the number of rows
- `SUM()`: the sum of the values in a column
- `MAX()`/`MIN()`: the largest/smallest value
- `AVG()`: the average of the values in a column
- `ROUND()`: round the values in the column

The GROUP BY statement comes after any WHERE statements, but before ORDER BY or LIMIT.

    SELECT year,
       AVG(imdb_rating)
    FROM movies
    GROUP BY year
    ORDER BY year;

- When we want to limit the results of a query based on values of the individual rows, use `WHERE`.
- When we want to limit the results of a query based on an aggregate property, use `HAVING`.

HAVING statement always comes after GROUP BY, but before ORDER BY and LIMIT.

DBBrowser

[DB Browser for SQLite](https://sqlitebrowser.org)

[Comparison with SQL - pandas 0.24.2 documentation](https://pandas.pydata.org/pandas-docs/stable/getting_started/comparison/comparison_with_sql.html)

Connecting to SQLite with Command Line

1. Open Python REPL
2. Import sqlite3
3. Connect to the database

    >>> conn = sqlite3.connect('rpg_db.sqlite3')
    >>> conn
    <sqlite3.Connection object at 0x104e2a110>

3.5. Create cursor

    curs = conn.cursor()

4. Create Query

    >>> query = 'SELECT COUNT(*) FROM armory_item;'
    >>> query
    'SELECT COUNT(*) FROM armory_item;'
    >>> result = curs.execute(query)
    >>> result
    <sqlite3.Cursor object at 0x104eccce0>
    >>> dir(result)
    ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__ne__', '__new__', '__next__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'arraysize', 'close', 'connection', 'description', 'execute', 'executemany', 'executescript', 'fetchall', 'fetchmany', 'fetchone', 'lastrowid', 'row_factory', 'rowcount', 'setinputsizes', 'setoutputsize']
    >>> help(result.fetchall)
    
    >>> result.fetchall()
    [(174,)]

Joining Tables

***Explicit Join***

    SELECT name, mana
    FROM charactercreator_mage
    INNER JOIN charactercreator_character
    ON charactercreator_mage.character_ptr_id = charactercreator_character.character_id;

The inner join ensures that data is not introduced with missing values, thus it is probably the right solution for ~90% of queries

***Implicit Join***

    SELECT name, mana
    FROM charactercreator_mage, charactercreator_character
    WHERE charactercreator_character.character_id = charactercreator_mage.character_ptr_id;

***Implicit Join with Aliases***

    SELECT cc.name, cm.mana
    FROM charactercreator_mage AS cm, charactercreator_character AS cc
    WHERE cc.character_id = cm.character_ptr_id;

The name of all the weapons owned by character 1

    SELECT cc.name as character_name, ai.name AS item_name
    FROM charactercreator_character AS cc,
    		 armory_item AS ai,
    		 charactercreator_character_inventory as cci
    WHERE cci.character_id = cc.character_id AND
    		   ai.item_id = cci.item_id AND
    		   cc.character_id = 1;

[Working with SQL using Python and Pandas - Dataquest](https://www.dataquest.io/blog/python-pandas-databases/)

### Inner Queries

    SELECT OrderID FROM Orders
    WHERE CustomerID IN (SELECT CustomerID FROM Customers WHERE Country = 'Mexico');

Implicit Join Version

    SELECT Orders.OrderID
    FROM Orders, Customers
    WHERE Orders.CustomerID = Customers.CustomerID 
    AND Country = 'Mexico';

Explicity Join

    SELECT Orders.OrderID
    FROM Orders
    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
    WHERE Country = 'Mexico';

SQL has strict rules for appending data:

- Tables must have the same number of columns.
- The columns must have the same data types in the same order as the first table.

A Simple Union (Similiar to pd.concat)

    SELECT *
    FROM table1
    UNION
    SELECT *
    FROM table2;

- `JOIN` will combine rows from different tables if the join condition is true.
- `LEFT JOIN` will return every row in the *left* table, and if the join condition is not met, `NULL` values are used to fill in the columns from the *right* table.
- *Primary key* is a column that serves a unique identifier for the rows in the table.
- *Foreign key* is a column that contains the primary key to another table.
- `CROSS JOIN` lets us combine all rows of one table with all rows of another table.
- `UNION` stacks one dataset on top of another.
- `WITH` allows us to define one or more temporary tables that can be used in the final query.