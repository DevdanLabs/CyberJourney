# Networking Core Protocols - Complete Study Notes & Pentester Writeup

## Room Overview

This room is the third room in the Networking series:

1. Networking Concepts
2. Networking Essentials
3. Networking Core Protocols ✅
4. Networking Secure Protocols

### Prerequisites

Before starting this room, it is recommended to understand:

* OSI Model
* TCP/IP Model
* Ethernet
* IP
* TCP

This room focuses on the most common application-layer protocols used daily on the Internet.

---

# Learning Objectives

After completing this room, I learned:

* WHOIS
* DNS
* HTTP
* HTTPS
* FTP
* SMTP
* POP3
* IMAP

More importantly, I learned how these protocols work behind graphical applications such as browsers and email clients.

---

# Application Layer Protocols

Most users interact with application layer protocols every day without realizing it.

Examples:

| Activity           | Protocol     |
| ------------------ | ------------ |
| Browsing websites  | HTTP / HTTPS |
| Looking up domains | DNS          |
| Downloading files  | FTP          |
| Sending email      | SMTP         |
| Receiving email    | POP3 / IMAP  |

These protocols operate on top of TCP/IP.

---

# DNS (Domain Name System)

## Purpose

DNS translates human-readable domain names into IP addresses.

Example:

```text
google.com
↓
142.250.x.x
```

Without DNS, users would need to memorize IP addresses for every website.

---

## DNS Records

### A Record

Maps:

```text
Hostname → IPv4 Address
```

Example:

```text
example.com → 172.17.2.172
```

---

### AAAA Record

Maps:

```text
Hostname → IPv6 Address
```

Example:

```text
example.com
↓
2606:2800:21f:cb07:6820:80da:af6b:8b2c
```

---

### CNAME Record

Maps:

```text
Hostname → Another Hostname
```

Example:

```text
www.example.com
↓
example.com
```

Alias relationship.

---

### MX Record

Maps:

```text
Domain → Mail Server
```

Example:

```text
example.com
↓
mail.example.com
```

Used when sending email.

---

## DNS Ports

| Protocol     | Port   |
| ------------ | ------ |
| DNS          | UDP 53 |
| DNS Fallback | TCP 53 |

---

## Commands

### nslookup

```bash
nslookup google.com
```

Example Output:

```text
Name: google.com
Address: 142.250.190.14
```

---

### dig

```bash
dig google.com
```

Provides detailed DNS information.

---

## Pentester Relevance

DNS enumeration is often the first step in reconnaissance.

Useful for:

* Discovering IP addresses
* Discovering mail servers
* Subdomain enumeration
* Infrastructure mapping

Typical workflow:

```text
Target Domain
↓
DNS Enumeration
↓
Subdomain Discovery
↓
Port Scanning
```

---

# WHOIS

## Purpose

WHOIS provides information about the owner of a domain.

Example:

```bash
whois example.com
```

---

## Information Available

Typical WHOIS data:

* Registrant Name
* Organization
* Email Address
* Phone Number
* Address
* Registrar
* Registration Date
* Expiration Date

---

## Important Fields

### Creation Date

When the domain was first registered.

Example:

```text
1988-10-12
```

---

### Updated Date

Last modification date.

---

### Expiration Date

When domain ownership expires.

---

### Registrar

Company responsible for domain registration.

Examples:

* GoDaddy
* Namecheap
* CSC Corporate Domains

---

## Common Domain Statuses

### clientTransferProhibited

Prevents transfer to another registrar.

### serverDeleteProhibited

Prevents deletion.

### serverTransferProhibited

Prevents transfer.

### serverUpdateProhibited

Prevents modification.

---

## Pentester Relevance

WHOIS can reveal:

* Contact information
* Email addresses
* Organization details
* Domain age
* Registrar information

Useful for:

* OSINT
* Reconnaissance
* Social engineering preparation

---

# HTTP and HTTPS

## Purpose

HTTP defines communication between web browsers and web servers.

---

## Ports

| Protocol | Port |
| -------- | ---- |
| HTTP     | 80   |
| HTTPS    | 443  |

---

## Difference Between HTTP and HTTPS

### HTTP

Data transmitted in plaintext.

### HTTPS

Data encrypted using TLS/SSL.

---

## Common HTTP Methods

### GET

Retrieve resources.

Example:

```http
GET /index.html HTTP/1.1
```

---

### POST

Submit data.

Example:

```http
POST /login
```

---

### PUT

Create or replace resources.

---

### DELETE

Delete resources.

---

## HTTP Request Structure

Example:

```http
GET / HTTP/1.1
Host: target.com
```

---

## Common Response Codes

### 200 OK

Request successful.

### 301 Moved Permanently

Permanent redirect.

### 302 Found

Temporary redirect.

### 403 Forbidden

Access denied.

### 404 Not Found

Resource missing.

### 500 Internal Server Error

Server error.

---

## Useful Headers

### Server

Example:

```http
Server: nginx/1.18.0 (Ubuntu)
```

Useful for fingerprinting.

---

### Content-Type

Examples:

```http
text/html
application/json
image/png
```

---

### Last-Modified

Indicates when a file was last changed.

---

# Practical Exercise - HTTP via Telnet

Connect:

```bash
telnet 10.48.158.0 80
```

Request:

```http
GET /flag.html HTTP/1.1
Host: 10.48.158.0

```

(Blank line required)

---

## Response

The page contained:

```html
<div class="hidden-text">THM{TELNET-HTTP}</div>
```

The text was hidden using CSS:

```css
color: white;
background-color: white;
```

---

## Answer

```text
THM{TELNET-HTTP}
```

---

## Lesson Learned

Web pages can hide content visually.

Viewing raw HTTP responses can reveal hidden information.

Useful pentesting techniques:

* View Source
* Developer Tools
* curl
* telnet
* netcat

---

# FTP (File Transfer Protocol)

## Purpose

FTP transfers files between systems.

---

## Port

| Protocol | Port   |
| -------- | ------ |
| FTP      | TCP 21 |

---

## Common Commands

### USER

Specify username.

### PASS

Specify password.

### LIST

List files.

### RETR

Download file.

### STOR

Upload file.

### QUIT

Close session.

---

## Client Command Mapping

| FTP Client   | FTP Protocol  |
| ------------ | ------------- |
| ls           | LIST          |
| get file.txt | RETR file.txt |
| put file.txt | STOR file.txt |

---

## Practical Exercise

Connect:

```bash
ftp 10.48.158.0
```

Login:

```text
Username: anonymous
Password: [blank]
```

List files:

```bash
ls
```

Download:

```bash
get flag.txt
```

Read file:

```bash
cat flag.txt
```

---

## Answer

```text
THM{FAST-FTP}
```

---

## Pentester Relevance

Anonymous FTP access is a common finding.

Look for:

* Backups
* Source code
* Configuration files
* Credentials

Example:

```text
backup.zip
database.sql
config.php
```

---

# SMTP (Simple Mail Transfer Protocol)

## Purpose

Send email.

---

## Port

| Protocol | Port   |
| -------- | ------ |
| SMTP     | TCP 25 |

---

## Common Commands

### HELO / EHLO

Start session.

### MAIL FROM

Specify sender.

### RCPT TO

Specify recipient.

### DATA

Begin email content.

### .

End email content.

### QUIT

Close connection.

---

## Example Session

```smtp
HELO client.thm
MAIL FROM: <user@client.thm>
RCPT TO: <strategos@server.thm>
DATA
Subject: Test

Hello World
.
QUIT
```

---

## Questions

### Which command starts email content?

Answer:

```text
DATA
```

---

### What indicates email completion?

Answer:

```text
.
```

(single dot on its own line)

---

# POP3 (Post Office Protocol v3)

## Purpose

Download emails from server.

---

## Port

| Protocol | Port    |
| -------- | ------- |
| POP3     | TCP 110 |

---

## Common Commands

### USER

Username.

### PASS

Password.

### STAT

Message count and size.

### LIST

List messages.

### RETR

Retrieve message.

### DELE

Delete message.

### QUIT

Logout.

---

## Example Session

```text
USER linda
PASS Pa$$123
LIST
RETR 4
```

---

## Question 1

What POP3 server software was running?

Observed:

```text
+OK Dovecot (Ubuntu) ready.
```

Answer:

```text
Dovecot
```

---

## Problem Encountered

Using telnet:

```text
-ERR [AUTH] Plaintext authentication disallowed on non-secure (SSL/TLS) connections.
```

The server required encrypted authentication.

---

## Solution

Use POP3S instead:

```bash
openssl s_client -connect 10.48.158.0:995 -quiet
```

Login:

```text
USER linda
PASS Pa$$123
```

Retrieve message:

```text
RETR 4
```

---

## Answer

```text
THM{TELNET_RETR_EMAIL}
```

---

## Important Lesson

Modern mail servers often disable plaintext authentication.

Telnet may no longer work even when documentation suggests it should.

---

# IMAP (Internet Message Access Protocol)

## Purpose

Synchronize email across multiple devices.

---

## Port

| Protocol | Port    |
| -------- | ------- |
| IMAP     | TCP 143 |

---

## POP3 vs IMAP

| POP3          | IMAP             |
| ------------- | ---------------- |
| Download      | Synchronize      |
| Single device | Multiple devices |
| Removes mail  | Keeps mail       |
| Lightweight   | More storage     |

---

## Common Commands

### LOGIN

Authenticate.

### SELECT

Choose mailbox.

### FETCH

Retrieve message.

### MOVE

Move messages.

### COPY

Copy messages.

### LOGOUT

End session.

---

## Example Session

```imap
LOGIN user password
SELECT inbox
FETCH 3 body[]
LOGOUT
```

---

## Question

What command retrieves the fourth email?

Answer:

```text
FETCH 4 body[]
```

---

# Protocol Summary

| Protocol | Transport | Default Port |
| -------- | --------- | ------------ |
| TELNET   | TCP       | 23           |
| DNS      | UDP/TCP   | 53           |
| HTTP     | TCP       | 80           |
| HTTPS    | TCP       | 443          |
| FTP      | TCP       | 21           |
| SMTP     | TCP       | 25           |
| POP3     | TCP       | 110          |
| IMAP     | TCP       | 143          |

---

# Key Pentesting Takeaways

This room was not about memorizing commands.

The main lesson was understanding how protocols communicate.

Most application protocols follow the same pattern:

```text
Client
↓
Command
↓
Server
↓
Response
```

Examples:

```text
HTTP  → GET
FTP   → RETR
SMTP  → DATA
POP3  → RETR
IMAP  → FETCH
```

Different command names.

Same communication model.

---

# Final Conclusion

This room provided practical exposure to the most common Internet protocols used in daily operations.

I learned:

* How DNS resolves names into IP addresses
* How WHOIS reveals domain ownership information
* How HTTP delivers web content
* How FTP transfers files
* How SMTP sends email
* How POP3 retrieves email
* How IMAP synchronizes email

Most importantly, I learned how these protocols operate beneath graphical applications and how to interact with them manually using tools such as:

```bash
whois
nslookup
dig
telnet
ftp
openssl s_client
```

This understanding is essential for network troubleshooting, packet analysis, reconnaissance, enumeration, and web application penetration testing.

The next logical step is learning how secure versions of these protocols protect communications using SSL/TLS in the Networking Secure Protocols room.
