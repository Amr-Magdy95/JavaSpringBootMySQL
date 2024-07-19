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
