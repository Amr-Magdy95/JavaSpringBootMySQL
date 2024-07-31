# Many-To-Many

## Many-To-Many Basics

> Tables are connected through an intermediary table

> Series --m-----m-- Reviews

> Series --1-----m--ReviewsData--m---1-- Reviewers

```sql
CREATE TABLE reviewers(
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL
);
CREATE TABLE series(
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    released_year YEAR ,
    genre VARCHAR(100) NOT NULL
);
CREATE TABLE reviews(
    id INT PRIMARY KEY AUTO_INCREMENT,
    rating DECIMAL(3,1),
    series_id INT,
    reviewer_id INT,
    FOREIGN KEY (series_id) REFERENCES series(id),
    FOREIGN KEY (reviewer_id) REFERENCES reviewers(id),

    CONSTRAINT is_rating_valid CHECK(rating <= 10 && rating >= 0.0 )
);
```

## Basic Queries

> Just a one to - many

```sql
SELECT title, rating
FROM series
LEFT JOIN reviews
ON series.id = reviews.series_id;

SELECT title, ROUND(AVG(rating), 2) AS avg_rating
FROM series
INNER JOIN reviews
ON series.id = reviews.series_id
GROUP BY title
ORDER BY avg_rating;
```

```sql
SELECT CONCAT_WS(' ', first_name, last_name) AS ReviewerFN, rating
FROM reviewers AS subject
INNER JOIN reviews AS object
ON subject.id = object.reviewer_id;
```

```sql
SELECT *
FROM series
LEFT JOIN reviews
ON reviews.series_id = series.id
WHERE rating IS NULL;
```

```sql
-- USING CASE
SELECT
    first_name,
    last_name,
    COUNT(rating) AS count,
    IFNULL(MIN(rating), 0) AS min,
    IFNULL(MAX(rating), 0) AS max,
    ROUND(IFNULL(AVG(rating), 0), 2) AS average,
    CASE
        WHEN COUNT(rating) >= 10 THEN 'POWERUSER'
        WHEN COUNT(rating) > 0 THEN 'ACTIVE'
        ELSE 'INACTIVE'
    END AS status
FROM
    reviewers
        LEFT JOIN
    reviews ON reviewers.id = reviews.reviewer_id
GROUP BY first_name , last_name;

-- USING IF
SELECT
    first_name,
    last_name,
    COUNT(rating) AS count,
    IFNULL(MIN(rating), 0) AS min,
    IFNULL(MAX(rating), 0) AS max,
    ROUND(IFNULL(AVG(rating), 0), 2) AS average,
    IF(COUNT(rating) > 0,
        'ACTIVE',
        'INACTIVE') AS status
FROM
    reviewers
        LEFT JOIN
    reviews ON reviewers.id = reviews.reviewer_id
GROUP BY first_name , last_name;
```

```sql
-- the order doesn't matter
SELECT title, rating, CONCAT_WS(' ', first_name, last_name) AS Reviewer
FROM reviews
INNER JOIN series
ON reviews.series_id = series.id
INNER JOIN reviewers
ON reviews.id = reviews.reviewer_id;
```
