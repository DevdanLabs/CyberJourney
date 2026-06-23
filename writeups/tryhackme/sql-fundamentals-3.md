# SQL Fundamentals

# Part 3 — CRUD Operations (Create, Read, Update, Delete)

---

# Introduction to CRUD

Now that we understand:

- Databases
- Tables
- Rows
- Columns
- Primary Keys
- Foreign Keys
- Database Statements
- Table Statements

we can finally begin working with actual data.

This is where SQL becomes truly useful.

Almost every application in the world performs four fundamental operations on data:

1. Create
2. Read
3. Update
4. Delete

Together, these operations are known as:

```text
CRUD
```

CRUD forms the foundation of database interaction.

Whether using:

- Social media platforms
- Online banking
- E-commerce websites
- Active Directory
- Security tools

the underlying systems are constantly performing CRUD operations.

---

# Why CRUD Matters

Imagine a social media platform.

When a user:

Creates a post:

```text
CREATE
```

Views a post:

```text
READ
```

Edits a post:

```text
UPDATE
```

Deletes a post:

```text
DELETE
```

The same pattern appears everywhere.

---

# CRUD Overview

| Operation | SQL Statement |
|------------|------------|
| Create | INSERT |
| Read | SELECT |
| Update | UPDATE |
| Delete | DELETE |

These four statements represent the core of SQL.

---

# Database Used in This Section

The room uses:

```sql
USE thm_books;
```

and a table called:

```text
books
```

containing records similar to:

| id | name | published_date | description |
|----|----|----|----|
| 1 | Android Security Internals | 2014-10-14 | Android Security Book |

---

# CREATE Operation (INSERT)

The CREATE operation adds new records to a table.

In SQL this is performed using:

```sql
INSERT INTO
```

---

# Why INSERT Exists

Imagine an online store.

A new customer registers.

The customer's information must be stored somewhere.

Before registration:

```text
No record exists
```

After registration:

```text
A new record exists
```

This process is an INSERT operation.

---

# INSERT Syntax

```sql
INSERT INTO table_name
(column1, column2, column3)
VALUES
(value1, value2, value3);
```

---

# Example

```sql
INSERT INTO books
(id, name, published_date, description)
VALUES
(
1,
"Android Security Internals",
"2014-10-14",
"An In-Depth Guide to Android's Security Architecture"
);
```

---

# Query Breakdown

## INSERT INTO

Specifies the target table.

```sql
INSERT INTO books
```

Meaning:

```text
Insert data into the books table.
```

---

## Column List

```sql
(id, name, published_date, description)
```

Specifies which columns will receive values.

---

## VALUES

Contains the data being inserted.

```sql
VALUES (...)
```

---

# Visual Representation

Before INSERT:

| id | name |
|----|----|
| Empty |

After INSERT:

| id | name |
|----|----|
| 1 | Android Security Internals |

---

# Common INSERT Errors

## Missing Required Values

If a column is:

```sql
NOT NULL
```

and no value is supplied:

```sql
INSERT INTO books (id)
VALUES (1);
```

The database will reject the query.

---

## Duplicate Primary Keys

If:

```sql
id = 1
```

already exists:

```sql
INSERT INTO books
VALUES (1, ...);
```

fails because Primary Keys must be unique.

---

# Cybersecurity Perspective

During attacks, INSERT statements may be abused to:

- Create rogue accounts
- Add administrator users
- Plant malicious data
- Manipulate application behavior

Example:

```sql
INSERT INTO users
(username, role)
VALUES
('attacker', 'admin');
```

---

# READ Operation (SELECT)

The READ operation retrieves data from a database.

This is the most frequently used SQL operation.

It is performed using:

```sql
SELECT
```

---

# Why SELECT Matters

Every time a website displays information:

- User profiles
- Products
- Comments
- Orders

a SELECT query is usually running behind the scenes.

---

# SELECT Syntax

```sql
SELECT columns
FROM table_name;
```

---

# Selecting All Columns

```sql
SELECT *
FROM books;
```

---

# What Does * Mean?

The asterisk means:

```text
Return every column.
```

---

# Example Output

| id | name | published_date | description |
|----|----|----|----|
| 1 | Android Security Internals | 2014-10-14 | Android Security Book |

---

# Selecting Specific Columns

Instead of all columns:

```sql
SELECT *
```

we can request specific ones.

Example:

```sql
SELECT name, description
FROM books;
```

---

# Output

| name | description |
|----|----|
| Android Security Internals | Android Security Book |

---

# Why Select Specific Columns?

Advantages:

- Faster queries
- Less data transferred
- Better performance
- Reduced exposure of sensitive information

---

# Real-World Example

Login systems often perform:

```sql
SELECT username, password
FROM users;
```

rather than:

```sql
SELECT *
FROM users;
```

---

# Cybersecurity Perspective

Most SQL Injection attacks focus on SELECT queries.

Attackers often attempt to retrieve:

- Usernames
- Password hashes
- API keys
- Tokens
- Sensitive business data

Example:

```sql
SELECT username,password
FROM users;
```

---

# UPDATE Operation

The UPDATE operation modifies existing records.

---

# Why UPDATE Exists

Imagine a user changes:

- Email address
- Phone number
- Password

The existing record must be updated.

---

# UPDATE Syntax

```sql
UPDATE table_name
SET column=value
WHERE condition;
```

---

# Example

```sql
UPDATE books
SET description =
"An In-Depth Guide to Android's Security Architecture."
WHERE id = 1;
```

---

# Query Breakdown

## UPDATE

Target table.

```sql
UPDATE books
```

---

## SET

Specifies new values.

```sql
SET description = "..."
```

---

## WHERE

Determines which record is modified.

```sql
WHERE id = 1
```

---

# Visual Example

Before:

| id | description |
|----|----|
| 1 | Old Description |

After:

| id | description |
|----|----|
| 1 | New Description |

---

# The Importance of WHERE

Consider:

```sql
UPDATE books
SET description = "Updated";
```

No WHERE clause exists.

---

Result:

| id | description |
|----|----|
| 1 | Updated |
| 2 | Updated |
| 3 | Updated |

Every record changes.

---

# Common Mistake

Forgetting:

```sql
WHERE
```

can accidentally modify an entire table.

This is one of the most common SQL mistakes.

---

# Cybersecurity Perspective

Attackers who gain database access may attempt:

```sql
UPDATE users
SET role='admin'
WHERE username='attacker';
```

to elevate privileges.

---

# DELETE Operation

DELETE removes records from a table.

---

# Why DELETE Exists

Data may need to be removed when:

- Users delete accounts
- Products are discontinued
- Records become obsolete

---

# DELETE Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

---

# Example

```sql
DELETE FROM books
WHERE id = 1;
```

---

# Query Breakdown

## DELETE

Specifies removal.

---

## FROM

Specifies the target table.

```sql
FROM books
```

---

## WHERE

Determines which row is removed.

```sql
WHERE id = 1
```

---

# Visual Example

Before:

| id | name |
|----|----|
| 1 | Android Security Internals |

After:

| id | name |
|----|----|
| Empty |

---

# The Importance of WHERE

Dangerous:

```sql
DELETE FROM books;
```

---

Result:

```text
Every row is deleted.
```

The table remains.

The data does not.

---

# DELETE vs DROP

Many beginners confuse these.

---

## DELETE

Removes rows.

Table remains.

```sql
DELETE FROM books;
```

---

## DROP

Removes entire table.

```sql
DROP TABLE books;
```

---

# Comparison

| Operation | Result |
|------------|------------|
| DELETE | Removes data |
| DROP | Removes table and data |

---

# CRUD Workflow Example

Imagine a bookstore application.

---

## Create

```sql
INSERT INTO books ...
```

Add a new book.

---

## Read

```sql
SELECT * FROM books;
```

Display inventory.

---

## Update

```sql
UPDATE books ...
```

Correct information.

---

## Delete

```sql
DELETE FROM books ...
```

Remove discontinued books.

---

# Pentester Perspective

Understanding CRUD is critical because almost every SQL Injection attack manipulates CRUD operations.

---

# Enumeration

Attackers commonly abuse:

```sql
SELECT
```

to retrieve information.

---

# Data Theft

Examples:

```sql
SELECT username,password
FROM users;
```

---

```sql
SELECT credit_card_number
FROM payments;
```

---

# Privilege Escalation

Attackers may abuse:

```sql
UPDATE
```

Example:

```sql
UPDATE users
SET role='admin';
```

---

# Persistence

Attackers may abuse:

```sql
INSERT
```

to create hidden accounts.

---

# Destructive Attacks

Attackers may abuse:

```sql
DELETE
```

or

```sql
DROP
```

to destroy data.

---

# Common Interview Questions

## What does CRUD stand for?

Create, Read, Update, Delete.

---

## Which SQL statement performs Create?

```sql
INSERT
```

---

## Which SQL statement performs Read?

```sql
SELECT
```

---

## Which SQL statement performs Update?

```sql
UPDATE
```

---

## Which SQL statement performs Delete?

```sql
DELETE
```

---

## Why is WHERE important?

Because it limits which records are affected.

Without WHERE:

- UPDATE may modify all rows
- DELETE may remove all rows

---

# Key Takeaways

In this section we learned:

- CRUD fundamentals
- INSERT
- SELECT
- UPDATE
- DELETE
- Query structure
- Real-world usage
- Common mistakes
- Security implications
- Pentester relevance

CRUD operations form the foundation of nearly all database interactions and are essential knowledge before learning more advanced SQL topics such as clauses, operators, functions, joins, and SQL Injection techniques.