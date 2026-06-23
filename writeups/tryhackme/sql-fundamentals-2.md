# SQL Fundamentals

# Part 2 — Primary Keys, Foreign Keys, SQL, DBMS, and Database/Table Statements

---

# Primary Keys and Foreign Keys

One of the biggest advantages of relational databases is the ability to create relationships between data.

Before learning SQL queries, it is important to understand how databases uniquely identify records and connect different tables together.

This is achieved through:

- Primary Keys
- Foreign Keys

---

# Primary Keys

A Primary Key is a column used to uniquely identify each row in a table.

Every record must have a unique value in the primary key column.

Think of it like a government-issued ID number.

Even if two people share the same name:

```text
John Smith
John Smith
```

their ID numbers are different.

The ID uniquely identifies them.

---

## Example Table

| id | username |
|----|----------|
| 1 | admin |
| 2 | alice |
| 3 | bob |

In this example:

```text
id
```

is the Primary Key.

---

# Characteristics of a Primary Key

A Primary Key:

- Must be unique
- Cannot contain duplicate values
- Cannot be NULL
- There can only be one Primary Key per table

---

## Invalid Example

| id | username |
|----|----------|
| 1 | admin |
| 1 | alice |

This is invalid because:

```text
id = 1
```

appears twice.

The database would reject this.

---

## Why Primary Keys Matter

Without a Primary Key:

```text
How do we uniquely identify a record?
```

Imagine:

| name |
|--------|
| John |
| John |
| John |

Which John should be updated?

Which John should be deleted?

A Primary Key solves this problem.

---

# Cybersecurity Relevance

Primary Keys are frequently targeted during:

- Database enumeration
- SQL Injection
- Application logic analysis

Attackers often search for:

```text
id
user_id
employee_id
customer_id
```

because they reveal how data is organized.

---

# Foreign Keys

Primary Keys identify records.

Foreign Keys connect records.

---

## What is a Foreign Key?

A Foreign Key is a column that references a Primary Key from another table.

It creates a relationship between tables.

---

# Example

Suppose we have two tables.

## Authors

| id | author_name |
|----|-------------|
| 1 | Vicky Li |
| 2 | Daniel Miessler |

---

## Books

| id | book_name | author_id |
|----|------------|------------|
| 1 | Bug Bounty Bootcamp | 1 |
| 2 | Practical Security | 2 |

Notice:

```text
author_id
```

contains values that exist inside:

```text
Authors.id
```

This makes:

```text
author_id
```

a Foreign Key.

---

# Relationship Visualization

```text
Authors
│
├── id (Primary Key)
│
└── Books
     │
     └── author_id (Foreign Key)
```

---

# Why Foreign Keys Matter

Without relationships:

| Book | Author |
|--------|---------|
| Book A | John |
| Book B | John |
| Book C | John |

The author's name is repeated many times.

---

With Foreign Keys:

Authors

| id | name |
|----|------|
| 1 | John |

Books

| id | title | author_id |
|----|-------|------------|
| 1 | Book A | 1 |
| 2 | Book B | 1 |
| 3 | Book C | 1 |

Less duplication.

Better organization.

Better scalability.

---

# Active Directory Analogy

If you've been building an Active Directory lab, this concept should feel familiar.

Example:

```text
User
 │
 └── Member Of
       │
       └── Domain Admins
```

Relationships in AD are conceptually similar to relationships in relational databases.

Many cybersecurity tools internally rely on these same relationship concepts.

---

# What is SQL?

Now that we understand how data is organized, the next question becomes:

> How do we interact with the database?

The answer is SQL.

---

# SQL Definition

SQL stands for:

```text
Structured Query Language
```

SQL is a language used to:

- Create databases
- Create tables
- Insert data
- Retrieve data
- Modify data
- Delete data

---

# Why SQL Exists

Imagine a database containing:

```text
10 million users
```

How do we retrieve:

```text
admin
```

from those 10 million users?

SQL provides a standardized way to ask the database for information.

---

# Example SQL Query

```sql
SELECT * FROM users;
```

Meaning:

```text
Show me every record from the users table.
```

---

# What is a DBMS?

A Database Management System (DBMS) is software that manages databases.

Think of the DBMS as the middleman between users and the database.

---

# Architecture

```text
User
 │
 ▼
SQL Query
 │
 ▼
DBMS
 │
 ▼
Database
```

---

# Common DBMS Examples

## Relational DBMS

- MySQL
- MariaDB
- PostgreSQL
- Microsoft SQL Server
- Oracle Database

---

## NoSQL DBMS

- MongoDB
- Redis
- Cassandra

---

# MySQL

Throughout this room we use:

```text
MySQL
```

MySQL is one of the most popular relational database management systems in the world.

It is widely used in:

- Web applications
- Enterprise systems
- E-commerce platforms
- Cloud services

---

# Accessing MySQL

The room uses:

```bash
mysql -u root -p
```

---

# Command Breakdown

## mysql

Starts the MySQL client.

---

## -u

Specifies the user.

Example:

```bash
-u root
```

---

## -p

Requests a password.

---

# Result

After successful authentication:

```text
mysql>
```

appears.

This means you are now inside the MySQL shell.

---

# Database Statements

Before storing data, we must create and manage databases.

---

# CREATE DATABASE

Creates a new database.

---

## Syntax

```sql
CREATE DATABASE database_name;
```

---

## Example

```sql
CREATE DATABASE thm_bookmarket_db;
```

---

# What Happens?

Before:

```text
MySQL Server
│
├── mysql
├── sys
└── information_schema
```

After:

```text
MySQL Server
│
├── mysql
├── sys
├── information_schema
└── thm_bookmarket_db
```

---

# SHOW DATABASES

Lists available databases.

---

## Syntax

```sql
SHOW DATABASES;
```

---

## Example Output

```text
mysql
sys
information_schema
thm_bookmarket_db
```

---

# Common Beginner Mistake

Incorrect:

```sql
SHOW DATABASE;
```

Correct:

```sql
SHOW DATABASES;
```

Notice the trailing:

```text
S
```

---

# USE Statement

Selects the active database.

---

## Syntax

```sql
USE database_name;
```

---

## Example

```sql
USE thm_bookmarket_db;
```

---

# Why USE Matters

Suppose:

```text
Database A
Database B
Database C
```

When creating a table:

```sql
CREATE TABLE users (...);
```

MySQL needs to know:

```text
Which database should receive the table?
```

The USE statement answers that question.

---

# DROP DATABASE

Deletes a database.

---

## Syntax

```sql
DROP DATABASE database_name;
```

---

## Example

```sql
DROP DATABASE test_db;
```

---

# Warning

This operation removes:

- The database
- All tables
- All records

permanently.

---

# Table Statements

Once a database exists, we can create tables.

---

# CREATE TABLE

Creates a table.

---

## Syntax

```sql
CREATE TABLE table_name (
 column_name datatype
);
```

---

# Example

```sql
CREATE TABLE book_inventory (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    publication_date DATE
);
```

---

# Column Breakdown

## book_id

```sql
INT
```

Stores integers.

---

## AUTO_INCREMENT

Automatically increases values.

Example:

```text
1
2
3
4
5
```

---

## PRIMARY KEY

Uniquely identifies records.

---

## book_name

```sql
VARCHAR(255)
```

Stores text up to 255 characters.

---

## NOT NULL

Prevents empty values.

---

## publication_date

```sql
DATE
```

Stores dates.

---

# SHOW TABLES

Displays tables in the active database.

---

## Syntax

```sql
SHOW TABLES;
```

---

## Example Output

```text
book_inventory
```

---

# DESCRIBE

Displays table structure.

---

## Syntax

```sql
DESCRIBE table_name;
```

or

```sql
DESC table_name;
```

---

## Example

```sql
DESC book_inventory;
```

---

# Example Output

```text
Field
Type
Null
Key
Default
Extra
```

---

# Why DESCRIBE is Useful

Allows us to quickly identify:

- Column names
- Data types
- Primary Keys
- Auto Increment fields

---

# ALTER TABLE

Used to modify existing tables.

---

## Example

Add a page count column:

```sql
ALTER TABLE book_inventory
ADD page_count INT;
```

---

# Common ALTER Operations

## Add Column

```sql
ADD column_name datatype
```

---

## Remove Column

```sql
DROP COLUMN column_name
```

---

## Change Datatype

```sql
MODIFY column_name datatype
```

---

# DROP TABLE

Deletes a table.

---

## Syntax

```sql
DROP TABLE table_name;
```

---

## Example

```sql
DROP TABLE book_inventory;
```

---

# Warning

This removes:

- Table structure
- All rows
- All data

permanently.

---

# Pentester Notes

When performing SQL Injection or database enumeration, attackers often look for:

## Databases

```sql
SHOW DATABASES;
```

---

## Tables

```sql
SHOW TABLES;
```

---

## Table Structure

```sql
DESC users;
```

---

Common high-value tables:

```text
users
accounts
customers
employees
payments
credentials
```

Understanding database and table statements helps attackers map database structures and helps defenders understand how databases are organized.

---

# Key Takeaways

In this section we learned:

- Primary Keys
- Foreign Keys
- Database relationships
- SQL fundamentals
- DBMS fundamentals
- MySQL basics
- CREATE DATABASE
- SHOW DATABASES
- USE
- DROP DATABASE
- CREATE TABLE
- SHOW TABLES
- DESCRIBE
- ALTER TABLE
- DROP TABLE

These concepts establish the foundation required before storing, retrieving, and manipulating data using CRUD operations, which will be covered in the next section.