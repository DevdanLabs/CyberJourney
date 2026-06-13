# TryHackMe - John the Ripper: The Basics (Walkthrough)

# Overview

This room introduces **John the Ripper (JtR)**, one of the most popular password cracking tools used by:

* Penetration Testers
* Red Team Operators
* Security Auditors
* Incident Responders
* Forensics Analysts

The room focuses on:

* Hash cracking
* Windows NTLM hashes
* Linux shadow hashes
* Single Crack Mode
* Custom Rules
* ZIP password cracking
* RAR password cracking
* SSH private key passphrase cracking

---

# Learning Objectives

By the end of this room, you should understand:

* How password cracking works
* How John identifies and attacks hashes
* How to use different John modes
* How helper tools such as zip2john and ssh2john work
* Real-world credential access workflows

---

# Understanding John's Workflow

Before touching any commands, it is important to understand the overall process.

Most attacks in this room follow the same pattern:

```text
Target
    ↓
Convert to John Format
    ↓
John the Ripper
    ↓
Password Recovery
```

Examples:

```text
ZIP File
↓
zip2john
↓
John
↓
Password
```

```text
SSH Key
↓
ssh2john
↓
John
↓
Passphrase
```

---

# Basic John Syntax

## Command

```bash
john [options] [file]
```

---

## Breakdown

### john

Starts John the Ripper.

### [options]

Additional settings that modify behavior.

Examples:

```bash
--wordlist
--format
--single
--rule
```

### [file]

Target hash file.

---

## Example

```bash
john hash.txt
```

---

# Identifying Hashes

Before cracking a hash, we should determine its type.

This is critical because John may not always identify hashes correctly.

---

## Using hash-id.py

### Download

```bash
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
```

### Run

```bash
python3 hash-id.py
```

### Example

Input:

```text
2e728dd31fb5949bc39cac5a9f066498
```

Output:

```text
Possible Hashes:

MD5
NTLM
```

---

# Finding Supported Formats

One of the most useful John commands:

```bash
john --list=formats
```

---

This produces hundreds of supported formats.

Examples:

```text
raw-md5
raw-sha1
raw-sha256
nt
sha512crypt
rar
zip
```

---

## Searching with grep

Instead of reading hundreds of lines:

```bash
john --list=formats | grep -i md5
```

---

### Breakdown

#### |

Pipe operator.

Sends output from one command into another.

---

#### grep

Search tool.

---

#### -i

Case-insensitive search.

Example:

```text
MD5
md5
Md5
```

All match.

---

### Example Output

```text
raw-md5
md5crypt
dynamic_0
```

---

# Task 04 - Basic Hash Cracking

## Objective

Identify and crack several common hash types.

---

# Hash 1

## Identify

```bash
python3 hash-id.py
```

Result:

```text
MD5
```

---

## Crack

```bash
john \
--format=raw-md5 \
--wordlist=/usr/share/wordlists/rockyou.txt \
hash1.txt
```

---

### Password

```text
biscuit
```

---

# Hash 2

### Type

```text
SHA1
```

### Password

```text
kangeroo
```

---

# Hash 3

### Type

```text
SHA256
```

### Password

```text
microphone
```

---

# Hash 4

### Type

```text
Whirlpool
```

### Password

```text
colossal
```

---

# Task 05 - Windows NTLM Hashes

# What is NTLM?

Windows stores password hashes using:

```text
NTLM
```

or

```text
NTHash
```

---

Common sources:

```text
SAM
NTDS.dit
Mimikatz
```

---

# Crack

```bash
john --format=nt ntlm.txt
```

---

### Why No Wordlist?

John automatically used its default cracking mode.

This was sufficient because the password was simple.

---

### Password

```text
mushroom
```

---

# Pentesting Relevance

Typical workflow:

```text
Compromise Host
↓
Dump SAM
↓
Recover NTLM
↓
Crack Hash
↓
Credential Access
```

---

# Task 06 - Linux Shadow Hashes

# Linux Password Storage

User information:

```bash
/etc/passwd
```

Password hashes:

```bash
/etc/shadow
```

---

# Example passwd Entry

```text
root:x:0:0::/root:/bin/bash
```

---

# Example shadow Entry

```text
root:$6$salt$hash
```

---

# Understanding $6$

```text
$1$ = MD5 Crypt

$5$ = SHA256 Crypt

$6$ = SHA512 Crypt
```

---

# Unshadow

John requires a combined format.

---

## Command

```bash
unshadow passwd.txt shadow.txt > unshadowed.txt
```

---

## Breakdown

### unshadow

Combines passwd and shadow information.

### >

Redirects output into a file.

---

# Crack

```bash
john \
--format=sha512crypt \
--wordlist=/usr/share/wordlists/rockyou.txt \
unshadowed.txt
```

---

### Password

```text
1234
```

---

# Task 07 - Single Crack Mode

# Purpose

Generate password guesses using:

* Username
* GECOS Data
* Home Directory

---

# Initial Problem

The hash file contained:

```text
<hash>
```

Single mode failed.

---

# Why?

John had no username information.

---

# Correct Format

```text
Joker:<hash>
```

---

# Crack

```bash
john \
--single \
--format=raw-md5 \
hash07.txt
```

---

# Password

```text
jok3r
```

---

# What Happened Internally?

John generated guesses such as:

```text
joker
joker1
joker123
jok3r
```

using word mangling rules.

---

# Task 08 - Custom Rules

# Purpose

Exploit predictable password patterns.

Example:

```text
Password1!
Welcome2025!
Company123$
```

---

# Rule Components

## c

Capitalize.

---

## Az

Append characters.

---

## A0

Prepend characters.

---

# Example Rule

```text
[List.Rules:PoloPassword]

cAz"[0-9][!£$%@]"
```

---

# Result

Input:

```text
password
```

Possible outputs:

```text
Password1!
Password2@
Password3$
```

---

# Using a Custom Rule

```bash
john \
--wordlist=rockyou.txt \
--rule=PoloPassword \
hash.txt
```

---

# Task 09 - ZIP Password Cracking

# Generate Hash

```bash
zip2john secure.zip > zip_hash.txt
```

---

# Crack

```bash
john \
--wordlist=/usr/share/wordlists/rockyou.txt \
zip_hash.txt
```

---

# Show Password

```bash
john --show zip_hash.txt
```

---

### Password

```text
pass123
```

---

# Extract ZIP

```bash
unzip secure.zip
```

Password:

```text
pass123
```

---

# Flag

```text
THM{w3ll_d0n3_h4sh_r0y4l}
```

---

# Common Mistake

Do NOT run:

```bash
cat secure.zip
```

ZIP files are binary files.

Use:

```bash
unzip -l secure.zip
```

instead.

---

# Task 10 - RAR Password Cracking

# Generate Hash

```bash
rar2john secure.rar > rar_hash.txt
```

---

# Crack

```bash
john \
--wordlist=/usr/share/wordlists/rockyou.txt \
rar_hash.txt
```

---

# Show Password

```bash
john --show rar_hash.txt
```

---

### Password

```text
password
```

---

# Extract

```bash
unrar x secure.rar
```

---

# Flag

```text
THM{r4r_4rch1ve5_th15_t1m3}
```

---

# Task 11 - SSH Private Key Cracking

# What is id_rsa?

SSH authentication key.

Private:

```text
id_rsa
```

Public:

```text
id_rsa.pub
```

---

# Convert Key

```bash
python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```

---

# Crack

```bash
john \
--wordlist=/usr/share/wordlists/rockyou.txt \
id_rsa_hash.txt
```

---

# Show Password

```bash
john --show id_rsa_hash.txt
```

---

### Passphrase

```text
mango
```

---

# Why This Matters

Real-world workflow:

```text
Find id_rsa
↓
Crack Passphrase
↓
SSH Login
↓
Shell Access
```

---

# Important Commands Summary

## Hash Identification

```bash
python3 hash-id.py
```

---

## List Formats

```bash
john --list=formats
```

---

## Search Formats

```bash
john --list=formats | grep -i md5
```

---

## Wordlist Attack

```bash
john --wordlist=rockyou.txt hash.txt
```

---

## Format-Specific Attack

```bash
john --format=raw-md5 \
--wordlist=rockyou.txt \
hash.txt
```

---

## Show Cracked Passwords

```bash
john --show hash.txt
```

---

## Single Crack Mode

```bash
john --single hash.txt
```

---

## ZIP Cracking

```bash
zip2john file.zip > hash.txt
```

---

## RAR Cracking

```bash
rar2john file.rar > hash.txt
```

---

## SSH Key Cracking

```bash
python3 /opt/john/ssh2john.py id_rsa > hash.txt
```

---

# Key Takeaways

* Hashes are not reversible.
* Password cracking relies on guessing and verification.
* Human-generated passwords are highly predictable.
* NTLM hashes remain important in Windows attacks.
* Linux uses salted hashes in `/etc/shadow`.
* Single Crack Mode exploits user information.
* Custom Rules exploit predictable password complexity.
* ZIP, RAR, and SSH keys can all be attacked through John conversion tools.
* Many privilege escalation and credential access attacks eventually involve password cracking.

# Skills Gained

* Hash Identification
* Password Cracking
* John the Ripper Usage
* Windows Credential Attacks
* Linux Credential Attacks
* Wordlist Attacks
* Rule-Based Attacks
* ZIP Cracking
* RAR Cracking
* SSH Key Cracking
* Credential Recovery Methodology
