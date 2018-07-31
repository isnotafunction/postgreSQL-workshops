# Notes

    \d - list all tables (know as 'relations' in psql)
    \d [table name] - give information on a given table
    \l - list all databases


1. SELECT first_name, surname FROM authors;   
2. SELECT *  FROM authors ORDER BY surname ASC LIMIT 3;   
3. SELECT * FROM authors WHERE location IS NOT NULL;   

** It is not possible to test for NULL values with comparison  operators, such as =, <, or <>.
We will have to use the IS NULL and IS NOT NULL operators instead.


4. Find everyone who is not in Nazareth (including nulls)   

SELECT * FROM authors WHERE location <> 'Nazareth' OR location IS NULL;

5. Find everyone with 'Wistle' in their surname (bonus points for case insensitivity)  

SELECT * FROM authors WHERE surname ILIKE '%wistle%'; (postgres syntax for case insensitive; standard syntax would be WHERE LOWER(surname) LIKE LOWER ('%Whistle%')  

6. Find the name of the publisher who released 'Python Made Easy'

    SELECT publishers.name FROM publishers INNER JOIN books ON books.publisher_id=publishers.id WHERE books.name = 'Python Made Easy';


7.  Find all the books published by 'No Starch Press’
    SELECT books.name, publishers.name FROM publishers INNER JOIN books ON books.publisher_id = publishers.id WHERE publishers.name='No Starch Press';


8. Show a list of every book and their authors
    SELECT books.name, authors.first_name, authors.surname 
    FROM books INNER JOIN book_authors ON book_authors.book_id = books.id 
    INNER JOIN authors ON authors.id = book_authors.author_id 
    ORDER BY book_authors.book_id;
9. Find all the books that Ted Burns authored
    SELECT books.name FROM books 
    INNER JOIN book_authors ON book_authors.book_id = books.id 
    INNER JOIN authors ON authors.id = book_authors.author_id 
    WHERE authors.first_name='Ted' 
    AND authors.surname='Burns';

## Intermediate Challenges:

**Agregate functions** -  compute a single result from multiple input rows.

 `AVG()` `COUNT()` `MIN()` `MAX()`and `SUM()`.
Often used with the `GROUP BY` clause in the `SELECT`statement.
Syntax:

    SELECT column_1, aggregate_function(column_2)
    FROM tbl_name
    GROUP BY column_1;

To filter groups, you use the HAVING clause instead of WHERE clause. The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.
It is important to understand the interaction between aggregates and SQL's WHERE and HAVING clauses. The fundamental difference between WHERE and HAVING is this: WHERE selects input rows before groups and aggregates are computed (thus, it controls which rows go into the aggregate computation), whereas HAVING selects group rows after groups and aggregates are computed. Thus, the WHERE clause must not contain aggregate functions;


**Subqueries**
Subqueries can be used with the SELECT, INSERT, UPDATE and DELETE statements along with the operators like =, <, >, >=, <=, IN, etc.
There are a few rules that subqueries must follow −

- Subqueries must be enclosed within parentheses.
- A subquery can have only one column in the SELECT clause, unless multiple columns are in the main query for the subquery to compare its selected columns.
- An ORDER BY cannot be used in a subquery, although the main query can use an ORDER BY. The GROUP BY can be used to perform the same function as the ORDER BY in a subquery.
- Subqueries that return more than one row can only be used with multiple value operators, such as the IN, EXISTS, NOT IN, ANY/SOME, ALL operator.
- The BETWEEN operator cannot be used with a subquery; however, the BETWEEN can be used within the subquery.


10.   Find everyone who wrote at least three books
    SELECT authors.surname FROM authors 
    INNER JOIN book_authors ON book_authors.author_id= authors.id 
    GROUP BY authors.id 
    HAVING COUNT(book_authors.author_id) > 2;


11.     Order the publishers by the number of books they have published.
    SELECT publishers.name, COUNT(books.name) FROM publishers 
    INNER JOIN books ON books.publisher_id = publishers.id 
    GROUP BY publishers.name 
    ORDER BY COUNT(books.name) DESC;


12. Find all books released after 1st Jan 1996 order by the number of ppl who wrote them:
    SELECT books.name, COUNT(authors.id) FROM books 
    INNER JOIN book_authors ON book_authors.book_id = books.id 
    INNER JOIN authors ON authors.id = book_authors.author_id 
    WHERE books.release_date >'01-Jan-96' 
    GROUP BY books.name 
    ORDER BY COUNT(authors.id) DESC;


13. What is the book with highest/lowest number of authors:
    SELECT books.name FROM books 
    INNER JOIN book_authors ON book_authors.book_id = books.id 
    GROUP BY books.name 
    ORDER BY COUNT(book_authors.book_id) DESC LIMIT 1;


14. Who wrote the largest number of books:
    SELECT authors.first_name, authors.surname, COUNT(book_authors.author_id) 
    AS total FROM authors
    INNER JOIN book_authors
    ON authors.id = book_authors.author_id
    GROUP BY authors.first_name, authors.surname
    ORDER BY total DESC LIMIT 1;

## Advanced(Hard)

