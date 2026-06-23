# SQL Fundamentals

# Part 4 — SQL Clauses (FROM, WHERE, DISTINCT, GROUP BY, ORDER BY, HAVING)

---

# Introduction to SQL Clauses

In the previous section, we learned the four core CRUD operations:

- INSERT
- SELECT
- UPDATE
- DELETE

However, retrieving all records from a table is rarely useful in real-world environments.

Imagine a database containing:

```text
10,000 users
50,000 orders
500,000 log entries
```

Returning every record would be inefficient and difficult to analyze.

This is where SQL clauses become important.

Clauses allow us to:

- Filter data
- Sort data
- Group data
- Remove duplicates
- Aggregate information

Think of clauses as instructions that refine the results returned by SQL statements.

---

# Why Clauses Matter

Suppose a security analyst wants to answer:

```text
How many failed logins occurred today?
```

Or:

```text
Which users logged in more than 10 times?
```

Or:

```text
Which vulnerabilities are considered critical?
```

Without clauses, retrieving this information would be difficult.

Clauses allow us to ask specific questions and receive meaningful answers.

---

# SQL Query Flow

One of the most important concepts to understand is how SQL processes queries.

Many beginners assume SQL executes from left to right.

In reality, SQL follows a logical execution order.

Example:

```sql
SELECT name
FROM users
WHERE role='admin'
ORDER BY name;
```

Logical execution order:

```text
FROM
↓
WHERE
↓
SELECT
↓
ORDER BY
```

Understanding this flow becomes critical when working with more advanced queries.

---

# FROM Clause

The FROM clause specifies which table should be queried.

Without FROM, SQL would not know where to retrieve information.

---

# Syntax

```sql
SELECT *
FROM table_name;
```

---

# Example

```sql
SELECT *
FROM books;
```

Meaning:

```text
Retrieve all records
from the books table.
```

---

# Visual Representation

Database:

```text
thm_books
│
├── books
├── users
├── reviews
└── orders
```

Query:

```sql
FROM books
```

tells SQL exactly where to look.

---

# Why FROM Matters

Consider:

```sql
SELECT *
```

Without:

```sql
FROM books
```

SQL has no idea which table should be searched.

---

# WHERE Clause

The WHERE clause filters records.

Think of WHERE as a search condition.

---

# Why WHERE Exists

Suppose a bookstore contains:

| id | category |
|----|-----------|
| 1 | Offensive Security |
| 2 | Defensive Security |
| 3 | Offensive Security |

If we only want:

```text
Offensive Security
```

books, we need a filter.

---

# Syntax

```sql
SELECT *
FROM books
WHERE condition;
```

---

# Example

```sql
SELECT *
FROM books
WHERE category='Offensive Security';
```

---

# Result

Only books matching:

```text
Offensive Security
```

are returned.

---

# Real-World Example

A login system may execute:

```sql
SELECT *
FROM users
WHERE username='admin';
```

This searches for a specific account.

---

# Pentester Perspective

SQL Injection attacks frequently manipulate:

```sql
WHERE
```

clauses.

Example:

```sql
WHERE username='admin'
```

becomes:

```sql
WHERE username='admin'
OR 1=1
```

allowing attackers to bypass intended filters.

---

# DISTINCT Clause

DISTINCT removes duplicate values.

---

# Why DISTINCT Exists

Suppose a table contains:

| name |
|--------|
| Ethical Hacking |
| Ethical Hacking |
| Bug Bounty Bootcamp |

The database contains duplicates.

Sometimes we only want unique values.

---

# Example

Without DISTINCT:

```sql
SELECT name
FROM books;
```

Result:

```text
Ethical Hacking
Ethical Hacking
Bug Bounty Bootcamp
```

---

# Using DISTINCT

```sql
SELECT DISTINCT name
FROM books;
```

Result:

```text
Ethical Hacking
Bug Bounty Bootcamp
```

Duplicates disappear.

---

# Real-World Use Cases

Finding unique:

- Usernames
- Countries
- Categories
- Operating systems
- Vulnerability types

---

# Security Perspective

Imagine a SIEM database.

You may want:

```sql
SELECT DISTINCT source_ip
FROM logs;
```

to identify unique IP addresses involved in attacks.

---

# GROUP BY Clause

GROUP BY organizes records into groups.

This is one of the most powerful SQL clauses.

---

# Why GROUP BY Exists

Suppose a bookstore contains:

| category |
|-----------|
| Offensive Security |
| Offensive Security |
| Defensive Security |

Instead of listing each record individually, we may want:

```text
How many books exist per category?
```

---

# Syntax

```sql
SELECT category, COUNT(*)
FROM books
GROUP BY category;
```

---

# Result

| category | count |
|-----------|--------|
| Offensive Security | 2 |
| Defensive Security | 1 |

---

# How GROUP BY Works

Step 1:

Group records.

```text
Offensive Security
├── Book A
└── Book B

Defensive Security
└── Book C
```

---

Step 2:

Apply a function.

Example:

```sql
COUNT(*)
```

---

Result:

```text
Offensive Security = 2
Defensive Security = 1
```

---

# Aggregate Functions and GROUP BY

GROUP BY is commonly used with:

```sql
COUNT()
SUM()
AVG()
MAX()
MIN()
```

These are called aggregate functions because they combine multiple records into one result.

---

# Security Use Cases

SOC Analysts often use:

```sql
GROUP BY username
```

to identify:

- Excessive logins
- Brute force attempts
- Suspicious user activity

---

Example:

```sql
SELECT username,
COUNT(*)
FROM login_logs
GROUP BY username;
```

---

# ORDER BY Clause

ORDER BY sorts results.

---

# Why ORDER BY Exists

Suppose a vulnerability database contains:

| severity |
|----------|
| Critical |
| Medium |
| High |

We may want results organized in a specific order.

---

# Syntax

```sql
SELECT *
FROM table
ORDER BY column;
```

---

# Ascending Order (ASC)

Ascending means:

```text
A → Z
1 → 9
Old → New
```

---

# Example

```sql
SELECT *
FROM books
ORDER BY published_date ASC;
```

---

# Result

Oldest books appear first.

---

# Descending Order (DESC)

Descending means:

```text
Z → A
9 → 1
New → Old
```

---

# Example

```sql
SELECT *
FROM books
ORDER BY published_date DESC;
```

---

# Result

Newest books appear first.

---

# Security Use Cases

Threat hunters often use:

```sql
ORDER BY timestamp DESC;
```

to view:

```text
Most recent events first
```

---

Example:

```sql
SELECT *
FROM logs
ORDER BY timestamp DESC;
```

---

# HAVING Clause

HAVING is often confusing for beginners because it looks similar to WHERE.

However, they serve different purposes.

---

# WHERE vs HAVING

A useful rule:

```text
WHERE filters rows

HAVING filters groups
```

---

# Execution Flow

```text
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
SELECT
↓
ORDER BY
```

---

# Example

Suppose we count books per category.

```sql
SELECT category,
COUNT(*)
FROM books
GROUP BY category;
```

Result:

| category | count |
|-----------|--------|
| Offensive Security | 3 |
| Defensive Security | 1 |

---

Now we only want categories with more than one book.

---

# Using HAVING

```sql
SELECT category,
COUNT(*)
FROM books
GROUP BY category
HAVING COUNT(*) > 1;
```

---

Result:

| category | count |
|-----------|--------|
| Offensive Security | 3 |

---

# Why Not Use WHERE?

Incorrect:

```sql
WHERE COUNT(*) > 1
```

At the time WHERE executes:

```text
COUNT()
does not exist yet.
```

Aggregation happens later.

Therefore:

```text
WHERE cannot filter aggregate results.
HAVING can.
```

---

# Real-World Example

Suppose a login database contains:

| username |
|-----------|
| admin |
| admin |
| admin |
| alice |
| bob |

---

Query:

```sql
SELECT username,
COUNT(*)
FROM login_logs
GROUP BY username
HAVING COUNT(*) > 2;
```

---

Result:

```text
admin
```

This quickly identifies accounts generating excessive activity.

---

# Pentester Perspective

HAVING occasionally appears in:

- SQL Injection payloads
- Database enumeration
- Reporting queries

Attackers and defenders alike use aggregation to identify patterns in data.

---

# Common Beginner Mistakes

## Forgetting GROUP BY

Incorrect:

```sql
SELECT category,
COUNT(*)
FROM books;
```

May generate errors depending on database configuration.

---

## Confusing WHERE and HAVING

Incorrect:

```sql
WHERE COUNT(*) > 5
```

Correct:

```sql
HAVING COUNT(*) > 5
```

---

## Forgetting ASC or DESC

SQL defaults to:

```text
ASC
```

but explicitly specifying order improves readability.

---

## Using DISTINCT Unnecessarily

DISTINCT can be expensive on very large datasets.

Only use it when duplicate removal is actually required.

---

# Cybersecurity Relevance

Understanding clauses is essential because they form the foundation of:

- Log analysis
- Threat hunting
- SIEM searches
- Vulnerability reporting
- Database auditing
- SQL Injection exploitation

Examples:

```sql
WHERE
```

Used for filtering.

---

```sql
GROUP BY
```

Used for detecting patterns.

---

```sql
ORDER BY
```

Used for organizing results.

---

```sql
HAVING
```

Used for filtering aggregated data.

---

# Key Takeaways

In this section we learned:

- FROM
- WHERE
- DISTINCT
- GROUP BY
- ORDER BY
- HAVING

We also learned:

- SQL query execution flow
- Row filtering vs group filtering
- Data aggregation concepts
- Sorting and duplicate removal
- Security and pentesting applications

These clauses transform SQL from a simple data retrieval language into a powerful tool for analysis, reporting, threat hunting, and database enumeration.

In the next section, we will explore SQL Operators and Functions, which allow us to perform comparisons, manipulate data, calculate values, and build more advanced queries.