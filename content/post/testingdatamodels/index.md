# Testing Data Models

To test the data access, we’ll write end to end tests. These types of tests run slower because they perform operations and run queries against an actual database that is similar to the one used in production.

To avoid polluting the development database, we’ll use a separate database for testing. One advantage of using a dedicated testing database is that we can add and remove records without affecting the data in the development or staging databases.

### Setting up a test database

1. **Setup package.json:** Use `cross-env` to provide a simple, cross-OS, way to set environment variables. In `package.json` set the test script to have the following value:

`"test": "cross-env DB_ENV=testing jest --watch"`

**2. Modify how scripts connect to the database:** This `DB_ENV` environment variable is available to the API as process.env.DB_ENV. Modify the way scripts connect to the database as follows:

```jsx
// ./data/dbConfig.js
const knex = require('knex');

const config = require('../knexfile.js');

// if the environment variable is not set, default to 'development'
// this variable is only set when running the "test" npm script using npm run test
const dbEnv = process.env.DB_ENV || 'development';

// the value of dbEnv will be either 'development' or 'testing'
// we pass it within brackets to select the corresponding configuration
// from knexfile.js
module.exports = knex(config[dbEnv]);
```

**3. Add a testing configuration to the `knexfile`**

```jsx
// ./knexfile.js

module.exports = {
  development: {
    client: 'sqlite3',
    connection: {
      filename: './data/hobbits.db3',
    },
    useNullAsDefault: true,
    migrations: {
      directory: './data/migrations',
    },
    seeds: {
      directory: './data/seeds',
    },
  },
  testing: {
    client: 'sqlite3',
    connection: {
      filename: './data/test.db3',
    },
    useNullAsDefault: true,
    migrations: {
      directory: './data/migrations',
    },
    seeds: {
      directory: './data/seeds',
    },
  },
};
```

To specify which environment to target during migrations and seeding use the —env command line argument.

To run migrations against the testing database, use: 

`npx knex migrate:latest --env=testing`

To run seeds against the testing database, use: 

`npx knex seed:run --env=testing`

## Example

```jsx
// our connection to the database
const db = require('../data/dbConfig.js');

// the data access file we are testing
const Hobbits = require('./hobbitsModel.js');

describe('hobbits model', () => {
  describe('insert()', () => {
    // this example uses async/await to make it easier to read and understand
    it('should insert the provided hobbits into the db', async () => {
      // this code expects that the table is empty, we'll handle that below
      // add data to the test database using the data access file
      await Hobbits.insert({ name: 'gaffer' });
      await Hobbits.insert({ name: 'sam' });

      // read data from the table
      const hobbits = await db('hobbits');

      // verify that there are now two records inserted
      expect(hobbits).toHaveLength(2);
    });
  });

	// note we're checking one hobbit at a time
	it('should insert the provided hobbit into the db', async () => {
	  let hobbit = await Hobbits.insert({ name: 'gaffer' });
	  expect(hobbit.name).toBe('gaffer');
	
	  // note how we're reusing the hobbit variable
	  hobbit = await Hobbits.insert({ name: 'sam' });
	  expect(hobbit.name).toBe('sam');
	});
});
```

To guarantee that the tables are cleared before running each test, add the following codes before the test cases:

```jsx
beforeEach(async () => {
  // this function executes and clears out the table before each test
  await db('hobbits').truncate();
});
```