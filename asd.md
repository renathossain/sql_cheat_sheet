# SQL Cheat Sheet

# Introduction

---

**SQL (Structured Query Language)** is a powerful tool for managing data in **Relational Database Management Systems (RDBMS)**. This cheat sheet provides essential SQL commands and explanations.

# Terminology

---

- A database stores objects called **tables**.
- Every table is broken up into smaller entities called **fields**, which is the name of a **column** in the table.
- Each entry (row in the table, containing the actual data) is called a **record**.
- A ************schema************ is specifying the structure of the database including the table names (outside the parentheses), all the fields in each table (inside the parentheses), and which field(s) in each table are keys (underlined fields). For example:

<aside>
📝 **Users**(user_id, first_name, last_name, address, email)
**Products**(product_id, product_name, description, price)
**Orders**(order_id, user, product_ordered, total_paid)

</aside>

# Installation

---

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

---

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

# Querying a Database

---

All queries follow a roughly `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY` structure.

## SELECT & FROM - Retrieve Columns from a Table

### Select a single field

```sql
SELECT first_name FROM customers;
```

### Select multiple fields

```sql
SELECT id, company, first_name FROM customers;
```

### Select all fields

```sql
SELECT * FROM customers;
```

## SELECT DISTINCT - Retrieve Unique Values

Select distinct considers tuples distinct even if some fields have same values. Tuples are only considered equivalent (and thus duplicates are excluded) if all of the fields are the same. Compare the following`SELECT` and different`SELECT DISTINCT` queries.

```sql
SELECT ship_name, ship_city FROM orders; --produces 48 rows
```

```sql
SELECT DISTINCT ship_name, ship_city FROM orders; --produces 15 rows
```

```sql
SELECT DISTINCT ship_name FROM orders; --produces 15 rows
```

```sql
SELECT DISTINCT ship_city FROM orders; --produces 12 rows
```