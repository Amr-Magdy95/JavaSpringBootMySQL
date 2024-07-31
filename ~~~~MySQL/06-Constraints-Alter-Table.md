# Constraints & ALTER TABLE

> NOT NULL is a Constraint

## UNIQUE CONSTRAINT

```sql
CREATE TABLE companies(
    supplier_id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(15) NOT NULL UNIQUE,
    address VARCHAR(255) NOT NULL,
    PRIMARY KEY (supplier_id)
);
```

## CHECK CONSTRAINT

> Extra Fancy Constraint on given column(s)

```sql
CREATE TABLE users(
    username VARCHAR(20) NOT NULL ,
    age INT CHECK (age > 0)
);

CREATE TABLE palindromes{
    word VARCHAR(100) CHECK (REVERSE(word) = word);
}
```

## Named Constraints

```sql
CREATE TABLE users (
    name VARCHAR(50),
    age INT,
    CONSTRAINT age_over_18 CHECK (age > 18);
);
```

## Multiple Column Constraints

```sql
CREATE TABLE companies(
    supplier_id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(15) NOT NULL UNIQUE,
    address VARCHAR(255) NOT NULL,
    PRIMARY KEY (supplier_id),
    CONSTRAINT name_address UNIQUE (name, address);

);
-- the combination of name and address must be unique for each row
```

## ALTER TABLE

### Adding Columns

```sql
ALTER TABLE companies
ADD COLUMN city VARCHAR(50);
-- ADD COLUMN city VARCHAR(50) NOT NULL;
-- ADD COLUMN city VARCHAR(50) NOT NULL DEFAULT 'Cosmo';

```

### Dropping Columns

```sql
ALTER TABLE companies
DROP COLUMN phone;
```

### Renaming

```sql
RENAME TABLE companies TO suppliers;
ALTER TABLE supplier RENAME TO companies;
ALTER TABLE companies
RENAME COLUMN name TO biz_name;
```

### Modifying Columns

```sql
ALTER TABLE suppliers
MODIFY biz_name VARCHAR(100) DEFAULT 'unknown';

-- To change column name and data type
ALTER TABLE suppliers
CHANGE business biz_name VARCHAR(50);
```

### Constraints

```sql
ALTER TABLE houses
ADD CONSTRAINT postive_pprice CHECK (purchase_price >= 0);
ALTER TABLE houses
DROP CONSTRAINT positive_pprice;
```
