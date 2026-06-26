# Gobuster: The Basics

> **Room:** Gobuster: The Basics  
> **Platform:** TryHackMe  
> **Difficulty:** Easy  
> **Category:** Web Enumeration / Reconnaissance  
> **Tools Used:** Gobuster, curl, DNS, HTTP, Linux Terminal

---

# Executive Summary

Enumeration is one of the most important phases of a penetration test. Before exploiting a target, security professionals first need to discover what resources are available. Hidden directories, forgotten files, internal websites, and subdomains often become the initial entry point for an attack.

In this room, we learned how to use **Gobuster**, one of the most popular brute-force enumeration tools in penetration testing. Gobuster can discover:

- Hidden web directories
- Hidden files
- DNS subdomains
- Virtual Hosts (VHosts)

Unlike vulnerability scanners, Gobuster does not identify security vulnerabilities directly. Instead, it helps expand the attack surface by revealing resources that may later contain vulnerabilities.

Throughout this room, we explored three primary Gobuster modes:

- **Directory Enumeration (`dir`)**
- **DNS Subdomain Enumeration (`dns`)**
- **Virtual Host Enumeration (`vhost`)**

Besides learning the tool itself, this room also provided valuable lessons about troubleshooting real-world enumeration issues, including DNS configuration problems, timeout-related false negatives, and duplicate entries inside commonly used wordlists.

---

# Learning Objectives

After completing this room, you should be able to:

- Understand the purpose of enumeration during penetration testing.
- Explain the difference between brute force and enumeration.
- Use Gobuster to enumerate directories and files.
- Enumerate DNS subdomains.
- Enumerate Virtual Hosts.
- Understand the difference between DNS enumeration and Virtual Host enumeration.
- Select appropriate wordlists for different enumeration tasks.
- Interpret Gobuster output correctly.
- Troubleshoot common enumeration issues.

---

# Prerequisites

Before starting this room, you should already understand:

- Basic Linux commands
- Basic networking concepts
- DNS fundamentals
- HTTP requests and responses
- Web application basics

If you have completed previous rooms such as:

- Linux Fundamentals
- Networking Fundamentals
- Web Application Basics

then this room builds naturally on those concepts.

---

# What is Gobuster?

Gobuster is an **open-source offensive security tool** written in **Go (Golang)** that performs brute-force enumeration using customizable wordlists.

Instead of guessing vulnerabilities, Gobuster guesses resource names.

For every word inside a wordlist, Gobuster constructs a request and checks whether that resource exists.

For example, suppose a website contains the following hidden directory:

```
https://example.com/admin
```

A normal visitor may never discover it.

However, if the wordlist contains:

```
admin
```

Gobuster automatically generates:

```
https://example.com/admin
```

If the server responds positively, Gobuster reports it as a discovered resource.

This simple idea makes Gobuster one of the most effective reconnaissance tools available.

---

# Why Does Gobuster Exist?

Modern websites often contain resources that are not linked anywhere publicly.

Examples include:

```
/admin
/login
/uploads
/backup
/api
/dev
/test
```

Developers may assume that if nobody knows these URLs exist, they are effectively hidden.

This is called **security through obscurity**, and it is not considered a reliable security practice.

Gobuster solves this problem by automatically testing thousands of possible names.

Instead of manually trying:

```
/admin
/login
/images
/uploads
```

Gobuster can test tens of thousands of possibilities within minutes.

---

# Enumeration vs Brute Force

These two terms are often confused, but they describe different concepts.

## Enumeration

Enumeration is the process of **discovering existing resources**.

The objective is information gathering.

Examples include:

- Finding hidden directories
- Discovering subdomains
- Identifying virtual hosts
- Enumerating usernames
- Listing SMB shares

Think of enumeration as creating a map of the target.

---

## Brute Force

Brute force is the technique used to discover something by trying every possible option.

For Gobuster:

```
Wordlist

↓

admin

↓

login

↓

images

↓

backup

↓

uploads
```

Every word becomes a request.

Eventually one matches an existing resource.

Therefore:

- **Enumeration** is the goal.
- **Brute Force** is the technique.

Gobuster combines both concepts.

---

# Where Gobuster Fits During a Penetration Test

Gobuster is usually used after identifying a live web server but before searching for vulnerabilities.

A simplified penetration testing workflow looks like this:

```
Reconnaissance

↓

Target Identification

↓

Enumeration (Gobuster)

↓

Attack Surface Discovery

↓

Manual Analysis

↓

Vulnerability Discovery

↓

Exploitation

↓

Privilege Escalation

↓

Post Exploitation
```

Notice that Gobuster belongs to the **Reconnaissance / Enumeration** phase.

Its purpose is not exploitation.

Its purpose is discovering potential attack surfaces.

---

# How Gobuster Works

Internally, Gobuster performs a simple but powerful process.

```
             Wordlist

                │

                ▼

          admin

          login

          images

          uploads

                │

                ▼

      Construct Request

                │

                ▼

      Send HTTP/DNS Request

                │

                ▼

      Receive Response

                │

                ▼

      Analyze Result

                │

                ▼

     Display Valid Resources
```

Every mode inside Gobuster follows this same general workflow.

The only difference is **what type of request** is sent.

---

# Gobuster Modes

Gobuster supports multiple operating modes.

The most commonly used are:

| Mode | Purpose |
|-------|----------|
| `dir` | Enumerate web directories and files |
| `dns` | Enumerate DNS subdomains |
| `vhost` | Enumerate virtual hosts |
| `fuzz` | Perform generic HTTP fuzzing |
| `s3` | Enumerate AWS S3 buckets |
| `gcs` | Enumerate Google Cloud Storage buckets |
| `tftp` | Enumerate TFTP servers |

This room focuses on the first three modes.

---

# Gobuster Command Structure

The general syntax looks like this:

```bash
gobuster <mode> [options]
```

Example:

```bash
gobuster dir \
-u http://example.com \
-w wordlist.txt
```

The command consists of three main components:

1. The mode (`dir`, `dns`, or `vhost`)
2. The target
3. The wordlist

Everything else simply customizes the scan.

---

# Commonly Used Flags

Some flags are shared across multiple Gobuster modes.

| Flag | Description |
|------|-------------|
| `-w` | Specify the wordlist |
| `-t` | Number of concurrent threads |
| `-o` | Save output to a file |
| `--delay` | Delay between requests |
| `--debug` | Enable debugging output |
| `--timeout` | Maximum wait time for responses |

Understanding these options allows you to balance scan speed, reliability, and stealth depending on the engagement.

---

# Red Team Perspective

Gobuster is rarely the final objective.

Instead, it discovers resources that may later become attack vectors.

For example:

```
Gobuster

↓

/backup

↓

backup.zip

↓

Database Credentials

↓

Admin Login

↓

Remote Code Execution
```

Many successful penetration tests begin with something as simple as a forgotten directory.

---

# Blue Team Perspective

Defenders should understand Gobuster because attackers use it frequently.

Organizations should:

- Remove unused directories.
- Remove backup files from web roots.
- Restrict access to administrative panels.
- Monitor abnormal enumeration traffic.
- Rate-limit repeated requests.
- Deploy WAF rules where appropriate.

If Gobuster discovers sensitive resources, attackers are unlikely to be the first to find them.

---

# Detection Opportunities

Gobuster generates recognizable traffic patterns.

Indicators include:

- Hundreds or thousands of HTTP requests within a short period.
- Sequential requests targeting different URLs.
- Numerous DNS lookups for different hostnames.
- Repeated changes to the HTTP `Host` header (during Virtual Host enumeration).

Security monitoring tools, WAFs, IDS, and SIEM platforms can often detect these behaviors.

---

# Key Takeaways

- Gobuster is an enumeration tool, not an exploitation tool.
- It discovers hidden resources using brute-force techniques.
- Enumeration significantly expands the attack surface.
- Gobuster supports multiple modes for different discovery tasks.
- Understanding how Gobuster works is more valuable than simply memorizing commands.

In the next section, we will explore **Directory Enumeration (`dir` mode)**, where we use Gobuster to discover hidden web directories and files, interpret HTTP status codes, and identify interesting resources inside a web application.

# Directory Enumeration (`dir` Mode)

Directory enumeration is one of the first activities performed during a web application penetration test. Before attempting to exploit vulnerabilities, a penetration tester first needs to understand **what resources are available** on the target.

A website may contain hidden directories, backup files, administrative panels, development environments, or APIs that are not linked anywhere on the homepage. Although these resources are not publicly visible, they can often be discovered through brute-force enumeration.

Gobuster's `dir` mode automates this process.

---

# Understanding Web Directories

Most web servers organize their content into directories and files.

For example, a typical website may look like this:

```text
/var/www/html
│
├── index.html
├── login.php
├── images/
├── css/
├── js/
├── uploads/
└── admin/
```

Visitors normally access these resources using URLs.

```
https://example.com/

↓

https://example.com/images/

↓

https://example.com/admin/

↓

https://example.com/login.php
```

However, not every directory is linked from the homepage.

For example:

```
https://example.com/backup/

https://example.com/dev/

https://example.com/private/
```

may still exist even though users cannot navigate to them through the website.

Finding these hidden resources is exactly what directory enumeration aims to accomplish.

---

# Why Directory Enumeration Matters

Imagine a company website with the following structure:

```text
/

├── index.html

├── about.html

├── contact.html

└── admin/
```

The homepage only links to:

- Home
- About
- Contact

An attacker browsing normally would never know that `/admin` exists.

Gobuster tests thousands of possible directory names automatically.

If one exists, Gobuster immediately reports it.

This dramatically increases the visible attack surface.

---

# Real-World Examples

Directory enumeration often discovers resources such as:

```text
/admin
/login
/dashboard
/api
/uploads
/images
/backup
/dev
/test
/private
```

Each of these directories could contain valuable information.

Examples include:

- Administrative panels
- Source code
- Configuration files
- Backup archives
- Sensitive documents
- File upload functionality
- API endpoints

Finding them is often the first step toward exploitation.

---

# How Gobuster `dir` Mode Works

Internally, Gobuster follows a simple workflow.

Suppose the target is:

```
http://example.thm
```

and the wordlist contains:

```text
admin
images
uploads
backup
```

Gobuster generates the following requests:

```text
GET /admin

GET /images

GET /uploads

GET /backup
```

Each request is sent to:

```
http://example.thm
```

Resulting in:

```text
http://example.thm/admin

http://example.thm/images

http://example.thm/uploads

http://example.thm/backup
```

Gobuster then analyzes each HTTP response.

---

# HTTP Status Codes

One advantage of Gobuster is that it immediately displays the HTTP status code for discovered resources.

Understanding these status codes is essential.

| Status Code | Meaning |
|-------------|----------|
| 200 | Resource exists and is accessible |
| 301 | Permanently redirected |
| 302 | Temporarily redirected |
| 403 | Resource exists but access is forbidden |
| 404 | Resource does not exist |

For example:

```text
/admin      Status: 403
```

This is still valuable.

Although access is denied, we now know the directory exists.

A `403 Forbidden` response is often more interesting than a `404 Not Found`.

---

# Basic Syntax

The basic command structure is:

```bash
gobuster dir -u <URL> -w <WORDLIST>
```

Example:

```bash
gobuster dir \
-u http://example.thm \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Let's break down each component.

---

## `dir`

Specifies that Gobuster should perform **directory and file enumeration**.

---

## `-u`

Specifies the target URL.

Example:

```bash
-u http://example.thm
```

Gobuster uses this as the base URL for every request.

---

## `-w`

Specifies the wordlist.

Example:

```bash
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Each word becomes part of a new URL.

For example:

```
admin

↓

http://example.thm/admin
```

---

# Important Optional Flags

While `-u` and `-w` are required, several optional flags greatly improve directory enumeration.

---

## `-x`

Searches for specific file extensions.

Example:

```bash
-x php,js
```

Gobuster will search for:

```text
admin.php

config.php

login.php

script.js
```

instead of only directories.

This is especially useful when targeting PHP or JavaScript applications.

---

## `-r`

Follows HTTP redirects.

Example:

```bash
-r
```

Without this option:

```
/login

↓

301 Redirect
```

Gobuster stops.

With `-r`, Gobuster follows the redirect automatically.

---

## `-k`

Skips TLS certificate validation.

Useful when testing:

- Self-signed certificates
- Internal web applications
- CTF environments
- Lab machines

Long flag:

```bash
--no-tls-validation
```

---

## `-t`

Controls the number of concurrent threads.

Example:

```bash
-t 50
```

Higher values:

- Faster scanning
- More CPU usage
- Higher network traffic

Lower values:

- Slower scanning
- Less resource usage
- Lower chance of detection

---

## `--timeout`

Specifies how long Gobuster waits for a response.

Example:

```bash
--timeout 10s
```

Increasing the timeout is useful when:

- The server is slow.
- The network is unstable.
- DNS responses are delayed.

We discovered during this room that timeout values can significantly affect enumeration results.

---

# Practical Example

The room instructs us to enumerate the directories of:

```text
www.offensivetools.thm
```

using:

```bash
gobuster dir \
-u http://offensivetools.thm \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Gobuster begins testing every directory name contained in the wordlist.

Eventually, one directory stands out:

```text
/secret
```

Finding a directory like this is exactly why enumeration is so valuable.

---

# Recursive Enumeration

One important limitation of Gobuster is that it **does not enumerate recursively**.

Suppose Gobuster discovers:

```text
/secret
```

It will **not** automatically continue scanning inside it.

Instead, we must manually start another scan.

Example:

```bash
gobuster dir \
-u http://offensivetools.thm/secret \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
-x js
```

This tells Gobuster to enumerate resources inside the `/secret` directory while also searching for JavaScript files.

---

# Discovering `flag.js`

The second enumeration reveals:

```text
/flag.js
```

This is an excellent example of why the `-x` flag is useful.

Without searching for `.js` files, this resource would not have been discovered.

---

# Reading JavaScript Files

Finding a file is only the first step.

To inspect its contents, we can simply request it using `curl`.

Example:

```bash
curl http://offensivetools.thm/secret/flag.js
```

Alternatively, we can open it directly in a browser.

Since JavaScript files are plain text, they can often reveal:

- API endpoints
- Hidden routes
- Secrets
- API keys
- Debugging code
- Feature flags
- Comments left by developers

In this room, the JavaScript file contained the challenge flag.

---

# Common Mistakes

New users often encounter the following issues.

## Forgetting the Protocol

Incorrect:

```text
example.thm
```

Correct:

```text
http://example.thm
```

Gobuster requires the protocol.

---

## Forgetting File Extensions

Searching only for directories may miss:

```text
config.php

login.php

admin.js
```

Using:

```bash
-x php,js
```

helps discover these files.

---

## Assuming Enumeration is Recursive

Gobuster only scans the directory you specify.

If `/admin` is discovered, you must manually scan:

```text
http://example.thm/admin
```

to discover resources inside it.

---

# Red Team Perspective

Directory enumeration is one of the most valuable reconnaissance techniques.

A single forgotten directory can lead to complete compromise.

Example attack chain:

```text
Gobuster

↓

/backup

↓

backup.zip

↓

Database Credentials

↓

Admin Login

↓

Remote Code Execution
```

Many real-world breaches begin with exposed directories or forgotten backup files.

---

# Blue Team Perspective

Defenders should regularly audit publicly accessible directories.

Recommendations include:

- Remove unused directories.
- Delete backup files from the web root.
- Restrict administrative panels.
- Disable directory listing.
- Monitor repeated requests for non-existent resources.

Even if a directory is not linked anywhere on the website, attackers can still discover it through enumeration.

---

# Detection Opportunities

Directory enumeration produces recognizable traffic patterns.

Indicators include:

- Hundreds of requests within a short period.
- Sequential requests for common directory names.
- Requests targeting administrative paths.
- Large numbers of `404 Not Found` responses.

These behaviors are commonly detected by:

- Web Application Firewalls (WAFs)
- Intrusion Detection Systems (IDS)
- Reverse proxies
- SIEM platforms

---

# Skills Gained

After completing this section, you should be able to:

- Explain the purpose of directory enumeration.
- Understand how Gobuster constructs requests.
- Interpret HTTP status codes.
- Use Gobuster's `dir` mode effectively.
- Search for specific file extensions.
- Perform recursive enumeration manually.
- Retrieve discovered files using tools such as `curl`.
- Recognize how hidden directories contribute to a larger attack surface.

In the next section, we will move from HTTP-based enumeration to **DNS Subdomain Enumeration**, where Gobuster discovers additional hosts by querying DNS records instead of web directories.

# DNS Subdomain Enumeration (`dns` Mode)

After discovering directories and files within a website, the next logical question is:

> **Are there other websites belonging to the same organization?**

Many organizations operate far more than a single website. They often host multiple services under different subdomains, each serving a different purpose.

Gobuster's `dns` mode allows us to discover these hidden subdomains through brute-force DNS enumeration.

---

# What is a Subdomain?

A subdomain is an additional hostname that belongs to a parent domain.

For example:

```text
example.com
```

may have several subdomains:

```text
www.example.com

blog.example.com

mail.example.com

api.example.com

vpn.example.com
```

Visually:

```text
                example.com
                     │
     ┌───────────────┼───────────────┐
     │               │               │
     ▼               ▼               ▼

www.example.com   blog.example.com   api.example.com
                                         │
                                         ▼

                               dev.api.example.com
```

Each subdomain can host a completely different application or service.

---

# Why Are Subdomains Important?

Organizations frequently separate services using subdomains.

Examples include:

| Subdomain | Purpose |
|-----------|----------|
| `www.example.com` | Public website |
| `mail.example.com` | Email server |
| `vpn.example.com` | VPN gateway |
| `api.example.com` | REST API |
| `dev.example.com` | Development environment |
| `staging.example.com` | Testing environment |
| `admin.example.com` | Administrative portal |

Not every subdomain receives the same level of maintenance.

A production website may be fully patched while a forgotten development server still contains critical vulnerabilities.

Because of this, discovering subdomains significantly expands the attack surface.

---

# DNS Enumeration vs Directory Enumeration

It is important to understand that DNS enumeration is fundamentally different from directory enumeration.

Directory enumeration asks:

```
Does this path exist?
```

Example:

```text
http://example.com/admin
```

DNS enumeration asks:

```
Does this hostname exist?
```

Example:

```text
admin.example.com
```

The two techniques operate at different layers.

| Directory Enumeration | DNS Enumeration |
|------------------------|-----------------|
| HTTP | DNS |
| Searches URLs | Searches hostnames |
| Finds files & folders | Finds subdomains |

---

# How DNS Works

Before understanding Gobuster's DNS mode, let's briefly review what DNS does.

Suppose a browser wants to visit:

```text
www.example.com
```

The browser first asks a DNS server:

```
What IP address belongs to www.example.com?
```

The DNS server responds:

```text
www.example.com

↓

10.10.10.5
```

Only then can the browser connect to the web server.

Without DNS, users would have to remember IP addresses instead of domain names.

---

# How Gobuster `dns` Mode Works

Gobuster performs this same DNS lookup repeatedly using every word from a wordlist.

Suppose our target is:

```text
example.thm
```

and our wordlist contains:

```text
www

mail

admin

vpn

blog
```

Gobuster constructs:

```text
www.example.thm

mail.example.thm

admin.example.thm

vpn.example.thm

blog.example.thm
```

Each hostname is sent as a DNS query.

The workflow looks like this:

```text
Wordlist

↓

www

mail

vpn

blog

↓

Construct Hostname

↓

www.example.thm

↓

DNS Query

↓

DNS Server

↓

Exists?

↓

Display Result
```

Unlike `dir` mode, no HTTP requests are sent.

Only DNS queries are performed.

---

# Basic Syntax

The general syntax is:

```bash
gobuster dns -d <DOMAIN> -w <WORDLIST>
```

Example:

```bash
gobuster dns \
-d example.thm \
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

---

# Command Breakdown

## `dns`

Selects DNS enumeration mode.

---

## `-d`

Specifies the target domain.

Example:

```bash
-d example.thm
```

Gobuster appends every wordlist entry to this domain.

For example:

```
mail

↓

mail.example.thm
```

---

## `-w`

Specifies the wordlist.

Example:

```bash
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

This wordlist contains thousands of commonly used subdomain names.

Examples include:

```text
www

mail

api

vpn

shop

blog

admin
```

---

# Useful Optional Flags

## `-i`

Displays the IP address associated with each discovered subdomain.

Example:

```bash
-i
```

Output:

```text
Found:

blog.example.thm

10.10.10.5
```

---

## `-r`

Specifies a custom DNS resolver.

Example:

```bash
-r 8.8.8.8
```

Useful when the default DNS server cannot resolve the target.

---

## `-t`

Controls the number of concurrent threads.

Higher values:

- Faster scanning
- More DNS traffic

Lower values:

- Slower
- More stable

---

## `--timeout`

Defines how long Gobuster waits for DNS responses.

Example:

```bash
--timeout 10s
```

Increasing the timeout may improve reliability on slow networks.

As we discovered during this room, this setting can have a significant impact on scan results.

---

# Practical Exercise

The room asks us to enumerate:

```text
offensivetools.thm
```

using:

```bash
gobuster dns \
-d offensivetools.thm \
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Initially, our scan produced inconsistent results.

Sometimes Gobuster reported:

```text
store

www

primary
```

Other scans returned only:

```text
primary
```

At first glance, this looked like a problem with Gobuster.

However, further investigation revealed a much more interesting explanation.

---

# Investigation: Timeout-Related False Negatives

The default timeout used by Gobuster was:

```text
Timeout: 1s
```

During repeated scans, discovered subdomains changed unexpectedly.

For example:

Scan 1:

```text
store

www

primary
```

Scan 2:

```text
www

primary
```

Scan 3:

```text
primary
```

This behavior should **not** occur during normal DNS enumeration.

The most likely explanation was that the internal DNS server was responding too slowly, causing Gobuster to give up before receiving some responses.

To test this hypothesis, we increased the timeout:

```bash
gobuster dns \
-d offensivetools.thm \
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt \
--timeout 10s \
-i
```

This immediately revealed an additional hostname:

```text
forum.offensivetools.thm
```

The lesson here is important:

> Enumeration results can be affected by network latency and timeout settings.

A missing result does not always mean the resource does not exist.

---

# Investigation: Duplicate Wordlist Entries

During enumeration, Gobuster reported both:

```text
www.offensivetools.thm

WWW.offensivetools.thm
```

Initially, this looked like two separate subdomains.

However, DNS is **case-insensitive**.

The following hostnames are identical:

```text
www.example.com

WWW.example.com

WwW.Example.com
```

To verify this behavior, we inspected the wordlist:

```bash
grep -in "^www$" /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

grep -in "^WWW$" /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Output:

```text
1:www

1176:WWW
```

This confirmed that the wordlist itself contained duplicate entries differing only by letter case.

Gobuster simply queried both entries independently.

After removing duplicate capitalization, the valid unique subdomains were:

```text
www

store

forum

primary
```

A total of **four unique subdomains**, matching the expected answer for the room.

---

# Lessons Learned

This investigation highlighted several important concepts that extend beyond this room.

### Enumeration Is Not Always Deterministic

Network conditions can influence scan results.

Always verify unexpected findings.

---

### Timeout Values Matter

Short timeout values may introduce false negatives.

Increasing the timeout can improve scan reliability.

---

### Never Trust a Single Scan

Professional penetration testers frequently repeat scans or use multiple tools to validate results.

Examples include:

- Gobuster
- dnsrecon
- Amass
- Subfinder
- Assetfinder

Cross-validation reduces the chance of missing valuable assets.

---

### Understand Your Wordlists

Enumeration quality depends heavily on the quality of the chosen wordlist.

Duplicate entries, outdated lists, or incomplete datasets can all affect results.

Understanding the tools you use is just as important as running them.

---

# Red Team Perspective

Subdomain enumeration is one of the most valuable reconnaissance techniques.

Forgotten services often become entry points during real-world engagements.

Examples include:

```text
dev.company.com

staging.company.com

old.company.com

backup.company.com
```

Development or staging systems frequently receive less security attention than production websites.

---

# Blue Team Perspective

Organizations should:

- Maintain an inventory of all public subdomains.
- Remove unused DNS records.
- Secure development environments.
- Apply the same security controls across all subdomains.
- Regularly audit exposed assets.

Every forgotten subdomain represents potential attack surface.

---

# Detection Opportunities

DNS enumeration generates many DNS queries within a short period.

Indicators include:

- Rapid requests for numerous hostnames.
- Queries for uncommon subdomain names.
- Sequential DNS lookups following common wordlists.

DNS logs, SIEM platforms, and intrusion detection systems can often detect this behavior.

---

# Skills Gained

After completing this section, you should be able to:

- Explain what a DNS subdomain is.
- Describe how Gobuster performs DNS enumeration.
- Understand the purpose of the `-d` and `-w` flags.
- Use optional flags such as `-i`, `-r`, and `--timeout`.
- Interpret DNS enumeration results correctly.
- Recognize the impact of timeout settings on scan accuracy.
- Understand why repeated scans and result validation are important during reconnaissance.

In the next section, we will explore **Virtual Host Enumeration (`vhost` mode)** and learn how it differs from DNS enumeration despite producing similar-looking hostnames.

# Virtual Host Enumeration (`vhost` Mode)

After learning how to enumerate directories and DNS subdomains, the final Gobuster mode covered in this room is **Virtual Host Enumeration**.

At first glance, virtual hosts appear very similar to subdomains because they often use hostnames such as:

```text
blog.example.com

shop.example.com

admin.example.com
```

However, despite looking identical, **Virtual Hosts and DNS Subdomains are two completely different technologies**.

Understanding this distinction is essential for web application penetration testing.

---

# What is a Virtual Host?

A Virtual Host (VHost) is a feature provided by web servers such as:

- Apache
- Nginx
- IIS

It allows multiple websites to run on the **same IP address**.

For example, suppose a server has the following IP:

```text
10.10.10.5
```

Instead of hosting only one website, it hosts several:

```text
Company Website

Blog

Shop

Internal Portal
```

Visually:

```text
              10.10.10.5

                    │

      ┌─────────────┼─────────────┐

      ▼             ▼             ▼

   Blog Site     Shop Site    Admin Portal
```

Every website shares the same IP address.

The web server decides which website to return based on the **HTTP Host header**.

---

# Understanding the HTTP Host Header

Whenever a browser requests a webpage, it includes a `Host` header.

For example:

```http
GET / HTTP/1.1
Host: blog.example.com
```

Another request might be:

```http
GET / HTTP/1.1
Host: shop.example.com
```

Notice something important.

The IP address remains exactly the same.

Only the value of:

```http
Host:
```

changes.

The web server reads this header and serves the correct website.

---

# How Virtual Hosts Work

Consider the following Apache configuration.

```apache
<VirtualHost *:80>

ServerName blog.example.com

DocumentRoot /var/www/blog

</VirtualHost>



<VirtualHost *:80>

ServerName shop.example.com

DocumentRoot /var/www/shop

</VirtualHost>
```

Incoming request:

```http
Host: blog.example.com
```

↓

Apache serves:

```text
/var/www/blog
```

Incoming request:

```http
Host: shop.example.com
```

↓

Apache serves:

```text
/var/www/shop
```

Although both websites share the same IP address, Apache returns different content depending on the Host header.

---

# DNS vs Virtual Hosts

This is one of the most important concepts in this room.

DNS and Virtual Hosts solve different problems.

DNS answers:

> **What IP address belongs to this hostname?**

Example:

```text
blog.example.com

↓

10.10.10.5
```

Virtual Hosting answers:

> **Which website should be served for this Host header?**

Example:

```http
Host: blog.example.com
```

↓

```text
Serve:

/var/www/blog
```

---

# DNS Enumeration vs Virtual Host Enumeration

Although the results may look similar, Gobuster performs completely different actions.

## DNS Mode

Gobuster sends DNS queries.

```
Wordlist

↓

blog

↓

blog.example.com

↓

DNS Query

↓

DNS Server

↓

Exists?
```

---

## VHost Mode

Gobuster sends HTTP requests.

```
Wordlist

↓

blog

↓

Host:

blog.example.com

↓

HTTP Request

↓

Web Server

↓

Valid Website?
```

Notice that **no DNS lookup is required** when performing Virtual Host enumeration.

Instead, Gobuster changes the HTTP Host header.

---

# Why Virtual Host Enumeration Matters

Imagine a company hosts several websites.

DNS only exposes:

```text
www.company.com
```

However, Apache is configured for:

```text
www.company.com

admin.company.com

git.company.com

internal.company.com
```

If those hostnames are not publicly registered in DNS, traditional DNS enumeration will never discover them.

However, if the web server accepts:

```http
Host: admin.company.com
```

Gobuster's `vhost` mode can still discover that website.

This makes Virtual Host enumeration an extremely valuable reconnaissance technique.

---

# Basic Syntax

The general syntax is:

```bash
gobuster vhost -u <URL> -w <WORDLIST>
```

Example:

```bash
gobuster vhost \
-u http://10.10.10.5 \
-w wordlist.txt
```

In most real-world scenarios, additional flags are required.

---

# Understanding the Complete Command

During this room, we used:

```bash
gobuster vhost \
-u "http://10.49.142.30" \
--domain offensivetools.thm \
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt \
--append-domain \
--exclude-length 250-320 \
--timeout 10s
```

Let's break down every option.

---

## `vhost`

Selects Virtual Host enumeration mode.

---

## `-u`

Specifies the target web server.

Example:

```bash
-u http://10.49.142.30
```

Notice that we use the **IP address**, not the hostname.

The IP tells Gobuster where to send the HTTP request.

---

## `--domain`

Specifies the base domain.

Example:

```bash
--domain offensivetools.thm
```

Gobuster combines this domain with every wordlist entry.

Example:

```
forum

↓

forum.offensivetools.thm
```

---

## `--append-domain`

Without this option:

```text
Host:

forum
```

which is invalid.

With:

```bash
--append-domain
```

Gobuster sends:

```http
Host: forum.offensivetools.thm
```

This flag is required in this lab because the target infrastructure does not automatically append the domain.

---

## `--exclude-length`

One of the most useful options.

Sometimes every invalid Virtual Host returns:

```text
404

Size: 280 bytes
```

Gobuster would incorrectly report thousands of false positives.

Filtering by response length removes these repetitive error pages.

Example:

```bash
--exclude-length 250-320
```

This ignores responses whose body length falls within that range.

---

## `--timeout`

Just like DNS enumeration, increasing the timeout improved scan stability in this lab.

Example:

```bash
--timeout 10s
```

---

# Practical Exercise

Using the command above, Gobuster discovered:

```text
forum.offensivetools.thm

store.offensivetools.thm

secret.offensivetools.thm

www.offensivetools.thm

WWW.offensivetools.thm
```

At first glance, this appears to be five Virtual Hosts.

However, we already learned from the DNS enumeration exercise that:

```text
www

WWW
```

represent the same hostname because DNS and HTTP hostnames are case-insensitive.

Therefore, the unique Virtual Hosts were:

```text
forum

store

secret

www
```

A total of **four unique Virtual Hosts**.

---

# Comparing DNS and Virtual Host Results

One interesting observation from this room was that the DNS and Virtual Host results were **not identical**.

DNS Enumeration discovered:

```text
primary

store

forum

www
```

Virtual Host Enumeration discovered:

```text
secret

store

forum

www
```

Notice the difference.

`primary` appeared only during DNS enumeration.

`secret` appeared only during Virtual Host enumeration.

This demonstrates an important lesson:

> **A DNS record does not guarantee a matching Virtual Host, and a Virtual Host does not necessarily require a public DNS record.**

For this reason, professional penetration testers commonly perform **both DNS and Virtual Host enumeration** during reconnaissance.

---

# Real-World Use Cases

Virtual Host enumeration frequently discovers:

- Development websites
- Internal portals
- Administrative dashboards
- Legacy applications
- Backup websites
- Customer portals

Examples:

```text
git.company.local

admin.company.local

staging.company.local

legacy.company.local
```

Many of these systems are forgotten or receive less security maintenance than the primary website.

---

# Red Team Perspective

Virtual Host enumeration is an excellent technique for expanding the attack surface.

Finding an undisclosed administrative portal or development website can dramatically change the direction of an engagement.

A forgotten Virtual Host may expose:

- Default credentials
- Debug pages
- Vulnerable applications
- Source code
- Sensitive APIs

---

# Blue Team Perspective

Organizations should:

- Remove unused Virtual Hosts.
- Ensure all Virtual Hosts receive security updates.
- Restrict access to internal services.
- Audit web server configurations regularly.
- Avoid exposing development environments to public networks.

Security reviews should include every configured Virtual Host, not only the public website.

---

# Detection Opportunities

Unlike DNS enumeration, Virtual Host enumeration produces HTTP traffic.

Common indicators include:

- Repeated requests to the same IP address.
- Constantly changing `Host` headers.
- Large numbers of unusual hostnames.
- Requests for hostnames that do not normally exist.

These behaviors are often visible in:

- Web server logs
- Reverse proxies
- Web Application Firewalls (WAFs)
- SIEM platforms

---

# Skills Gained

After completing this section, you should be able to:

- Explain what a Virtual Host is.
- Describe how the HTTP Host header works.
- Distinguish between DNS Subdomains and Virtual Hosts.
- Use Gobuster's `vhost` mode effectively.
- Understand the purpose of `--append-domain` and `--exclude-length`.
- Interpret Virtual Host enumeration results correctly.
- Recognize why both DNS and Virtual Host enumeration are important during reconnaissance.

In the next section, we will document the troubleshooting process encountered during this room, including DNS configuration issues, timeout-related false negatives, duplicate wordlist entries, and practical lessons learned while investigating the lab environment.

# Troubleshooting & Lessons Learned

One of the most valuable aspects of this room was not the intended exercises themselves, but the unexpected issues encountered while completing them.

Rather than simply following the lab instructions, we investigated each problem, identified its root cause, verified our assumptions, and applied practical workarounds.

These troubleshooting exercises closely resemble real-world penetration testing, where tools and environments do not always behave exactly as documentation suggests.

---

# Troubleshooting #1 — Gobuster Could Not Resolve the Target Hostname

## Problem

The very first directory enumeration failed.

Command:

```bash
gobuster dir \
-u http://offensivetools.thm \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Output:

```text
Error:
lookup offensivetools.thm

NXDOMAIN
```

Gobuster was unable to resolve the hostname.

---

## Initial Hypothesis

Possible causes included:

- Gobuster misconfiguration
- Incorrect command syntax
- DNS server failure
- AttackBox DNS configuration
- Lab machine failure

Instead of guessing, each possibility was investigated individually.

---

## Investigation

The first step was testing DNS resolution directly.

```bash
nslookup offensivetools.thm
```

Result:

```text
NXDOMAIN
```

Next, we queried the lab machine's DNS server directly.

```bash
nslookup offensivetools.thm <LAB_MACHINE_IP>
```

Result:

```text
Name:

offensivetools.thm
```

This immediately revealed something important.

The DNS server itself was functioning correctly.

The problem existed somewhere between the AttackBox resolver and the lab DNS server.

---

## Root Cause

The AttackBox DNS resolver was not forwarding requests correctly.

The room documentation instructed us to modify:

```text
/etc/resolv-dnsmasq
```

However, the current AttackBox image was configured differently.

The active configuration used:

```text
/etc/resolv-dnsmasq.conf
```

Even after updating the configuration, the resolver continued behaving inconsistently.

This strongly suggested an AttackBox configuration issue rather than a Gobuster problem.

---

## Workaround

To continue the lab, a temporary hosts entry was added.

```bash
sudo nano /etc/hosts
```

Example:

```text
<LAB_MACHINE_IP> offensivetools.thm
```

After manually mapping the hostname, Gobuster immediately functioned as expected.

---

## Lessons Learned

Never assume the offensive tool is responsible for an error.

Always verify each layer independently:

```
Application

↓

DNS Resolution

↓

Resolver

↓

Network

↓

Target
```

A structured troubleshooting process is significantly more reliable than random trial and error.

---

# Troubleshooting #2 — Outdated Room Documentation

While investigating the DNS issue, another inconsistency appeared.

The room instructed users to edit:

```text
/etc/resolv-dnsmasq
```

However, the current AttackBox actually used:

```text
/etc/resolv-dnsmasq.conf
```

The active `dnsmasq` configuration confirmed this:

```text
resolv-file=/etc/resolv-dnsmasq.conf
```

This indicates that the room documentation was written for an older AttackBox image.

---

## Lessons Learned

Training platforms evolve over time.

Software versions, operating systems, and container images change.

Documentation may not always reflect the latest environment.

Always verify system configurations instead of assuming the documentation is current.

---

# Troubleshooting #3 — DNS Enumeration Produced Different Results

The next issue appeared during DNS enumeration.

Running the same Gobuster command multiple times produced different results.

Example:

Run 1:

```text
store

www

primary
```

Run 2:

```text
www

primary
```

Run 3:

```text
primary
```

This behavior should never occur on a stable DNS server.

---

## Initial Hypothesis

Possible explanations included:

- Gobuster bug
- DNS caching
- Packet loss
- Threading issues
- Timeout configuration

---

## Investigation

Examining the Gobuster output revealed:

```text
Timeout:

1 second
```

The default timeout was extremely short.

To test whether slow DNS responses were being discarded, the timeout was increased.

```bash
gobuster dns \
-d offensivetools.thm \
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt \
--timeout 10s \
-i
```

Result:

```text
forum.offensivetools.thm

store.offensivetools.thm

www.offensivetools.thm

primary.offensivetools.thm
```

An additional subdomain appeared immediately.

---

## Root Cause

The most likely explanation was that the internal DNS server occasionally responded slower than the default one-second timeout.

Gobuster terminated some requests before responses arrived.

This produced **false negatives**.

---

## Lessons Learned

Timeout values directly influence enumeration accuracy.

Fast scans are not always better.

Increasing timeout values may significantly improve discovery results on slow or unstable networks.

---

# Understanding False Negatives

A **false negative** occurs when a tool reports that something does not exist even though it actually does.

Example:

Reality:

```text
forum.offensivetools.thm

exists
```

Gobuster Output:

```text
Not Found
```

The subdomain exists.

The tool simply failed to discover it because of environmental conditions.

Professional penetration testers always verify unexpected results.

---

# Troubleshooting #4 — Duplicate Results (`www` vs `WWW`)

After increasing the timeout, Gobuster reported:

```text
www.offensivetools.thm

WWW.offensivetools.thm
```

Initially, this appeared to be two different Virtual Hosts.

However, DNS hostnames are **case-insensitive**.

The following hostnames are identical:

```text
www.example.com

WWW.example.com

WwW.Example.com
```

---

## Investigation

The wordlist was examined directly.

```bash
grep -in "^www$" \
/usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

grep -in "^WWW$" \
/usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Output:

```text
1:www

1176:WWW
```

The duplicate entries originated from the wordlist itself.

Gobuster simply queried both entries independently.

---

## Root Cause

The duplicate output was not caused by Gobuster.

It was caused by duplicate wordlist entries that differed only by capitalization.

---

## Lessons Learned

Enumeration quality depends heavily on the quality of the selected wordlist.

Understanding how your tools process input is just as important as knowing how to execute them.

---

# DNS Enumeration vs Virtual Host Enumeration

Another interesting observation was that DNS enumeration and Virtual Host enumeration produced different results.

DNS enumeration discovered:

```text
primary

store

forum

www
```

Virtual Host enumeration discovered:

```text
secret

store

forum

www
```

This demonstrates an important concept.

DNS records and Virtual Hosts are related but independent.

A DNS record does not guarantee a corresponding Virtual Host.

Likewise, a Virtual Host may exist even without a publicly accessible DNS record.

Professional web reconnaissance should therefore include both techniques.

---

# General Lessons Learned

This room reinforced several important penetration testing principles.

## Never Trust a Single Scan

Unexpected results should always be verified.

Repeat the scan.

Change parameters.

Use another tool.

Validate manually.

---

## Understand the Tool

Knowing the command syntax is only the beginning.

Understanding:

- how Gobuster constructs requests,
- how DNS resolution works,
- how HTTP Host headers work,
- and how timeout values affect results

allows you to interpret output correctly instead of blindly trusting it.

---

## Documentation Is Not Always Correct

Real-world systems evolve.

Training platforms evolve.

Software versions change.

Documentation can become outdated.

Always validate assumptions against the actual environment.

---

## Enumeration Is an Investigative Process

Successful reconnaissance is rarely about running a single command.

Instead, it is a continuous cycle:

```text
Scan

↓

Observe

↓

Question

↓

Verify

↓

Investigate

↓

Repeat
```

This mindset is one of the most valuable skills a penetration tester can develop.

---

# Key Takeaways

This room provided much more than an introduction to Gobuster.

It demonstrated that:

- Reconnaissance tools are only as reliable as the environment they operate in.
- Small configuration changes can significantly affect scan results.
- Timeout values influence discovery accuracy.
- Wordlists are not always perfect.
- DNS and Virtual Hosts should be enumerated separately.
- Effective penetration testing requires curiosity, verification, and analytical thinking rather than simply executing tools.

The troubleshooting process itself became one of the most educational parts of the room, reinforcing that understanding **why** something happens is far more valuable than simply obtaining the correct answer.

# Conclusion

This room introduced the fundamentals of **Gobuster**, one of the most widely used reconnaissance tools in web application penetration testing.

Although Gobuster is often perceived as a simple directory brute-forcing tool, this room demonstrated that it is capable of much more. Using different operating modes, Gobuster can enumerate:

- Hidden web directories
- Hidden files
- DNS subdomains
- Virtual Hosts

Each mode targets a different part of the attack surface, allowing penetration testers to build a more complete picture of the target before attempting exploitation.

---

# What We Learned

Throughout this room, we explored three major Gobuster modes.

## Directory Enumeration (`dir`)

Using HTTP requests, Gobuster can discover hidden directories and files that are not linked from the website.

This helps identify:

- Administrative portals
- Development directories
- Backup files
- APIs
- Hidden JavaScript files

Directory enumeration is often one of the earliest steps during web application reconnaissance.

---

## DNS Enumeration (`dns`)

Using DNS queries, Gobuster brute-forces subdomains belonging to a target domain.

This technique expands the attack surface by identifying additional services that may not be visible from the primary website.

Examples include:

- Development servers
- VPN gateways
- APIs
- Internal portals
- Legacy applications

---

## Virtual Host Enumeration (`vhost`)

Unlike DNS enumeration, Virtual Host enumeration works by modifying the HTTP `Host` header.

This allows Gobuster to discover websites hosted on the same server, even when corresponding DNS records are unavailable.

Understanding the difference between DNS records and Virtual Hosts is an essential concept for web penetration testing.

---

# Important Technical Concepts

This room reinforced several fundamental cybersecurity concepts.

- Enumeration expands the attack surface.
- Brute force is the technique used to perform enumeration.
- DNS and HTTP operate at different layers.
- Virtual Hosts are configured by the web server, not by DNS.
- HTTP response codes provide valuable reconnaissance information.
- Wordlists directly influence enumeration quality.
- Timeout values can affect scan accuracy.

These concepts extend far beyond Gobuster and are applicable to many other reconnaissance tools.

---

# Skills Gained

After completing this room, I can now:

- Explain the purpose of enumeration during penetration testing.
- Distinguish between enumeration and brute-force techniques.
- Use Gobuster's `dir`, `dns`, and `vhost` modes effectively.
- Understand the purpose of common Gobuster flags.
- Perform recursive directory enumeration manually.
- Enumerate DNS subdomains.
- Enumerate Virtual Hosts using the HTTP `Host` header.
- Interpret HTTP status codes during reconnaissance.
- Understand the relationship between DNS, HTTP, and Virtual Hosts.
- Troubleshoot enumeration issues caused by DNS configuration or timeout settings.

---

# Real-World Lessons Learned

One of the most valuable aspects of this room was the troubleshooting process.

Several unexpected issues were encountered, including:

- An outdated AttackBox DNS configuration.
- Documentation that no longer matched the latest AttackBox image.
- Timeout-related false negatives during DNS enumeration.
- Duplicate entries inside the DNS wordlist (`www` vs `WWW`).

Rather than treating these as obstacles, they became opportunities to investigate how Gobuster and the underlying technologies actually work.

This reinforced an important lesson:

> **Tools should never be trusted blindly. Their output should always be interpreted, validated, and verified.**

Understanding *why* a tool behaves a certain way is far more valuable than simply memorizing its commands.

---

# Red Team Perspective

Gobuster is one of the first tools commonly used after identifying a live web server.

Its primary objective is to discover additional attack surface.

Examples include:

- Hidden directories
- Administrative interfaces
- Backup files
- Development environments
- Internal websites
- Forgotten Virtual Hosts

Many successful penetration tests begin with assets that developers never intended to expose publicly.

Enumeration often provides the roadmap for later exploitation.

---

# Blue Team Perspective

From a defensive standpoint, organizations should assume that attackers will perform enumeration.

Recommended defensive measures include:

- Remove unused directories and files.
- Delete backups from web roots.
- Disable unnecessary Virtual Hosts.
- Remove obsolete DNS records.
- Restrict administrative interfaces.
- Monitor abnormal HTTP and DNS traffic.
- Apply rate limiting where appropriate.
- Regularly inventory exposed web assets.

Reducing unnecessary attack surface is one of the most effective security improvements an organization can make.

---

# Detection Opportunities

Gobuster generates recognizable patterns that defenders can monitor.

Examples include:

### Directory Enumeration

- Large numbers of sequential HTTP requests
- Frequent `404 Not Found` responses
- Requests targeting common administrative paths

### DNS Enumeration

- Numerous DNS queries within a short period
- Requests for common subdomain names
- Brute-force hostname patterns

### Virtual Host Enumeration

- Repeated requests to the same IP
- Constantly changing HTTP `Host` headers
- Unusual hostname requests

These behaviors are commonly detected by:

- Web Application Firewalls (WAF)
- Intrusion Detection Systems (IDS)
- DNS monitoring
- Reverse proxies
- SIEM platforms

---

# Common Mistakes

New users frequently make the following mistakes:

- Forgetting to specify the correct protocol (`http://` or `https://`).
- Using an inappropriate wordlist.
- Assuming Gobuster performs recursive directory enumeration.
- Confusing DNS subdomains with Virtual Hosts.
- Ignoring timeout settings.
- Trusting a single scan without verification.
- Misinterpreting duplicate results caused by wordlists.

Recognizing these pitfalls early helps build more reliable reconnaissance workflows.

---

# Next Learning Path

Gobuster provides a strong foundation for web reconnaissance.

The next logical topics to study include:

- FFUF (advanced web fuzzing)
- Content Discovery
- OWASP Top 10
- Authentication Bypass
- File Inclusion
- IDOR
- Burp Suite Intruder
- Web Application Enumeration methodologies

These topics build directly on the enumeration techniques introduced in this room.

---

# Final Thoughts

At first glance, Gobuster appears to be a straightforward brute-force tool.

However, this room demonstrated that effective enumeration requires much more than running commands.

A penetration tester must understand:

- how DNS resolution works,
- how HTTP requests are constructed,
- how Virtual Hosts operate,
- how web servers respond,
- and how environmental factors such as timeout values or resolver configuration can influence results.

Perhaps the biggest lesson from this room is that penetration testing is an investigative process.

The goal is not simply to obtain output from a tool.

The goal is to understand the target, question unexpected results, verify assumptions, and continuously expand the attack surface through careful analysis.

That investigative mindset is ultimately far more valuable than memorizing any individual command.

---

# References

- Gobuster Official Repository: https://github.com/OJ/gobuster
- OWASP Web Security Testing Guide (WSTG): https://owasp.org/www-project-web-security-testing-guide/
- SecLists Wordlists: https://github.com/danielmiessler/SecLists
- TryHackMe – Gobuster: The Basics

---

# Tags

`TryHackMe` `Gobuster` `Web Enumeration` `Directory Enumeration`
`DNS Enumeration` `Virtual Hosts` `Reconnaissance`
`Cybersecurity` `Penetration Testing` `Red Team`
`HTTP` `DNS` `Web Application Security`