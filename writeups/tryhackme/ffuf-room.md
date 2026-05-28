# FFUF Room Writeup – TryHackMe

## Overview

In this room, I learned the basics of using FFUF (Fuzz Faster U Fool) for web enumeration and fuzzing. The room covered:

* Directory and file fuzzing
* Extension fuzzing
* Filtering and matching responses
* Parameter fuzzing
* Value fuzzing
* Brute force attacks
* VHOST enumeration
* Proxy integration
* Saving output files

---

# Task 1 – Basic Directory Fuzzing

## Command

```bash
ffuf -u http://10.49.154.77/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt
```

## Explanation

* `-u` specifies the target URL
* `FUZZ` is the placeholder replaced by entries from the wordlist
* `-w` specifies the wordlist

FFUF attempted requests such as:

```txt
/admin
/login
/config
/uploads
```

## Findings

Example results:

```txt
README.md               [Status: 200]
config                  [Status: 301]
docs                    [Status: 301]
robots.txt              [Status: 200]
```

### Interesting Results

* `README.md` could contain sensitive information
* `robots.txt` may disclose hidden paths
* `config/` and `docs/` directories existed

---

# Task 2 – Custom Keywords

## Command

```bash
ffuf -u http://10.49.154.77/NORAJ -w /usr/share/seclists/Discovery/Web-Content/big.txt:NORAJ
```

## Explanation

FFUF allows custom placeholders instead of the default `FUZZ`.

Here:

* `NORAJ` acts as the keyword placeholder
* `big.txt:NORAJ` tells FFUF to replace `NORAJ` using the wordlist

---

# Task 3 – Extension Fuzzing

## Discovering Extensions

### Command

```bash
ffuf -u http://10.49.154.77/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
```

## Purpose

To identify supported file extensions and determine the website technology.

## Example Requests

```txt
index.php
index.asp
index.aspx
```

## Result

The server responded positively to PHP extensions, indicating the application likely used PHP.

---

# Task 4 – File Enumeration with Extensions

## Command

```bash
ffuf -u http://10.49.154.77/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt \
-e .php,.txt
```

## Explanation

* `-e` appends extensions automatically
* FFUF generated requests such as:

```txt
admin.php
login.php
config.txt
```

This method is more efficient than using a generic file list.

---

# Task 5 – Directory Enumeration

## Command

```bash
ffuf -u http://10.49.154.77/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt
```

## Purpose

To discover hidden directories.

## Example Findings

```txt
/admin/
/docs/
/config/
```

---

# Task 6 – Filtering and Matching

## Filtering 403 Responses

### Command

```bash
ffuf -u http://10.49.154.77/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt \
-fc 403
```

## Explanation

* `-fc 403` filters all HTTP 403 responses

This reduced noisy output from inaccessible files.

---

## Matching Only HTTP 200 Responses

### Command

```bash
ffuf -u http://10.49.154.77/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt \
-mc 200
```

## Explanation

* `-mc 200` displays only HTTP 200 responses

---

## Filtering Empty Responses

### Command

```bash
ffuf -u http://10.49.154.77/config/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt \
-fs 0
```

## Explanation

* `-fs 0` filters responses with size 0

Useful for removing empty include files.

---

## Regex Filtering

### Command

```bash
ffuf -u http://10.49.154.77/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt \
-fr '/\..*'
```

## Explanation

This filtered responses beginning with a dot:

```txt
.htaccess
.php
.htm
```

This reduced false positives while preserving legitimate 403 results.

---

# Task 7 – Parameter Fuzzing

## Command

```bash
ffuf -u 'http://10.49.154.77/sqli-labs/Less-1/?FUZZ=1' \
-c \
-w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt \
-fw 39
```

## Purpose

To discover valid GET parameters.

## Explanation

FFUF attempted:

```txt
?id=1
?page=1
?file=1
```

The `-fw 39` option filtered common invalid responses.

---

# Task 8 – Value Fuzzing

## Command

```bash
ruby -e '(0..255).each{|i| puts i}' | \
ffuf -u 'http://10.49.191.194/sqli-labs/Less-1/?id=FUZZ' \
-c \
-w - \
-fw 33
```

## Explanation

* Ruby generated values from 0–255
* `-w -` instructed FFUF to read input from standard input (stdin)

## Findings

Valid IDs discovered:

```txt
1
2
3
4
5
...
14
```

These IDs returned responses with different word counts and sizes, indicating valid database entries.

---

# Task 9 – Brute Force with POST Requests

## Command

```bash
ffuf -u http://10.49.191.194/sqli-labs/Less-11/ \
-c \
-w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt \
-X POST \
-d 'uname=Dummy&passwd=FUZZ&submit=Submit' \
-fs 1435 \
-H 'Content-Type: application/x-www-form-urlencoded'
```

## Explanation

* `-X POST` specifies POST requests
* `-d` sends POST data
* `FUZZ` replaces password values
* `-H` sets the content type header
* `-fs 1435` filters failed login responses

## Result

Valid password discovered:

```txt
p@ssword
```

---

# Task 10 – VHOST Enumeration

## Direct Subdomain Enumeration

### Command

```bash
ffuf -u http://FUZZ.mydomain.com \
-c \
-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Explanation

FFUF attempted:

```txt
admin.mydomain.com
dev.mydomain.com
api.mydomain.com
```

---

## VHOST Enumeration Using Host Header

### Command

```bash
ffuf -u http://mydomain.com \
-c \
-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
-H 'Host: FUZZ.mydomain.com' \
-fs 0
```

## Explanation

Instead of relying on DNS resolution, FFUF manipulated the HTTP Host header to discover hidden virtual hosts.

This technique can expose:

* internal admin panels
* staging environments
* private applications

---

# Task 11 – Proxy Usage

## Sending All Traffic Through Burp Suite

### Command

```bash
ffuf -u http://10.49.191.194/FUZZ \
-c \
-w /usr/share/seclists/Discovery/Web-Content/common.txt \
-x http://127.0.0.1:8080
```

## Explanation

* `-x` routes all traffic through a proxy

Useful for:

* intercepting requests
* inspecting responses
* using Burp Suite tools

---

## Replay Only Matched Results

### Command

```bash
ffuf -u http://10.49.191.194/FUZZ \
-c \
-w /usr/share/seclists/Discovery/Web-Content/common.txt \
-replay-proxy http://127.0.0.1:8080
```

## Explanation

Only matching results are sent to the proxy, reducing clutter in Burp history.

---

# Task 12 – Saving Output Files

## Save Output as Markdown

### Command

```bash
ffuf -u http://target/FUZZ \
-w wordlist.txt \
-o ffuf.md \
-of md
```

## Explanation

* `-o` specifies output filename
* `-of md` saves output in Markdown format

---

# Task 13 – Reusing Raw HTTP Requests

## Command

```bash
ffuf -request request.txt -w wordlist.txt
```

## Explanation

* `-request` loads a raw HTTP request file

Useful for:

* authenticated requests
* complex POST requests
* API fuzzing

---

# Task 14 – Verbose Output

## Command

```bash
ffuf -u http://target/FUZZ \
-w wordlist.txt \
-v
```

## Explanation

* `-v` enables verbose output

This prints:

* full URLs
* redirect locations
* additional request information

---

# Conclusion

In this room, I learned how to use FFUF for:

* web content discovery
* parameter fuzzing
* response analysis
* brute force attacks
* virtual host discovery
* proxy integration

The most important lesson was understanding how to analyze differences in responses using:

* status codes
* response size
* word count
* line count

FFUF proved to be a powerful and flexible tool for web enumeration and fuzzing tasks.
