# CyberChef: The Basics

## Executive Summary

CyberChef is a powerful web-based data transformation tool developed by GCHQ that simplifies a wide range of cybersecurity tasks through an intuitive graphical interface. Instead of relying on multiple command-line utilities or writing custom scripts, analysts can combine various operations into reusable recipes to encode, decode, encrypt, decrypt, extract, and transform data with ease.

In this room, I learned the fundamentals of CyberChef, including its interface, workflow, and the concept of recipes. I explored commonly used operations such as Base64 encoding/decoding, URL encoding/decoding, UNIX timestamp conversion, Base85 decoding, and data extraction techniques for IP addresses, email addresses, and URLs. Through practical exercises, I gained hands-on experience transforming different types of data and understanding how CyberChef streamlines security investigations.

This room also introduced an important analytical workflow: defining an objective, selecting appropriate operations, evaluating the output, and refining the process when necessary. This methodology mirrors the investigative approach used by security professionals during incident response, digital forensics, malware analysis, and penetration testing.

---

## Learning Objectives

By completing this room, I was able to:

- Understand what CyberChef is and why it is widely used in cybersecurity.
- Navigate CyberChef's interface efficiently.
- Understand the purpose of Operations, Recipes, Input, and Output panes.
- Learn how CyberChef recipes process data sequentially.
- Perform common data transformations using built-in operations.
- Extract useful information such as IP addresses, email addresses, and URLs from raw data.
- Encode and decode data using Base64 and Base85.
- Encode and decode URLs using percent encoding.
- Convert between UNIX timestamps and human-readable dates.
- Apply CyberChef to practical cybersecurity scenarios.

---

## Prerequisites

Although not mandatory, the following TryHackMe rooms provide useful background knowledge before completing this room:

- Hashing Basics
- Cryptography Basics

Recommended foundational knowledge includes:

- Basic understanding of ASCII and binary representation
- Difference between encoding, hashing, and encryption
- Basic networking concepts (IP addresses and URLs)
- Basic familiarity with web technologies

---

# Room Overview

CyberChef is often described as the **"Swiss Army Knife for Cybersecurity Data."** It provides hundreds of built-in operations that allow analysts to manipulate, decode, encode, parse, extract, compress, decompress, encrypt, and decrypt data without writing code.

Unlike traditional command-line utilities, CyberChef provides a visual workflow where operations can be chained together into reusable recipes. Each operation processes the output of the previous one, making even complex transformations easy to build and understand.

Throughout this room, I explored:

- Accessing CyberChef online and offline
- Understanding the four main interface components
- Creating and modifying recipes
- Working with common data transformation operations
- Extracting useful artifacts from raw text
- Solving practical cybersecurity exercises using CyberChef

---

# Why CyberChef Matters

CyberChef has become one of the most widely used utilities across multiple cybersecurity disciplines because it significantly reduces the time required to analyze and transform data.

Instead of switching between numerous command-line tools such as:

- `base64`
- `xxd`
- `openssl`
- `gzip`
- `hexdump`
- Python scripts

CyberChef combines hundreds of operations into a single interface that works directly inside a web browser or as an offline application.

Its flexibility makes it valuable for professionals working in:

- Security Operations Center (SOC)
- Digital Forensics & Incident Response (DFIR)
- Penetration Testing
- Malware Analysis
- Threat Hunting
- Capture The Flag (CTF)
- Security Research

Common real-world use cases include:

- Decoding Base64 payloads
- Parsing encoded URLs
- Converting UNIX timestamps
- Extracting Indicators of Compromise (IoCs)
- Decoding phishing artifacts
- Analyzing malware strings
- Building repeatable transformation workflows using recipes

By mastering CyberChef, security practitioners can analyze suspicious data much faster while maintaining a repeatable and well-documented investigation process.

# Concepts Covered

This room introduced the fundamental concepts behind CyberChef and demonstrated how it simplifies data transformation tasks commonly encountered in cybersecurity. Rather than focusing on programming or command-line utilities, CyberChef provides an intuitive visual interface where multiple operations can be chained together into reusable recipes.

The following concepts were covered throughout this room:

- CyberChef Overview
- Data Transformation
- Recipes
- Operations
- Data Encoding
- Data Decoding
- Base64 Encoding
- Base85 Encoding
- URL Encoding & Decoding
- UNIX Timestamp Conversion
- Data Extraction
- Workflow-Oriented Data Processing

---

# Terminology

| Term | Definition |
|------|------------|
| CyberChef | A web-based data transformation tool developed by GCHQ that provides hundreds of operations for encoding, decoding, encryption, extraction, parsing, and analysis. |
| Operation | A single action performed on data, such as Base64 decoding or URL encoding. |
| Recipe | A sequence of operations executed from top to bottom. |
| Input | The raw data that will be processed. |
| Output | The processed result after all operations have been executed. |
| Encoding | Converting data into another representation without hiding its contents. |
| Decoding | Reversing an encoding process to restore the original data. |
| Base64 | A binary-to-text encoding scheme using 64 printable ASCII characters. |
| Base85 | A more compact binary-to-text encoding scheme than Base64. |
| URL Encoding | Converting reserved URL characters into percent-encoded values. |
| UNIX Timestamp | The number of seconds elapsed since January 1, 1970 UTC (Unix Epoch). |
| Extractor | An operation that searches input data for specific artifacts such as IP addresses, URLs, or email addresses. |

---

# What is CyberChef?

CyberChef is an all-in-one data transformation platform designed to simplify common cybersecurity tasks. Instead of relying on multiple standalone tools, CyberChef combines hundreds of operations into a single application with a graphical interface.

It is commonly described as the **Swiss Army Knife of Cybersecurity** because it provides numerous utilities for working with digital data.

Some of its capabilities include:

- Encoding and decoding data
- Encryption and decryption
- Compression and decompression
- Parsing structured data
- Data extraction
- Format conversion
- Hash generation
- Binary manipulation

CyberChef is available both as an online web application and as an offline local version, allowing analysts to work securely even without an Internet connection.

---

# Why CyberChef Exists

Security professionals frequently encounter data in many different formats.

Examples include:

- Base64 encoded PowerShell commands
- URL encoded phishing links
- Hexadecimal payloads
- Binary data
- JWT tokens
- Compressed malware strings
- UNIX timestamps

Without CyberChef, analysts often need multiple tools such as:

- base64
- openssl
- xxd
- hexdump
- Python scripts
- Bash commands

CyberChef consolidates these tasks into one interface, significantly improving investigation speed and reducing workflow complexity.

---

# Understanding Recipes

The most important concept in CyberChef is the **Recipe**.

A recipe is simply a sequence of operations that are executed in order.

```
Input
    │
    ▼
Operation 1
    │
    ▼
Operation 2
    │
    ▼
Operation 3
    │
    ▼
Output
```

Each operation receives the output of the previous operation.

For example:

```
Encoded Data
      │
      ▼
From Base64
      │
      ▼
Gunzip
      │
      ▼
From JSON
      │
      ▼
Readable Data
```

This workflow is conceptually similar to Linux pipelines.

Linux example:

```bash
cat payload.txt | base64 -d | gunzip
```

CyberChef performs the same sequence visually using recipes.

---

# Understanding the CyberChef Interface

CyberChef's interface is divided into four primary areas.

## 1. Operations

The Operations panel contains every transformation supported by CyberChef.

Examples include:

- From Base64
- To Base64
- From Hex
- AES Encrypt
- AES Decrypt
- ROT13
- URL Encode
- URL Decode
- Hash
- Extract IP Addresses

Operations can be searched and dragged directly into the Recipe panel.

---

## 2. Recipe

The Recipe panel is where operations are organized.

Each operation can also be configured using its own parameters.

Example recipe:

```
From Base64
↓

URL Decode
↓

Extract URLs
```

Recipes can be:

- Saved
- Loaded
- Cleared
- Executed manually using **BAKE!**
- Executed automatically using **Auto Bake**

---

## 3. Input

The Input panel contains the data to be processed.

CyberChef supports multiple input methods:

- Typing
- Copy & Paste
- Drag & Drop
- File Upload
- Folder Upload

Multiple input tabs may also be created for comparing different datasets.

---

## 4. Output

The Output panel displays the result after the recipe has been executed.

Useful features include:

- Copy Output
- Save Output
- Replace Input with Output
- Maximize Output

---

# Common Operation Categories

This room introduced several operation categories that are frequently used during cybersecurity investigations.

## Extractors

Extractors search raw text and automatically identify useful artifacts.

Examples:

- Extract IP Addresses
- Extract Email Addresses
- Extract URLs

Example:

```
Input:

User:
admin@test.com

Server:
192.168.1.20

Website:
https://tryhackme.com
```

Output:

```
admin@test.com
192.168.1.20
https://tryhackme.com
```

These operations are especially useful when processing large log files or forensic artifacts.

---

## Date & Time Operations

CyberChef can convert timestamps between machine-readable and human-readable formats.

### To UNIX Timestamp

Converts:

```
Sun 1 September 2024 00:40:58 UTC
```

into:

```
1725151258
```

### From UNIX Timestamp

Converts:

```
1725151258
```

back into:

```
Sun 1 September 2024 00:40:58 UTC
```

This functionality is widely used when analyzing security logs.

---

## Data Format Operations

CyberChef supports numerous binary-to-text encoding formats.

Examples include:

- Base64
- Base58
- Base62
- Base85
- Hexadecimal
- Binary
- Decimal

These operations allow analysts to convert data into formats that are easier to transmit, store, or inspect.

---

# Understanding Base64

Base64 is one of the most common encoding formats used in cybersecurity.

Unlike encryption, Base64 does **not** protect data.

Its purpose is simply to represent binary data using printable ASCII characters.

## How Base64 Works

The encoding process follows four basic steps:

1. Convert characters into ASCII values.
2. Convert ASCII values into binary.
3. Split binary into 6-bit groups.
4. Map each 6-bit value to the Base64 index table.

Example:

```
THM

↓

ASCII

↓

Binary

↓

6-bit Groups

↓

Base64

↓

VEhN
```

Because Base64 uses 64 possible symbols:

```
2⁶ = 64
```

each output character represents exactly six bits.

Padding (`=`) is added whenever necessary so that the output length remains a multiple of four characters.

---

# Understanding URL Encoding

Certain characters have special meanings inside URLs.

Examples include:

| Character | Encoded Value |
|-----------|---------------|
| : | %3A |
| / | %2F |
| . | %2E |
| = | %3D |
| ? | %3F |
| # | %23 |

URL Encoding replaces reserved characters with percent-encoded hexadecimal values.

Example:

```
https://tryhackme.com
```

becomes

```
https%3A%2F%2Ftryhackme%2Ecom
```

The **URL Decode** operation restores the original URL.

---

# Understanding UNIX Timestamp

A UNIX Timestamp stores time as the number of seconds since:

```
1 January 1970
00:00:00 UTC
```

also known as the **Unix Epoch**.

Instead of storing a date like:

```
2024-09-01 00:40:58
```

systems store:

```
1725151258
```

This numeric representation is:

- Smaller
- Faster to process
- Independent of regional date formats

Many operating systems, SIEM platforms, cloud services, and security appliances use UNIX timestamps extensively.

---

# CyberChef Workflow

A structured workflow helps analysts use CyberChef efficiently.

```
Define Objective
        │
        ▼
Provide Input
        │
        ▼
Choose Operations
        │
        ▼
Review Output
        │
   Correct?
   ├── Yes → Finished
   └── No → Refine Recipe
```

This iterative process mirrors the methodology used during real-world security investigations.

---

# Real-World Applications

CyberChef is widely used across multiple cybersecurity disciplines.

## Security Operations Center (SOC)

- Decode malicious URLs
- Analyze suspicious logs
- Extract Indicators of Compromise (IoCs)
- Convert timestamps

---

## Digital Forensics & Incident Response (DFIR)

- Decode forensic artifacts
- Analyze encoded evidence
- Extract useful indicators
- Transform binary data

---

## Penetration Testing

- Decode payloads
- Encode PowerShell commands
- Modify web payloads
- Analyze application responses

---

## Malware Analysis

- Decode embedded strings
- Inspect obfuscated payloads
- Convert binary representations
- Analyze encoded configuration data

CyberChef greatly accelerates these tasks by eliminating repetitive scripting and providing a consistent, repeatable workflow for transforming and analyzing data.

# Task Walkthrough

This room consists of several tasks that gradually introduce CyberChef, its interface, workflow, and commonly used operations. Rather than focusing on complex cybersecurity concepts, the room emphasizes developing familiarity with CyberChef's workflow through practical exercises.

---

# Task 1 — Introduction

## Objective

Understand what CyberChef is and why it is widely used in cybersecurity.

## Discussion

CyberChef is a browser-based data transformation tool developed by **GCHQ**. It provides hundreds of built-in operations that allow users to manipulate data without writing scripts or using multiple command-line tools.

Instead of switching between utilities such as:

- base64
- openssl
- xxd
- hexdump
- Python

CyberChef combines all of these capabilities into a single interface.

Some common tasks include:

- Encoding and decoding
- Encryption and decryption
- Data extraction
- Hash generation
- Compression
- Binary conversion
- Timestamp conversion

---

## Key Takeaways

- CyberChef is designed to simplify repetitive data transformation tasks.
- It is heavily used in SOC, DFIR, Malware Analysis, and Penetration Testing.
- Recipes allow multiple operations to be chained together.

---

# Task 2 — Accessing CyberChef

## Objective

Learn how to access CyberChef.

## Online Version

The easiest way to use CyberChef is through a web browser.

Advantages:

- No installation required
- Always updated
- Accessible from anywhere

---

## Offline Version

CyberChef can also be downloaded and executed locally.

Advantages:

- Works without Internet
- Better for sensitive investigations
- Suitable for offline labs
- Keeps confidential data on the analyst's machine

---

## Best Practice

When handling:

- malware samples
- customer information
- incident response evidence
- internal logs

the offline version is generally preferred.

---

# Task 3 — Navigating the Interface

## Objective

Understand the four major interface components.

CyberChef consists of four primary areas.

---

## Operations

The Operations panel contains every transformation supported by CyberChef.

Examples:

- From Base64
- To Base64
- URL Decode
- ROT13
- Extract URLs
- AES Encrypt

Operations can be searched and dragged into the Recipe panel.

---

## Recipe

The Recipe panel is where operations are organized.

Example:

```
From Base64

↓

URL Decode

↓

Extract URLs
```

Recipes may be:

- Saved
- Loaded
- Cleared
- Executed using **BAKE!**
- Automatically executed using **Auto Bake**

---

## Input

The Input panel accepts:

- Plain text
- Files
- Folders

Data may be entered through:

- Typing
- Copy & Paste
- Drag & Drop
- Upload

---

## Output

The Output panel displays the processed data.

Useful features include:

- Copy Output
- Save Output
- Replace Input
- Maximize Output

---

## Key Takeaways

CyberChef follows a straightforward workflow:

```
Operations

↓

Recipe

↓

Input

↓

Output
```

---

# Task 4 — Before Anything Else

## Objective

Understand the recommended workflow before processing data.

Rather than randomly trying operations, CyberChef encourages analysts to follow a structured process.

```
Define Objective

↓

Provide Input

↓

Choose Operations

↓

Review Output
```

If the output is incorrect, the process is repeated until the desired result is achieved.

---

## Example

Suppose an analyst discovers:

```
SGVsbG8=
```

The workflow becomes:

**Objective**

Determine whether the string contains readable information.

↓

**Input**

Paste the string into CyberChef.

↓

**Operation**

Apply **From Base64**.

↓

**Output**

```
Hello
```

Objective achieved.

---

## Key Takeaways

Successful CyberChef usage depends on understanding the data rather than guessing operations.

---

# Task 5 — Practice, Practice, Practice

## Objective

Become familiar with several common operation categories.

The room introduced three categories frequently used during investigations.

---

## Extractors

Used to identify useful artifacts.

Examples:

- Extract IP Addresses
- Extract Email Addresses
- Extract URLs

These operations are especially useful when processing:

- Logs
- Email dumps
- Text files
- Forensic artifacts

---

## Date & Time

Operations introduced:

- From UNIX Timestamp
- To UNIX Timestamp

These allow timestamps to be converted between machine-readable and human-readable formats.

---

## Data Format

Operations introduced:

- From Base64
- To Base64
- From Base85
- From Base58
- To Base62
- URL Decode

The room also explained how Base64 works internally using ASCII values, binary representation, and the Base64 lookup table.

---

# Task 5 Questions

## Question 1

**Question**

What is the hidden email address?

**Operation Used**

```
Extract Email Addresses
```

**Answer**

```
hidden@hotmail.com
```

---

## Question 2

**Question**

What is the hidden IP address that ends in ".232"?

**Operation Used**

```
Extract IP Addresses
```

**Answer**

```
102.20.11.232
```

---

## Question 3

**Question**

Which domain address starts with the letter "T"?

**Operation Used**

```
Extract URLs
```

**Answer**

```
TryHackMe.com
```

---

## Question 4

**Question**

What is the binary value of decimal number 78?

**Method**

Using the ASCII table provided in the room.

**Answer**

```
01001110
```

---

## Question 5

**Question**

What is the URL encoded value of:

```
https://tryhackme.com/r/careers?
```

**Operation Used**

```
URL Encode
```

**Answer**

```
https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Fcareers%3F
```

---

# Task 6 — Your First Official Cook

## Objective

Apply the operations learned in previous tasks to solve practical exercises.

This task reinforces the CyberChef workflow:

```
Input

↓

Recipe

↓

Output
```

using several different operation categories.

---

## Question 1

**Question**

Using the downloaded file, which IP starts and ends with "10"?

**Operation Used**

```
Extract IP Addresses
```

**Answer**

```
10.10.2.10
```

---

## Question 2

**Question**

What is the Base64 encoded value of:

```
Nice Room!
```

**Operation Used**

```
To Base64
```

**Answer**

```
TmljZSBSb29tIQ==
```

---

## Question 3

**Question**

What is the URL decoded value of:

```
https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Froom%2Fcyberchefbasics
```

**Operation Used**

```
URL Decode
```

**Answer**

```
https://tryhackme.com/r/room/cyberchefbasics
```

---

## Question 4

**Question**

What is the datetime representation of UNIX timestamp:

```
1725151258
```

**Operation Used**

```
From UNIX Timestamp
```

**Answer**

```
Sun 1 September 2024 00:40:58 UTC
```

---

## Question 5

**Question**

What is the Base85 decoded string of:

```
<+oue+DGm>Ap%u7
```

**Operation Used**

```
From Base85
```

**Answer**

```
This is fun!
```

---

# Room Summary

Throughout this room, I learned how to navigate CyberChef and apply its most common operations to solve practical cybersecurity problems.

The exercises demonstrated how CyberChef simplifies data transformation tasks by combining multiple operations into reusable recipes, making it an essential utility for analysts working in:

- Security Operations Center (SOC)
- Digital Forensics & Incident Response (DFIR)
- Penetration Testing
- Malware Analysis
- Threat Hunting

Although this room focuses on beginner-level operations, the workflow established here forms the foundation for using CyberChef in more advanced security investigations.

# Operations Used

Unlike Linux-based rooms where commands are executed through a terminal, CyberChef relies on **operations**. An operation is a built-in function that performs a specific transformation on the input data. Multiple operations can be chained together into a recipe to perform more complex data processing.

The following operations were used throughout this room.

---

# Extract IP Addresses

## Category

Extractors

## Purpose

Searches the input and extracts all valid IPv4 and IPv6 addresses.

## Syntax

```
Extract IP Addresses
```

## Example Input

```
Client: 192.168.1.10
Server: 10.10.2.10
Gateway: 172.16.0.1
```

## Example Output

```
192.168.1.10
10.10.2.10
172.16.0.1
```

## Output Interpretation

CyberChef scans the entire input and returns every valid IP address it detects.

## Common Use Cases

- Log analysis
- Threat hunting
- Malware configuration analysis
- Network investigations
- IOC extraction

## Alternatives

- grep (Linux)
- Regular Expressions
- Python (`re` module)

---

# Extract Email Addresses

## Category

Extractors

## Purpose

Searches input data and extracts every valid email address.

## Syntax

```
Extract Email Addresses
```

## Example Input

```
Admin: admin@example.com
Support: help@tryhackme.com
```

## Example Output

```
admin@example.com
help@tryhackme.com
```

## Output Interpretation

Returns every string matching the standard email address format.

## Common Use Cases

- Phishing investigations
- OSINT
- Database analysis
- IOC extraction

## Alternatives

- grep
- Regex
- Python

---

# Extract URLs

## Category

Extractors

## Purpose

Extracts all valid URLs from the provided input.

## Syntax

```
Extract URLs
```

## Example Input

```
Visit:
https://tryhackme.com
https://github.com
```

## Example Output

```
https://tryhackme.com
https://github.com
```

## Output Interpretation

Returns every detected URL including its protocol.

## Common Use Cases

- Phishing analysis
- Malware analysis
- Log investigations
- IOC extraction

## Alternatives

- Regular Expressions
- Python
- grep

---

# To Base64

## Category

Data Format

## Purpose

Encodes plaintext into Base64 format.

## Syntax

```
To Base64
```

## Example Input

```
Hello World
```

## Example Output

```
SGVsbG8gV29ybGQ=
```

## Output Interpretation

The original text is converted into a Base64 encoded representation.

## Common Use Cases

- Encoding payloads
- Email MIME content
- API communication
- Web applications

## Alternatives

Linux

```bash
echo "Hello World" | base64
```

Python

```python
base64.b64encode()
```

---

# From Base64

## Category

Data Format

## Purpose

Decodes Base64 encoded text back into its original representation.

## Syntax

```
From Base64
```

## Example Input

```
SGVsbG8gV29ybGQ=
```

## Example Output

```
Hello World
```

## Output Interpretation

Restores the original plaintext from its Base64 representation.

## Common Use Cases

- PowerShell payload analysis
- Malware investigation
- JWT inspection
- CTF challenges

## Alternatives

Linux

```bash
echo "SGVsbG8=" | base64 -d
```

Python

```python
base64.b64decode()
```

---

# URL Encode

## Category

Data Format

## Purpose

Converts reserved URL characters into percent-encoded values.

## Syntax

```
URL Encode
```

## Example Input

```
https://tryhackme.com
```

## Example Output

```
https%3A%2F%2Ftryhackme%2Ecom
```

## Output Interpretation

Reserved characters such as `:`, `/`, and `.` are encoded to ensure safe transmission within URLs.

## Common Use Cases

- Web development
- API requests
- URL parameter construction
- Web security testing

## Alternatives

Python

```python
urllib.parse.quote()
```

JavaScript

```javascript
encodeURIComponent()
```

---

# URL Decode

## Category

Data Format

## Purpose

Converts percent-encoded characters back into readable text.

## Syntax

```
URL Decode
```

## Example Input

```
https%3A%2F%2Ftryhackme%2Ecom
```

## Example Output

```
https://tryhackme.com
```

## Output Interpretation

Returns the original URL before encoding.

## Common Use Cases

- Phishing analysis
- Web application testing
- HTTP request analysis
- Log analysis

## Alternatives

Python

```python
urllib.parse.unquote()
```

JavaScript

```javascript
decodeURIComponent()
```

---

# From UNIX Timestamp

## Category

Date / Time

## Purpose

Converts UNIX timestamps into human-readable date and time.

## Syntax

```
From UNIX Timestamp
```

## Example Input

```
1725151258
```

## Example Output

```
Sun 1 September 2024 00:40:58 UTC
```

## Output Interpretation

Displays the corresponding UTC date and time.

## Common Use Cases

- SIEM investigations
- Linux log analysis
- Windows Event correlation
- Cloud logging

## Alternatives

Linux

```bash
date -d @1725151258
```

Python

```python
datetime.fromtimestamp()
```

---

# To UNIX Timestamp

## Category

Date / Time

## Purpose

Converts human-readable dates into UNIX timestamps.

## Syntax

```
To UNIX Timestamp
```

## Example Input

```
Sun 1 September 2024 00:40:58 UTC
```

## Example Output

```
1725151258
```

## Output Interpretation

Returns the number of seconds elapsed since the Unix Epoch.

## Common Use Cases

- Timeline analysis
- Event correlation
- Log normalization

## Alternatives

Linux

```bash
date +%s
```

Python

```python
datetime.timestamp()
```

---

# From Base85

## Category

Data Format

## Purpose

Decodes Base85 encoded data into plaintext.

## Syntax

```
From Base85
```

## Example Input

```
<+oue+DGm>Ap%u7
```

## Example Output

```
This is fun!
```

## Output Interpretation

The encoded Base85 string is converted back into readable text.

## Common Use Cases

- Malware analysis
- CTF challenges
- Reverse engineering
- Encoded configuration analysis

## Alternatives

Python

```python
base64.a85decode()
```

---

# Workflow Used Throughout the Room

Every practical exercise in this room followed the same workflow.

```
Define Objective
        │
        ▼
Provide Input
        │
        ▼
Choose Operation(s)
        │
        ▼
Execute Recipe
        │
        ▼
Review Output
        │
     Success?
     ├── Yes → Finished
     └── No → Modify Recipe
```

This iterative workflow mirrors how security professionals approach data analysis during real-world investigations. Rather than randomly applying operations, analysts first identify the type of data they are dealing with, choose the most appropriate transformations, verify the output, and refine the recipe when necessary.

# Troubleshooting

Although CyberChef is designed to be intuitive, several common mistakes can prevent operations from producing the expected results. Understanding these issues helps analysts troubleshoot data transformation problems more efficiently.

---

## Problem 1 — Incorrect Operation Selected

### Issue

The output appears as unreadable characters or unexpected symbols.

### Example

Attempting to decode a hexadecimal string using **From Base64**.

```
48656c6c6f
```

↓

```
From Base64
```

↓

```
Invalid or unreadable output
```

### Root Cause

The selected operation does not match the actual format of the input data.

### Solution

Identify the data format before choosing an operation.

Possible indicators include:

| Data Pattern | Likely Format |
|--------------|---------------|
| Ends with `=` | Base64 |
| Only 0-9 and A-F | Hexadecimal |
| Contains `%2F`, `%3A` | URL Encoded |
| Consists of dots and dashes | Morse Code |

---

## Problem 2 — Operations Executed in the Wrong Order

### Issue

The recipe completes successfully but the output is still unreadable.

### Root Cause

CyberChef executes recipes from **top to bottom**. If the operations are placed in the wrong order, the final output will also be incorrect.

### Incorrect Recipe

```
From Hex

↓

From Base64
```

### Correct Recipe

```
From Base64

↓

From Hex
```

The correct order depends entirely on how the original data was encoded.

---

## Problem 3 — Confusing Encoding with Encryption

### Issue

Many beginners assume Base64 protects or hides data.

### Reality

Base64 is **not encryption**.

Anyone can recover the original data simply by applying:

```
From Base64
```

### Lesson Learned

Always distinguish between:

- Encoding
- Encryption
- Hashing

These technologies solve completely different problems.

---

## Problem 4 — Forgetting to Use the Correct Variant

Several operations contain multiple configuration options.

Examples include:

- Base64
- Base85
- URL Encoding
- AES
- XOR

Using incorrect parameters may produce unexpected results.

Always verify:

- Character encoding
- Keys
- Padding
- Encoding alphabet
- Delimiters

---

## Problem 5 — Misinterpreting UNIX Timestamps

### Issue

The converted date appears incorrect.

### Root Cause

Time zone differences.

Many security logs use:

```
UTC
```

instead of local system time.

### Solution

Always verify:

- UTC
- Local Time
- SIEM timezone
- Operating system timezone

before correlating security events.

---

# Common Mistakes

Some mistakes frequently made by beginners include:

- Applying random operations until something "looks correct."
- Forgetting that recipes execute sequentially.
- Using **To Base64** instead of **From Base64**.
- Assuming every random-looking string is encrypted.
- Ignoring the structure or characteristics of the input data.
- Forgetting to inspect operation parameters.

Developing a habit of identifying the data format first significantly improves efficiency.

---

# Security Implications

Although CyberChef appears to be a simple utility, improper usage may introduce security risks.

## Sensitive Data Exposure

The online version should not be used for highly confidential information such as:

- Password databases
- Private keys
- Customer information
- Internal company documents
- Sensitive forensic evidence

Whenever possible, use the offline version for investigations involving confidential data.

---

## Data Integrity

Incorrect transformations may alter the original evidence.

During forensic investigations, analysts should preserve the original data and perform transformations on copies rather than modifying the source directly.

---

## Recipe Validation

Always validate that each operation produces the intended output before adding additional transformations.

Small mistakes early in the recipe often propagate through every subsequent operation.

---

# Pentester Notes

CyberChef is an excellent companion tool during penetration testing.

## Reconnaissance

Useful for:

- Decoding discovered URLs
- Extracting domains
- Identifying IP addresses
- Inspecting encoded application responses

---

## Enumeration

CyberChef simplifies the inspection of:

- API responses
- JWT tokens
- HTTP parameters
- Base64 encoded data
- Configuration files

---

## Exploitation

Frequently used to:

- Encode payloads
- Decode PowerShell commands
- Modify encoded parameters
- Analyze server responses
- Build custom payload transformations

---

## Privilege Escalation

Useful for inspecting:

- Scheduled task payloads
- Encoded PowerShell scripts
- Obfuscated configuration files

---

## Post Exploitation

CyberChef assists in analyzing:

- Browser artifacts
- Log files
- Encoded credentials
- Configuration backups

---

# SOC Analyst Notes

Security Operations Center analysts frequently rely on CyberChef during alert investigations.

Common activities include:

- Decoding phishing URLs
- Extracting Indicators of Compromise (IoCs)
- Converting UNIX timestamps
- Decoding Base64 encoded payloads
- Parsing suspicious strings found in logs

Because CyberChef supports reusable recipes, repetitive investigation workflows become significantly faster.

---

# Digital Forensics Notes

CyberChef is widely used during forensic investigations because it enables rapid transformation of evidence without requiring custom scripts.

Typical forensic tasks include:

- Decoding encoded artifacts
- Extracting IP addresses
- Recovering URLs
- Converting timestamps
- Inspecting malware strings
- Parsing structured data

Recipes can also be saved, allowing investigations to remain consistent and reproducible.

---

# Key Takeaways

This room introduced CyberChef as much more than an encoding and decoding utility.

Some of the most important lessons learned include:

- CyberChef provides hundreds of built-in operations for transforming digital data.
- Recipes allow multiple operations to be chained together into repeatable workflows.
- Identifying the data format before selecting operations is essential.
- Base64 is an encoding mechanism, **not encryption**.
- URL Encoding, Base64, Base85, and UNIX Timestamps are commonly encountered in real-world security investigations.
- Extractor operations greatly simplify the identification of valuable artifacts such as IP addresses, URLs, and email addresses.
- CyberChef significantly reduces the need for multiple standalone utilities and custom scripts.

Most importantly, this room reinforces an investigation mindset:

```
Identify the data

↓

Choose the appropriate operation

↓

Validate the output

↓

Repeat if necessary
```

This structured workflow is applicable across penetration testing, incident response, malware analysis, digital forensics, threat hunting, and Security Operations Center (SOC) investigations.

# Skills Gained

By completing this room, I developed practical skills that are directly applicable to multiple cybersecurity disciplines.

## CyberChef Fundamentals

- Understood the purpose and capabilities of CyberChef.
- Learned how CyberChef simplifies data transformation tasks.
- Distinguished CyberChef from traditional command-line utilities.

---

## Interface Navigation

Learned how to efficiently use CyberChef's four primary interface components:

- Operations
- Recipe
- Input
- Output

Also became familiar with features such as:

- BAKE!
- Auto Bake
- Save Recipe
- Load Recipe
- Multiple Input Tabs

---

## Data Transformation

Gained hands-on experience performing common data transformations, including:

- Base64 Encoding
- Base64 Decoding
- Base85 Decoding
- URL Encoding
- URL Decoding
- UNIX Timestamp Conversion

---

## Data Extraction

Learned how to extract useful artifacts from raw text using CyberChef's Extractor operations.

Examples include:

- IPv4 Addresses
- IPv6 Addresses
- Email Addresses
- URLs

These operations are particularly useful during log analysis and forensic investigations.

---

## Workflow Development

Developed a structured methodology for analyzing unknown data.

```
Identify the Objective
        │
        ▼
Provide Input
        │
        ▼
Select Operations
        │
        ▼
Review Output
        │
        ▼
Refine if Necessary
```

This workflow encourages analytical thinking instead of relying on trial and error.

---

# Concepts Reinforced

This room strengthened several concepts introduced in previous TryHackMe rooms.

- ASCII Representation
- Binary Data
- Data Encoding
- Base64
- URL Encoding
- Networking Concepts
- Web Fundamentals
- UNIX Time
- Indicators of Compromise (IoCs)

It also reinforced the distinction between:

- Encoding
- Encryption
- Hashing

Understanding these differences is essential for avoiding common misconceptions in cybersecurity.

---

# Industry Relevance

CyberChef is one of the most widely adopted utilities in the cybersecurity industry due to its flexibility and ease of use.

It is commonly used by professionals working in:

- Security Operations Center (SOC)
- Incident Response (IR)
- Digital Forensics (DFIR)
- Threat Hunting
- Malware Analysis
- Penetration Testing
- Reverse Engineering
- Capture The Flag (CTF)

Typical real-world tasks include:

- Decoding phishing URLs
- Analyzing encoded PowerShell commands
- Extracting Indicators of Compromise (IoCs)
- Parsing web requests
- Converting timestamps
- Decoding malware configuration strings
- Inspecting encoded application responses

Rather than replacing scripting languages such as Python or PowerShell, CyberChef complements them by providing a fast and visual way to perform common transformations.

---

# Knowledge Gaps Remaining

Although this room provides an excellent introduction, many advanced CyberChef features remain unexplored.

Future areas to study include:

- AES Encryption & Decryption
- RSA Operations
- XOR Encoding
- JWT Analysis
- Regular Expressions
- Compression Algorithms
- Binary Parsing
- JSON & XML Manipulation
- Magic Operations
- Recipe Automation
- Malware String Analysis

Learning these operations will significantly expand CyberChef's usefulness during security assessments and investigations.

---

# Recommended Next Learning Path

After completing this room, the following topics naturally build upon the skills learned here:

### Web Application Security

Understanding how encoded parameters appear within HTTP requests and responses.

Examples:

- URL Parameters
- Cookies
- JWT Tokens
- HTTP Headers

---

### Digital Forensics

Applying CyberChef to:

- Timeline Analysis
- Metadata Analysis
- Evidence Processing
- Artifact Extraction

---

### SOC & Incident Response

Using CyberChef alongside SIEM platforms to:

- Decode malicious URLs
- Analyze encoded payloads
- Extract Indicators of Compromise
- Convert timestamps
- Investigate phishing emails

---

### Malware Analysis

Using CyberChef to inspect:

- Encoded PowerShell commands
- Obfuscated malware strings
- Embedded configuration files
- Encoded shellcode

---

### Capture The Flag (CTF)

CyberChef appears frequently in CTF challenges involving:

- Base Encodings
- XOR
- ROT13
- Hex
- Binary
- Compression
- Multi-layer Encodings

Mastering CyberChef greatly speeds up solving these challenges.

---

# References

## Official Documentation

- GCHQ CyberChef
- CyberChef GitHub Repository

## TryHackMe Rooms

- CyberChef: The Basics
- Hashing Basics
- Cryptography Basics
- Networking Concepts
- Web Application Basics

---

# Key Takeaways

- CyberChef is an all-in-one platform for transforming and analyzing data.
- Recipes allow multiple operations to be chained into repeatable workflows.
- Understanding the structure of the input data is more important than randomly applying operations.
- Base64, URL Encoding, and UNIX Timestamps are encountered frequently during security investigations.
- Extractor operations simplify the identification of important artifacts such as IP addresses, URLs, and email addresses.
- CyberChef is an essential supporting tool for SOC analysts, DFIR practitioners, penetration testers, malware analysts, and CTF players.

Most importantly, this room reinforces a practical mindset:

```
Understand the Data

↓

Choose the Correct Transformation

↓

Verify the Result

↓

Repeat if Necessary
```

This analytical workflow extends far beyond CyberChef itself and forms the foundation of effective problem-solving in cybersecurity.

---

# Skills Checklist

- [x] Understand CyberChef fundamentals
- [x] Navigate the CyberChef interface
- [x] Create and execute recipes
- [x] Extract IP addresses
- [x] Extract email addresses
- [x] Extract URLs
- [x] Encode data using Base64
- [x] Decode Base64 data
- [x] Decode Base85 data
- [x] Encode and decode URLs
- [x] Convert UNIX timestamps
- [x] Apply a structured data analysis workflow
- [x] Understand CyberChef's role in cybersecurity investigations

---

# Tags

`TryHackMe` `CyberChef` `Data Transformation` `Base64` `Base85` `URL Encoding` `URL Decoding` `UNIX Timestamp` `Extractors` `SOC` `DFIR` `Threat Hunting` `Malware Analysis` `Penetration Testing` `CTF` `Cybersecurity`