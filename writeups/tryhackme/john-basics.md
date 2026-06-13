# John the Ripper: The Basics

## Executive Summary

This room introduced **John the Ripper (JtR)**, one of the most widely used password-cracking tools in cybersecurity. The room covered the fundamentals of password cracking, hash identification, Windows and Linux authentication hashes, Single Crack Mode, custom rules, and cracking password-protected ZIP archives, RAR archives, and SSH private keys.

Throughout the room, multiple password recovery techniques were demonstrated using real-world workflows commonly encountered during penetration testing, red team engagements, and security assessments.

---

# Learning Objectives

By completing this room, the following skills were developed:

* Identifying common hash types
* Cracking password hashes using John the Ripper
* Cracking Windows NTLM hashes
* Cracking Linux `/etc/shadow` hashes
* Using Single Crack Mode
* Understanding and creating custom rules
* Cracking password-protected ZIP archives
* Cracking password-protected RAR archives
* Cracking SSH private key passphrases
* Understanding password attack methodologies

---

# Prerequisites

Before attempting this room, familiarity with the following topics is recommended:

* Cryptography Basics
* Public Key Cryptography Basics
* Hashing Basics
* Linux command line fundamentals

---

# Concepts Covered

## Hashing

Hashing is the process of converting data of any size into a fixed-length output using a hashing algorithm.

Examples:

| Algorithm | Typical Length     |
| --------- | ------------------ |
| MD5       | 32 hex characters  |
| SHA1      | 40 hex characters  |
| SHA256    | 64 hex characters  |
| Whirlpool | 128 hex characters |

Hashes are:

* Deterministic
* One-way
* Fixed length
* Resistant to reversal

---

## Dictionary Attacks

A dictionary attack works by:

1. Taking a candidate password
2. Hashing it
3. Comparing it to the target hash

Example:

```text
password
↓
MD5
↓
5f4dcc3b5aa765d61d8327deb882cf99
```

If the generated hash matches the target hash, the password is recovered.

---

## Wordlists

The room primarily uses:

```text
rockyou.txt
```

A famous password list derived from the 2009 RockYou breach.

Common passwords found in RockYou include:

```text
123456
password
qwerty
dragon
welcome
```

---

# John the Ripper Fundamentals

## Basic Syntax

```bash
john [options] [file]
```

### Example

```bash
john hash.txt
```

---

## Wordlist Mode

### Syntax

```bash
john --wordlist=<wordlist> <hashfile>
```

### Example

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### Purpose

Uses a password dictionary to attempt password recovery.

---

## Displaying Cracked Passwords

### Syntax

```bash
john --show hash.txt
```

### Purpose

Displays passwords previously recovered by John.

---

# Hash Identification

## hash-id.py

Used to identify hash formats.

### Download

```bash
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
```

### Usage

```bash
python3 hash-id.py
```

### Example

```text
Possible Hashes:

MD5
NTLM
```

---

# Task 04 – Cracking Basic Hashes

## Objective

Identify and crack common hash formats.

---

## Results

| File      | Hash Type | Password   |
| --------- | --------- | ---------- |
| hash1.txt | MD5       | biscuit    |
| hash2.txt | SHA1      | kangeroo   |
| hash3.txt | SHA256    | microphone |
| hash4.txt | Whirlpool | colossal   |

---

## Methodology

### Identify Hash

```bash
python3 hash-id.py
```

### Crack Hash

```bash
john --format=raw-md5 --wordlist=rockyou.txt hash1.txt
```

---

# Task 05 – Cracking Windows NTLM Hashes

## Theory

Windows stores password hashes using NTLM/NTHash.

Sources include:

* SAM Database
* NTDS.dit
* Mimikatz dumps

---

## Attack Workflow

```text
Dump Hash
↓
Identify Format
↓
Crack Hash
↓
Recover Credentials
```

---

## Command

```bash
john --format=nt ntlm.txt
```

---

## Password Recovered

```text
mushroom
```

---

## Pentester Relevance

NTLM hashes are frequently used in:

* Credential Access
* Lateral Movement
* Active Directory attacks

---

# Task 06 – Cracking Linux Shadow Hashes

## Linux Password Storage

Linux stores password hashes in:

```bash
/etc/shadow
```

User account information remains in:

```bash
/etc/passwd
```

---

## Unshadow

### Purpose

Combines passwd and shadow data into a format understood by John.

---

### Syntax

```bash
unshadow passwd.txt shadow.txt > unshadowed.txt
```

---

## Crack

```bash
john \
--format=sha512crypt \
--wordlist=rockyou.txt \
unshadowed.txt
```

---

## Root Password

```text
1234
```

---

## Security Notes

Linux modern password hashes typically use:

* Salt
* SHA512Crypt
* SHA256Crypt

Unlike NTLM, hashes are salted.

---

# Task 07 – Single Crack Mode

## Purpose

Single Crack Mode generates password candidates using:

* Username
* GECOS information
* Account metadata

---

## Example

Username:

```text
Joker
```

Potential passwords:

```text
joker
joker1
joker123
jok3r
```

---

## Required Format

### Incorrect

```text
1efee03cdcb96d90ad48ccc7b8666033
```

### Correct

```text
Joker:1efee03cdcb96d90ad48ccc7b8666033
```

---

## Command

```bash
john --single --format=raw-md5 hash07.txt
```

---

## Password Recovered

```text
jok3r
```

---

## Troubleshooting

### Problem

Single Crack Mode failed initially.

### Root Cause

The username was missing from the hash file.

### Solution

Modified the format:

```text
Joker:<hash>
```

### Result

Password successfully recovered.

---

# Task 08 – Custom Rules

## Purpose

Custom rules allow attackers to exploit predictable password patterns.

---

## Example Password

```text
Polopassword1!
```

Pattern:

```text
Capital Letter
↓
Word
↓
Number
↓
Symbol
```

---

## Example Rule

```text
[List.Rules:PoloPassword]

cAz"[0-9][!£$%@]"
```

---

## Calling Custom Rules

```bash
john --rule=PoloPassword
```

---

## Key Takeaway

Users tend to follow predictable complexity patterns.

---

# Task 09 – Cracking ZIP Archives

## zip2john

Converts ZIP archives into a format understood by John.

---

### Syntax

```bash
zip2john secure.zip > zip_hash.txt
```

---

### Crack

```bash
john --wordlist=rockyou.txt zip_hash.txt
```

---

## Password Recovered

```text
pass123
```

---

## Flag

```text
THM{w3ll_d0n3_h4sh_r0y4l}
```

---

## Lesson Learned

Do not run:

```bash
cat secure.zip
```

unless you enjoy summoning random binary artifacts into your terminal.

---

# Task 10 – Cracking RAR Archives

## rar2john

Converts RAR archives into John-compatible hashes.

---

### Syntax

```bash
rar2john secure.rar > rar_hash.txt
```

---

### Crack

```bash
john --wordlist=rockyou.txt rar_hash.txt
```

---

## Password Recovered

```text
password
```

---

## Flag

```text
THM{r4r_4rch1ve5_th15_t1m3}
```

---

# Task 11 – Cracking SSH Private Keys

## SSH Key Authentication

Private key:

```text
id_rsa
```

Public key:

```text
id_rsa.pub
```

---

## ssh2john

Converts encrypted SSH private keys into a crackable format.

---

### Syntax

```bash
python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```

---

### Crack

```bash
john --wordlist=rockyou.txt id_rsa_hash.txt
```

---

## Password Recovered

```text
mango
```

---

## Pentester Relevance

Common workflow:

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

# Pentester Notes

## Enumeration Value

Useful artifacts include:

```text
SAM
NTDS.dit
/etc/shadow
ZIP archives
RAR archives
id_rsa
```

---

## Exploitation Value

Recovered passwords may provide:

* SSH access
* RDP access
* VPN access
* Domain access
* Database access

---

## Lateral Movement

Credential reuse remains common.

Recovered passwords may work across multiple systems.

---

## Detection Opportunities

Defenders should monitor:

* SAM access
* NTDS.dit access
* Mimikatz execution
* Shadow file access
* Large-scale password cracking activity

---

# Skills Gained

* Hash Identification
* Password Cracking
* NTLM Cracking
* Linux Shadow Cracking
* Single Crack Mode
* Custom Rule Usage
* ZIP Cracking
* RAR Cracking
* SSH Key Cracking
* Credential Recovery Workflows

---

# Key Takeaways

1. Hashes are not encryption.
2. Password cracking relies on guessing and verification.
3. Human password behavior is predictable.
4. Strong hashing algorithms cannot compensate for weak passwords.
5. John the Ripper is highly versatile and supports numerous credential formats.
6. Many penetration testing attack chains ultimately rely on credential recovery.

---

# Future Learning Path

Recommended next topics:

* Password Attacks
* Hydra
* Hashcat
* Linux Privilege Escalation
* Windows Privilege Escalation
* Active Directory Fundamentals
* Kerberoasting
* Credential Access Techniques
* Red Team Operations

---

# References

* John the Ripper Documentation
* SecLists
* RockYou Wordlist
* TryHackMe – John the Ripper: The Basics

---

# Tags

```text
JohnTheRipper
PasswordCracking
Hashing
NTLM
Linux
Windows
SSH
Zip2John
Rar2John
SSH2John
Cybersecurity
TryHackMe
Pentesting
RedTeam
CredentialAccess
```
