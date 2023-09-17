# SQL Cheat Sheet

## Introduction
SQL (Structured Query Language) is a powerful tool for managing data in Relational Database Management Systems (RDBMS). This cheat sheet provides essential SQL commands and explanations.

## Terminology
* A database stores objects called **tables**.
* Every table is broken up into smaller entities called **fields**, which is the name of a **column** in the table.
* Each entry (row in the table, containing the actual data) is called a **record**.

## Installation
To get started with SQL, you'll need to install an SQL server like MySQL or MariaDB. For installation instructions on Arch-based Linux distributions like Manjaro, [follow this link](https://www.geeksforgeeks.org/how-to-install-and-configure-mysql-on-arch-based-linux-distributionsmanjaro/).

## Basic SQL Commands

### SELECT - Retrieve Data
Use `SELECT` to retrieve data from a table.

**Example:**
```sql
-- Select a single field
SELECT <field_name> FROM <table_name>;

-- Select multiple fields
SELECT <field1>, <field2> FROM <table_name>;

-- Select all fields
SELECT * FROM <table_name>;
```

### SELECT DISTINCT - Retrieve Unique Values

Use `SELECT DISTINCT` to retrieve unique values from specified columns. This command considers the combination of values in the specified columns, ensuring that only distinct combinations are included in the result. If there are duplicates in one of the columns but not in the combination, those rows will still be included in the result.

**Example:**
```sql
-- Retrieve distinct combinations
SELECT DISTINCT <field1>, <field2>, ... FROM table_name;
```
