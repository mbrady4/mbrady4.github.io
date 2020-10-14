# Postgres

[Postgres.app - the easiest way to get started with PostgreSQL on the Mac](https://postgresapp.com/)

The database has to be created in Postgres with PGAdmin versus SQLite it will create itself through a knex migration. 

You can set the Database_URL as:

`postgresql://postgres:lqt38be@localhost/hobbits`

In the above 'postgres' is the username and 'lqt38be' is the example password, 'hobbits' is the table name, and 'localhost' is the database name.

To run migration and seeds in different environments we simply use:

`knex migrate:latest --env=production`

`knex seed:run --env=production`

## Heroku

1. Connect your repo with automatic deploys.
2. Add "Heroku Postgres" as an add-on resource
3. Add necessary environment variables under "config vars". Should be "DB_ENV" = 'production'
4. Connect to the app via the command line/console.

    Run `knex migrate:latest` to create tables

    Run `knex seed:run` to seed the database

### Walkthrough of setting up and deploying a server to Heroku

[https://youtu.be/R49CwOS_SCU?t=3447](https://youtu.be/R49CwOS_SCU?t=3447)