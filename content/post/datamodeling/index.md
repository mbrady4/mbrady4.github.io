# Data Modeling

Normalization is the process of designing or refactoring database tables for maximum consistency and minimum redundancy. With Javascript objects we're used to denormalized data, stored with ease of use and speed in mind. Non-normalized tables are considers ineffective in relational databases. 

**Guidlines for data normalization:** 

- Each record has a primary key
- No fields are repeated
- All fields relate directly to the key data
- Each field entry contains a single data point
- There are no redundant entires

Denormalized data increases the probability of contradictory, confusing, or unusable data. Normalizing data often requires separating concerns into distinct tables. 

Determining how data is related can provide a set of guidelines for table representation and guides the use of freign keys to connect said tables. 

## One to One Relationships

*Example: Farms and Farm Budgets. Each budget is specific to a farm and each farm has only one budget.*

- The foreign key should always have a `unique` constraint to prevent duplicate entries. In the example above, we wouldn’t want to allow multiple projections records for one farm.
- The foreign key can be in either table. For example, we may have had a `projection_id` in the `farms` table instead. A good rule of thumb is to put the foreign key in whichever table is more auxiliary to the other.
- You can represent one-to-one data in a single table *without* creating anomalies. However, it is sometimes prudent to use two tables as shown above to keep separate concerns in separate tables.

## One to Many Relationships

*Example: Farms and Employees. Each farm has many employees, but each employee is unique and only works at one farm.*

- Manage this type of relationship by adding a foreign key on the “many” table of the relationship that points to the primary key on the “one” table.
- In a many-to-many relationship, the foreign key (in this case farm_id) should not be unique.

## Many to Many Relationships

*Example: Farms and types of animals. Each farm has many types of animals and multiple of each type of animal is present at multiple farms.* 

- To model this relationship, we need to introduce an intermediary table that holds foreign keys that reference the primary key on the related tables. We now have a farms, animals, and farm_animals table.
- While each foreign key on the intermediary table is not unique, the combinations of keys should be unique.

# Multi-Table Setups in Knex

In Knex, foreign key restrictions don’t automatically work. Whenever using foreign keys in your schema, add the following code to your knexfile. This will prevent users from entering bad data into a foreign key column.

```jsx
development: {
  client: 'sqlite3',
  useNullAsDefault: true, 
  connection: {
    filename: './data/database.db3',
  },
  // needed when using foreign keys
  pool: {
    afterCreate: (conn, done) => {
      // runs after a connection is made to the sqlite engine
      conn.run('PRAGMA foreign_keys = ON', done); // turn on FK enforcement
    },
  },
},
```

### Migrations

Note that the foreign key can only be created after the reference table.

```jsx
exports.up = function(knex, Promise) {
  return knex.schema
    .createTable('farms', tbl => {
      tbl.increments();
      tbl.string('farm_name', 128)
        .notNullable();
    })
    // we can chain together createTable
    .createTable('ranchers', tbl => {
      tbl.increments();
      tbl.string('rancher_name', 128);
      tbl.integer('farm_id')
        // forces integer to be positive
        .unsigned()
        .notNullable()
        .references('id')
        // this table must exist already
        .inTable('farms')
    })
};
```

In the down function, the opposite is true. We always want to drop a table with a foreign key before dropping the table it references.

```jsx
exports.down = function(knex, Promise) {
  // drop in the opposite order
  return knex.schema
    .dropTableIfExists('ranchers')
    .dropTableIfExists('farms')
};
```

In the case of a many-to-many relationship, the syntax for creating an intermediary table is identical, except for one additional piece. We need a way to make sure our combination of foreign keys is unique.

```jsx
.createTable('farm_animals', tbl => {
  tbl.integer('farm_id')
    .unsigned()
    .notNullable()
    .references('id')
    // this table must exist already
    .inTable('farms')
  tbl.integer('animal_id')
    .unsigned()
    .notNullable()
    .references('id')
    // this table must exist already
    .inTable('animals')

  // the combination of the two keys becomes our primary key
  // will enforce unique combinations of ids
  tbl.primary(['farm_id', 'animal_id']);
});
```

### Cascading

If a user attempt to delete a record that is referenced by another record (such as attempting to delete `Morton Ranch` when entries in our `ranchers` table reference that record), by default, the database will block the action. The same thing can happen when updating a record when a foreign key reference.

If we want that to override this default, we can delete or update with **cascade**. With cascade, deleting a record also deletes all referencing records, we can set that up in our schema.

```jsx
.createTable('ranchers', tbl => {
    tbl.increments();
    tbl.string('rancher_name', 128);
    tbl.integer('farm_id')
    .unsigned()
    .notNullable()
    .references('id')
    .inTable('farms')
    .onUpdate('CASCADE');
    .onDelete('CASCADE')
})
```

### Seeds

Order is also a concern when seeding. We want to create seeds in the **same** order we created our tables. In other words, don’t create a seed with a foreign key, until that reference record exists.

In our example, make sure to write the `01-farms` seed file and then the `02-ranchers` seed file.

However, we run into a problem with truncating our seeds, because we want to truncate `02-ranchers` *before* `01-farms`. A library called `knex-cleaner` provides an easy solution for us.

Run `knex seed:make 00-cleanup` and `npm install knex-cleaner`. Inside the cleanup seed, use the following code.

```jsx
const cleaner = require('knex-cleaner');

exports.seed = function(knex) {
  return cleaner.clean(knex, {
    mode: 'truncate', // resets ids
    ignoreTables: ['knex_migrations', 'knex_migrations_lock'], // don't empty migration tables
  });
};
```

This removes all tables (excluding the two tables that track migrations) in the correct order before any seed files run.

Table Projects

- Unique ID (increments)
- Name (text, required)
- Description (text, not required)
- Completed (boolean, not required, default to false)

Task

- Unique ID (increments)
- Project ID (required)
- Description (text, required)
- Notes (text, not required)
- Completed (boolean, not required, default to false)

Resource

- Unique ID (increments)
- Name (text, required, no duplicates)
- Description (text, optional)

Project_Resources

- Table_ID
- Project_ID
- Primary Key is combination of foreign keys