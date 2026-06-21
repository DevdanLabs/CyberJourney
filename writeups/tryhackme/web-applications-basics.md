# Web Application Basics

## Room Information

| Field | Value |
|----------|----------|
| Platform | TryHackMe |
| Room Name | Web Application Basics |
| Difficulty | Easy |
| Category | Web Fundamentals |
| Status | Completed |

---

# Executive Summary

Web applications are one of the most common technologies used on the Internet today. Almost every online service, from social media and e-commerce platforms to banking systems and cloud applications, relies on web technologies.

Before learning web application security, it is important to understand how web applications actually work behind the scenes. Many beginners attempt to learn topics such as SQL Injection, Cross-Site Scripting (XSS), or API exploitation without first understanding how browsers communicate with servers. This often results in memorizing attack techniques without understanding the underlying concepts.

This room introduces the fundamental building blocks of modern web applications. It explains how users interact with websites through browsers, how Front End and Back End components work together, how URLs are structured, and how HTTP communication enables data exchange between clients and servers.

Understanding these fundamentals creates a strong foundation for future topics including:

- Burp Suite
- Authentication
- Session Management
- OWASP Top 10
- SQL Injection
- Cross-Site Scripting (XSS)
- IDOR
- API Security
- Web Application Pentesting

---

# Learning Objectives

By completing this room, I learned:

- What a web application is
- How web applications are structured
- The difference between Front End and Back End components
- The role of HTML, CSS, and JavaScript
- The purpose of databases in web applications
- The function of web servers and supporting infrastructure
- The role of Web Application Firewalls (WAFs)
- How URLs are structured
- How HTTP requests and responses work
- The meaning of common HTTP methods
- How HTTP status codes communicate server responses
- The purpose of HTTP security headers

---

# Why This Room Matters

Many cybersecurity topics eventually lead back to web applications.

Consider the following attack categories:

| Attack Type | Depends on Understanding HTTP? |
|------------|-------------------------------|
| SQL Injection | Yes |
| Cross-Site Scripting (XSS) | Yes |
| Authentication Bypass | Yes |
| Session Hijacking | Yes |
| IDOR | Yes |
| CSRF | Yes |
| API Exploitation | Yes |
| File Upload Attacks | Yes |

Without understanding how web applications communicate, these attacks often appear to be random tricks rather than logical abuses of web functionality.

This room builds the foundation required for understanding why these vulnerabilities exist.

---

# What Is a Web Application?

A web application is software that users access through a web browser.

Unlike traditional desktop applications, web applications run on remote servers and are accessed through technologies such as:

- Google Chrome
- Mozilla Firefox
- Microsoft Edge
- Safari

Examples of web applications include:

- Gmail
- YouTube
- Facebook
- GitHub
- Amazon
- TryHackMe

When a user visits one of these services, they are interacting with a web application rather than a simple webpage.

---

## Website vs Web Application

Many people use the terms interchangeably, but they are not exactly the same.

### Website

A website primarily provides information.

Examples:

- Blogs
- News websites
- Documentation pages
- Company profile pages

Users mainly consume information.

Example:

```text
User
  ↓
Reads content
```

---

### Web Application

A web application allows users to interact with and manipulate data.

Examples:

- Online banking
- Social media platforms
- E-commerce stores
- Cloud dashboards

Users provide input and the application processes it.

Example:

```text
User
  ↓
Login
  ↓
Server Processes Request
  ↓
Database Query
  ↓
Response
```

---

## Real-World Example

Consider logging into a social media platform.

The user enters:

```text
Username
Password
```

The application then:

1. Receives the credentials
2. Processes the login request
3. Checks the database
4. Creates a session
5. Returns a response

This entire process happens within a web application.

---

# Web Application Architecture

A web application is made up of multiple components working together.

High-level architecture:

```text
User
 │
 ▼
Browser
 │
 ▼
Front End
 │
 ▼
Internet
 │
 ▼
Back End
 │
 ▼
Database
```

Each layer has a specific responsibility.

---

## Planet Analogy

TryHackMe compares a web application to a planet.

This analogy is useful because users only see the surface.

For example:

```text
Planet Surface
```

represents:

```text
Buttons
Menus
Images
Forms
Pages
```

However, beneath the surface there are many hidden systems:

```text
Infrastructure
Databases
Servers
Security Controls
```

Similarly, web application users only see the Front End while most of the important processing happens behind the scenes.

---

# Front End Components

The Front End is everything a user can directly see and interact with through a browser.

Examples:

- Login forms
- Buttons
- Search bars
- Navigation menus
- Images
- Product pages

The Front End is primarily built using:

1. HTML
2. CSS
3. JavaScript

---

# HTML

## What Is HTML?

HTML stands for:

```text
HyperText Markup Language
```

HTML provides the structure of a webpage.

Think of HTML as the skeleton of a website.

Without HTML, a webpage would have no structure.

---

## Example

```html
<h1>Welcome</h1>

<p>Hello World</p>
```

The browser interprets this as:

```text
Heading:
Welcome

Paragraph:
Hello World
```

---

## Why HTML Exists

Browsers need instructions describing:

- What content exists
- How content is organized
- What elements should appear

HTML provides those instructions.

---

## Real-World Example

When visiting a login page, HTML defines:

```html
<input>
<button>
<form>
```

These elements create the visual structure users interact with.

---

## Pentester Relevance

HTML can reveal useful information.

Common discoveries include:

### Hidden Fields

```html
<input type="hidden" value="admin">
```

### Developer Comments

```html
<!-- TODO Remove admin page -->
```

### Internal References

```html
<a href="/backup">
```

These findings can help during reconnaissance and enumeration.

---

# CSS

## What Is CSS?

CSS stands for:

```text
Cascading Style Sheets
```

CSS controls the appearance of a webpage.

If HTML is the skeleton, CSS is the skin and clothing.

---

## Example

```css
h1 {
    color: blue;
}
```

The browser displays:

```text
Blue Heading
```

instead of a default heading.

---

## Responsibilities

CSS controls:

- Colors
- Fonts
- Layouts
- Responsive design
- Animations
- Positioning

---

## Why CSS Exists

Without CSS:

```text
Functional
But ugly
```

With CSS:

```text
Professional
Organized
User Friendly
```

---

## Pentester Relevance

CSS is rarely a direct attack target.

However, it may reveal:

```text
/admin.css
/staff.css
/internal.css
```

These files may indicate hidden functionality.

During reconnaissance, CSS files are often inspected for references to:

- Hidden pages
- Internal resources
- Administrative interfaces

---

# JavaScript

## What Is JavaScript?

JavaScript is a programming language executed within the browser.

It allows webpages to become interactive.

---

## Example

```javascript
alert("Welcome!");
```

When executed:

```text
Popup Appears
```

---

## Why JavaScript Exists

HTML describes content.

CSS controls appearance.

JavaScript controls behavior.

Example:

```text
User clicks button
      ↓
JavaScript executes
      ↓
Request sent to server
      ↓
Response received
      ↓
Page updates
```

---

## Real-World Examples

JavaScript enables:

- Dynamic content loading
- Form validation
- Chat applications
- Notifications
- Search suggestions
- Interactive dashboards

---

## Pentester Relevance

JavaScript is extremely valuable during reconnaissance.

Unlike server-side code, JavaScript is sent directly to the browser.

This means attackers can inspect it.

Common findings include:

### Hidden Endpoints

```javascript
/api/admin/users
```

### Internal APIs

```javascript
https://internal-api.company.com
```

### Exposed Secrets

```javascript
const apiKey = "123456";
```

### Hidden Functionality

```javascript
if(user.role == "admin")
```

Because of this, JavaScript analysis is a common activity during web application assessments.

---

# Back End Components

The Back End consists of systems users do not directly see.

The Back End processes requests, stores data, and generates responses.

Users interact with the Front End.

The Front End communicates with the Back End.

The Back End performs the actual work.

---

## Example Workflow

```text
User enters credentials
           ↓
Browser sends request
           ↓
Server receives request
           ↓
Database verifies credentials
           ↓
Server generates response
           ↓
Browser displays result
```

The user only sees:

```text
Login Successful
```

Most processing occurs behind the scenes.

# URL Fundamentals

A URL (Uniform Resource Locator) is the address used to locate and access resources on the Internet.

Every time a user opens a website, clicks a hyperlink, downloads a file, watches a video, or interacts with a web application, a URL is involved somewhere in the process.

Examples:

```text
https://tryhackme.com
https://github.com
https://www.google.com/search?q=web+security
```

While URLs may appear simple, they contain multiple components that provide important information about how a request is handled.

For web application security professionals, URLs are one of the most important sources of information during reconnaissance and enumeration.

---

# Why URLs Matter

When most users look at a URL, they simply see:

```text
A website address
```

A pentester sees:

```text
Protocol
Domain
Technology clues
Application structure
Parameters
Potential attack surfaces
```

Many web vulnerabilities begin with analyzing and manipulating URLs.

Examples:

- SQL Injection
- IDOR
- Open Redirect
- Local File Inclusion (LFI)
- Remote File Inclusion (RFI)
- Path Traversal

Understanding URLs is therefore a fundamental web security skill.

---

# URL Anatomy

Consider the following URL:

```text
https://user:password@tryhackme.com:443/view-room?id=1#task3
```

This URL contains several individual components:

```text
https://
│
├── Scheme

user:password@
│
├── User Information

tryhackme.com
│
├── Host / Domain

:443
│
├── Port

/view-room
│
├── Path

?id=1
│
├── Query String

#task3
│
└── Fragment
```

Each component serves a specific purpose.

---

# Scheme

## What Is a Scheme?

The scheme specifies which protocol should be used to communicate with the server.

Examples:

```text
http
https
ftp
```

A scheme appears at the beginning of a URL.

Example:

```text
https://tryhackme.com
```

The scheme is:

```text
https
```

---

## Why Does It Exist?

Computers need to know how communication should occur.

Different protocols provide different rules.

Examples:

| Protocol | Purpose |
|-----------|----------|
| HTTP | Web traffic |
| HTTPS | Encrypted web traffic |
| FTP | File transfers |
| SSH | Remote administration |

The scheme tells the browser which protocol to use.

---

## HTTP

Example:

```text
http://example.com
```

Default Port:

```text
80
```

Characteristics:

- Unencrypted
- Data transmitted in plaintext
- Vulnerable to interception

---

### Security Risk

Suppose a user logs in using HTTP.

The browser sends:

```text
Username
Password
```

Across the network without encryption.

An attacker monitoring network traffic may capture:

```text
Credentials
Session Cookies
Personal Information
```

---

## HTTPS

Example:

```text
https://example.com
```

Default Port:

```text
443
```

Characteristics:

- Encrypted communication
- Uses TLS (Transport Layer Security)
- Protects sensitive information

---

### Why HTTPS Matters

Without HTTPS:

```text
Attacker
    ↓
Reads Traffic
```

With HTTPS:

```text
Attacker
    ↓
Sees Encrypted Data
```

This is why modern websites enforce HTTPS whenever possible.

---

## Pentester Perspective

One of the first things a tester checks is:

```text
Does the application use HTTPS?
```

Questions include:

- Is HTTP still enabled?
- Can HTTPS be downgraded?
- Are cookies protected?
- Is HSTS configured?

---

# User Information

## What Is User Information?

Older URL formats allowed authentication information directly within the URL.

Example:

```text
http://admin:password@example.com
```

Components:

```text
admin
```

Username

```text
password
```

Password

---

## Why Was It Used?

Historically, browsers could automatically authenticate users when accessing protected resources.

Example:

```text
http://username:password@server.com
```

---

## Why Is It Rare Today?

Because it creates serious security risks.

The credentials may appear in:

- Browser history
- Proxy logs
- Screenshots
- Shared links
- Server logs

---

## Security Risk

Example:

```text
http://admin:SuperSecretPassword@example.com
```

Anyone who sees the URL immediately gains access to the credentials.

---

## Pentester Perspective

Finding credentials embedded in URLs often indicates:

- Legacy systems
- Misconfigurations
- Poor security practices

This can sometimes result in immediate access to protected resources.

---

# Host / Domain

## What Is a Domain?

The domain identifies the destination website.

Example:

```text
tryhackme.com
```

The domain is the human-friendly version of an IP address.

---

## Why Does It Exist?

Humans remember:

```text
google.com
```

more easily than:

```text
142.250.190.78
```

Domains provide a user-friendly way to access services.

---

## Behind the Scenes

Before connecting:

```text
tryhackme.com
```

must be converted into:

```text
IP Address
```

using DNS.

Example:

```text
tryhackme.com
      ↓
DNS Lookup
      ↓
104.x.x.x
```

---

## Security Perspective

Domains are commonly abused in phishing attacks.

Example:

Legitimate:

```text
google.com
```

Malicious:

```text
g00gle.com
```

or

```text
goog1e.com
```

This technique is called:

```text
Typosquatting
```

---

## Pentester Perspective

During reconnaissance, testers investigate:

- Subdomains
- Related domains
- DNS records
- Publicly exposed services

Examples:

```text
admin.company.com
vpn.company.com
mail.company.com
```

These may expose additional attack surfaces.

---

# Port

## What Is a Port?

A port identifies a specific service running on a server.

Think of an IP address as an apartment building.

The port is the apartment number.

Example:

```text
IP Address = Building

Port = Apartment
```

---

## Common Ports

| Port | Service |
|--------|----------|
| 80 | HTTP |
| 443 | HTTPS |
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |

---

## Example

```text
https://example.com:8443
```

Port:

```text
8443
```

The server is listening on a non-standard HTTPS port.

---

## Pentester Perspective

Unusual ports often indicate:

- Administrative interfaces
- Development environments
- Alternative services
- Forgotten applications

Example:

```text
https://example.com:8080
```

might reveal:

```text
Tomcat
Jenkins
Management Panels
```

---

# Path

## What Is a Path?

The path identifies the specific resource requested from the server.

Example:

```text
https://example.com/products
```

Path:

```text
/ products
```

---

## Why Does It Exist?

A website may contain thousands of resources.

Examples:

```text
/login
/profile
/products
/admin
/uploads
```

The path tells the server which resource is requested.

---

## Pentester Perspective

Paths are one of the most important attack surfaces.

Common targets include:

```text
/admin
/backup
/uploads
/api
/config
```

Enumeration tools frequently search for hidden paths.

Examples:

```bash
ffuf
gobuster
dirsearch
feroxbuster
```

---

# Query String

## What Is a Query String?

A query string provides additional information to the application.

Example:

```text
?id=1
```

Format:

```text
?parameter=value
```

---

## Example

```text
https://example.com/profile?id=1
```

Parameter:

```text
id=1
```

The application may interpret this as:

```text
Display profile #1
```

---

## Multiple Parameters

Example:

```text
?id=1&sort=name
```

Parameters:

```text
id=1
sort=name
```

---

## Why Query Strings Matter

Unlike many other URL components, users can easily modify parameters.

Example:

Original:

```text
?id=1
```

Modified:

```text
?id=2
```

---

## Pentester Perspective

Query strings are among the most common attack surfaces.

Potential vulnerabilities:

### SQL Injection

```text
?id=1'
```

### IDOR

```text
/profile?id=100
```

changed to:

```text
/profile?id=101
```

### Local File Inclusion

```text
?page=home.php
```

changed to:

```text
?page=../../etc/passwd
```

---

# Fragment

## What Is a Fragment?

A fragment identifies a specific location within a webpage.

Example:

```text
https://example.com/tutorial#chapter5
```

Fragment:

```text
#chapter5
```

---

## Purpose

Fragments help users jump directly to specific sections of content.

Examples:

- Documentation
- Wikis
- Tutorials
- Long articles

---

## Important Security Note

Fragments are not sent to the server.

For example:

```text
https://example.com/page#admin
```

The server only receives:

```text
https://example.com/page
```

The fragment is processed entirely by the browser.

---

## Pentester Perspective

Although fragments are generally lower risk, modern web frameworks sometimes use fragments for routing.

Examples:

- React
- Angular
- Vue

Improper client-side handling can occasionally introduce security issues.

---

# Pentester Notes: How URLs Become Attack Surfaces

When viewing a URL, a user sees:

```text
A webpage address
```

A pentester sees:

```text
Protocol
↓
HTTPS Enabled?

Domain
↓
Legitimate?

Port
↓
Interesting Services?

Path
↓
Hidden Resources?

Query Parameters
↓
Manipulatable?

Fragment
↓
Client-Side Logic?
```

This mindset is essential because many web vulnerabilities begin with a simple question:

```text
What happens if I change part of this URL?
```

That question drives much of modern web application testing.

# HTTP Messages

Now that we understand how URLs work, the next step is understanding how web browsers and web servers communicate.

Whenever a user interacts with a web application, information is exchanged using HTTP messages.

Examples:

- Opening a webpage
- Logging into an account
- Uploading a file
- Searching for products
- Updating a profile

All of these actions rely on HTTP messages.

---

# What Are HTTP Messages?

HTTP messages are structured packets of information exchanged between:

```text
Client (Browser)
        ↔
Web Server
```

There are two types of HTTP messages:

```text
HTTP Request
```

Sent by the client.

and

```text
HTTP Response
```

Sent by the server.

---

## Real-World Analogy

Imagine ordering food at a restaurant.

```text
Customer
    ↓
Order
    ↓
Kitchen
    ↓
Food
    ↓
Customer
```

In HTTP:

```text
Browser
    ↓
HTTP Request
    ↓
Web Server
    ↓
HTTP Response
    ↓
Browser
```

The request is the order.

The response is the food delivered back to the customer.

---

# HTTP Communication Flow

Example:

A user visits:

```text
https://example.com/login
```

The browser sends:

```http
GET /login HTTP/1.1
```

The server processes the request.

The server responds:

```http
HTTP/1.1 200 OK
```

along with the page content.

---

# HTTP Message Structure

Both HTTP Requests and HTTP Responses share a similar structure.

```text
Start Line

Headers

Empty Line

Body
```

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=password
```

Let's break this down.

---

# Start Line

The Start Line is the first line of an HTTP message.

It immediately tells us:

```text
What kind of message is this?
```

For requests:

```http
POST /login HTTP/1.1
```

For responses:

```http
HTTP/1.1 200 OK
```

The exact structure differs between requests and responses.

---

# Headers

Headers contain metadata.

Metadata means:

```text
Information about the information
```

Headers provide additional instructions for:

- Browsers
- Servers
- Proxies
- Security systems

Example:

```http
Host: example.com
User-Agent: Chrome
Content-Type: application/json
```

Headers are one of the most important areas in web security testing.

---

# Empty Line

A blank line separates:

```text
Headers
```

from

```text
Body
```

Without this separator, the receiver would not know where headers end and where body data begins.

---

# Body

The body contains the actual data.

Examples:

```text
Username
Password
JSON Data
File Uploads
Images
```

Not all requests contain a body.

GET requests usually do not.

POST requests usually do.

---

# HTTP Requests

An HTTP Request is sent by the client to the server.

Purpose:

```text
Request information
Send information
Trigger actions
```

Examples:

```text
Open a webpage
Login
Register
Upload a file
Delete a record
```

---

# Request Structure

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=password
```

Structure:

```text
Request Line
Headers
Body
```

---

# Request Line

The Request Line is the first line of every HTTP Request.

Format:

```text
METHOD PATH HTTP_VERSION
```

Example:

```http
GET /profile HTTP/1.1
```

Three components exist:

```text
Method
Path
Version
```

---

# HTTP Methods

HTTP Methods define the action the client wants the server to perform.

Think of methods as verbs.

Example:

```text
Read
Create
Update
Delete
```

Different methods represent different actions.

---

# GET Method

## Purpose

Retrieve information from the server.

---

## Example

```http
GET /products HTTP/1.1
```

Meaning:

```text
Give me the products page.
```

---

## Real-World Examples

- Opening webpages
- Viewing profiles
- Reading blog posts
- Searching products

---

## Characteristics

GET requests:

```text
Read data
Should not modify data
```

---

## Security Considerations

Parameters are often visible.

Example:

```text
/search?q=laptop
```

Data may appear in:

- Browser history
- Proxy logs
- Server logs
- Shared URLs

Sensitive information should never be sent through GET requests.

---

## Pentester Relevance

GET requests are heavily used during:

```text
Reconnaissance
Enumeration
Directory Discovery
```

Tools:

```text
ffuf
Gobuster
Dirsearch
Feroxbuster
```

primarily generate GET requests.

---

# POST Method

## Purpose

Send data to the server.

---

## Example

```http
POST /login HTTP/1.1
```

Meaning:

```text
I am sending data.
```

---

## Real-World Examples

- Login
- Registration
- Contact Forms
- Comments
- File Uploads

---

## Example Body

```text
username=admin
password=password
```

---

## Security Considerations

POST requests often accept user input.

Potential vulnerabilities:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- SSTI

---

## Pentester Relevance

Whenever a POST request accepts user input, testers ask:

```text
Can this input be manipulated?
```

---

# PUT Method

## Purpose

Replace or update an existing resource.

---

## Example

```http
PUT /users/1 HTTP/1.1
```

Meaning:

```text
Replace user 1's information.
```

---

## Common Usage

REST APIs

Example:

```json
{
  "name": "Devdan",
  "email": "devdan@example.com"
}
```

---

## Security Considerations

Applications must verify:

```text
Who is allowed to update this resource?
```

---

## Pentester Relevance

Improper authorization often leads to:

```text
Privilege Escalation
IDOR
Broken Access Control
```

---

# PATCH Method

## Purpose

Partially update a resource.

---

## Difference Between PUT and PATCH

PUT:

```text
Replace everything
```

PATCH:

```text
Modify only specific fields
```

---

Example:

Current user:

```json
{
  "name": "Devdan",
  "email": "old@email.com",
  "role": "user"
}
```

PATCH:

```json
{
  "email": "new@email.com"
}
```

Only updates the email field.

---

# DELETE Method

## Purpose

Remove a resource.

---

## Example

```http
DELETE /users/5 HTTP/1.1
```

Meaning:

```text
Delete user 5.
```

---

## Security Considerations

Deletion operations should require:

- Authentication
- Authorization
- Logging

---

## Pentester Relevance

DELETE endpoints are high-value targets.

Questions include:

```text
Can unauthorized users delete data?
```

---

# HEAD Method

## Purpose

Retrieve only headers.

No response body is returned.

---

## Example

```http
HEAD /file.zip HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK
Content-Length: 50000000
```

---

## Use Cases

- Determine file size
- Check modification dates
- Verify content types

---

## Pentester Relevance

Useful for:

```text
Information Gathering
Reconnaissance
```

---

# OPTIONS Method

## Purpose

Ask the server which methods are supported.

---

## Example

```http
OPTIONS /api/users HTTP/1.1
```

Response:

```http
Allow: GET, POST, PUT, DELETE
```

---

## Pentester Relevance

Helps identify:

```text
Available Functionality
Potential Attack Surface
```

---

# TRACE Method

## Purpose

Diagnostic method.

The server echoes the received request.

---

## Example

```http
TRACE / HTTP/1.1
```

---

## Security Considerations

TRACE can introduce security risks.

Many organizations disable it.

---

## Pentester Relevance

Historically associated with:

```text
Cross-Site Tracing (XST)
```

---

# CONNECT Method

## Purpose

Create a network tunnel.

Most commonly used with HTTPS proxies.

---

## Example

```http
CONNECT google.com:443 HTTP/1.1
```

---

## Typical Usage

- HTTPS Proxies
- VPN Solutions
- Secure Tunneling

---

# Request Headers

Headers provide additional information about a request.

Common examples:

| Header | Purpose |
|----------|----------|
| Host | Target website |
| User-Agent | Browser information |
| Referer | Previous page |
| Cookie | Session information |
| Content-Type | Data format |

---

# Host Header

Example:

```http
Host: tryhackme.com
```

Identifies the target website.

---

## Pentester Relevance

Can sometimes lead to:

```text
Host Header Injection
Password Reset Poisoning
```

---

# User-Agent Header

Example:

```http
User-Agent: Mozilla/5.0
```

Identifies the browser.

---

## Pentester Relevance

Sometimes applications trust User-Agent values incorrectly.

Since User-Agent is user-controlled, it should never be trusted.

---

# Referer Header

Example:

```http
Referer: https://google.com
```

Indicates where the user came from.

---

## Pentester Relevance

Applications occasionally use Referer-based access controls.

These are often bypassable.

---

# Cookie Header

Example:

```http
Cookie: session=abc123
```

Stores session information.

---

## Why Cookies Matter

Cookies often represent:

```text
Identity
Authentication
Session State
```

---

## Pentester Relevance

Cookies are among the most valuable pieces of data for attackers.

Compromised cookies can lead to:

```text
Session Hijacking
Account Takeover
```

---

# Content-Type Header

Example:

```http
Content-Type: application/json
```

Defines the body format.

The server uses this information to process incoming data correctly.

---

# Request Body Formats

Different applications use different body formats.

Understanding these formats is critical during API testing and web application assessments.

---

# URL Encoded

Content-Type:

```http
application/x-www-form-urlencoded
```

Example:

```text
username=admin&password=password
```

Commonly used in:

- Login Forms
- Registration Forms
- Search Forms

---

# Multipart Form Data

Content-Type:

```http
multipart/form-data
```

Used for:

- File Uploads
- Image Uploads
- Document Uploads

---

## Pentester Relevance

Important for testing:

```text
File Upload Vulnerabilities
```

---

# JSON

Content-Type:

```http
application/json
```

Example:

```json
{
  "username": "admin",
  "role": "user"
}
```

---

## Why JSON Is Popular

Advantages:

- Lightweight
- Easy to read
- Easy to parse

---

## Pentester Relevance

Most modern APIs use JSON.

Understanding JSON is essential for:

- API Security Testing
- Mobile Application Testing
- JWT Analysis

---

# XML

Content-Type:

```http
application/xml
```

Example:

```xml
<user>
    <name>Devdan</name>
</user>
```

---

## Pentester Relevance

XML processing vulnerabilities include:

```text
XXE
(XML External Entity)
```

One of the classic web application vulnerabilities.

---

# Key Takeaways

- HTTP Requests are the foundation of web communication.
- Every request contains a start line, headers, and optionally a body.
- HTTP Methods define actions performed by the server.
- Request headers provide important metadata.
- Body formats vary depending on application requirements.
- Most modern web attacks involve manipulating requests and observing responses.
- Understanding HTTP Requests is essential before learning Burp Suite and web exploitation.

# HTTP Messages

Now that we understand how URLs work, the next step is understanding how web browsers and web servers communicate.

Whenever a user interacts with a web application, information is exchanged using HTTP messages.

Examples:

- Opening a webpage
- Logging into an account
- Uploading a file
- Searching for products
- Updating a profile

All of these actions rely on HTTP messages.

---

# What Are HTTP Messages?

HTTP messages are structured packets of information exchanged between:

```text
Client (Browser)
        ↔
Web Server
```

There are two types of HTTP messages:

```text
HTTP Request
```

Sent by the client.

and

```text
HTTP Response
```

Sent by the server.

---

## Real-World Analogy

Imagine ordering food at a restaurant.

```text
Customer
    ↓
Order
    ↓
Kitchen
    ↓
Food
    ↓
Customer
```

In HTTP:

```text
Browser
    ↓
HTTP Request
    ↓
Web Server
    ↓
HTTP Response
    ↓
Browser
```

The request is the order.

The response is the food delivered back to the customer.

---

# HTTP Communication Flow

Example:

A user visits:

```text
https://example.com/login
```

The browser sends:

```http
GET /login HTTP/1.1
```

The server processes the request.

The server responds:

```http
HTTP/1.1 200 OK
```

along with the page content.

---

# HTTP Message Structure

Both HTTP Requests and HTTP Responses share a similar structure.

```text
Start Line

Headers

Empty Line

Body
```

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=password
```

Let's break this down.

---

# Start Line

The Start Line is the first line of an HTTP message.

It immediately tells us:

```text
What kind of message is this?
```

For requests:

```http
POST /login HTTP/1.1
```

For responses:

```http
HTTP/1.1 200 OK
```

The exact structure differs between requests and responses.

---

# Headers

Headers contain metadata.

Metadata means:

```text
Information about the information
```

Headers provide additional instructions for:

- Browsers
- Servers
- Proxies
- Security systems

Example:

```http
Host: example.com
User-Agent: Chrome
Content-Type: application/json
```

Headers are one of the most important areas in web security testing.

---

# Empty Line

A blank line separates:

```text
Headers
```

from

```text
Body
```

Without this separator, the receiver would not know where headers end and where body data begins.

---

# Body

The body contains the actual data.

Examples:

```text
Username
Password
JSON Data
File Uploads
Images
```

Not all requests contain a body.

GET requests usually do not.

POST requests usually do.

---

# HTTP Requests

An HTTP Request is sent by the client to the server.

Purpose:

```text
Request information
Send information
Trigger actions
```

Examples:

```text
Open a webpage
Login
Register
Upload a file
Delete a record
```

---

# Request Structure

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=password
```

Structure:

```text
Request Line
Headers
Body
```

---

# Request Line

The Request Line is the first line of every HTTP Request.

Format:

```text
METHOD PATH HTTP_VERSION
```

Example:

```http
GET /profile HTTP/1.1
```

Three components exist:

```text
Method
Path
Version
```

---

# HTTP Methods

HTTP Methods define the action the client wants the server to perform.

Think of methods as verbs.

Example:

```text
Read
Create
Update
Delete
```

Different methods represent different actions.

---

# GET Method

## Purpose

Retrieve information from the server.

---

## Example

```http
GET /products HTTP/1.1
```

Meaning:

```text
Give me the products page.
```

---

## Real-World Examples

- Opening webpages
- Viewing profiles
- Reading blog posts
- Searching products

---

## Characteristics

GET requests:

```text
Read data
Should not modify data
```

---

## Security Considerations

Parameters are often visible.

Example:

```text
/search?q=laptop
```

Data may appear in:

- Browser history
- Proxy logs
- Server logs
- Shared URLs

Sensitive information should never be sent through GET requests.

---

## Pentester Relevance

GET requests are heavily used during:

```text
Reconnaissance
Enumeration
Directory Discovery
```

Tools:

```text
ffuf
Gobuster
Dirsearch
Feroxbuster
```

primarily generate GET requests.

---

# POST Method

## Purpose

Send data to the server.

---

## Example

```http
POST /login HTTP/1.1
```

Meaning:

```text
I am sending data.
```

---

## Real-World Examples

- Login
- Registration
- Contact Forms
- Comments
- File Uploads

---

## Example Body

```text
username=admin
password=password
```

---

## Security Considerations

POST requests often accept user input.

Potential vulnerabilities:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- SSTI

---

## Pentester Relevance

Whenever a POST request accepts user input, testers ask:

```text
Can this input be manipulated?
```

---

# PUT Method

## Purpose

Replace or update an existing resource.

---

## Example

```http
PUT /users/1 HTTP/1.1
```

Meaning:

```text
Replace user 1's information.
```

---

## Common Usage

REST APIs

Example:

```json
{
  "name": "Devdan",
  "email": "devdan@example.com"
}
```

---

## Security Considerations

Applications must verify:

```text
Who is allowed to update this resource?
```

---

## Pentester Relevance

Improper authorization often leads to:

```text
Privilege Escalation
IDOR
Broken Access Control
```

---

# PATCH Method

## Purpose

Partially update a resource.

---

## Difference Between PUT and PATCH

PUT:

```text
Replace everything
```

PATCH:

```text
Modify only specific fields
```

---

Example:

Current user:

```json
{
  "name": "Devdan",
  "email": "old@email.com",
  "role": "user"
}
```

PATCH:

```json
{
  "email": "new@email.com"
}
```

Only updates the email field.

---

# DELETE Method

## Purpose

Remove a resource.

---

## Example

```http
DELETE /users/5 HTTP/1.1
```

Meaning:

```text
Delete user 5.
```

---

## Security Considerations

Deletion operations should require:

- Authentication
- Authorization
- Logging

---

## Pentester Relevance

DELETE endpoints are high-value targets.

Questions include:

```text
Can unauthorized users delete data?
```

---

# HEAD Method

## Purpose

Retrieve only headers.

No response body is returned.

---

## Example

```http
HEAD /file.zip HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK
Content-Length: 50000000
```

---

## Use Cases

- Determine file size
- Check modification dates
- Verify content types

---

## Pentester Relevance

Useful for:

```text
Information Gathering
Reconnaissance
```

---

# OPTIONS Method

## Purpose

Ask the server which methods are supported.

---

## Example

```http
OPTIONS /api/users HTTP/1.1
```

Response:

```http
Allow: GET, POST, PUT, DELETE
```

---

## Pentester Relevance

Helps identify:

```text
Available Functionality
Potential Attack Surface
```

---

# TRACE Method

## Purpose

Diagnostic method.

The server echoes the received request.

---

## Example

```http
TRACE / HTTP/1.1
```

---

## Security Considerations

TRACE can introduce security risks.

Many organizations disable it.

---

## Pentester Relevance

Historically associated with:

```text
Cross-Site Tracing (XST)
```

---

# CONNECT Method

## Purpose

Create a network tunnel.

Most commonly used with HTTPS proxies.

---

## Example

```http
CONNECT google.com:443 HTTP/1.1
```

---

## Typical Usage

- HTTPS Proxies
- VPN Solutions
- Secure Tunneling

---

# Request Headers

Headers provide additional information about a request.

Common examples:

| Header | Purpose |
|----------|----------|
| Host | Target website |
| User-Agent | Browser information |
| Referer | Previous page |
| Cookie | Session information |
| Content-Type | Data format |

---

# Host Header

Example:

```http
Host: tryhackme.com
```

Identifies the target website.

---

## Pentester Relevance

Can sometimes lead to:

```text
Host Header Injection
Password Reset Poisoning
```

---

# User-Agent Header

Example:

```http
User-Agent: Mozilla/5.0
```

Identifies the browser.

---

## Pentester Relevance

Sometimes applications trust User-Agent values incorrectly.

Since User-Agent is user-controlled, it should never be trusted.

---

# Referer Header

Example:

```http
Referer: https://google.com
```

Indicates where the user came from.

---

## Pentester Relevance

Applications occasionally use Referer-based access controls.

These are often bypassable.

---

# Cookie Header

Example:

```http
Cookie: session=abc123
```

Stores session information.

---

## Why Cookies Matter

Cookies often represent:

```text
Identity
Authentication
Session State
```

---

## Pentester Relevance

Cookies are among the most valuable pieces of data for attackers.

Compromised cookies can lead to:

```text
Session Hijacking
Account Takeover
```

---

# Content-Type Header

Example:

```http
Content-Type: application/json
```

Defines the body format.

The server uses this information to process incoming data correctly.

---

# Request Body Formats

Different applications use different body formats.

Understanding these formats is critical during API testing and web application assessments.

---

# URL Encoded

Content-Type:

```http
application/x-www-form-urlencoded
```

Example:

```text
username=admin&password=password
```

Commonly used in:

- Login Forms
- Registration Forms
- Search Forms

---

# Multipart Form Data

Content-Type:

```http
multipart/form-data
```

Used for:

- File Uploads
- Image Uploads
- Document Uploads

---

## Pentester Relevance

Important for testing:

```text
File Upload Vulnerabilities
```

---

# JSON

Content-Type:

```http
application/json
```

Example:

```json
{
  "username": "admin",
  "role": "user"
}
```

---

## Why JSON Is Popular

Advantages:

- Lightweight
- Easy to read
- Easy to parse

---

## Pentester Relevance

Most modern APIs use JSON.

Understanding JSON is essential for:

- API Security Testing
- Mobile Application Testing
- JWT Analysis

---

# XML

Content-Type:

```http
application/xml
```

Example:

```xml
<user>
    <name>Devdan</name>
</user>
```

---

## Pentester Relevance

XML processing vulnerabilities include:

```text
XXE
(XML External Entity)
```

One of the classic web application vulnerabilities.

---

# Key Takeaways

- HTTP Requests are the foundation of web communication.
- Every request contains a start line, headers, and optionally a body.
- HTTP Methods define actions performed by the server.
- Request headers provide important metadata.
- Body formats vary depending on application requirements.
- Most modern web attacks involve manipulating requests and observing responses.
- Understanding HTTP Requests is essential before learning Burp Suite and web exploitation.

# Security Headers

Throughout this room, we have learned how browsers and web servers communicate using HTTP.

However, communication alone is not enough.

Modern web applications must also defend themselves against various attacks such as:

- Cross-Site Scripting (XSS)
- Clickjacking
- Session Hijacking
- Protocol Downgrade Attacks
- Information Disclosure
- MIME-Type Confusion Attacks

One of the easiest ways to improve web application security is through the use of HTTP Security Headers.

Security headers are additional instructions sent by the server that tell browsers how to behave securely.

Example:

```http
HTTP/1.1 200 OK

Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=31536000
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin
```

When a browser receives these headers, it enforces additional security rules that help reduce the attack surface.

---

# Why Security Headers Matter

Without security headers:

```text
Browser
    ↓
Makes assumptions
    ↓
Potentially unsafe behavior
```

With security headers:

```text
Server
    ↓
Provides explicit instructions
    ↓
Browser follows secure behavior
```

Think of security headers as safety rules provided by the website administrator.

---

# Content-Security-Policy (CSP)

## What Is CSP?

Content Security Policy (CSP) is a security mechanism designed to reduce the risk of Cross-Site Scripting (XSS) attacks.

It allows website administrators to define which sources of content are trusted.

---

## Why CSP Exists

Consider a vulnerable application:

```html
<script src="https://evil.com/malware.js"></script>
```

Without restrictions, the browser downloads and executes the malicious script.

Result:

```text
Cross-Site Scripting (XSS)
```

---

## How CSP Helps

The server sends:

```http
Content-Security-Policy: default-src 'self'
```

Meaning:

```text
Only load content
from this website itself.
```

The browser will refuse:

```text
https://evil.com/malware.js
```

because it is not trusted.

---

# Example CSP Header

```http
Content-Security-Policy:
default-src 'self';
script-src 'self' https://cdn.tryhackme.com;
style-src 'self';
```

---

## Understanding Each Directive

### default-src

```http
default-src 'self'
```

Default policy.

Meaning:

```text
Only allow resources
from the current website.
```

---

### script-src

```http
script-src 'self' https://cdn.tryhackme.com
```

JavaScript may only load from:

```text
Current Website
cdn.tryhackme.com
```

---

### style-src

```http
style-src 'self'
```

CSS files may only load from:

```text
Current Website
```

---

# Real-World Example

Allowed:

```html
<script src="https://cdn.tryhackme.com/app.js"></script>
```

Blocked:

```html
<script src="https://evil.com/payload.js"></script>
```

---

# Red Team Perspective

When testing CSP:

Questions include:

```text
Is CSP present?

How restrictive is it?

Can it be bypassed?

Are unsafe directives enabled?
```

---

## Weak CSP Example

```http
Content-Security-Policy: script-src *
```

This effectively allows scripts from anywhere.

Protection is minimal.

---

# Blue Team Perspective

CSP should be viewed as:

```text
Mitigation
```

not

```text
A replacement for secure coding.
```

Even with CSP, applications should still properly validate and sanitize user input.

---

# Strict-Transport-Security (HSTS)

## What Is HSTS?

HSTS stands for:

```text
HTTP Strict Transport Security
```

It forces browsers to communicate using HTTPS.

---

# Why HSTS Exists

Suppose a user enters:

```text
http://example.com
```

instead of:

```text
https://example.com
```

This creates an opportunity for attackers to intercept communications.

---

## Potential Attack

Without HSTS:

```text
User
    ↓
HTTP
    ↓
Attacker
    ↓
Website
```

This can lead to:

```text
Man-In-The-Middle Attacks
SSL Stripping
Credential Theft
```

---

# Example HSTS Header

```http
Strict-Transport-Security:
max-age=63072000;
includeSubDomains;
preload
```

---

# HSTS Directives

## max-age

```http
max-age=63072000
```

Specifies how long browsers should remember the rule.

Example:

```text
63,072,000 seconds
≈ 2 years
```

---

## includeSubDomains

Extends HSTS protection to:

```text
www.example.com
api.example.com
admin.example.com
```

---

## preload

Allows the website to be added to browser preload lists.

This means:

```text
Browser knows HTTPS
is required
before first visit.
```

---

# Red Team Perspective

Missing HSTS may allow:

```text
HTTPS Downgrade Attacks
SSL Stripping
```

---

# Blue Team Perspective

Modern production websites should almost always use HSTS.

---

# X-Content-Type-Options

## What Is It?

This header prevents browsers from guessing content types.

Example:

```http
X-Content-Type-Options: nosniff
```

---

# Why Does It Exist?

Browsers sometimes try to be helpful.

Suppose the server sends:

```http
Content-Type: text/plain
```

but the content looks like:

```html
<script>alert(1)</script>
```

Some browsers may attempt to interpret it as HTML.

This behavior is called:

```text
MIME Sniffing
```

---

# Why Is MIME Sniffing Dangerous?

Attackers may exploit browser guessing behavior to:

- Execute malicious scripts
- Abuse uploaded files
- Trigger XSS-like behavior

---

# How nosniff Helps

```http
X-Content-Type-Options: nosniff
```

tells the browser:

```text
Do not guess.

Only trust the
Content-Type header.
```

---

# Red Team Perspective

Missing nosniff may increase the attack surface for:

```text
File Upload Vulnerabilities
Content-Type Confusion
MIME Abuse
```

---

# Blue Team Perspective

The header is easy to implement and widely recommended.

---

# Referrer-Policy

## What Is Referrer Information?

When users click links, browsers may tell the destination website where the user came from.

Example:

```text
https://bank.com/account?id=123
```

User clicks:

```text
https://example.com
```

Browser may send:

```http
Referer: https://bank.com/account?id=123
```

---

# Why Is This a Problem?

Sensitive information may leak.

Examples:

- User IDs
- Internal URLs
- Query Parameters
- Session Tokens

---

# Example Referrer Policies

```http
Referrer-Policy: no-referrer
```

```http
Referrer-Policy: same-origin
```

```http
Referrer-Policy: strict-origin
```

```http
Referrer-Policy: strict-origin-when-cross-origin
```

---

# no-referrer

```http
Referrer-Policy: no-referrer
```

Meaning:

```text
Never send
referrer information.
```

Highest privacy.

---

# same-origin

```http
Referrer-Policy: same-origin
```

Meaning:

```text
Only send referrer
to pages within
the same website.
```

---

# strict-origin

Only sends:

```text
https://bank.com
```

instead of:

```text
https://bank.com/account?id=123
```

This reduces information leakage.

---

# strict-origin-when-cross-origin

Modern default behavior.

Same-origin requests:

```text
Full URL
```

Cross-origin requests:

```text
Origin only
```

---

# Red Team Perspective

Improper referrer handling can reveal:

- Internal paths
- User identifiers
- Sensitive query strings

---

# Blue Team Perspective

A proper Referrer Policy reduces accidental information disclosure.

---

# Checking Security Headers

Security headers can be inspected in several ways.

---

## Browser Developer Tools

Open:

```text
F12
```

Navigate:

```text
Network
    ↓
Request
    ↓
Response Headers
```

---

## Curl

Command:

```bash
curl -I https://example.com
```

---

### Command Breakdown

```bash
curl
```

Retrieves content from URLs.

```bash
-I
```

Requests headers only.

---

### Example Output

```http
HTTP/1.1 200 OK
Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=31536000
```

---

## Burp Suite

Navigate:

```text
Proxy
    ↓
HTTP History
    ↓
Response
```

Security headers appear within the response headers section.

---

# Common Security Header Misconfigurations

## Missing CSP

Risk:

```text
Higher XSS Impact
```

---

## Missing HSTS

Risk:

```text
HTTPS Downgrade
SSL Stripping
```

---

## Missing nosniff

Risk:

```text
MIME Type Confusion
```

---

## Missing Referrer Policy

Risk:

```text
Information Leakage
```

---

# Red Team Summary

Security headers provide useful reconnaissance information.

Questions attackers often ask:

```text
Which security headers exist?

Which are missing?

Are they configured correctly?

Can they be bypassed?
```

Security headers rarely eliminate vulnerabilities entirely, but they often increase exploitation difficulty.

---

# Blue Team Summary

Security headers are one of the easiest security improvements organizations can implement.

Benefits include:

- Reduced XSS risk
- Better HTTPS enforcement
- Reduced information disclosure
- Improved browser security

They should be part of every web application's baseline security configuration.

---

# Detection Opportunities

Security teams can monitor:

- Missing security headers
- Weak CSP configurations
- Unexpected redirects
- Insecure cookies
- Unencrypted traffic

Common tools:

- Burp Suite
- OWASP ZAP
- SecurityHeaders.io
- Nessus
- Nmap NSE Scripts

---

# Key Takeaways

- Security headers provide browser-side security controls.
- CSP helps mitigate Cross-Site Scripting (XSS).
- HSTS forces HTTPS communication.
- X-Content-Type-Options prevents MIME sniffing.
- Referrer-Policy controls referrer information disclosure.
- Security headers improve security but do not replace secure coding practices.
- Properly configured headers significantly reduce web application attack surface.

---

# Skills Gained

After completing this room, I gained foundational knowledge in:

### Web Fundamentals

- Web Application Architecture
- Front End Components
- Back End Components

### URL Analysis

- Scheme Analysis
- Domain Analysis
- Port Identification
- Query Parameter Analysis

### HTTP Fundamentals

- HTTP Requests
- HTTP Responses
- HTTP Methods
- HTTP Status Codes

### Header Analysis

- Request Headers
- Response Headers
- Security Headers

### Web Security Foundations

- Attack Surface Identification
- Security Header Analysis
- Basic Reconnaissance
- HTTP Traffic Analysis

---

# Future Learning Path

This room serves as a prerequisite for:

1. Burp Suite Basics
2. OWASP Top 10
3. Authentication Fundamentals
4. Session Management
5. IDOR
6. Cross-Site Scripting (XSS)
7. SQL Injection
8. API Security
9. JWT Security
10. Advanced Web Application Pentesting

The concepts learned here form the foundation for nearly every web security topic encountered later in a cybersecurity learning journey.

---

# Final Thoughts

Web Application Basics may appear simple compared to exploitation-focused rooms, but it provides some of the most important knowledge for aspiring web application pentesters.

Most web vulnerabilities ultimately involve manipulating:

```text
URLs
Requests
Headers
Parameters
Responses
```

Understanding these concepts transforms web application testing from guessing and memorization into a logical process of analyzing communication between clients and servers.

Before moving into topics such as Burp Suite, SQL Injection, XSS, or API Security, it is essential to be comfortable reading and understanding HTTP traffic.

This room provides that foundation.