# One-To-Many & Joins

## Relationship Basics

Broad Relationship Types

- One to One Relationship
- One to Many Relationship book ---1---m--- reviews
  - FOREIGN KEY of book's PRIMARY KEY in reviews table
  - FOREIGN KEYS reference another table in the given table
- Many to Many Relationship book--m---m--authors

```sql
CREATE TABLE customer (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL,
);

CREATE TABLE orders(
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATE,
    amount DECIMAL(8, 2),
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

## CROSS JOINS

```sql
-- cross joins combine everything , not really useful
SELECT *
FROM customers, orders;
```

## INNER JOINS

> Selects all records from A, B where the join condition is met

> INNER JOIN targets the overlap of two tables

> LEFT JOIN targets the overlap of two tables and the left table

> RIGHT JOIN targets the overlap of two tables and the right table

```sql
SELECT *
FROM customers
INNER JOIN orders
ON customers.id = orders.customer_id;
```

## INNER JOIN with GROUP BY

```sql
SELECT first_name, last_name, SUM(amount) AS TOTAL
FROM customers
INNER JOIN orders
ON customers.id = orders.customer_id
GROUP BY customers.first_name, customers.last_name
ORDER BY TOTAL;
```

## LEFT JOIN

> Selects everything from A along with any matching records in B

```sql

SELECT *
FROM customers
LEFT JOIN orders
ON  orders.customer_id = customers.id;
```

## LEFT JOIN With GROUP BY

```sql
SELECT first_name, last_name, IFNULL(SUM(amount), 0) AS money_spent
FROM customers
LEFT JOIN orders
ON orders.customer_id = customers.id
GROUP BY first_name, last_name;
;
```

## RIGHT JOIN

> Selects everything from B along with any matching records of A

```sql
SELECT *
FROM customers
RIGHT JOIN orders
ON customers.id = orders.customer_id
;
```

## ON DELETE CASCADE

> Exists with Foreign Keys

```sql
CREATE TABLE orders(
  -- ...
  FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE;
);
```
