# sql_cheat_sheet
## Overview
* Stands for Structured Query Language. It is an ANSI standard for managing data held in a Relational Database Management System (RDBMS).
* An SQL server is a software that needs to be downloaded and installed in order to manage SQL **databases**, such as the open source MySQL (on Arch Linux it is called MariaDB, to install follow the link: <ul>https://www.geeksforgeeks.org/how-to-install-and-configure-mysql-on-arch-based-linux-distributionsmanjaro/</ul>).
* A database stores objects called **tables**.
* Every table is broken up into smaller entities called **fields**, which is the name of a **column** in the table.
* Each entry (row in the table, containing the actual data) is called a **record**.
## Commands for querying data
### SELECT - Select fields from table
`SELECT <field_name> FROM <table_name>;` to select single field
`SELECT <field1>, <field2>, <field4>, ... FROM <table_name>;` to select multiple fields
`SELECT * FROM <table_name>;` to select all fields
