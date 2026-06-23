# SQL Fundamentals

# Part 5 — SQL Operators, Functions, Security Relevance, and Final Notes

---

# Introduction

So far, we have learned:

- What databases are
- Relational vs Non-Relational databases
- Tables, rows, and columns
- Primary and Foreign Keys
- SQL and DBMS concepts
- Database and table statements
- CRUD operations
- SQL clauses

At this point, we can already retrieve and manipulate data.

However, real-world SQL queries often require additional logic and data processing capabilities.

This is where:

- Operators
- Functions

become extremely important.

Operators allow us to compare values and build conditions.

Functions allow us to manipulate and analyze data.

Together, they significantly increase the power and flexibility of SQL.

---

# SQL Operators

Operators allow us to compare values, combine conditions, and determine whether records should be included in query results.

Think of operators as decision-making tools.

---

# Logical Operators

Logical operators evaluate conditions and return either:

```text
TRUE
```

or

```text
FALSE
```

---

# LIKE Operator

The LIKE operator searches for patterns within strings.

Unlike:

```sql
=
```

which requires an exact match,

LIKE allows partial matching.

---

## Syntax

```sql
SELECT *
FROM books
WHERE name LIKE pattern;
```

---

## Wildcards

The most commonly used wildcard is:

```sql
%
```

Meaning:

```text
Zero or more characters
```

---

## Example

```sql
SELECT *
FROM books
WHERE name LIKE '%Hack%';
```

---

This matches:

```text
Ethical Hacking
Car Hacker's Handbook
Hack The Box
```

because all contain:

```text
Hack
```

somewhere in the string.

---

# Why LIKE Matters

Searching exact values is often impractical.

Example:

```sql
WHERE username='administrator'
```

requires an exact match.

But:

```sql
WHERE username LIKE 'admin%'
```

can find:

```text
admin
administrator
admin01
admin_backup
```

---

# Security Relevance

Attackers frequently use:

```sql
LIKE
```

during database enumeration.

Example:

```sql
SELECT *
FROM users
WHERE username LIKE 'adm%';
```

This helps discover potential administrative accounts.

---

# AND Operator

AND requires every condition to be true.

---

## Syntax

```sql
SELECT *
FROM books
WHERE condition1
AND condition2;
```

---

## Example

```sql
SELECT *
FROM books
WHERE category='Offensive Security'
AND name='Bug Bounty Bootcamp';
```

---

Only records matching BOTH conditions are returned.

---

## Truth Table

| A | B | A AND B |
|---|---|---|
| True | True | True |
| True | False | False |
| False | True | False |
| False | False | False |

---

# OR Operator

OR requires at least one condition to be true.

---

## Example

```sql
SELECT *
FROM books
WHERE name LIKE '%Android%'
OR name LIKE '%iOS%';
```

---

Result:

```text
Android Security Internals
iOS Security Guide
```

---

## Truth Table

| A | B | A OR B |
|---|---|---|
| True | True | True |
| True | False | True |
| False | True | True |
| False | False | False |

---

# Security Relevance

One of the most famous SQL Injection payloads uses OR:

```sql
' OR 1=1 --
```

Because:

```sql
1=1
```

is always true.

This can cause applications to return unexpected results.

---

# NOT Operator

NOT reverses a condition.

---

## Example

```sql
SELECT *
FROM books
WHERE NOT category='Offensive Security';
```

---

Result:

```text
All books except Offensive Security books
```

---

# BETWEEN Operator

BETWEEN retrieves values within a range.

---

## Example

```sql
SELECT *
FROM books
WHERE id BETWEEN 2 AND 5;
```

---

Equivalent to:

```sql
WHERE id >= 2
AND id <= 5
```

---

# Comparison Operators

Comparison operators compare values directly.

---

# Equal (=)

```sql
WHERE username='admin'
```

Returns exact matches.

---

# Not Equal (!=)

```sql
WHERE category!='USB attacks'
```

Returns everything except USB attacks.

---

# Greater Than (>)

```sql
WHERE amount > 100
```

Returns values larger than 100.

---

# Less Than (<)

```sql
WHERE amount < 100
```

Returns values smaller than 100.

---

# Greater Than or Equal (>=)

```sql
WHERE amount >= 300
```

Returns:

```text
300 and above
```

---

# Less Than or Equal (<=)

```sql
WHERE amount <= 50
```

Returns:

```text
50 and below
```

---

# SQL Functions

Functions allow us to manipulate and analyze data.

Think of them as built-in tools provided by SQL.

---

# String Functions

String functions operate on text.

---

# CONCAT()

CONCAT combines multiple strings together.

---

## Syntax

```sql
CONCAT(value1, value2, value3)
```

---

## Example

```sql
SELECT CONCAT(
name,
' is a ',
category,
' book.'
)
FROM books;
```

---

Result:

```text
Android Security Internals is a Defensive Security book.
```

---

# Real-World Usage

Useful when generating reports or formatting output.

---

# GROUP_CONCAT()

GROUP_CONCAT combines multiple rows into a single string.

---

## Example

```sql
SELECT category,
GROUP_CONCAT(name)
FROM books
GROUP BY category;
```

---

Result:

```text
Offensive Security:
Bug Bounty Bootcamp, Ethical Hacking

Defensive Security:
Android Security Internals
```

---

# Security Relevance

GROUP_CONCAT frequently appears in SQL Injection attacks.

Attackers may use:

```sql
GROUP_CONCAT(username)
```

or

```sql
GROUP_CONCAT(table_name)
```

to retrieve multiple results in a single response.

---

# SUBSTRING()

SUBSTRING extracts part of a string.

---

## Syntax

```sql
SUBSTRING(
string,
start_position,
length
)
```

---

## Example

```sql
SELECT SUBSTRING(
published_date,
1,
4
)
FROM books;
```

---

Result:

```text
2014
2021
2016
```

Only the year is returned.

---

# LENGTH()

LENGTH counts characters.

---

## Example

```sql
SELECT LENGTH(name)
FROM books;
```

---

Result:

```text
26
19
21
```

depending on the title length.

---

# Security Relevance

LENGTH is commonly used in Blind SQL Injection.

Example:

```sql
LENGTH(database())
```

This can reveal information about hidden database names.

---

# Aggregate Functions

Aggregate functions combine multiple rows into a single result.

---

# COUNT()

COUNT returns the number of records.

---

## Example

```sql
SELECT COUNT(*)
FROM books;
```

---

Result:

```text
5
```

---

# Use Cases

- Counting users
- Counting events
- Counting vulnerabilities

---

# SUM()

SUM adds numeric values together.

---

## Example

```sql
SELECT SUM(price)
FROM books;
```

---

Result:

```text
249.95
```

---

# Use Cases

- Revenue calculations
- Cost analysis
- Asset valuation

---

# MAX()

MAX returns the highest value.

---

## Example

```sql
SELECT MAX(published_date)
FROM books;
```

---

Result:

```text
2021-12-21
```

---

# Use Cases

- Latest event
- Highest score
- Most recent login

---

# MIN()

MIN returns the smallest value.

---

## Example

```sql
SELECT MIN(published_date)
FROM books;
```

---

Result:

```text
2014-10-14
```

---

# Use Cases

- Earliest record
- Oldest asset
- First login

---

# SQL Injection Foundations

One of the biggest reasons cybersecurity professionals learn SQL is because of:

```text
SQL Injection
```

(SQLi)

---

# What is SQL Injection?

SQL Injection occurs when user-controlled input is interpreted as part of an SQL query.

Instead of supplying data:

```text
username
```

the attacker supplies:

```text
SQL commands
```

---

# Example

Application query:

```sql
SELECT *
FROM users
WHERE username='admin';
```

Attacker input:

```sql
admin' OR 1=1 --
```

Resulting query:

```sql
SELECT *
FROM users
WHERE username='admin'
OR 1=1;
```

Since:

```sql
1=1
```

is always true,

the query logic changes.

---

# Why SQL Fundamentals Matter

Notice how SQL Injection relies heavily on concepts from this room:

- SELECT
- WHERE
- AND
- OR
- LIKE
- COUNT
- GROUP_CONCAT

Without understanding SQL, SQL Injection becomes memorization instead of understanding.

---

# Red Team Perspective

Understanding SQL allows attackers to:

- Enumerate databases
- Discover tables
- Extract sensitive information
- Escalate privileges
- Maintain persistence

---

# Blue Team Perspective

Understanding SQL allows defenders to:

- Audit queries
- Detect suspicious behavior
- Investigate database activity
- Secure database access
- Prevent SQL Injection

---

# Detection Perspective

Security teams often monitor:

- Unusual SQL queries
- Large data exports
- Unexpected database access
- Authentication anomalies

---

# Skills Gained

By completing SQL Fundamentals, you learned:

## Database Concepts

- Relational databases
- Non-relational databases
- Tables
- Rows
- Columns

---

## Database Relationships

- Primary Keys
- Foreign Keys

---

## SQL Basics

- SQL syntax
- Database statements
- Table statements

---

## CRUD Operations

- INSERT
- SELECT
- UPDATE
- DELETE

---

## Clauses

- FROM
- WHERE
- DISTINCT
- GROUP BY
- ORDER BY
- HAVING

---

## Operators

- LIKE
- AND
- OR
- NOT
- BETWEEN
- Comparison operators

---

## Functions

- CONCAT
- GROUP_CONCAT
- SUBSTRING
- LENGTH
- COUNT
- SUM
- MAX
- MIN

---

# Knowledge Gaps Remaining

Topics not covered in this room include:

- JOINs
- UNION
- Subqueries
- Views
- Stored Procedures
- Triggers
- Database Security Hardening
- SQL Injection Techniques

---

# Recommended Next Topics

After this room, consider learning:

1. SQL Joins
2. Database Relationships
3. SQL Injection Fundamentals
4. Web Application Fundamentals
5. Authentication Mechanisms
6. OWASP Top 10
7. Web Pentesting

---

# Key Takeaways

Databases are everywhere.

SQL provides a standardized method for interacting with relational databases.

CRUD operations allow us to manage data.

Clauses allow us to filter and organize data.

Operators allow us to build logical conditions.

Functions allow us to manipulate and analyze information.

Understanding SQL is essential for both offensive and defensive cybersecurity roles and serves as a foundational skill for web security, threat hunting, database administration, and penetration testing.

---

# References

- TryHackMe — SQL Fundamentals
- MySQL Documentation
- OWASP SQL Injection Prevention Cheat Sheet
- PostgreSQL Documentation
- MariaDB Documentation

---

# Tags

```text
SQL
MySQL
Databases
Relational Databases
NoSQL
Cybersecurity
Web Security
Database Security
SQL Injection
TryHackMe
Learning
CyberJourney
Pentesting
Blue Team
Red Team
```