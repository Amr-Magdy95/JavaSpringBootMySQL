# Refining Selections

## DISTINCT Clause

```sql
  SELECT DISTINCT author_lname FROM books; // goes after select before column name
  SELECT DISTINCT author_fname, author_lname FROM books; // selecting distinct author fname, lname combinations
```

## ORDER BY

> for sorting results

**it comes after the select**

```sql
   SELECT * FROM books ORDER BY author_fname;
   SELECT * FROM books ORDER BY author_fname ASC;
   -- Ascending order by default
   SELECT * FROM books ORDER BY author_fname DESC; -- descending order
   SELECT title, author_fname FROM books ORDER BY 2; -- order by the second column we're selecting
  SELECT title, author_fname, author_lname FROM books ORDER BY author_fname, author_lname; -- sort by fname first then by lname
```

## LIMIT Clause

> to control the number of results we get back

```sql
  SELECT * FROM books ORDER BY author_fname DESC LIMIT 5; -- start at row 0 and go for 5 rows
  SELECT * FROM books ORDER BY fname LIMIT 2,5; -- start at row 2 and go for 5 rows

```

## LIKE Operator

> for better searching

```sql
SELECT * FROM books WHERE author_fname LIKE '%da%'; -- % is a wildcard for finding any number of character
SELECT * FROM books WHERE author_fname LIKE '\_da%'; -- % is a wildcard for finding exactly one character
-- Escaping wildcards --> '%\%%' --> \% match a percent sign instead of a wildcard
```
