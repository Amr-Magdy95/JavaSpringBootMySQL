# Logical Operators

## NOT EQUAL

```sql
    SELECT title
    FROM books
    WHERE released_year != 2017;
```

## NOT LIKE

```sql
SELECT *
FROM books
WHERE title NOT LIKE '% %';
```

## GREATER THAN

```sql
SELECT *
FROM books
WHERE released_year > 2010;
```

## LESS THAN

```sql
SELECT *
FROM books
WHERE released_year < 2010;
-- WHERE released_year <= 2010;
```

## EQUAL TO

```sql
SELECT *
FROM books
WHERE released_year == 2010;
```

## Logical AND

> &&

```sql
SELECT *
FROM books
WHERE author_lname="Geiman" AND released_year>2010;
```

## Logical OR

```sql
SELECT *
FROM books
WHERE pages < 200 OR title LIKE '%e%';
```

## BETWEEN

```sql
SELECT *
FROM books
WHERE released_year BETWEEN 2004 AND 2015;
-- NOT BETWEEN
```

## Comparing Dates

> You can use >, <

> You can use CAST -- to convert one data type to another

```sql
SELECT *
FROM people
WHERE YEAR(birthdate) < 2005;

SELECT *
FROM people
WHERE HOUR(birthtime) > 12;

SELECT CAST('9:00:00' AS TIME);
```

## THE IN OPERATOR

```sql
SELECT *
FROM books
WHERE author_lname = '1' OR author_lname = '2' OR author_lname = '3';

SELECT *
FROM books
WHERE author_lname IN ('1', '2', '3', '4') ;
-- NOT IN

SELECT *
FROM books
WHERE released_year IN (SELECT released_year FROM books WHERE released_year != 0);
```

## CASE Statement

```sql
SELECT title, released_year,
    CASE
        WHEN released_year >= 2000 THEN 'Modern Lit'
        -- As Many WHEN ... THEN ... As you want
        ELSE '20th Century Lit'
    END AS GENRE
FROM books;
```

## IS NULL

```sql
SELECT *
FROM books
WHERE author_lname IS NULL;
-- IS NOT NULL
```
