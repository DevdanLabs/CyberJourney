# Hashing Basics – Task Walkthrough

> This document contains the practical walkthrough and challenge-solving methodology used throughout the TryHackMe Hashing Basics room.
>
> The goal is not only to obtain answers but to understand the reasoning, tools, and techniques used to solve each task.

---

# Task 2 – Hash Functions

## Objective

Understand how cryptographic hash functions work and observe the Avalanche Effect.

---

## Concepts Covered

* Hash Functions
* MD5
* SHA1
* SHA256
* Avalanche Effect
* File Integrity

---

## Understanding the Avalanche Effect

Two files were provided:

```text
file1.txt = T
file2.txt = U
```

Although the files differ by only one bit, their resulting hashes are completely different.

This demonstrates one of the most important properties of cryptographic hash functions:

> A very small change in the input should produce a dramatically different output.

---

## Commands Used

Display file contents:

```bash
cat file1.txt
cat file2.txt
```

View hexadecimal representation:

```bash
hexdump -C file1.txt
hexdump -C file2.txt
```

Generate MD5 hashes:

```bash
md5sum *.txt
```

Generate SHA1 hashes:

```bash
sha1sum *.txt
```

Generate SHA256 hashes:

```bash
sha256sum *.txt
```

---

## Questions

### What is the SHA256 hash of passport.jpg?

Calculate the hash:

```bash
sha256sum passport.jpg
```

The first value in the output is the answer.

---

### What is the output size of MD5?

MD5 produces:

```text
128 bits
```

Convert bits to bytes:

```text
128 ÷ 8 = 16 bytes
```

---

### If a hash is 8 bits long, how many possible values exist?

Formula:

```text
2^8
```

Result:

```text
256
```

---

## Key Takeaways

* Hashes are deterministic.
* Hashes are fixed-length outputs.
* Small input changes create large output changes.
* Hashes are useful for integrity verification.

---

# Task 3 – Password Storage and Authentication

## Objective

Understand how password hashing is used for authentication systems.

---

## Concepts Covered

* Plaintext Password Storage
* Encrypted Password Storage
* Password Hashing
* Credential Theft

---

## Insecure Password Storage

### Plaintext Storage

Example:

```text
Username: admin
Password: password123
```

If a database is compromised, every password is immediately exposed.

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

The encryption key must also be stored somewhere.

If an attacker obtains both the database and encryption key, all passwords can be recovered.

---

## Secure Password Storage

Instead of storing passwords:

```text
Password
↓
Hash Function
↓
Store Hash
```

During login:

```text
Entered Password
↓
Hash Function
↓
Compare With Stored Hash
```

Authentication succeeds when both hashes match.

---

## Key Takeaways

* Passwords should never be stored in plaintext.
* Encryption is not ideal for password verification systems.
* Password hashing is the preferred solution.

---

# Task 4 – Salting and Rainbow Tables

## Objective

Understand rainbow tables and why salts are necessary.

---

## Concepts Covered

* Rainbow Tables
* Password Salting
* Password Security

---

## Rainbow Tables

A rainbow table is a database of:

```text
Password → Hash
```

Example:

| Password | Hash                             |
| -------- | -------------------------------- |
| 123456   | e10adc3949ba59abbe56e057f20f883e |
| password | 5f4dcc3b5aa765d61d8327deb882cf99 |

Attackers can perform lookups instead of brute-force attacks.

---

## Salting

A salt is random data added to a password before hashing.

Example:

```text
Password:
123456

Salt:
abc123

Combined:
123456abc123
```

Hashing is performed on the combined value.

---

## Benefits of Salting

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
123456 + SaltA
↓
Hash
↓
XYZ

123456 + SaltB
↓
Hash
↓
DEF
```

The resulting hashes are different.

---

## Questions

### Manual Rainbow Table Lookup

Locate the provided hash inside the rainbow table and identify its matching password.

---

### Online Hash Lookup

Use:

* CrackStation
* Hashes.com

Search for the provided hash value and identify the matching plaintext password.

---

### Should Passwords Be Encrypted?

Answer based on authentication requirements.

Remember:

Authentication systems only need to verify passwords, not recover them.

---

## Key Takeaways

* Salts prevent rainbow table attacks.
* Identical passwords should not generate identical hashes.
* Modern password storage relies on salted hashes.

---

# Task 5 – Recognising Password Hashes

## Objective

Learn how to identify password hash formats.

---

## Concepts Covered

* Linux Password Hashes
* Windows Password Hashes
* Hash Identification

---

## Linux Password Hashes

Stored in:

```bash
/etc/shadow
```

General format:

```text
$prefix$options$salt$hash
```

---

## Common Linux Prefixes

| Prefix | Algorithm   |
| ------ | ----------- |
| $y$    | yescrypt    |
| $7$    | scrypt      |
| $2a$   | bcrypt      |
| $2b$   | bcrypt      |
| $2y$   | bcrypt      |
| $6$    | sha512crypt |
| $1$    | md5crypt    |

---

## Windows Password Hashes

Typically stored in:

```text
SAM
```

Common algorithm:

```text
NTLM
```

---

## Useful Hashcat Enumeration Trick

One of the most useful techniques learned during this room:

Find hash modes directly from Hashcat:

```bash
hashcat --help | grep -i bcrypt
```

Example:

```bash
hashcat --help | grep -i sha256
```

Example:

```bash
hashcat --help | grep -i ntlm
```

Example:

```bash
hashcat --help | grep -i sha512
```

This is much faster than manually searching documentation.

---

## Questions

### Hash Size in yescrypt

Use:

* Linux documentation
* Hashcat example hashes
* Algorithm documentation

---

### Cisco-ASA MD5 Hash Mode

Use:

```bash
hashcat --help | grep -i cisco
```

or check Hashcat Example Hashes.

---

### Cisco-IOS $9$

Identify the algorithm associated with the `$9$` prefix.

---

## Key Takeaways

* Context is important when identifying hashes.
* Prefixes provide strong indicators.
* Correct identification is required before cracking.

---

# Task 6 – Password Cracking

## Objective

Crack multiple password hashes using Hashcat.

---

## Concepts Covered

* Dictionary Attacks
* Hashcat
* Password Cracking
* Wordlists

---

## General Methodology

Always follow this workflow:

```text
Identify Hash
↓
Determine Hashcat Mode
↓
Choose Attack Mode
↓
Select Wordlist
↓
Run Attack
↓
Review Results
```

---

## Hashcat Syntax

```bash
hashcat -m <hash_mode> -a <attack_mode> hashfile wordlist
```

---

## Attack Mode Used

Dictionary Attack:

```bash
-a 0
```

---

# Hash 1

## Identification

Hash begins with:

```text
$2a$
```

This indicates:

```text
bcrypt
```

---

## Find Mode

```bash
hashcat --help | grep -i bcrypt
```

---

## Crack

```bash
hashcat -m <mode> -a 0 hash1.txt /usr/share/wordlists/rockyou.txt
```

---

# Hash 2

## Identification

Hash length:

```text
64 hexadecimal characters
```

Likely:

```text
SHA256
```

---

## Find Mode

```bash
hashcat --help | grep -i sha256
```

---

## Crack

```bash
hashcat -m <mode> -a 0 hash2.txt /usr/share/wordlists/rockyou.txt
```

---

# Hash 3

## Identification

Hash begins with:

```text
$6$
```

Indicates:

```text
sha512crypt
```

---

## Find Mode

```bash
hashcat --help | grep -i sha512
```

---

## Crack

```bash
hashcat -m <mode> -a 0 hash3.txt /usr/share/wordlists/rockyou.txt
```

---

# Hash 4

## Initial Approach

Hash appears to be:

```text
32 hexadecimal characters
```

Potentially:

* MD5
* NTLM
* Other 32-character hash formats

---

## Initial Attempt

```bash
hashcat -m 0 -a 0 hash4.txt /usr/share/wordlists/rockyou.txt
```

---

## Result

```text
Status: Exhausted
```

No password was recovered.

---

## Investigation

Hashcat successfully processed the hash.

The failure indicated:

* The hash format was valid.
* The password did not exist inside rockyou.txt.

---

## Alternative Approach

Use public hash lookup databases:

* CrackStation
* Hashes.com

The hash was successfully resolved using a lookup database instead of dictionary cracking.

---

## Lessons Learned

Not every password hash should be brute-forced immediately.

Efficient workflow:

```text
Hash
↓
Identify
↓
Search Public Databases
↓
Dictionary Attack
↓
Rule-Based Attack
↓
Brute Force
```

---

# Task 7 – Integrity Checking and HMAC

## Objective

Understand how hashing verifies file integrity and how HMAC provides authenticity.

---

## Concepts Covered

* File Integrity
* SHA256 Verification
* HMAC

---

## File Integrity Verification

Generate file hash:

```bash
sha256sum Fedora.iso
```

Compare with the official published hash.

If they match:

```text
Downloaded File
=
Official File
```

---

## HMAC

HMAC combines:

```text
Hash Function
+
Secret Key
```

Provides:

* Integrity
* Authenticity

---

## Key Takeaways

* Hashes verify integrity.
* HMAC verifies integrity and authenticity.

---

# Task 8 – Encoding vs Hashing

## Objective

Understand the difference between encoding and hashing.

---

## Concepts Covered

* Base64 Encoding
* Base64 Decoding

---

## Decode Base64

Input:

```text
RU5jb2RlREVjb2RlCg==
```

Decode:

```bash
echo "RU5jb2RlREVjb2RlCg==" | base64 -d
```

---

## Key Takeaways

Encoding is not encryption.

Anyone can reverse Base64 encoding without a key.

---

# Troubleshooting Notes

## Hashcat Returned "Exhausted"

### Cause

Password was not found in the selected wordlist.

### Example

```text
Status...........: Exhausted
```

### Resolution

Try:

* Larger wordlists
* Rule attacks
* Mask attacks
* Public hash databases

---

# Lessons Learned

* Hashes cannot be decrypted.
* Password cracking is a process of guessing inputs and comparing hashes.
* Hash identification is critical before cracking.
* Hashcat mode selection is one of the most important skills.
* Salting significantly improves password security.
* Integrity verification is one of the most common real-world uses of hashing.
* HMAC adds authenticity on top of integrity.
* Encoding, hashing, and encryption solve different problems.
