# Aggregate Functions

> Grouping Data and Performing Analysis

**Not on the row level instead on groups**

## COUNT keyword

> count num of rows that match conditions

```sql
  SELECT COUNT(*) FROM books; -- COUNT the number of rows
  SELECT title, COUNT(*) FROM books; -- invalid
  SELECT COUNT(DISTINCT author_fname) FROM books;
```

## GROUP BY keyword

> aggregates identical data into single rows

> makes group of rows with the given criteria
> uses an aggregate function to display proper data

```sql
SELECT author_lname, COUNT(\*) FROM books GROUP BY author_lname;
SELECT author_lname, COUNT(\*) AS 'books_written' FROM books GROUP BY author_lname ORDER BY 'books_written';
```

## MIN/MAX

```sql
SELECT MAX(pages) FROM books;
```

## Subqueries

```sql
  SELECT title, pages FROM books ORDER BY pages DESC LIMIT 1;

  SELECT *
  FROM books
  WHERE pages = (SELECT MIN(pages)
  FROM books);
```

## GROUP BY Multiple Columns

```sql
  SELECT author_lname, author_fname COUNT(*) FROM books
  GROUP BY author_lname, author_fname;
  SELECT CONCAT_WS(" ",author_lname, author_fname) AS FROM books
  GROUP BY author;
```

## MIN and MAX with GROUP BY

```sql
SELECT *, MIN(released_year) AS 'first_book' FROM books
GROUP BY author_lname, author_fname
```

## SUM

> straightforward

```sql
  SELECT SUM(pages) FROM books;

  SELECT author_lname, SUM(pages)
  FROM books
  GROUP BY pages;
```

## AVG

> Average

```sql
SELECT AVG(stock_quantity) FROM books;

SELECT AVG(stock_quantity)
FROM books
GROUP BY released_year;
```

## DOCS FOR Aggregate Functions
