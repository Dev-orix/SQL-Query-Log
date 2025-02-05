
# SQL Query Log




## Overview
This repository contains a structured log of my SQL skillset, covering various SQL functionalities, commands, and examples for quick reference.

---

## 📌 Aggregate Functions
- `AVG()` - Returns an average value
- `ROUND()` - Specifies precision after a decimal
- `COUNT()` - Returns number of values
- `MAX()` - Returns the maximum value
- `MIN()` - Returns the minimum value
- `SUM()` - Returns the sum of all values

---

## 📌 ALTER Table
Allows modifications to an existing table structure.

### Adding Columns:
```sql
ALTER TABLE table_name
ADD COLUMN new_col TYPE;
```

### Alter Constraints:
```sql
ALTER TABLE table_name
ALTER COLUMN col_name
SET DEFAULT value;
```

Can also use `ADD CONSTRAINT constraint_name` instead of `SET DEFAULT`.

### Example:
```sql
ALTER TABLE information RENAME TO new_info;
ALTER TABLE new_info RENAME COLUMN person TO people;
ALTER TABLE new_info ALTER COLUMN people DROP NOT NULL;
```

---

## 📌 BETWEEN
Used to match a value against a range of values.

```sql
SELECT COUNT(column)
FROM table
WHERE column BETWEEN 8 AND 9;
```

Use `NOT BETWEEN` to exclude the specified range.

---

## 📌 CASE (Conditional Logic)
Works like an `IF/ELSE` statement in SQL.

```sql
SELECT customer_id,
CASE
    WHEN customer_id <= 100 THEN 'Premium'
    WHEN customer_id BETWEEN 101 AND 200 THEN 'Plus'
    ELSE 'Normal'
END AS customer_class
FROM customer;
```

Another example:
```sql
SELECT SUM(
    CASE rental_rate
        WHEN 0.99 THEN 1
        ELSE 0
    END) AS number_of_bargains
FROM film;
```

---

## 📌 CAST
Converts one data type to another.

```sql
SELECT CAST('5' AS INTEGER);
SELECT CAST(date AS TIMESTAMP) FROM table;
```

---

## 📌 String Manipulation
**Upper Case:**
```sql
SELECT UPPER(email) FROM customer;
```

**Lower Case:**
```sql
SELECT LOWER(email) FROM customer;
```

**Title Case:**
```sql
SELECT INITCAP(title) FROM film;
```

---

## 📌 CHECK (Constraints)
Used to enforce specific conditions in a table.

```sql
CREATE TABLE employees(
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birthdate DATE CHECK (birthdate > '1900-01-01'),
    hire_date DATE CHECK (hire_date > birthdate),
    salary INTEGER CHECK (salary > 0)
);
```

---

## 📌 COALESCE
Returns the first non-null value.

```sql
SELECT item, (price - COALESCE(discount, 0)) AS final FROM table;
```

---

## 📌 COUNT / COUNT DISTINCT
```sql
SELECT COUNT(name) FROM table;
SELECT COUNT(DISTINCT name) FROM table;
```

---

## 📌 CREATE TABLE
```sql
CREATE TABLE players (
    player_id SERIAL PRIMARY KEY,
    age INTEGER NOT NULL
);
```

Another example:
```sql
CREATE TABLE account (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(50) NOT NULL,
    email VARCHAR(250) UNIQUE NOT NULL,
    created_on TIMESTAMP NOT NULL,
    last_login TIMESTAMP
);
```

---

## 📌 DELETE
Removes rows from a table.

```sql
DELETE FROM table WHERE row_id = 1;
DELETE FROM tableA USING tableB WHERE tableA.id = tableB.id;
DELETE FROM table;
```

---

## 📌 DROP
Removes columns, indexes, or constraints.

```sql
ALTER TABLE table_name DROP COLUMN col_name;
ALTER TABLE table_name DROP COLUMN IF EXISTS col_name;
```

---

## 📌 JOINS
**INNER JOIN**:
```sql
SELECT * FROM TableA
INNER JOIN TableB
ON TableA.col_match = TableB.col_match;
```

**FULL OUTER JOIN**:
```sql
SELECT * FROM customer
FULL OUTER JOIN payment
ON customer.customer_id = payment.customer_id
WHERE customer.customer_id IS NULL OR payment.payment_id IS NULL;
```

**LEFT JOIN** (Returns all records from left table and matching records from right table):
```sql
SELECT * FROM TableA
LEFT JOIN TableB
ON TableA.id = TableB.id;
```

**RIGHT JOIN** (Opposite of LEFT JOIN):
```sql
SELECT * FROM TableA
RIGHT JOIN TableB
ON TableA.id = TableB.id;
```

---

## 📌 UNION
Combines results of multiple queries.

```sql
SELECT column_name FROM table1
UNION
SELECT column_name FROM table2;
```

---

## 📌 LIKE / ILIKE
Pattern matching with wildcards.

```sql
SELECT * FROM customer WHERE first_name ILIKE 'J%' AND last_name LIKE '%her%';
```

---

## 📌 ORDER BY
Sorts query results.

```sql
SELECT column_1, column_2 FROM table ORDER BY column_1 ASC;
```

---

## 📌 UPDATE
Modifies existing records.

```sql
UPDATE table SET column1 = value1 WHERE condition;
```

Updating based on another table:
```sql
UPDATE TableA
SET original_col = TableB.new_col
FROM TableB
WHERE TableA.id = TableB.id;
```

---


## 📌 VIEW
A VIEW stores a specific query, allowing simplified data retrieval.

### Example:
```sql
CREATE VIEW customer_info AS
SELECT first_name, last_name, address FROM customer
INNER JOIN address ON customer.address_id = address.address_id;
```

### Usage:
```sql
SELECT * FROM customer_info;
```

### Modification:
To update a VIEW, use:
```sql
CREATE OR REPLACE VIEW customer_info AS ...;
```

## 📌 WHERE
The `WHERE` clause filters rows based on conditions.

### Syntax:
```sql
SELECT column1, column2 FROM table WHERE condition;
```
**Example:**
```sql
SELECT first_name, last_name FROM customers WHERE name = 'David';
```

## 📌 Window Functions

### FIRST_VALUE(column)
Returns the first value in the table or partition.

### LAST_VALUE(column)
Returns the last value in the table or partition.

#### Example:
```sql
SELECT year, city,
FIRST_VALUE(city) OVER(ORDER BY year ASC) AS first_city,
LAST_VALUE(city) OVER(ORDER BY year ASC RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_city
FROM hosts
ORDER BY year ASC;
```

### ROW_NUMBER()
Assigns a unique row number to each row.

#### Example:
```sql
SELECT col_name, ROW_NUMBER() OVER() AS row_num FROM table_name ORDER BY row_num;
```

### LAG
Returns a column's value from the previous row.

#### Example:
```sql
SELECT column1, column2, LAG(column2, 1) OVER (ORDER BY column1 ASC) FROM table ORDER BY column1 ASC;
```

### LEAD
Returns a column's value from the next row.

#### Example:
```sql
WITH hosts AS (
    SELECT DISTINCT year, city FROM summer_medals
)
SELECT year, city,
LEAD(city, 1) OVER (ORDER BY year ASC) AS next_city,
LEAD(city, 2) OVER (ORDER BY year ASC) AS after_next_city
FROM hosts
ORDER BY year ASC;
```

### NTILE(n)
Splits the dataset into `n` approximately equal parts.

#### Example:
```sql
WITH disciplines AS (
    SELECT DISTINCT discipline FROM summer_medals
)
SELECT discipline, NTILE(15) OVER () AS page FROM disciplines ORDER BY page ASC;
```

#### Another Example:
```sql
WITH Athlete_Medals AS (
    SELECT Athlete, COUNT(*) AS Medals FROM Summer_Medals GROUP BY Athlete HAVING COUNT(*) > 1
)
SELECT Athlete, Medals, NTILE(3) OVER(ORDER BY Medals DESC) AS Third FROM Athlete_Medals ORDER BY Medals DESC, Athlete ASC;
```

## 📌 PARTITION BY
Divides a dataset into partitions based on unique column values.

#### Example:
```sql
SELECT column1, column2, column3,
LAG(column3) OVER (PARTITION BY column2 ORDER BY column2 ASC, column1 ASC)
FROM table_name
ORDER BY column2 ASC, column1 ASC;
```

## 📌 Ranking Functions

### ROW_NUMBER()
Assigns a unique number to each row.

### RANK()
Assigns the same rank to identical values, skipping numbers where necessary.

### DENSE_RANK()
Similar to `RANK()`, but without skipping numbers.

#### Example:
```sql
SELECT country, games,
ROW_NUMBER() OVER (ORDER BY games DESC) AS row_n,
RANK() OVER (ORDER BY games DESC) AS rank_n,
DENSE_RANK() OVER (ORDER BY games DESC) AS dense_rank_n
FROM table
ORDER BY games DESC, country ASC;
```



