# SQL Commands Introduction
# sql commands workshop


1. connect to postgres> `psql` or `pgcli`
2. `CREATE DATABASE blog_workshop;`
  1. if there is already a database called blog_workshop  > `DROP DATABASE [name]`
  2. if there are more connections to that db, restart postgres `brew services restart postgresql`  and then DROP
3. connect to database `\c blog_workshop`
4. run SQL build file  `\i init.sql`

commands:

1. `SELECT * FROM users;`
2. `SELECT username, location FROM users;`
3. `SELECT * FROM users WHERE age>50;`
4. `SELECT first_name, last_name, location FROM users WHERE location='Saxilby, UK' AND age>40;`

** single quotes for values and double quotes for column names!

5. `SELECT user_id FROM blog_posts WHERE text_content LIKE '%departure%';`

**LIKE operator syntax: 

    SELECT column1, column2, ...
    FROM table_name
    WHERE columnN LIKE pattern;

 '%' and '_' are wildcards 
 `WHERE text_content LIKE` `'``%d``'` ends with ‘d’
 `WHERE text_content LIKE 'd%'` begins with ‘d’
`WHERE text_content LIKE '%d%'` has ‘d’ in any position
`WHERE text_content LIKE '_d%'` has ‘d’ in second position


6. `SELECT user_id, text_content FROM blog_posts WHERE id IN(3,6);`

**IN operator is a shorthand for multiple OR conditions
Syntax:

    SELECT column_name(s)
    FROM table_name
    WHERE column_name IN (value1, value2, ...);


7. `SELECT id, CASE WHEN age<20 THEN 'true' ELSE 'false' END AS teenagers FROM users;`

** CASE WHEN is like an if/else statement
Syntax:

    CASE WHEN condition THEN result
         [WHEN ...]
         [ELSE result]
    END

** AS is used to make aliases (temporary columns that only exist for the duration of the query)
Syntax:

    SELECT column_name AS alias_name
    FROM table_name;


8. `INSERT INTO blog_posts(text_content, user_id) VALUES ('Hello World', 1);`

** you don’t use WHERE user_id =1 but actually add user_id in values.


9. `UPDATE blog_posts SET user_id=5 WHERE text_content='Hello World';`

** UPDATE Syntax:

    UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;

10.

    INSERT INTO post_comments(post_id, reply_to, user_id, text_content) VALUES (
    (SELECT id FROM blog_posts WHERE text_content LIKE '%Peculiar%'),
     NULL,
     3,
      'Interesting post');

