# SQL Fundamentals

## Executive Summary

Databases are one of the most important technologies in modern computing. Nearly every application, website, operating system, cloud service, authentication system, and security tool relies on databases to store, retrieve, and manage information.

Understanding how databases work is an essential skill for cybersecurity professionals. Whether working as a Security Analyst, Penetration Tester, Security Engineer, Threat Hunter, Incident Responder, or Bug Bounty Hunter, you will eventually interact with databases.

This room introduces the fundamental concepts of databases and Structured Query Language (SQL), the language used to communicate with relational databases. It covers database structures, relational and non-relational databases, tables, rows, columns, keys, and the core SQL concepts needed to manage and retrieve data.

By the end of this room, you will understand how relational databases are organized, how SQL interacts with them, and why these concepts are critical in both offensive and defensive security.

---

# Learning Objectives

After completing this room, you should be able to:

- Explain what a database is
- Differentiate between relational and non-relational databases
- Understand tables, rows, and columns
- Explain primary keys and foreign keys
- Understand what SQL is and why it is used
- Create and manage databases and tables
- Perform CRUD operations
- Filter and organize data using clauses
- Use operators to compare and retrieve information
- Use SQL functions to manipulate and analyze data
- Understand how SQL relates to cybersecurity and SQL Injection attacks

---

# Prerequisites

This room is beginner-friendly and does not require previous SQL knowledge.

Recommended knowledge:

- Basic computer literacy
- Understanding of files and folders
- Basic Linux command-line familiarity
- Completion of Linux Fundamentals (recommended but not required)

---

# Why Databases Matter

Before learning SQL, it is important to understand why databases exist in the first place.

Imagine running a small online bookstore.

You need to store:

- Customer accounts
- Orders
- Books
- Reviews
- Payment information

Initially, this data could be stored in text files or spreadsheets.

As the business grows:

- More customers join
- More products are added
- More transactions occur

Managing this information manually becomes extremely difficult.

Problems quickly appear:

- Duplicate records
- Slow searches
- Data inconsistency
- Difficult updates
- Poor scalability

Databases solve these problems by organizing information in a structured and efficient way.

---

# Real-World Database Usage

Databases are everywhere.

## Social Media

Platforms such as:

- Facebook
- Instagram
- TikTok
- LinkedIn

store:

- User profiles
- Posts
- Comments
- Likes
- Followers
- Messages

inside databases.

---

## Streaming Services

Platforms such as:

- Netflix
- Disney+
- Spotify

store:

- User accounts
- Watch history
- Recommendations
- Subscription details

inside databases.

---

## E-Commerce

Platforms such as:

- Amazon
- Shopee
- Tokopedia

store:

- Products
- Orders
- Customer information
- Shipping details

inside databases.

---

## Cybersecurity Tools

Many security technologies rely heavily on databases:

### SIEM Platforms

Examples:

- Splunk
- Elastic
- QRadar

Store:

- Logs
- Alerts
- Events
- Detection data

---

### Active Directory

Stores:

- Users
- Groups
- Computers
- Permissions

---

### Vulnerability Management

Stores:

- Scan results
- Vulnerabilities
- Asset information

---

Without databases, modern computing would not function.

---

# What is a Database?

A database is:

> An organized collection of structured information that can be stored, accessed, updated, and analyzed efficiently.

Think of a database as a digital filing cabinet.

Instead of storing paper documents, it stores information electronically.

Example:

```text
Database
│
├── Users
├── Products
├── Orders
├── Logs
└── Reviews
```

Each section stores a different type of information.

---

# Core Database Characteristics

Good databases provide:

## Organization

Data is stored systematically.

---

## Accessibility

Information can be retrieved quickly.

---

## Consistency

Data follows predefined rules.

---

## Scalability

Can handle increasing amounts of information.

---

## Security

Can restrict who accesses or modifies data.

---

# Types of Databases

There are many database types, but two major categories dominate modern computing:

1. Relational Databases (SQL)
2. Non-Relational Databases (NoSQL)

---

# Relational Databases

Relational databases store data in structured tables.

Example:

| id | username | email |
|----|----------|--------|
| 1 | admin | admin@example.com |
| 2 | alice | alice@example.com |

Every record follows the same structure.

---

## Key Characteristics

- Structured data
- Rows and columns
- Strong consistency
- Relationships between tables
- SQL-based querying

---

## Common Relational Databases

Examples include:

- MySQL
- MariaDB
- PostgreSQL
- Microsoft SQL Server
- Oracle Database

---

## When Relational Databases Are Used

Relational databases are commonly used when:

- Accuracy is critical
- Relationships exist between data
- Data structure rarely changes

Examples:

- Banking systems
- E-commerce platforms
- Inventory systems
- Enterprise applications

---

# Non-Relational Databases (NoSQL)

NoSQL databases store data in more flexible formats.

Instead of tables, they may use:

- Documents
- Key-value pairs
- Graphs
- Collections

Example:

```json
{
  "name": "Thomas Anderson",
  "occupation": "The One",
  "skills": [
    "Hacking",
    "Kung Fu"
  ]
}
```

Notice how data does not need to follow a strict table structure.

---

# Key Characteristics

- Flexible schemas
- Easier horizontal scaling
- Handles unstructured data well
- Better for rapidly changing datasets

---

## Common NoSQL Databases

Examples:

- MongoDB
- Redis
- Cassandra
- CouchDB

---

## When NoSQL Is Used

Examples include:

- Social media platforms
- IoT systems
- Big data applications
- Real-time analytics

---

# SQL vs NoSQL Comparison

| Feature | SQL | NoSQL |
|----------|----------|----------|
| Structure | Fixed | Flexible |
| Tables | Yes | Usually No |
| Relationships | Strong | Limited |
| Consistency | High | Variable |
| Scalability | Vertical | Horizontal |
| Query Language | SQL | Varies |

---

# Cybersecurity Perspective

Understanding the difference matters because attackers target both types.

Examples:

### SQL Databases

Common attack:

```text
SQL Injection
```

---

### NoSQL Databases

Common attack:

```text
NoSQL Injection
```

---

Different database technologies require different attack and defense strategies.

---

# Relational Database Structure

Relational databases are built using three fundamental components:

```text
Database
│
└── Tables
     │
     ├── Rows
     └── Columns
```

Understanding these three concepts is critical before learning SQL.

---

# Tables

A table stores a collection of related information.

Example:

```text
Books
```

A bookstore may have a table called:

```text
Books
```

containing information about all books in inventory.

---

# Columns

Columns define the type of information stored.

Example:

| book_id | book_name | publication_date |
|----------|----------|----------|

Each column represents an attribute.

---

Example:

```text
book_id
```

Stores identifiers.

```text
book_name
```

Stores titles.

```text
publication_date
```

Stores dates.

---

# Rows

Rows represent individual records.

Example:

| book_id | book_name | publication_date |
|----------|----------|----------|
| 1 | Android Security Internals | 2014-10-14 |

This row represents one book.

Every additional book creates another row.

---

# Understanding Data Types

Each column stores a specific type of data.

Common types include:

## Integer (INT)

Whole numbers.

Examples:

```text
1
25
500
```

---

## String (VARCHAR)

Text values.

Examples:

```text
Devdan
TryHackMe
CyberSecurity
```

---

## Date

Stores dates.

Example:

```text
2026-06-23
```

---

## Float / Decimal

Numbers with decimals.

Examples:

```text
3.14
99.99
```

---

# Why Data Types Matter

Data types help maintain data integrity.

Example:

If a column expects an integer:

```text
Age = 25
```

works.

However:

```text
Age = CyberSecurity
```

should be rejected.

This prevents invalid data from entering the database.

---

# Key Takeaways

In this section we learned:

- What databases are
- Why databases exist
- Real-world database use cases
- Relational databases
- Non-relational databases
- Tables
- Rows
- Columns
- Data types
- Why structured data matters

These concepts form the foundation for everything that follows in SQL Fundamentals.

In the next section, we will explore how relational databases uniquely identify records using **Primary Keys** and how multiple tables are connected using **Foreign Keys** before moving into SQL itself.