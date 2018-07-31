### step2 notes
create database folder and db_build.sql file.
BEGIN -- start a transaction block; all statements after a BEGIN command will be executed in a single transaction until an explicit COMMIT or ROLLBACK is given. By default (without BEGIN), PostgreSQL executes transactions in "autocommit" mode, that is, each statement is executed in its own transaction and a commit is implicitly performed at the end of the statement

BEGIN is a PostgreSQL language extension. It is equivalent to the SQL-standard command START TRANSACTION,

COMMIT -- commit the current transaction

DROP TABLE IF EXISTS superheroes CASCADE;

DROP TABLE always removes any indexes, rules, triggers, and constraints that exist for the target table. However, to drop a table that is referenced by a view or a foreign-key constraint of another table, CASCADE must be specified. (CASCADE will remove a dependent view entirely, but in the foreign-key case it will only remove the foreign-key constraint, not the other table entirely.)

IF EXISTS > Do not throw an error if the table does not exist. A notice is issued in this case.
CASCADE> Automatically drop objects that depend on the table (such as views).

CREATE TABLE
The name of the table must be distinct from the name of any other table, sequence, index, view, or foreign table in the same schema, or name of data-type


    CREATE TABLE superheroes(
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    superpower TEXT NOT NULL,
    weight INTEGER DEFAULT 100
    );
- All tables should have an integer `id` that is set as a `PRIMARY KEY` - this is used relate databased together (integer PRIMARY KEY helps with performance)
- PRIMARY KEY also adds UNIQUE and NOT NULL 
- SQL DATA TYPES - VARCHAR, INTEGER, TEXT…
- NOT NULL > throw an error if not set
- DEFAULT 100 > changes null values to be 100

### step 3 notes

[npm pg module - documentation](https://node-postgres.com/)

connection pool - a cache of database connections maintained so that the connections can be reused when future requests to the database are required. Opening and maintaining a database connection for each user, especially requests made to a dynamic database-driven website application, is costly and wastes resources. In connection pooling, after a connection is created, it is placed in the pool and it is used again so that a new connection does not have to be established. If all the connections are being used, a new connection is made and is added to the pool. 