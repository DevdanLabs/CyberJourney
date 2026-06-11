# Hashing Basics - TryHackMe Writeup

## Executive Summary

This room introduced the fundamentals of cryptographic hashing and its role in modern cybersecurity. We explored how hash functions work, why they are important, how they are used in password storage and authentication systems, how password hashes are recognized and cracked, and how hashing helps verify data integrity. We also covered HMACs, rainbow tables, password salting, and the differences between hashing, encoding, and encryption.

This room serves as a foundational prerequisite for password auditing, password cracking, digital forensics, malware analysis, web application security, Active Directory security, and incident response.

---

# Learning Objectives

By completing this room, I learned:

* What cryptographic hash functions are
* How hashing differs from encryption and encoding
* The Avalanche Effect and hash collisions
* How passwords should be securely stored
* Why salts are important
* How rainbow tables work
* How to recognize common password hash formats
* Basic password cracking methodology
* How hashing is used for integrity verification
* The purpose of HMACs
* The difference between integrity, authenticity, and confidentiality

---

# Prerequisites

* Cryptography Basics
* Public Key Cryptography Basics

---

# Concepts Covered

## Cryptographic Hash Functions

### What is a Hash Function?

A cryptographic hash function is a mathematical function that:

* Accepts input of any size
* Produces output of fixed size
* Is deterministic
* Is computationally difficult to reverse
* Produces significantly different outputs for small input changes

Example:

```text
Input:
hello

SHA256:
2cf24dba5fb0a30e...
```

---

## Properties of Good Hash Functions

### Deterministic

The same input always produces the same output.

```text
SHA256("hello")
=
SHA256("hello")
```

---

### Fixed-Length Output

Regardless of input size, output size remains constant.

Example:

| Algorithm | Output Size         |
| --------- | ------------------- |
| MD5       | 128 bits (16 bytes) |
| SHA1      | 160 bits (20 bytes) |
| SHA256    | 256 bits (32 bytes) |
| SHA512    | 512 bits (64 bytes) |

---

### One-Way Function

Hashes cannot be reversed to recover the original input.

```text
Password
↓
Hash
↓
Digest
```

There is no decryption process.

---

### Avalanche Effect

A single-bit change in input produces a dramatically different output.

Example:

```text
T
↓
MD5
↓
b9ece18c950afbfa6b0fdbfa4ff731d3

U
↓
MD5
↓
4c614360da93c0a041b22e537de151eb
```

Although T and U differ by only one bit, the resulting hashes are completely different.

---

## Hash Collisions

### Definition

A collision occurs when two different inputs generate the same hash output.

```text
Input A
↓
Hash
↓
ABC

Input B
↓
Hash
↓
ABC
```

---

### Why Collisions Exist

Hash outputs are finite while possible inputs are practically infinite.

This follows the Pigeonhole Principle.

Example:

For a 4-bit hash:

```math
2^4 = 16
```

Only 16 possible outputs exist.

Eventually, different inputs must share the same output.

---

### MD5 and SHA1

MD5 and SHA1 are considered insecure because practical collision attacks have been demonstrated.

Modern systems should avoid using them for security-sensitive applications.

---

# Hashing and Password Storage

## Insecure Methods

### Plaintext Storage

Example:

| Username | Password    |
| -------- | ----------- |
| admin    | password123 |

If the database is leaked, all passwords are immediately exposed.

---

### Encrypted Password Storage

Example:

```text
Password
↓
AES Encryption
↓
Ciphertext
```

Problem:

The encryption key must be stored somewhere.

If an attacker obtains both the database and key, all passwords can be decrypted.

---

## Secure Password Storage

Instead of storing passwords, store hashes.

Registration:

```text
Password
↓
Hash Function
↓
Store Hash
```

Login:

```text
Entered Password
↓
Hash Function
↓
Compare Against Stored Hash
```

If the hashes match, authentication succeeds.

---

# Rainbow Tables

## Definition

A rainbow table is a precomputed database of:

```text
Password → Hash
```

Example:

| Hash                             | Password |
| -------------------------------- | -------- |
| e10adc3949ba59abbe56e057f20f883e | 123456   |
| 5f4dcc3b5aa765d61d8327deb882cf99 | password |

---

## Risk

Attackers can perform lookups instead of expensive brute-force operations.

---

# Salting Passwords

## What is a Salt?

A salt is a random value added to a password before hashing.

Example:

```text
Password:
123456

Salt:
abc123

Result:
123456abc123
```

Hashing is performed on the combined value.

---

## Benefits

Two users with the same password generate different hashes.

Without salt:

```text
123456
↓
Hash
↓
ABC

123456
↓
Hash
↓
ABC
```

With salt:

```text
123456 + Salt1
↓
Hash
↓
XYZ

123456 + Salt2
↓
Hash
↓
DEF
```

This prevents rainbow table attacks from being practical.

---

# Modern Password Hashing Algorithms

Recommended:

* Argon2
* bcrypt
* scrypt
* PBKDF2

Not Recommended:

* MD5
* SHA1

---

# Recognizing Password Hashes

## Linux Password Hashes

Stored in:

```bash
/etc/shadow
```

Format:

```text
$prefix$options$salt$hash
```

---

### Common Prefixes

| Prefix           | Algorithm   |
| ---------------- | ----------- |
| $y$              | yescrypt    |
| $7$              | scrypt      |
| $2a$, $2b$, $2y$ | bcrypt      |
| $6$              | sha512crypt |
| $1$              | md5crypt    |

---

### Example

```text
$y$j9T$76UzfgEM5PnymhQ7TlJey1$/OOS...
```

Components:

* Prefix: yescrypt
* Options: j9T
* Salt: 76UzfgEM5PnymhQ7TlJey1
* Hash: /OOS...

---

## Windows Password Hashes

Stored in:

```text
SAM (Security Accounts Manager)
```

Common format:

```text
NTLM
```

NTLM hashes are visually similar to MD4 and MD5 hashes.

Context is important when identifying them.

---

# Password Cracking

## Key Principle

Hashes are not decrypted.

Instead:

```text
Candidate Password
↓
Hash
↓
Compare
```

Repeat until a match is found.

---

## Dictionary Attacks

Use a wordlist such as:

```text
rockyou.txt
```

Example:

```bash
hashcat -a 0 hash.txt rockyou.txt
```

---

## Brute Force Attacks

Try all possible combinations.

Example:

```text
a
b
c
...
aa
ab
ac
...
```

Computationally expensive.

---

## Common Tools

### Hashcat

GPU-accelerated password cracking tool.

Example:

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

---

### John the Ripper

CPU-focused password cracking tool.

Commonly used in Linux environments.

---

# Hashing for Integrity Verification

## File Integrity

Hashing allows verification that a file has not changed.

Example:

```bash
sha256sum Fedora.iso
```

Compare the resulting hash against the official published hash.

If they match:

```text
Downloaded File
=
Official File
```

with very high confidence.

---

## Malware Analysis

Malware samples are commonly identified using:

* MD5
* SHA1
* SHA256

Hashes act as digital fingerprints.

---

## Digital Forensics

Investigators calculate hashes before and after evidence acquisition to demonstrate that evidence has not been modified.

---

# HMAC

## Definition

HMAC stands for:

```text
Hash-based Message Authentication Code
```

It combines:

```text
Hash Function
+
Secret Key
```

---

## Purpose

Provides:

### Integrity

The message was not modified.

### Authenticity

The sender possesses the secret key.

---

## Formula

```math
HMAC(K,M) = H((K ⊕ opad) || H((K ⊕ ipad) || M))
```

Where:

* K = Secret Key
* M = Message

---

## Real-World Usage

* API Authentication
* AWS Request Signing
* JWT Signatures
* Secure Communications

---

# Encoding vs Hashing vs Encryption

| Feature                  | Encoding      | Hashing        | Encryption |
| ------------------------ | ------------- | -------------- | ---------- |
| Reversible               | Yes           | No             | Yes        |
| Uses Key                 | No            | No             | Yes        |
| Protects Confidentiality | No            | No             | Yes        |
| Verifies Integrity       | No            | Yes            | No         |
| Common Examples          | Base64, UTF-8 | SHA256, bcrypt | AES, RSA   |

---

## Base64 Example

Encoding:

```bash
echo "TryHackMe" | base64
```

Output:

```text
VHJ5SGFja01lCg==
```

Decoding:

```bash
echo "VHJ5SGFja01lCg==" | base64 -d
```

Output:

```text
TryHackMe
```

---

# Commands Used

## md5sum

### Purpose

Calculate MD5 hashes.

### Syntax

```bash
md5sum file
```

---

## sha1sum

### Purpose

Calculate SHA1 hashes.

### Syntax

```bash
sha1sum file
```

---

## sha256sum

### Purpose

Calculate SHA256 hashes.

### Syntax

```bash
sha256sum file
```

---

## base64

### Purpose

Encode or decode Base64 data.

### Encode

```bash
echo "text" | base64
```

### Decode

```bash
echo "encoded" | base64 -d
```

---

## hashcat

### Purpose

Password hash cracking.

### Syntax

```bash
hashcat -m <hash_mode> -a <attack_mode> hashfile wordlist
```

### Example

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

---

# Pentester Notes

## Enumeration Relevance

Recognizing hash formats is essential during:

* Database breaches
* Linux privilege escalation
* Windows credential dumping

---

## Credential Access

Password hashes frequently appear in:

* /etc/shadow
* SAM database
* Active Directory dumps

---

## Common Mistakes

* Assuming hashes can be decrypted
* Using MD5 or SHA1 for password storage
* Reusing salts
* Storing passwords in plaintext

---

# Key Takeaways

* Hashing is one-way and cannot be reversed.
* Hash collisions are unavoidable but should be computationally infeasible to engineer.
* Passwords should be stored using salted password hashing algorithms.
* bcrypt, scrypt, and Argon2 are preferred for password storage.
* Hashcat and John the Ripper are common password-cracking tools.
* Hashes can verify file integrity.
* HMAC provides both integrity and authenticity.
* Encoding, hashing, and encryption serve different purposes.

---

# Skills Gained

* Hash identification
* Password storage concepts
* Rainbow table understanding
* Password cracking fundamentals
* Hashcat usage
* Integrity verification
* HMAC fundamentals
* Cryptographic reasoning

---

# Future Learning Path

Recommended next topics:

1. John the Ripper
2. Hashcat Advanced Usage
3. Linux Password Security
4. Windows NTLM Authentication
5. Active Directory Credential Attacks
6. Kerberos Authentication
7. Digital Signatures
8. PKI and Certificates
9. JWT Security
10. API Security Testing

---

# References

* TryHackMe: Hashing Basics
* Hashcat Documentation
* John the Ripper Documentation
* Linux `man 5 shadow`
* RFC 2104 (HMAC)

---

# Tags

`#Cryptography`
`#Hashing`
`#PasswordSecurity`
`#Hashcat`
`#JohnTheRipper`
`#HMAC`
`#Integrity`
`#TryHackMe`
`#Cybersecurity`
