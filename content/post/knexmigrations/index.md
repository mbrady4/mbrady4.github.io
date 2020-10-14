+++
title = "Knex Migrations"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
When a schema needs to be updated, a developer must feel confident that the changes go into effect everywhere. This means schema updates on the developer’s local machine, on any testing or staging versions, on the production database, and then on any other developer’s local machines. This is where migrations come into play.

A **database migration** describes changes made to the structure of a database. Migrations include things like adding new objects, adding new tables, and modifying existing objects or tables.

To use Knex and migrations we need to use knex cli. Install knex globally with `npm install -g knex`. This allows you to use Knex commands within any repo that has knex as a local dependency.

## Initializing Knex

Add knex and sqlite3 to your repo:

`npm install knex sqlite3`

You can manually create a config object to get started with Knex, but the best practice is to use the following command:

`knex init`

This command will generate a file in your root folder called knexfile.js. It will be auto populated with three config objects, based on different environments. We can delete all except for the development object.

```sql
// knexfile.js 
module.exports = {

  development: {
    // our DBMS driver
    client: 'sqlite3',
    // the location of our db
    connection: {
      filename: './data/database_file.db3',
    },
    // necessary when using sqlite3
    useNullAsDefault: true,

		// generates migration files in a data/migrations/ folder
    migrations: {
      directory: './data/migrations'
    }
  }

};
```

Rather than hardcode access to a config object in our files, we can now use the following familiar syntax:

```sql
const knex = require('knex');

const config = require('../knexfile.js');

// we must select the development object from our knexfile
const db = knex(config.development);

// export for use in codebase
module.exports = db;
```

## Migrations with Knex

To generate a new migration we use: 

```sql
knex migrate:make [migration-name]
```

If we need to create a new accounts table, we might run: 

```sql
knex migrate:make create-accounts
```

inside data/migrations/ a new file has appeared. Migrations have a timestamp in their filenames automatically. Wither you like this or not, do not edit migration names.

The migration file will have both an up and a down function. Within the up function, we write the ended database changes. Within the down function, we write the code to undo the up functions. This allows us to undo any changes made to the schema if necessary.

```sql
exports.up = function(knex, Promise) {
  // don't forget the return statement
  return knex.schema.createTable('accounts', tbl => {
    // creates a primary key called id
    tbl.increments();
    // creates a text field called name which is both required and unique
    tbl.text('name', 128).unique().notNullable();
    // creates a numeric field called budget which is required
    tbl.decimal('budget').notNullable();
  });
};

exports.down = function(knex, Promise) {
  // drops the entire table
  return knex.schema.dropTableIfExists('accounts');
};
```

To run this migration (and any other existing migrations), use: 

```sql
knex migrate:latest
```

Note if the database does not exist, this command will auto-generate one.

### Changes and rollbacks

If later down the road, we realize you need to update your schema, you shouldn’t edit the migration file. Instead, you will want to create a new migration with the command:

`knex migrate:make accounts-schema-update`

Once we’ve written our updates into this file we save and close with:

`knex migrate:latest`

If we migrate our database and then quickly realize something isn’t right, we can edit the migration file. However, first, we need to **rolllback** (or undo) our last migration with:

`knex migrate:rollback`

Finally, we are free to rerun that file with `knex migrate` latest.

**NOTE**: A rollback should not be used to edit an old migration file once that file has accepted into a master branch. However, an entire team may use a rollback to return to a previous version of a database.

## Knex Seeds

Often we want to pre-populate our database with sample data for testing. Seeds allow us to add and reset sample data easily. The Knex command-line tool offers a way to seed our database; in other words, pre-populate our tables. 

Similarly to migrations, we want to customize where our seed files are generated using our `knexfile`

```sql
development: {
    client: 'sqlite3',
    connection: {
      filename: './data/produce.db3',
    },
    useNullAsDefault: true,
    // generates migration files in a data/migrations/ folder
    migrations: {
      directory: './data/migrations'
    },
    seeds: {
      directory: './data/seeds'
    }
  }
```

To create a seed run: `knex seed:make 001-seedName`

Numbering is a good idea because Knex doesn’t attach a timestamp to the name like migrate does. Adding numbers to the file name, we can control the order in which they run.

We want to create seeds for our accounts table:

`knex seed:make 001-accounts`

A file will appear in the designated seed folder.

```sql
exports.seed = function(knex, Promise) {
  // we want to remove all data before seeding
  // truncate will reset the primary key each time
  return knex('accounts').truncate()
    .then(function () {
      // add data into insert
      return knex('accounts').insert([
        { name: 'Stephenson', budget: 10000 },
        { name: 'Gordon & Gale', budget: 40400 },
      ]);
    });
};
```

Run the seed files by typing:

`knex seed:run`