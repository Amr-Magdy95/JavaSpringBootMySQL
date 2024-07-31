# Data Types

## CHAR vs VARCHAR

> Both allow us to store text

> they differ in the way they're stored and retrieved, and maximum length

> CHAR has a fixed length, if you enter a smaller input, the input is right-padded with whitespaces to fit that length

> VARCHAR has a variable length, it fits to the input so long it doesn't exceed the max value 0-255

## TINYINT, INT, BIGINT, etc

> INTEGER and INT are the same

> TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT

> COULD SIGNED OR UNSIGNED

## DECIMAL

> allows us to store precise decimals

> DECIMAL(a, b) --> 'a' total number of digits, 'b' digits after the decimal point
> DECIMAL(5, 2) --> 999.99 largest number we could store here

```sql
CREATE TABLE products (price DECIMAL(5, 2));
```

## FLOAT & DOUBLE

> Store larger numbers using less space compared to DECIMAL BUT at the cost of PERCISION

> FLOAT RUNS AT PERCISION ISSUES AFTER AROUND 7 digits

> DOUBLE RUNS AT PERCISION ISSUES AFTER AROUND 15 digits

```sql
CREATE TABLE numbers (x FLOAT, y DOUBLE);
```

## DATE and TIME

> DATE stores a date with no time involved 'YYYY-MM-DD'

> TIME stores time with no date involved 'HH:MM:SS'

> DATETIME stores both 'YYYY-MM-DD HH:MM:SS'

## Working with Dates

```sql
CREATE TABLE people(
    name VARCHAR(100),
    birthdate DATE,
    birthtime TIME,
    birthdt DATETIME
);
```

## CURDATE, CURTIME, NOW

> NOW() = CURRENT_TIMESTAMP

```sql
INSERT INTO people (name, birthdate, birthtime, birth dt)
VALUES ('Minerva',CURDATE(), CURTIME(), NOW());
```

## Date Functions

```sql

SELECT birthdate, DAY(birthdate) FROM people;
SELECT birthdate, DAYOFWEEK(birthdate), DAYOFYEAR(birthdate), MONTHNAME(birthdate), YEAR(birthdt) FROM people;

```

## Time Functions

```sql
SELECT name, birthname, HOUR(birthtime) FROM people;
-- SECOND, MINUTE
```

## Formatting Dates

> DATE_FORMAT(format_string)

> https://www.w3schools.com/sql/func_mysql_date_format.asp

## Date Math

> DATEDIFF(expr1 - expr2)

> DATE_ADD(add, interval expr)

> DATE_SUB(add, interval expr)

## TIMESTAMPS

> same as datetime but smaller range and less memory

## DEFAULT & ON UPDATE TIMESTAMPS

```sql
CREATE TABLE captions(
    text VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```
