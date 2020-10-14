# Knex

knex is a "batteries included" SQL query builder library, and sqlite3 allows us to interface with a sqlite database.

```sql
npm install knex sqlite3
```

Next, we need to setup a basic (not best practice) config file:

```sql
const knex = require('knex');

const config = {
  client: 'sqlite3',
  connection: {
		// file path should be with respect to root of repo, not config file
    filename: './data/posts.db3',
  },
  useNullAsDefault: true,
};

module.exports = knex(config);
```

To use the query builder elsewhere in our code, we need to call knex and pass in a config object. The filename should point towards a pre-existing database file which can be recognized by the .db3 extension. **The file path to the database should be with respect to the root of the repo, not the configuration file itself.**

To import Knex to a file, we use:

```sql
const db = require('../data/db-config.js);
```

## Queries with Knex

**All Knex queries return promises.**

```sql
// resolves to an array of users
db.select('*').from('users');
db('users');

// resolves to an array containing any users that match the where
db('users').where({ id:3 });

// resolves to an array containing the id of the user
db('users').insert({ name: 'amanda', age: 76 })

// resolve to count of records updated
db('users').where({ id:5 }).update({ age: 77 });

// resolve to count of records deleted
db('users').where({ age: 33 }).del();
```