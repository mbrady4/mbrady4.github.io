+++
title = "Multi-Page Queries"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## Foreign Keys

Foreign keys are a type of table field used for creating links between tables. Like primary keys, they are most often integers that identify (rather than store) data. However, whereas a primary key is used to id rows in a table, foreign keys are used to connect a record in one table to a record in a second table.

## Joins

Using joins requires that the two tables of interest contain at least one field with shared information.

![Multi-Page%20Queries%20729e855189c04383b6a8183124c5469f/Untitled.png](Multi-Page%20Queries%20729e855189c04383b6a8183124c5469f/Untitled.png)

```sql
// SQL
select d.id, d.name, e.id, e.first_name, e.last_name, e.salary
from employees as e
join departments as d
  on e.department_id = d.id
order by d.name, e.last_name

// Knex
db('employees as e')
  .join('departments as d', 'e.department_id', 'd.id')
  .select('d.id', 'd.name', 'e.first_name', 'e.last_name', 'e.salary')
```