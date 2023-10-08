# SQL Cheat Sheet

# Introduction

**SQL (Structured Query Language)** is a powerful tool for managing data in **Relational Database Management Systems (RDBMS)**. This cheat sheet guides you through the basics of SQL and provides essential SQL commands and explanations.

# Terminology

- A database stores objects called **tables**.
- Every table is broken up into smaller entities called **fields**, which is the name of a **column** in the table.
- Each entry (row/tuple in the table, containing the actual data) is called a **record**.
- The tuples of a table may not be distinct (tables are **bags** not sets).
- A ************schema************ is specifying the structure of the database including the table names (outside the parentheses), all the fields in each table (inside the parentheses), and which field(s) in each table are keys (underlined fields). For example:

```
**Users**(user_id, first_name, last_name, address, email)
**Products**(product_id, product_name, description, price)
**Orders**(order_id, user, product_ordered, total_paid)
```

# Installation

To get started with SQL, you'll need to install an SQL server like **MySQL** or **MariaDB**. For installation instructions on Arch-based Linux distributions like Manjaro, [follow this link](https://www.geeksforgeeks.org/how-to-install-and-configure-mysql-on-arch-based-linux-distributionsmanjaro/).

Then, download [this sample dataset](https://github.com/dalers/mywind) or from this repo. To import the database and start playing around with it, execute the following commands:

1. Open terminal and login into MariaDB

```bash
mariadb -u <your_username> -p
```

1. Create a new database called “*northwind*” and exit

```sql
CREATE DATABASE northwind;
EXIT;
```

1. Import two files: “*northwind.sql* “and “*northwind-data.sql*” into the “*********northwind*********” database

```bash
mariadb -u <your_username> -p northwind < northwind.sql
mariadb -u <your_username> -p northwind < northwind-data.sql
```

# Getting Started

To start using MariaDB, enter the following in a terminal:

```bash
mariadb -u <your_username> -p
```

Then, a prompt like this will be displayed:

```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 11.1.2-MariaDB Arch Linux

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

The “*none*” in “*MariaDB [(none)]>*” indicates that no database is selected.

## Select a Database

```sql
USE northwind;
```

## Show List of Tables

```sql
SHOW TABLES;
```

## Show Fields of a Table

```sql
DESCRIBE customers;
```

## Execute Code in .sql File

```bash
mariadb -u <your_username> -p < "filename.sql"
```

# Querying a Database

All queries follow a roughly `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY` structure.

## SELECT & FROM - Retrieve Columns from a Table

Displays the all the tuples for the specified fields. Aliases (`AS` keyword for field names, no keyword for table names) can be used to shorten the name of both the field names and the table names (especially useful in more complex queries to reduce amount of typing).

### Select a Single Fields

```sql
SELECT first_name AS name
FROM customers C;
```

### Select Multiple Fields

```sql
SELECT id, company, first_name AS name
FROM customers C;
```

### Select All Fields

```sql
SELECT *
FROM customers C;
```

### Operations on Fields

## SELECT DISTINCT - Retrieve Unique Values

`SELECT DISTINCT` considers tuples distinct as long as there is at least 1 field where the values differ. `SELECT DISTINCT` will consider tuples distinct even if some fields have same values. Tuples are only considered equivalent (and thus its duplicates are excluded) if the values are the same in all of the fields. Compare the following`SELECT` and different`SELECT DISTINCT` queries:

```sql
SELECT ship_name, ship_city
FROM orders; --produces 48 rows
```

```sql
SELECT DISTINCT ship_name, ship_city
FROM orders; --produces 15 rows
```

```sql
SELECT DISTINCT ship_name
FROM orders; --produces 15 rows
```

```sql
SELECT DISTINCT ship_city
FROM orders; --produces 12 rows
```

## WHERE - Filter Records

Retrieves only the tuples that meet certain conditions.

### All Fields Equal to Exactly One Value

```sql
SELECT id, product_name, category, list_price
FROM products
WHERE list_price = 30;
```

### IN - Field Equal to Certain Set of Values

```sql
SELECT id, product_name, category, list_price
FROM products
WHERE list_price IN (18, 30, 40);
```

### Comparison Operators Include =, >, <. ≥, ≤, <> or ≠

```sql
SELECT id, product_name, category, list_price
FROM products
WHERE list_price > 15;
```

### BETWEEN - Field in Within A Numerical Range

```sql
SELECT id, product_name, category, list_price
FROM products
WHERE list_price BETWEEN 20 AND 30;
```

### NOT - Negation of Condition

```sql
SELECT id, product_name, category, list_price
FROM products
WHERE list_price NOT BETWEEN 20 AND 30;
```

### LIKE - Strings That Match Certain Pattern

- % - matches zero, one, or multiple characters of any kind
- _ - matches exactly one character of any kind

There are endless possibilities for pattern expressions, but some common patterns are:

### Starts With Certain Phrase

```sql
SELECT id, first_name, last_name
FROM customers
WHERE first_name LIKE "A%"; -- starts with A
```

### Ends With Certain Phrase

```sql
SELECT id, first_name, last_name
FROM customers
WHERE first_name LIKE "%e"; -- ends with e
```

### Phrase in Any Location, but Must Include the Phrase

```sql
SELECT id, first_name, last_name
FROM customers
WHERE first_name LIKE "%re%"; -- include re
```

### Hangman - Only Missing Some Letters

```sql
SELECT id, first_name, last_name
FROM customers
WHERE first_name LIKE "K_ren"; -- don't know what 2nd letter is
```

### IS NULL or IS NOT NULL

## ORDER BY - Sorts the Database by Certain Fields

### Sort by 1 Column Ascending

```sql
SELECT id, first_name, last_name
FROM customers
ORDER BY first_name ASC; -- ASC keyword can be omitted
```

### Sort by 1 Column Descending

```sql
SELECT id, first_name, last_name
FROM customers
ORDER BY first_name DESC;
```

### Sort by Multiple Columns

Sorting by multiple columns will first sort by the first column. If there are still tuples that have the same value for the first column, they will be sorted by the second column.

```sql
SELECT id, first_name, last_name
FROM customers
ORDER BY first_name DESC, last_name DESC;
-- Notice that the output contains in this order:
-- (25, "John", "Rodman")
-- (12, "John", "Edwards")
-- Since "John" was equal, it then sorted on the 2nd column
```

## Built-in Functions

## GROUP BY

## HAVING

## CASE WHEN - Generate Tuples Conditionally

```sql
SELECT
    CASE
        WHEN G.GRADE < 8 THEN "NULL"
        ELSE S.NAME
    END AS STAT,
    G.GRADE,
    S.MARKS
FROM STUDENTS S
CROSS JOIN GRADES G
WHERE
    MIN_MARK <= S.MARKS AND
    S.MARKS <= MAX_MARK
ORDER BY GRADE DESC, S.NAME­
```

## COALESCE - Return First Non-Null Expression

It's often used to handle situations where you want to retrieve a value from a list of columns but you're not sure which column will have a non-null value.

## OVER

The general structure of an OVER statements:

```sql
<window function> OVER(
	[PARTITION BY <column names>]
	[ORDER BY <column names>]
	[ <ROW or RANGE clause> ]
) AS Alias
```

### PARTITION BY

### ORDER BY

Cumulative price example:

```sql
SELECT order_date, 
       revenue,
       SUM(revenue) OVER (ORDER BY order_date) AS cumulative_revenue
FROM sales
ORDER BY order_date;
```

### ROW or RANGE Clause

# Joins

## Self JOIN

## INNER JOIN

## LEFT OUTER JOIN

## RIGHT OUTER JOIN

## FULL OUTER JOIN

## UNION

## UNION ALL

## INTERSECT

# Modify a Database

Create Database

Drop Database

Create Table

Drop Table

## INSERT INTO - Insert New Records Into a Table

### Specify Only Certain Columns

The other columns will have NULL values in them.

```sql
INSERT INTO privileges (id)
VALUES (5), (6), (7);
```

### Insert Into All Columns

```sql
INSERT INTO privileges
VALUES (3, "Free Lunches"), (4, "Health Insurance");
```

```sql
INSERT INTO employee_privileges
VALUES (5, 2);
```

### Insert Query

```sql
INSERT INTO tableau;
SELECT *
FROM source_table;
```

## DELETE FROM – Delete Existing Records in Table

### Delete All Records Matching Certain Condition

```sql
DELETE FROM privileges
WHERE id BETWEEN 3 AND 7;
```

### Delete All Records

If a condition is not specified, all values are deleted without deleting the table itself.

```sql
DELETE FROM employee_privileges;
```

# WITH - Temporary Query

# Temporary Table

Temporary tables are tables that only exist for the duration of the query and are automatically dropped at the end of the session. They can be useful when performing more complex queries. They are akin to variables in a programming language.

```sql
CREATE TABLE TEMPORARY TempTable (
    ID INT(16),
    Name VARCHAR(50)
);
```

# Recursion

If you want to build an ordered list of tuples, where the next tuple depends on the previous tuple in some way, a recursive query is effective:

```sql
WITH RECURSIVE FactorialCTE(n, factorial) AS (
    -- Base case
    SELECT 1, 1
    UNION ALL
    -- Recursive part
    SELECT n + 1, (n + 1) * factorial -- How to build next record
    FROM FactorialCTE
    WHERE n < 10 -- Complement of termination condition (n >= 10)
)
SELECT * FROM FactorialCTE;
```

# Custom Functions

Useful for creating queries where the result can vary based on some input condition.

```sql
CREATE FUNCTION AddNumbers (@num1 INT, @num2 INT)
RETURNS INT
AS
BEGIN
    RETURN @num1 + @num2;
END;
```
