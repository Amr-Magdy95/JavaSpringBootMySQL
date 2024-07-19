# Big Picture

> Database Server --has--> Databases --has--> Tables

> Databases can be created, dropped, used

## Databases

```sql
  CREATE DATABASE books;

  DROP DATABASE IF EXISTS books;

  USE books;

```

## Tables

> Tables can be created, dropped, queried, described

```sql
  CREATE TABLE Persons (
    PersonID int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) DEFAULT 'doe',
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),

    PRIMARY KEY('PersonID')
);

DROP TABLE Persons;

DESC Persons;
```

## CRUD

```sql
  SELECT * FROM persons;
  SELECT * FROM persons WHERE age >20;
  SELECT personId AS id FROM persons ;

  INSERT INTO table (col1, col2) VALUES(val1, val2);
  UPDATE cats SET breed="blah" WHERE breed="blue"; -- try selecting before updating
  UPDATE cats SET breed="blah", name="cat" WHERE name="kitty";
  DELETE FROM cats WHERE name="cat"

```

> Most of SQL is about writing fancy SELECT queries

## String Functions

### CONCAT

> combining strings for cleaner output

```sql
CONCAT(col1, col2, ....,coln)
SELECT CONCAT(fname, ' ', lname) AS 'Full Name' FROM books;
CONCAT_WS(separator, col1, col2, ...,col2); WS = With Separator
SELECT CONCAT_WS(' ', fname, lname) AS 'Full Name' FROM books;
```

### SUBSTRING

```sql
SELECT SUBSTRING("Hello World", 1, 4); // from index 1 for length of 4
    -- SUBSTRING("Hello World", 4); // start at ind 4 and go to the end
    -- SUBSTRING("Hello World" FROM 4);
    -- SUBSTRING("Hello World" FROM 4 FOR 2);
  -- Combine SUBSTRING with CONCAT
  SELECT CONCAT(SUBSTR(title, 1, 10), '...') AS 'short title' FROM books;
```

### REPLACE

> replace parts of a string

> REPLACE(string_to_operate_on, to_replace, to_replace_with);

```sql
SELECT REPLACE("Hello World", "hello", "hola");
```

### REVERSE

```sql SELECT REVERSE("HELLO WORLD");

```

### CHAR_LENGTH

```sql SELECT CHAR_LENGTH("Hello");

```

### UPPER and LOWER

```sql
SELECT LOWER("HELLO");
SELECT UPPER("hello");
```

### OTHER STRING FUNCTIONS

> INSERT, LEFT, RIGHT, REPEAT, TRIM(LEADING/TRAILING/BOTH);
