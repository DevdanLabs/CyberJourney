# Burp Suite: The Basics

## Executive Summary

Burp Suite is one of the most important tools in modern web application security testing. It acts as an intercepting proxy between a browser and a web server, allowing security professionals to capture, inspect, modify, and replay HTTP/HTTPS traffic.

This room introduces the fundamental concepts of Burp Suite Community Edition, including its architecture, major modules, navigation system, dashboard, configuration settings, and the core functionality of the Burp Proxy.

Throughout the room, practical demonstrations show how Burp Suite can be used to intercept requests, analyze web traffic, bypass client-side controls, manage testing scope, and perform basic web application security assessments.

Understanding Burp Suite is a critical milestone for anyone pursuing web application penetration testing, bug bounty hunting, application security, or red team operations.

---

# Learning Objectives

By completing this room, I learned how to:

- Understand the purpose of Burp Suite in web application security testing
- Differentiate between Community, Professional, and Enterprise editions
- Navigate the Burp Suite interface efficiently
- Configure Burp Suite settings and project options
- Use the Burp Proxy to intercept HTTP/HTTPS traffic
- Capture and review requests using HTTP History
- Configure browser proxy settings
- Understand HTTPS interception using Burp CA certificates
- Build a Site Map during web application reconnaissance
- Configure target scope to reduce noise
- Manipulate requests before they reach the server
- Understand how client-side validation can be bypassed
- Observe a basic Reflected Cross-Site Scripting (XSS) attack

---

# Prerequisites

Before starting this room, it is helpful to understand:

- Basic web application architecture
- Client-server communication
- HTTP and HTTPS protocols
- Web browsers and developer tools
- Fundamental networking concepts

Recommended prior knowledge:

- Networking Fundamentals
- HTTP Basics
- Web Application Basics
- JavaScript Essentials

---

# What is Burp Suite?

## Definition

Burp Suite is a Java-based web application security testing framework developed by PortSwigger.

It is considered the industry standard toolkit for:

- Web Application Penetration Testing
- API Security Testing
- Bug Bounty Hunting
- Application Security Assessments
- Security Research

The primary purpose of Burp Suite is to allow testers to inspect and manipulate communication between a client and a web server.

---

## Why Does Burp Suite Exist?

Modern web applications communicate through HTTP and HTTPS requests.

A typical user only sees:

```text
Buttons
Forms
Pages
Images
Menus
```

However, behind every action is an HTTP request.

For example:

```text
User clicks "Login"
```

may generate:

```http
POST /login HTTP/1.1

username=admin
password=password123
```

Browsers normally hide these details.

Burp Suite exposes them.

---

## High-Level Architecture

Without Burp:

```text
Browser
    │
    ▼
Web Server
```

With Burp:

```text
Browser
    │
    ▼
Burp Proxy
    │
    ▼
Web Server
```

Every request passes through Burp before reaching the target application.

This allows the tester to:

- View requests
- Modify requests
- Drop requests
- Replay requests
- Analyze responses

---

# Burp Suite Editions

Burp Suite is available in three primary editions.

---

## Burp Suite Community Edition

The free edition used throughout this room.

Features include:

- Proxy
- Repeater
- Intruder (rate limited)
- Decoder
- Comparer
- Sequencer
- Extensions

Advantages:

- Free
- Excellent learning platform
- Suitable for many manual testing activities

Limitations:

- No automated scanner
- No project saving
- Limited Intruder performance
- Reduced extension support

---

## Burp Suite Professional

The version most commonly used by professional penetration testers.

Additional features include:

- Automated vulnerability scanner
- Faster Intruder attacks
- Project saving
- Reporting functionality
- Burp Collaborator
- Enhanced extension support
- API integration

Professional is widely considered one of the most powerful web testing tools available.

---

## Burp Suite Enterprise

Enterprise is designed for continuous automated scanning.

Instead of manual testing:

```text
Tester
↓
Burp
↓
Target
```

Enterprise performs:

```text
Scheduled Scan
↓
Target
↓
Report Generation
```

It is commonly used in:

- Application Security Programs
- DevSecOps Pipelines
- Continuous Security Monitoring

---

# Core Features of Burp Suite Community

Although Community Edition lacks some advanced capabilities, it still provides a powerful collection of tools.

---

# Proxy

## Purpose

Intercept and inspect HTTP/HTTPS traffic.

## Function

Acts as a man-in-the-middle between browser and server.

Example:

```http
GET /profile HTTP/1.1
Host: example.com
```

The request can be viewed and modified before reaching the server.

---

## Pentesting Relevance

Used for:

- Reconnaissance
- Enumeration
- Parameter discovery
- Authentication testing
- Session analysis
- Input manipulation

---

# Repeater

## Purpose

Resend requests repeatedly with modifications.

Workflow:

```text
Capture Request
        ↓
Modify Request
        ↓
Send
        ↓
Review Response
        ↓
Repeat
```

---

## Common Uses

- SQL Injection testing
- XSS testing
- IDOR testing
- Authorization testing
- API testing

---

## Example

Original request:

```http
GET /user?id=5
```

Modified requests:

```http
GET /user?id=1
```

```http
GET /user?id=2
```

```http
GET /user?id=3
```

---

# Intruder

## Purpose

Automate large numbers of requests.

Commonly used for:

- Brute forcing
- Fuzzing
- Parameter discovery
- Endpoint discovery

---

## Example

Testing passwords:

```text
admin:password
admin:123456
admin:qwerty
admin:admin123
```

Instead of sending manually, Intruder automates the process.

---

## Community Edition Limitation

Intruder is intentionally rate-limited in Community Edition.

---

# Decoder

## Purpose

Encode and decode data.

Supported formats include:

- URL Encoding
- Base64
- Hexadecimal
- HTML Encoding

---

## Example

Original:

```text
admin@gmail.com
```

Encoded:

```text
admin%40gmail.com
```

---

## Security Relevance

Useful when analyzing:

- Cookies
- Tokens
- Parameters
- Encoded payloads

---

# Comparer

## Purpose

Compare two pieces of data.

Comparison modes:

- Word comparison
- Byte comparison

---

## Use Cases

- Comparing user vs admin responses
- Comparing API responses
- Analyzing authentication differences
- Authorization testing

---

# Sequencer

## Purpose

Evaluate randomness in generated values.

Examples:

- Session cookies
- CSRF tokens
- Password reset tokens
- API keys

---

## Why It Matters

Poor randomness can allow:

- Session prediction
- Account takeover
- Authentication bypass

---

## Example

Weak sessions:

```text
session=1001
session=1002
session=1003
```

Predictable tokens can become a critical security vulnerability.

---

# Extensions and BApp Store

One of Burp Suite's greatest strengths is extensibility.

Extensions can be developed using:

- Java
- Python (Jython)
- Ruby (JRuby)

---

## BApp Store

The BApp Store functions similarly to:

- Chrome Web Store
- VS Code Marketplace

It allows users to install community-created extensions.

---

## Popular Example: Logger++

Logger++ enhances:

- Request logging
- Traffic analysis
- Search capabilities
- Traffic filtering

Many professionals consider Logger++ an essential extension.

---

# Key Concepts Learned

This section introduced the foundational concepts behind Burp Suite:

- Burp Suite acts as an intercepting proxy
- Web application testing revolves around manipulating HTTP traffic
- Burp Community provides powerful manual testing capabilities
- Proxy and Repeater form the foundation of most web penetration testing workflows
- Intruder provides automation and fuzzing capabilities
- Decoder, Comparer, and Sequencer support specialized testing scenarios
- Burp's extensibility makes it adaptable to many security testing requirements

---

# Pentester Notes

Understanding Burp Suite is one of the most important skills for:

- Web Application Pentesters
- Bug Bounty Hunters
- Application Security Engineers
- Red Team Operators

Virtually every advanced web attack demonstrated in future rooms—including:

- Cross-Site Scripting (XSS)
- SQL Injection (SQLi)
- Insecure Direct Object References (IDOR)
- Authentication Bypass
- Session Attacks
- API Exploitation

will begin with traffic captured through Burp Suite.

# Dashboard, Navigation, and Configuration

After understanding what Burp Suite is and becoming familiar with its core tools, the next step is learning how to navigate and configure the framework efficiently.

Although many beginners immediately jump into the Proxy tab, understanding the Dashboard, navigation system, and configuration options provides a much stronger foundation and makes future testing significantly easier.

---

# The Burp Suite Dashboard

## Purpose of the Dashboard

The Dashboard serves as Burp Suite's central monitoring interface.

Think of it as the command center of the application.

While most active testing takes place inside modules such as Proxy, Repeater, and Intruder, the Dashboard provides visibility into:

- Background tasks
- Burp activity
- Scanner findings (Professional Edition)
- Vulnerability information
- Application events

---

## Dashboard Overview

When Burp Suite launches, the Dashboard is usually the first screen displayed.

Historically, the Dashboard contained four primary sections:

```text
+----------------+----------------+
| Tasks          | Issue Activity |
+----------------+----------------+
| Event Log      | Advisory       |
+----------------+----------------+
```

Modern versions of Burp Suite have evolved, but the underlying concepts remain the same.

---

# Tasks

## Purpose

The Tasks section manages automated background activities.

Examples include:

```text
Live Passive Crawl
Passive Scanning
Logging Activities
Site Mapping
```

---

## Live Passive Crawl

The default task in Burp Community Edition is:

```text
Live Passive Crawl
```

This task automatically records pages and endpoints visited through the proxy.

For example:

```text
/login
/profile
/dashboard
/settings
```

As these pages are visited, Burp gradually builds a Site Map.

---

## Why This Matters

During reconnaissance, a penetration tester often needs to understand:

- Application structure
- Available endpoints
- Hidden functionality
- API routes

Passive crawling assists this process automatically.

---

# Event Log

## Purpose

The Event Log records internal Burp Suite activities.

Examples include:

```text
Proxy started
Project loaded
Certificate loaded
Extension loaded
```

---

## Real-World Usage

The Event Log becomes extremely useful when troubleshooting issues such as:

- Browser connection failures
- Proxy configuration errors
- Certificate problems
- Extension failures

---

## Analogy

In Linux terms, the Event Log is similar to:

```bash
journalctl
```

or

```bash
/var/log/
```

It provides insight into what Burp itself is doing.

---

# Issue Activity (Professional Edition)

## Purpose

Issue Activity displays vulnerabilities discovered by Burp's automated scanner.

Examples:

```text
Cross-Site Scripting
SQL Injection
XXE
SSRF
CSRF
```

---

## Severity Classification

Findings are usually categorized as:

```text
High
Medium
Low
Information
```

---

## Community Edition

Burp Community does not include the automated vulnerability scanner.

As a result, this section is largely unused.

---

# Advisory

## Purpose

Advisory provides detailed information about identified vulnerabilities.

If Issue Activity reports:

```text
Cross-Site Scripting
```

Advisory explains:

- What the vulnerability is
- Why it matters
- Potential impact
- References
- Suggested remediation

---

## Example Content

A vulnerability advisory often contains:

```text
Description
Impact
References
Remediation
```

This information can later be included in penetration testing reports.

---

# Using Built-In Help

Throughout Burp Suite, small question mark icons are available.

```text
?
```

Clicking these icons opens documentation related to the current section.

---

## Why This Is Valuable

Many testers immediately search online when they encounter an unfamiliar feature.

However, Burp's internal documentation is:

- Comprehensive
- Up-to-date
- Context-sensitive

Learning to use the built-in documentation is a professional habit worth developing.

---

# Navigation in Burp Suite

As testing becomes more complex, efficient navigation becomes increasingly important.

A typical web application assessment may involve constantly moving between:

```text
Target
Proxy
Repeater
Intruder
Decoder
Comparer
```

Burp's navigation system is designed to make this workflow efficient.

---

# Module Navigation

The primary navigation bar contains Burp's major modules.

Examples include:

```text
Dashboard
Target
Proxy
Intruder
Repeater
Sequencer
Decoder
Comparer
Extensions
```

Each module serves a specific purpose.

---

## Mental Model

Think of Burp as a toolbox:

```text
Burp Suite
│
├── Target
├── Proxy
├── Repeater
├── Intruder
├── Decoder
└── Comparer
```

Each module is a different tool within the framework.

---

# Sub-Tabs

Many modules contain additional sub-tabs.

For example:

```text
Proxy
├── Intercept
├── HTTP History
├── WebSockets History
└── Options
```

---

## Example: Proxy Module

### Intercept

Used to:

- Capture requests
- Modify requests
- Forward requests
- Drop requests

---

### HTTP History

Stores all traffic passing through Burp.

Contains:

- Requests
- Responses
- Status codes
- Hosts
- MIME types

---

### WebSockets History

Captures and stores WebSocket traffic.

Useful when testing:

- Chat applications
- Real-time applications
- WebSocket APIs

---

# Detaching Tabs

Burp allows modules to be detached into separate windows.

For example:

```text
Proxy Window
```

and

```text
Repeater Window
```

can be displayed simultaneously.

---

## Benefits

This improves workflow when:

- Comparing requests
- Analyzing responses
- Performing repetitive testing

Many experienced testers use detached windows during larger engagements.

---

# Keyboard Shortcuts

Burp provides built-in shortcuts for rapid navigation.

| Shortcut | Module |
|-----------|-----------|
| Ctrl + Shift + D | Dashboard |
| Ctrl + Shift + T | Target |
| Ctrl + Shift + P | Proxy |
| Ctrl + Shift + I | Intruder |
| Ctrl + Shift + R | Repeater |

---

## Most Commonly Used Shortcuts

In practice, the most frequently used are:

```text
Ctrl + Shift + P
```

Proxy

```text
Ctrl + Shift + R
```

Repeater

```text
Ctrl + Shift + T
```

Target

These modules form the core workflow of most web penetration tests.

---

# Burp Suite Settings

Burp provides extensive configuration options.

Understanding these settings is essential as projects become more complex.

---

# User Settings vs Project Settings

One of the most important concepts introduced in this room is the distinction between:

```text
User Settings
```

and

```text
Project Settings
```

---

# User Settings

## Definition

User Settings are global settings.

They affect the entire Burp Suite installation.

Examples:

```text
Theme
Font Size
Hotkeys
Update Settings
```

---

## Characteristics

```text
Persistent
Global
Shared Across Projects
```

Changes remain in effect every time Burp Suite is launched.

---

# Project Settings

## Definition

Project Settings apply only to the current project.

Examples:

```text
Target Scope
Proxy Rules
Session Handling
TLS Certificates
```

---

## Characteristics

```text
Project Specific
Temporary
Session Dependent
```

---

# Community Edition Limitation

Burp Suite Community Edition does not support project saving.

As a result:

```text
Project Settings
```

are lost when Burp closes.

User Settings remain intact.

---

# Settings Interface

The Settings menu contains several useful sections.

---

## Search

Allows settings to be located quickly.

Examples:

```text
TLS
Cookie
Proxy
Scope
```

Searching is often much faster than manually browsing categories.

---

## Type Filter

Allows filtering by:

```text
User
Project
All
```

This makes troubleshooting significantly easier.

---

## Categories

Settings are organized into categories such as:

```text
Suite
Tools
Network
User Interface
Extensions
Sessions
```

---

# Cookie Jar

One important configuration area introduced in the room is:

```text
Sessions
```

which contains:

```text
Cookie Jar
```

---

## What Is a Cookie Jar?

A Cookie Jar stores and manages cookies during testing.

Example:

```http
Set-Cookie: session=abc123
```

Burp can automatically manage and reuse these cookies.

---

## Security Relevance

Cookie management is essential for:

- Authenticated testing
- Session analysis
- CSRF testing
- Multi-step workflows

---

# Updates Category

The:

```text
Suite
```

category contains:

```text
Updates
```

settings.

These control:

- Update checks
- Automatic updates
- Version notifications

---

# Hotkeys Category

The:

```text
Hotkeys
```

section allows customization of Burp shortcuts.

This is particularly useful for advanced users who spend long periods inside the framework.

---

# Client TLS Certificates

Burp supports Client-Side TLS certificates.

These certificates can be configured globally or overridden on a per-project basis.

This capability is important when testing:

- Banking applications
- Enterprise portals
- VPN gateways
- Government systems

that require mutual TLS authentication.

---

# Key Concepts Learned

This section introduced several foundational concepts:

- The Dashboard serves as Burp's monitoring center
- Navigation is organized into modules and sub-tabs
- Keyboard shortcuts significantly improve workflow efficiency
- User Settings are global and persistent
- Project Settings are temporary and project-specific
- Cookie management plays an important role in authenticated testing
- Burp offers extensive customization options for professional workflows

---

# Pentester Notes

One of the biggest mistakes beginners make is assuming:

```text
Proxy = Burp Suite
```

In reality:

```text
Proxy
```

is only one component of a much larger framework.

Understanding navigation, settings, scope management, session handling, and project configuration is what separates basic tool usage from professional proficiency.

Before learning advanced attacks such as SQL Injection, IDOR, Authentication Bypass, or API exploitation, it is important to become comfortable moving through Burp Suite efficiently and understanding how its configuration system works.

# The Burp Proxy: The Heart of Web Application Testing

The Burp Proxy is the most important component of Burp Suite.

While Burp contains many powerful modules, nearly every web application assessment begins with traffic passing through the Proxy.

Understanding how the Proxy works is essential because almost every future web attack—including SQL Injection, Cross-Site Scripting (XSS), Authentication Bypass, IDOR, SSRF, and API exploitation—starts by observing and manipulating HTTP requests.

---

# Understanding the Burp Proxy

## What Is a Proxy?

A proxy acts as an intermediary between a client and a server.

Normally:

```text
Browser
    │
    ▼
Web Server
```

With Burp Suite:

```text
Browser
    │
    ▼
Burp Proxy
    │
    ▼
Web Server
```

Every request passes through Burp before reaching the target.

This allows the tester to:

- View requests
- Modify requests
- Drop requests
- Replay requests
- Analyze responses

---

# Why Is This Important?

Web applications are ultimately controlled through HTTP requests.

A user may see:

```text
Login Button
```

However, the server receives:

```http
POST /login HTTP/1.1

username=admin
password=password123
```

Security testing focuses on understanding and manipulating these requests rather than interacting solely with the user interface.

---

# Intercepting Requests

## What Is Request Interception?

Request interception allows Burp to pause traffic before it reaches the target server.

When interception is enabled:

```text
Browser
    │
    ▼
Burp
 (STOP)
```

The request is held inside Burp.

The server never receives it until the tester explicitly forwards it.

---

## Example

A browser generates:

```http
GET /profile HTTP/1.1
Host: example.com
```

Burp captures the request and displays it in the Intercept tab.

The tester can now:

- Review the request
- Modify parameters
- Change headers
- Forward the request
- Drop the request

---

# Intercept Controls

The Intercept tab provides several key controls.

---

## Forward

Purpose:

Send the captured request to the target server.

Workflow:

```text
Request Captured
        ↓
Forward
        ↓
Server Receives Request
```

---

## Drop

Purpose:

Discard the request entirely.

Workflow:

```text
Request Captured
        ↓
Drop
        ↓
Request Destroyed
```

The server never receives the request.

---

## Intercept Is On

When enabled:

```text
Browser
        ↓
Burp
    (Pause)
```

Every request is intercepted.

---

## Intercept Is Off

When disabled:

```text
Browser
        ↓
Burp
        ↓
Server
```

Requests flow normally.

Traffic is still recorded.

Only the interruption is disabled.

---

# Understanding Traffic Manipulation

One of Burp's most powerful features is the ability to modify traffic before the server receives it.

---

## Example

Original request:

```http
GET /user?id=5 HTTP/1.1
```

Modified request:

```http
GET /user?id=1 HTTP/1.1
```

The browser requested:

```text
User 5
```

The server receives:

```text
User 1
```

because Burp modified the request in transit.

---

## Why This Matters

Many web vulnerabilities involve manipulating:

- IDs
- Tokens
- Cookies
- Headers
- Parameters

The ability to change these values is fundamental to penetration testing.

---

# HTTP History

After understanding interception, the next important feature is HTTP History.

Many experienced testers spend more time in HTTP History than in Intercept.

---

## What Is HTTP History?

HTTP History records all traffic passing through Burp.

Every request and response is stored automatically.

---

## Example Entries

```text
GET /
POST /login
GET /profile
POST /api/user
GET /logout
```

---

## Information Captured

Each entry contains:

- Request
- Response
- Host
- Method
- URL
- Status Code
- MIME Type
- Response Length

---

# Important Concept

## Burp Captures Traffic Even When Intercept Is Off

Many beginners assume:

```text
Intercept Off
=
No Traffic Captured
```

This is incorrect.

Burp continues logging everything.

Example:

```text
Browser
        ↓
Burp
        ↓
Server
```

The traffic still appears in:

```text
Proxy
→ HTTP History
```

---

# Why HTTP History Is So Valuable

A typical workflow:

```text
Browse Website
        ↓
Interesting Request Found
        ↓
Send to Repeater
        ↓
Manual Testing
```

Instead of intercepting every request, testers often browse normally and analyze traffic afterward.

---

# WebSocket History

Modern web applications frequently use WebSockets.

Examples include:

- Chat systems
- Real-time dashboards
- Trading platforms
- Notification systems

---

## What Is a WebSocket?

Unlike HTTP:

```text
Request
↓
Response
↓
Connection Closed
```

WebSockets maintain an ongoing connection:

```text
Browser
⇄
Server
```

allowing real-time communication.

---

## Burp Support

Burp automatically records:

```text
Proxy
→ WebSockets History
```

allowing inspection and analysis of WebSocket messages.

---

# Response Interception

Most beginners focus on requests.

However, Burp can also intercept responses.

---

# Default Behaviour

Normally:

```text
Browser
        ↓
Request
        ↓
Server

Server
        ↓
Response
        ↓
Browser
```

The response passes directly to the browser.

---

# Response Interception Enabled

With response interception:

```text
Browser
        ↓
Request
        ↓
Server

Server
        ↓
Response
        ↓
Burp
     (STOP)
```

The response is paused before reaching the browser.

---

## Why Intercept Responses?

Response interception allows testers to:

- Inspect server responses
- Modify page content
- Analyze server behavior
- Test client-side functionality

---

# Example Use Cases

A tester might:

- Modify HTML
- Remove client-side restrictions
- Alter JavaScript
- Analyze hidden values

before the browser processes the response.

---

# Response Interception Rules

Burp allows flexible control over which responses should be intercepted.

Example rule:

```text
Intercept response if request was intercepted
```

Workflow:

```text
Request Intercepted
        ↓
Forward
        ↓
Response Intercepted
```

This creates a complete request-response analysis workflow.

---

# Match and Replace

One of Burp's most underrated features is Match and Replace.

---

# Purpose

Automatically modify traffic using predefined rules.

No manual editing required.

---

# Example: User-Agent Modification

Original request:

```http
User-Agent: Chrome
```

Automatic replacement:

```http
User-Agent: Firefox
```

Every request is modified automatically.

---

# Example: Cookie Manipulation

Original:

```http
Cookie: role=user
```

Replacement:

```http
Cookie: role=admin
```

Burp performs the modification automatically.

---

# Why Match and Replace Matters

Useful for:

- Automation
- Header manipulation
- Testing workflows
- Simulating different clients
- Large-scale testing

---

# Typical Pentesting Workflow

A real-world assessment often follows this pattern:

```text
Browse Application
        ↓
HTTP History
        ↓
Interesting Request Found
        ↓
Send to Repeater
        ↓
Manual Testing
        ↓
Potential Vulnerability
        ↓
Send to Intruder
        ↓
Automation/Fuzzing
```

Everything starts with the Proxy.

---

# Red Team Perspective

The Burp Proxy is used for:

## Reconnaissance

Identifying:

- Endpoints
- Parameters
- APIs
- Hidden functionality

---

## Enumeration

Discovering:

- Admin panels
- Internal routes
- Debug pages
- API endpoints

---

## Exploitation

Testing for:

- SQL Injection
- XSS
- IDOR
- SSRF
- Authentication flaws
- Authorization flaws

---

## Post-Exploitation Validation

Analyzing:

- Sessions
- Tokens
- Roles
- Privileges

---

# Blue Team Perspective

Application Security teams use Burp Proxy to:

- Validate security controls
- Review HTTP traffic
- Reproduce vulnerabilities
- Verify fixes

---

# Common Beginner Mistakes

## Mistake 1

Assuming Intercept Off means Burp stopped capturing traffic.

Reality:

```text
Traffic is still logged.
```

---

## Mistake 2

Leaving Intercept On permanently.

Result:

```text
Browser constantly freezes.
```

Most professionals keep:

```text
Intercept Off
```

during normal browsing.

---

## Mistake 3

Ignoring HTTP History.

Many important findings originate from historical traffic analysis.

---

# Key Concepts Learned

This section introduced the core functionality of Burp Proxy:

- Burp acts as an intercepting proxy
- Requests can be captured and modified before reaching the server
- Responses can also be intercepted and analyzed
- HTTP History records all traffic automatically
- WebSocket traffic can be inspected
- Match and Replace allows automated traffic modification
- Most web penetration testing workflows begin inside the Proxy module

---

# Pentester Notes

If there is one lesson to remember from this section, it is this:

> Web applications are controlled through HTTP requests, not through their graphical interfaces.

The Burp Proxy gives testers visibility and control over those requests.

This capability forms the foundation for virtually every advanced web application attack covered in later rooms, including:

- SQL Injection
- Cross-Site Scripting
- Authentication Bypass
- IDOR
- SSRF
- API Security Testing

Mastering the Proxy module is one of the most important steps toward becoming proficient in web application penetration testing.

# Browser Integration and HTTPS Proxying

At this stage, we understand how Burp Proxy intercepts and manipulates traffic. The next challenge is getting our browser to communicate through Burp and understanding how Burp is able to inspect HTTPS traffic despite encryption.

This section covers:

- Connecting a browser to Burp Suite
- Browser proxy configuration
- FoxyProxy
- Burp Browser
- HTTPS interception
- TLS certificates
- PortSwigger Certificate Authority (CA)
- Man-in-the-Middle (MITM) concepts

Understanding these topics is critical because nearly every modern web application uses HTTPS.

---

# Connecting Through the Burp Proxy

## The Problem

Burp Suite cannot automatically intercept browser traffic.

The browser must first be configured to send requests through Burp.

Without configuration:

```text
Browser
    │
    ▼
Web Server
```

With configuration:

```text
Browser
    │
    ▼
Burp Proxy
    │
    ▼
Web Server
```

Only after this configuration can Burp inspect and manipulate traffic.

---

# Browser Proxy Settings

A proxy server acts as an intermediary between a client and a destination server.

In Burp's default configuration:

```text
IP Address:
127.0.0.1

Port:
8080
```

---

## Why 127.0.0.1?

```text
127.0.0.1
```

represents:

```text
localhost
```

which always refers to the local machine.

This means:

```text
Browser
        ↓
127.0.0.1:8080
        ↓
Burp Suite
```

Traffic never leaves the machine before reaching Burp.

---

# Using FoxyProxy

One common method for browser integration is FoxyProxy.

---

## What Is FoxyProxy?

FoxyProxy is a browser extension that allows quick switching between proxy configurations.

Instead of manually editing browser proxy settings:

```text
Enable Proxy
Disable Proxy
Enable Proxy
Disable Proxy
```

FoxyProxy provides one-click control.

---

## Typical Burp Configuration

```text
Title:
Burp

IP:
127.0.0.1

Port:
8080
```

---

## Workflow

```text
Firefox
        ↓
FoxyProxy
        ↓
Burp
        ↓
Target Website
```

---

## Advantages

- Quick activation
- Quick deactivation
- Multiple proxy profiles
- Commonly used in bug bounty workflows

---

# Burp Browser

Modern versions of Burp include a dedicated browser.

This is generally the easiest option for beginners.

---

## Why Use Burp Browser?

The browser is preconfigured for Burp.

No manual setup is required.

Workflow:

```text
Burp Suite
        ↓
Open Browser
        ↓
Browse Target
```

Traffic automatically passes through Burp.

---

## Advantages

- Minimal configuration
- Faster setup
- Reduced troubleshooting
- Easier learning experience

---

## Real-World Usage

Many professionals still prefer:

```text
Firefox + FoxyProxy
```

However:

```text
Burp Browser
```

is often ideal for training environments and labs.

---

# Intercepting Browser Traffic

Once the browser is connected:

```text
Intercept On
```

causes requests to pause before reaching the target.

Example:

```text
Browser
        ↓
Burp
     (STOP)
        ↓
Server
```

---

## Observable Behaviour

The browser appears to hang:

```text
Loading...
Loading...
Loading...
```

This occurs because Burp is holding the request.

---

## Forwarding the Request

Clicking:

```text
Forward
```

allows the request to continue.

---

## Common Beginner Mistake

Many new users think:

```text
The website is broken.
```

In reality:

```text
Burp is intercepting the request.
```

---

# HTTPS and the Burp Proxy

Everything works smoothly with HTTP.

However, HTTPS introduces a challenge.

---

# Understanding HTTPS

HTTPS is:

```text
HTTP + TLS
```

TLS provides:

- Encryption
- Integrity
- Authentication

---

## Normal HTTPS Communication

```text
Browser
    ⇄
Web Server
```

The traffic is encrypted.

No intermediary should be able to read it.

---

# The HTTPS Problem

Suppose we browse:

```text
https://google.com
```

The browser expects:

```text
Google Certificate
```

signed by a trusted Certificate Authority.

Examples:

- DigiCert
- GlobalSign
- Let's Encrypt

---

However, when Burp is in the middle:

```text
Browser
    ⇄
Burp
    ⇄
Google
```

The certificate presented to the browser comes from:

```text
PortSwigger
```

instead of Google.

---

## Browser Reaction

The browser sees:

```text
Expected:
Google Certificate

Received:
PortSwigger Certificate
```

and displays:

```text
Certificate Warning
Connection Not Secure
```

because the certificate is not trusted.

---

# The PortSwigger Certificate Authority

To solve this issue, Burp creates its own Certificate Authority.

Known as:

```text
PortSwigger CA
```

---

## Purpose

The PortSwigger CA allows Burp to generate certificates for any website.

Example:

```text
google.com
github.com
example.com
```

Burp dynamically creates certificates on demand.

---

## Browser Trust

By default:

```text
Browser
does NOT trust
PortSwigger CA
```

Therefore:

```text
HTTPS Warning
```

appears.

---

# Installing the Burp CA Certificate

To trust Burp:

1. Visit:

```text
http://burp/cert
```

2. Download:

```text
cacert.der
```

3. Open browser certificate settings

4. Import the certificate

5. Trust the CA

---

After importing:

```text
Browser
Trusts
PortSwigger CA
```

---

# What Happens Internally?

This is one of the most important concepts in web security testing.

---

## Without Burp

```text
Browser
    ⇄
Target
```

One TLS session.

---

## With Burp

```text
Browser
    ⇄
Burp
    ⇄
Target
```

Two separate TLS sessions.

---

### Session 1

```text
Browser
⇄
Burp
```

---

### Session 2

```text
Burp
⇄
Target
```

---

Burp decrypts traffic from the browser and re-encrypts it before sending it to the server.

This allows Burp to inspect HTTPS communication.

---

# Understanding MITM

This process is technically a:

```text
Man-In-The-Middle (MITM)
```

operation.

---

## Normal MITM Attack

An attacker secretly inserts themselves between:

```text
Victim
⇄
Server
```

This is malicious.

---

## Burp MITM

The user intentionally trusts Burp.

```text
Browser
⇄
Burp
⇄
Server
```

This is authorized and used for security testing.

---

# Security Implications

Without installing the Burp CA:

```text
HTTPS traffic
cannot be inspected properly
```

---

With the Burp CA installed:

```text
HTTPS traffic
can be decrypted and analyzed
```

---

# Why This Matters for Pentesters

Most modern applications use:

```text
HTTPS
```

Examples:

- Banking portals
- APIs
- SaaS platforms
- Corporate applications
- E-commerce websites

Without HTTPS interception, many security assessments would be impossible.

---

# Red Team Perspective

HTTPS interception enables:

- Authentication testing
- Session analysis
- Cookie inspection
- API testing
- Authorization testing
- Vulnerability discovery

---

# Blue Team Perspective

Application Security teams use HTTPS interception to:

- Validate security controls
- Review traffic
- Debug applications
- Verify fixes

---

# Common Beginner Mistakes

## Mistake 1

Forgetting to trust the Burp CA.

Result:

```text
Certificate errors everywhere.
```

---

## Mistake 2

Leaving Intercept enabled permanently.

Result:

```text
Browser appears frozen.
```

---

## Mistake 3

Assuming HTTPS prevents Burp from reading traffic.

Reality:

```text
Burp becomes a trusted intermediary.
```

---

# Key Concepts Learned

This section introduced several critical concepts:

- Browsers must be configured to use Burp
- FoxyProxy simplifies proxy management
- Burp Browser provides a preconfigured testing environment
- HTTPS uses TLS encryption
- Burp uses a trusted CA certificate to intercept HTTPS traffic
- HTTPS interception relies on a controlled MITM architecture
- Modern web penetration testing depends heavily on HTTPS inspection

---

# Pentester Notes

One of the most common misconceptions among beginners is:

> HTTPS means nobody can see the traffic.

A more accurate statement is:

> HTTPS prevents untrusted parties from seeing the traffic.

Once a browser explicitly trusts the PortSwigger Certificate Authority, Burp Suite becomes a trusted intermediary capable of decrypting, inspecting, modifying, and re-encrypting HTTPS communications.

This capability forms the foundation of modern web application penetration testing and enables the analysis of authenticated sessions, APIs, tokens, cookies, and countless other attack surfaces hidden behind encrypted traffic.

# Target Tab, Site Mapping, and Scoping

After learning how to intercept and inspect traffic with the Burp Proxy, the next step is understanding how Burp helps map and organize target applications.

The Target tab is one of the most valuable components of Burp Suite during reconnaissance and enumeration.

Rather than simply capturing requests, the Target tab helps testers:

- Build an application map
- Identify endpoints
- Organize discovered content
- Define testing scope
- Reference known vulnerabilities

In real-world web penetration tests, the Target tab often becomes the primary reconnaissance workspace.

---

# Understanding the Target Tab

The Target tab contains three primary sections:

```text
Site Map
Issue Definitions
Scope Settings
```

Each serves a different purpose during a security assessment.

---

# Site Map

## What Is a Site Map?

The Site Map is Burp Suite's representation of the target application structure.

As traffic passes through Burp, every discovered page, endpoint, and resource is automatically recorded.

Example:

```text
example.com
│
├── /
├── /login
├── /profile
├── /products
├── /admin
└── /api
```

The Site Map gradually grows as the tester interacts with the application.

---

# How the Site Map Is Built

Every request passing through Burp contributes to the Site Map.

Example workflow:

```text
Visit Homepage
        ↓
Visit Login Page
        ↓
Visit Profile Page
        ↓
Visit API Endpoint
```

Burp records:

```text
/
/login
/profile
/api
```

without requiring manual documentation.

---

# Why Site Maps Matter

Large web applications often contain:

- Hundreds of pages
- Thousands of endpoints
- Multiple APIs
- Hidden functionality

Manually tracking this information is impractical.

The Site Map solves this problem by creating an organized representation of the application.

---

# Reconnaissance Through Site Mapping

One of the primary goals of reconnaissance is understanding:

```text
What exists?
```

before asking:

```text
Can it be exploited?
```

The Site Map helps answer:

- Which pages exist?
- Which APIs exist?
- Which endpoints are reachable?
- Which resources appear unusual?

---

# Example

Suppose a website exposes:

```text
/
/products
/contact
```

During browsing, Burp also discovers:

```text
/admin
/debug
/backup
```

These endpoints may not appear anywhere in the user interface.

However, they still become visible in the Site Map.

---

# Hidden Endpoint Discovery

One of the most valuable uses of the Site Map is identifying unusual endpoints.

Consider:

```text
/
/about
/contact
/products
```

Everything appears normal.

Then:

```text
/internal-admin
```

appears unexpectedly.

This should immediately raise questions.

---

## Questions a Pentester Should Ask

- Why does this endpoint exist?
- Is it accessible?
- Does it require authentication?
- Is it intended for administrators?
- Was it accidentally exposed?

These observations frequently lead to vulnerability discoveries.

---

# API Enumeration

Modern applications often rely heavily on APIs.

Examples:

```text
/api/user
/api/profile
/api/products
/api/orders
```

Even if these endpoints are invisible in the browser interface, Burp records them automatically.

---

# Why API Mapping Matters

API endpoints often contain:

- Sensitive data
- Business logic
- Authorization checks
- Administrative functionality

Many modern vulnerabilities are discovered within APIs rather than traditional web pages.

---

# Burp Community vs Professional

The Site Map exists in both editions.

However:

## Community Edition

Site Map is built through:

```text
Manual Browsing
```

---

## Professional Edition

Site Map can be expanded through:

```text
Automated Crawling
```

Burp automatically follows links and explores the application.

---

# Site Map Workflow

A common enumeration workflow:

```text
Open Application
        ↓
Browse Every Page
        ↓
Review Site Map
        ↓
Identify Interesting Endpoints
        ↓
Investigate Further
```

---

# Issue Definitions

Although Burp Community does not include automated vulnerability scanning, it still includes a useful knowledge base.

This section is called:

```text
Issue Definitions
```

---

# Purpose

Issue Definitions contains detailed information about vulnerabilities recognized by Burp's scanner.

Examples include:

- SQL Injection
- Cross-Site Scripting
- XML External Entity Injection
- Server-Side Request Forgery
- Cross-Site Request Forgery

---

# Information Provided

Each entry typically contains:

```text
Description
Impact
References
Remediation
```

---

# Why This Is Useful

During manual testing, a tester may discover a vulnerability but need:

- Accurate terminology
- Technical references
- Remediation guidance

Issue Definitions provides this information directly within Burp.

---

# Example Use Cases

A tester identifies:

```text
Reflected XSS
```

Issue Definitions can provide:

- Explanation of the vulnerability
- Security impact
- Relevant references
- Mitigation recommendations

This information is especially useful when writing reports.

---

# Scope Settings

One of the most important concepts introduced in this room is:

```text
Scoping
```

---

# Why Scope Exists

Modern websites rarely consist of a single domain.

A typical application may load resources from:

```text
Target Website
Google Analytics
Cloudflare
Stripe
CDN Providers
Embedded Videos
```

Without scoping, Burp captures everything.

---

# Example Without Scope

HTTP History:

```text
target.com
google.com
youtube.com
gstatic.com
github.com
cloudflare.com
```

Very quickly:

```text
Thousands of Requests
```

become difficult to manage.

---

# Example With Scope

HTTP History:

```text
target.com
```

only.

This dramatically reduces noise.

---

# Defining Scope

Burp allows testers to specify:

```text
Included Targets
```

and

```text
Excluded Targets
```

---

## Example

In Scope:

```text
example.com
```

Out of Scope:

```text
google.com
youtube.com
cloudflare.com
```

---

# Adding Targets to Scope

The easiest method is:

```text
Target
        ↓
Site Map
        ↓
Right Click Target
        ↓
Add To Scope
```

Burp then treats the selected target as the primary testing scope.

---

# Logging and Scope

When Burp asks:

```text
Do you want to stop logging out-of-scope traffic?
```

the answer is usually:

```text
Yes
```

for most penetration testing engagements.

---

# Important Distinction

Many beginners misunderstand scope.

Adding a target to scope:

```text
Does NOT
automatically stop interception.
```

It simply defines:

```text
What Burp considers relevant.
```

Additional proxy settings are required to restrict interception.

---

# Restricting Interception to Scope

To fully ignore irrelevant traffic:

```text
Proxy
        ↓
Proxy Settings
        ↓
Intercept Client Requests
        ↓
URL Is In Target Scope
```

This ensures Burp only intercepts traffic belonging to approved targets.

---

# Benefits of Proper Scoping

Proper scope management provides:

- Cleaner HTTP History
- Faster analysis
- Reduced noise
- Better organization
- Improved reporting

---

# Real-World Pentest Methodology

One of the first actions many professionals perform is:

```text
Add Target To Scope
```

before any testing begins.

Why?

Because:

```text
Noise Hides Findings
```

The cleaner the traffic, the easier it becomes to identify vulnerabilities.

---

# Red Team Perspective

Site Mapping supports:

## Reconnaissance

Discover:

- Pages
- APIs
- Endpoints
- Hidden functionality

---

## Enumeration

Identify:

- Admin panels
- Debug pages
- Internal routes
- Backup files

---

## Attack Surface Analysis

Determine:

- What can be attacked?
- What requires authentication?
- What appears sensitive?

---

# Blue Team Perspective

Application Security teams use Site Maps to:

- Understand application architecture
- Verify exposed functionality
- Review application inventory
- Validate access controls

---

# Common Beginner Mistakes

## Mistake 1

Ignoring the Site Map.

Many important findings are visible there before any exploitation occurs.

---

## Mistake 2

Testing before mapping.

Enumeration should always precede exploitation.

---

## Mistake 3

Failing to define scope.

This often results in:

```text
Thousands of Irrelevant Requests
```

and poor workflow efficiency.

---

# Key Concepts Learned

This section introduced several critical reconnaissance concepts:

- The Site Map automatically records discovered content
- APIs are captured alongside traditional pages
- Unusual endpoints often indicate hidden functionality
- Issue Definitions provide vulnerability references and remediation guidance
- Scope helps focus testing efforts
- Proper scoping dramatically improves workflow efficiency

---

# Pentester Notes

One of the most important lessons in web application security is:

> You cannot attack what you have not discovered.

Before SQL Injection testing, before XSS testing, before authentication testing, a tester must first understand the application.

The Target tab serves as Burp Suite's reconnaissance center.

It helps build a map of the application, identify potential attack surfaces, and organize findings before deeper testing begins.

Many successful penetration tests begin not with exploitation, but with careful observation of the Site Map and the discovery of endpoints that should not be there.

# Example Attack, XSS Demonstration, and Room Conclusion

After learning how Burp Suite captures, analyzes, and manipulates web traffic, the room concludes with a practical demonstration that ties all previous concepts together.

The goal is not simply to perform a basic Cross-Site Scripting (XSS) attack.

Instead, the exercise demonstrates one of the most important principles in web application security:

> Never trust client-side validation.

This lesson forms the foundation of countless web vulnerabilities and highlights why security testing focuses on requests sent to the server rather than restrictions enforced by the browser.

---

# The Scenario

The target application contains a support ticket form:

```text
Contact Email
Message
```

At first glance, the form appears secure.

When attempting to enter:

```html
<script>alert("Successful XSS")</script>
```

into the email field, the browser immediately rejects the input.

---

# What Is Happening?

The application uses:

```text
Client-Side Validation
```

to restrict input.

The browser checks:

```text
Allowed Characters
```

before allowing form submission.

Because:

```html
<
>
"
```

are not valid email characters, the browser prevents the submission.

---

# Common Beginner Assumption

Many beginners conclude:

```text
The attack failed.
```

or

```text
The application is secure.
```

This is incorrect.

The browser only controls the user interface.

The server ultimately decides what data is accepted.

---

# Understanding Client-Side Validation

## Definition

Client-side validation occurs within the browser.

Examples include:

- JavaScript validation
- HTML5 validation
- Input restrictions
- Form validation rules

---

## Example

```javascript
if(emailIsInvalid)
{
    blockSubmission();
}
```

The browser prevents submission.

---

# Advantages

Client-side validation improves:

- User experience
- Performance
- Immediate feedback

---

# Disadvantages

Client-side validation cannot be trusted as a security control because:

```text
The attacker controls the browser.
```

---

# Understanding Server-Side Validation

## Definition

Server-side validation occurs on the server.

Example:

```python
if not valid_email:
    reject_request()
```

The attacker cannot directly control this logic.

---

# Why Server-Side Validation Matters

Even if an attacker:

- Disables JavaScript
- Uses Burp Suite
- Uses curl
- Uses a custom client

the server still performs validation.

This is why server-side validation is the true security boundary.

---

# The Attack Workflow

The room demonstrates how Burp Suite can bypass client-side controls.

---

## Step 1

Submit valid data.

Example:

```text
Email:
pentester@example.thm

Message:
Test Attack
```

---

## Step 2

Enable interception.

```text
Proxy
↓
Intercept On
```

---

## Step 3

Submit the form.

The browser sends:

```http
POST /ticket HTTP/1.1

email=pentester@example.thm
message=Test+Attack
```

Burp captures the request before it reaches the server.

---

## Step 4

Modify the email parameter.

Original:

```text
pentester@example.thm
```

Modified:

```html
<script>alert("Successful XSS")</script>
```

---

## Step 5

URL encode the payload.

Using:

```text
Ctrl + U
```

inside Burp.

---

# Why URL Encoding Is Necessary

Certain characters have special meanings within HTTP requests.

Examples:

```text
<
>
"
&
=
```

These characters should be encoded before transmission.

---

## Example

Original:

```html
<script>alert("XSS")</script>
```

Encoded:

```text
%3Cscript%3Ealert%28%22XSS%22%29%3C%2Fscript%3E
```

---

# What Happens Next?

The request is forwarded:

```text
Forward
```

and sent to the server.

The browser's validation has already been bypassed.

The server now receives the malicious payload.

---

# Reflected Cross-Site Scripting (XSS)

The vulnerability demonstrated in this room is:

```text
Reflected XSS
```

---

# What Is XSS?

Cross-Site Scripting occurs when user-controlled input is executed as client-side code inside another user's browser.

Most commonly:

```text
JavaScript
```

is executed.

---

# Types of XSS

## Reflected XSS

Payload exists only within the request and response cycle.

Example:

```http
GET /search?q=test
```

Response:

```html
You searched for: test
```

---

Malicious request:

```http
GET /search?q=<script>alert(1)</script>
```

Response:

```html
You searched for:
<script>alert(1)</script>
```

The browser executes the script.

---

## Stored XSS

Payload is stored by the application.

Examples:

- Comments
- User profiles
- Messages
- Support tickets

Every user viewing the content may execute the payload.

---

## DOM-Based XSS

The vulnerability exists entirely within client-side JavaScript.

The server may never process the payload.

---

# Why Did the Alert Appear?

The browser received:

```html
<script>
alert("Successful XSS")
</script>
```

The browser interpreted this as executable JavaScript.

The result:

```text
Popup Alert
```

which confirms successful code execution.

---

# Why This Demonstration Matters

The goal is not the alert box itself.

The real lesson is:

```text
Client-side controls can be bypassed.
```

This principle extends far beyond XSS.

---

# Real-World Examples

## Quantity Manipulation

Browser restriction:

```text
Quantity:
1-10
```

Captured request:

```http
quantity=10
```

Modified request:

```http
quantity=1000
```

---

## Role Manipulation

Original:

```http
role=user
```

Modified:

```http
role=admin
```

---

## Price Manipulation

Original:

```http
price=100
```

Modified:

```http
price=1
```

---

The exact same principle applies.

Burp allows modification after browser validation has already occurred.

---

# Core Security Lesson

One of the most important lessons from this room is:

> Browsers are convenience tools, not security boundaries.

Applications must never rely solely on client-side controls.

All security validation must occur on the server.

---

# Burp Suite Workflow Review

Throughout this room, we repeatedly used the following workflow:

```text
Browser
        ↓
Burp Proxy
        ↓
HTTP History
        ↓
Repeater
        ↓
Manual Testing
```

This workflow forms the foundation of modern web application penetration testing.

---

# Skills Gained

By completing this room, I gained practical experience with:

## Burp Suite Fundamentals

- Dashboard navigation
- Module navigation
- Settings management
- User and project settings

---

## Proxy Usage

- Request interception
- Request modification
- Traffic analysis
- HTTP History review

---

## Browser Integration

- Proxy configuration
- Burp Browser usage
- HTTPS interception concepts

---

## Reconnaissance

- Site Map creation
- Endpoint discovery
- Scope management
- Application mapping

---

## Security Testing

- Request manipulation
- Client-side validation bypass
- URL encoding
- Basic XSS testing

---

# Pentester Notes

One of the most important mindset shifts for aspiring web penetration testers is:

> Stop thinking like a user and start thinking like the server.

Users see:

```text
Buttons
Forms
Menus
Pages
```

Servers see:

```http
Requests
Headers
Cookies
Parameters
Tokens
```

Burp Suite gives us visibility into what the server actually receives.

That visibility is what enables vulnerability discovery.

---

# Key Takeaways

- Burp Suite is the industry-standard web application testing framework.
- The Proxy module is the foundation of most web assessments.
- HTTP requests can be intercepted, modified, and replayed.
- HTTPS traffic can be inspected using Burp's trusted CA certificate.
- Site Maps assist with reconnaissance and endpoint discovery.
- Scope management reduces noise and improves workflow efficiency.
- Client-side validation is not a security control.
- Server-side validation is essential.
- Burp Suite enables direct interaction with the application's true attack surface.

---

# Future Learning Path

After completing Burp Suite: The Basics, recommended next topics include:

## Burp Suite Modules

- Repeater
- Intruder
- Decoder
- Comparer
- Sequencer

---

## Web Application Security

- HTTP Requests and Responses
- Authentication Mechanisms
- Session Management
- Cookies
- JWT Tokens

---

## Common Web Vulnerabilities

- Cross-Site Scripting (XSS)
- SQL Injection (SQLi)
- IDOR
- CSRF
- SSRF
- XXE

---

## API Security

- REST APIs
- GraphQL APIs
- API Authentication
- API Authorization

---

# Conclusion

Burp Suite is much more than a proxy.

It is a complete web application security testing framework that provides visibility into the communication between clients and servers.

This room established the foundation required for future web penetration testing activities by introducing the essential concepts of interception, traffic analysis, site mapping, scope management, HTTPS inspection, and request manipulation.

Mastering these fundamentals is a critical step toward becoming proficient in web application security assessment and offensive security operations.