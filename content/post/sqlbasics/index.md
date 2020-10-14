# SQL

A query is a SQL statement used to retrieve data from a database. Note, spaces and line enters do not matter in SQL

## `SELECT`

```sql
// generic format
select <selection> from <table name>;

// all columns
select * from employees

// specific columns
select first_name, last_name, salary from employees;
```

## `INSERT`

An insert statement adds a new record in the database. All non-null fields must be listed out in the same order as their values. Note that some fields, like ids and timestamps, may be auto-generated and do not need to be included in an INSERT statement.

```sql
// generic format 
insert into <table name> (<selection>) values (<values>)

// specific columns and data
insert into Customers (Country, CustomerName, ContactName, Address, City, PostalCode)
values ('USA', 'Lambda School', 'Austen Allred', '1 Lambda Court', 'Provo', '84601');
```

The values in an insert statement must not violate any restrictions and constraints that the database has in place, such as expected datatypes.

## `UPDATE`

When modifying a record, we identify a single record or a set of records to update using a WHERE clause.

```sql
update <table name> set <field> = <value> where <condition>;

update Customers
set City = 'Silicon Valley', Country = 'USA'
where CustomerName = 'Lambda School'
```

## `DELETE`

```sql
delete from <table name> where <condition>;

delete from Customers
where CustomerName = 'Lambda School`;
```

## `WHERE`

```sql
select City, CustomerName, ContactName
from Customers
where Country = 'France' and City = 'Paris'

select * from employees where salary >= 50000

select * from Customers
where CustomerId=3;
```

## `ORDER BY`

Query results are shown in the same order the data was inserted. To control how the data is sorted, we can use the `ORDER BY` clause

```sql
// sorts the results first by salary in descending order, then by last name in 
// ascending order
select * from employees order by salary desc, last_name;

select * from employees where salary > 50000 order by last_name;

// The numbers refer to the position of the fields in the selection portion of the 
// query, so 1 would be name, 2 would be salary and so on.
select name, salary, department from employees order by 3, 2 desc;
```

The `ORDER BY` clause goes after WHERE (filtering) clauses.

## `LIMIT`

```sql
select * from products
limit 10

select * from products
order by price desc
limit 5
```