# Public Key Cryptography Basics

## TryHackMe Writeup

---

# Executive Summary

Public Key Cryptography Basics introduces the fundamental concepts of asymmetric cryptography and its role in securing modern communications. Unlike symmetric cryptography, where both parties share the same secret key, asymmetric cryptography uses a pair of mathematically related keys: a public key and a private key.

This room covers the foundations of RSA, Diffie-Hellman Key Exchange, SSH key authentication, digital signatures, certificates, and PGP/GPG. These technologies form the backbone of HTTPS, SSH, VPNs, secure email systems, and digital identity verification across the modern Internet.

---

# Learning Objectives

By completing this room, I learned:

* The purpose of public key cryptography
* The difference between symmetric and asymmetric encryption
* How RSA encryption works
* How Diffie-Hellman establishes shared secrets
* How SSH key authentication works
* How digital signatures verify authenticity and integrity
* How certificates establish trust on the Internet
* How PGP/GPG encrypts and signs files and emails

---

# Prerequisites

Before completing this room:

* Cryptography Basics
* Basic Linux command-line usage
* Understanding of modulo operations
* Understanding of symmetric encryption concepts

---

# Concepts Covered

## Authentication

Verifying the identity of a user, system, or service.

Example:

```text
Are you really communicating with the intended server?
```

---

## Authenticity

Ensuring that a message genuinely originates from the claimed sender.

Example:

```text
Did this file really come from Microsoft?
```

---

## Integrity

Ensuring data has not been altered.

Example:

```text
Has this file changed since it was signed?
```

---

## Confidentiality

Ensuring only authorized parties can access data.

Example:

```text
Can anyone else read this message?
```

---

## Symmetric Cryptography

Uses one shared key.

Examples:

* AES
* ChaCha20

Advantages:

* Fast
* Efficient

Disadvantages:

* Key distribution problem

---

## Asymmetric Cryptography

Uses:

* Public Key
* Private Key

Examples:

* RSA
* ECC
* Diffie-Hellman

Advantages:

* Solves key exchange problems
* Supports digital signatures

Disadvantages:

* Slower than symmetric encryption

---

# Public Key Cryptography Overview

## The Key Exchange Problem

Symmetric encryption requires both parties to know the same key.

Problem:

```text
How do two parties securely exchange that key?
```

Public key cryptography solves this problem.

---

## Analogy

| Analogy     | Cryptographic Equivalent |
| ----------- | ------------------------ |
| Secret Code | Symmetric Encryption Key |
| Lock        | Public Key               |
| Lock Key    | Private Key              |

Anyone can use the lock.

Only the owner can unlock it.

---

# RSA

## Purpose

Secure communication using public and private keys.

---

## Security Foundation

RSA relies on the difficulty of:

```text
Factoring large integers
```

Multiplication:

```text
Easy
```

Factorization:

```text
Difficult
```

---

## Important Variables

| Variable | Meaning                |
| -------- | ---------------------- |
| p        | Prime Number           |
| q        | Prime Number           |
| n        | p × q                  |
| φ(n)     | Euler Totient Function |
| e        | Public Exponent        |
| d        | Private Exponent       |
| m        | Plaintext              |
| c        | Ciphertext             |

---

## Key Generation

Public Key:

```text
(n, e)
```

Private Key:

```text
(n, d)
```

---

## Encryption

```text
c = m^e mod n
```

---

## Decryption

```text
m = c^d mod n
```

---

## Example

Given:

```text
p = 4391
q = 6659
```

Calculate:

```text
n = p × q
```

Result:

```text
n = 29239669
```

---

Calculate:

```text
φ(n) = (p−1)(q−1)
```

Result:

```text
φ(n) = 29228620
```

---

## Pentester Notes

### Enumeration

Look for:

* Weak certificates
* Weak RSA keys
* Old key sizes

### CTF Relevance

Common variables:

```text
p
q
n
e
d
c
m
```

---

# Diffie-Hellman Key Exchange

## Purpose

Establish a shared secret without transmitting it.

---

## Problem Solved

```text
How can two parties create the same secret key without sending it?
```

---

## Public Variables

```text
p
g
```

Known to everyone.

---

## Private Variables

```text
a
b
```

Known only to each party.

---

## Public Keys

Alice:

```text
A = g^a mod p
```

Bob:

```text
B = g^b mod p
```

---

## Shared Secret

Alice:

```text
K = B^a mod p
```

Bob:

```text
K = A^b mod p
```

Both produce:

```text
g^(ab) mod p
```

---

## Lab Exercise

### Given

```text
p = 29
g = 5
a = 12
b = 17
```

---

### Public Key A

```text
A = 5^12 mod 29
```

Result:

```text
A = 7
```

---

### Public Key B

```text
B = 5^17 mod 29
```

Result:

```text
B = 9
```

---

### Shared Secret

```text
9^12 mod 29
```

Result:

```text
24
```

---

### Verification

```text
7^17 mod 29
```

Result:

```text
24
```

Shared Secret:

```text
24
```

---

## Security Implications

Diffie-Hellman alone:

```text
Does NOT authenticate identities
```

Therefore:

```text
MITM attacks are possible
```

Without certificates or signatures.

---

# SSH

## Purpose

Secure remote access.

---

## Server Authentication

When connecting:

```bash
ssh user@host
```

SSH displays:

```text
Host fingerprint
```

Example:

```text
SHA256:...
```

---

## Why?

To detect:

```text
Man-In-The-Middle Attacks
```

---

# Client Authentication

Two methods:

## Password Authentication

```text
Username + Password
```

---

## Key Authentication

```text
Public Key
Private Key
```

Preferred.

---

# Generating SSH Keys

Command:

```bash
ssh-keygen -t ed25519
```

---

## Common Algorithms

| Algorithm | Purpose               |
| --------- | --------------------- |
| RSA       | Legacy standard       |
| DSA       | Digital signatures    |
| ECDSA     | Elliptic Curve DSA    |
| Ed25519   | Modern recommendation |

---

# Important Files

## Private Key

Examples:

```text
id_rsa
id_ed25519
```

Never share.

---

## Public Key

Examples:

```text
id_rsa.pub
id_ed25519.pub
```

Safe to distribute.

---

## Authorized Keys

```bash
~/.ssh/authorized_keys
```

Contains trusted public keys.

---

## Known Hosts

```bash
~/.ssh/known_hosts
```

Stores trusted server fingerprints.

---

# Task Solution

SSH Private Key Header:

```text
-----BEGIN RSA PRIVATE KEY-----
```

Answer:

```text
RSA
```

---

# Digital Signatures

## Purpose

Provide:

* Authenticity
* Integrity
* Non-Repudiation

---

## Process

Sender:

```text
Hash File
↓
Sign Hash
↓
Private Key
```

---

Receiver:

```text
Verify Signature
↓
Public Key
```

---

## Why Not Use Images?

A signature image can be copied.

Digital signatures require:

```text
Private Key Ownership
```

---

# Certificates

## Purpose

Bind:

```text
Identity
+
Public Key
```

---

## Problem Solved

Without certificates:

```text
How do we know the public key belongs to the real server?
```

---

## Certificate Authority (CA)

Trusted organization that issues certificates.

Examples:

* DigiCert
* Sectigo
* GlobalSign

---

## Chain of Trust

```text
Website Certificate
        ↓
Intermediate CA
        ↓
Root CA
        ↓
Browser Trust
```

---

# HTTPS Workflow

```text
Browser
    ↓
Receives Certificate
    ↓
Validates Chain
    ↓
Establishes TLS Session
```

---

# Task Solutions

### What does a remote web server use to prove itself?

Answer:

```text
certificate
```

---

### What would you use to get a free TLS certificate?

Answer:

```text
Let's Encrypt
```

---

# PGP and GPG

## PGP

Pretty Good Privacy

Used for:

* Email Encryption
* File Encryption
* Digital Signatures

---

## GPG

GNU Privacy Guard

Open-source implementation of OpenPGP.

---

# Generate Key Pair

Command:

```bash
gpg --full-gen-key
```

---

# Import Key

Command:

```bash
gpg --import tryhackme.key
```

Purpose:

```text
Import public/private key into GPG keyring
```

---

# List Keys

```bash
gpg --list-keys
```

---

# List Secret Keys

```bash
gpg --list-secret-keys
```

---

# Decrypt File

```bash
gpg --decrypt message.gpg
```

---

# Task Walkthrough

## Objective

Decrypt an encrypted GPG message.

---

## Files

```text
message.gpg
tryhackme.key
```

---

## Step 1

Import key.

```bash
gpg --import tryhackme.key
```

---

## Step 2

Decrypt file.

```bash
gpg --decrypt message.gpg
```

---

## Result

Recovered secret word:

```text
pineapple
```

---

# Questions and Answers

## Common Use of Asymmetric Encryption

Question:

```text
In the analogy presented, what real object is analogous to the public key?
```

Answer:

```text
Lock
```

---

## RSA

Question:

```text
What is n?
```

Answer:

```text
29239669
```

---

Question:

```text
What is φ(n)?
```

Answer:

```text
29228620
```

---

## Diffie-Hellman

Question:

```text
What is A?
```

Answer:

```text
7
```

---

Question:

```text
What is B?
```

Answer:

```text
9
```

---

Question:

```text
What is the shared key?
```

Answer:

```text
24
```

---

## SSH

Question:

```text
What algorithm does the SSH private key use?
```

Answer:

```text
RSA
```

---

## Certificates

Question:

```text
What does a remote web server use to prove itself?
```

Answer:

```text
certificate
```

---

Question:

```text
What provides free TLS certificates?
```

Answer:

```text
Let's Encrypt
```

---

## GPG

Question:

```text
What secret word was recovered?
```

Answer:

```text
pineapple
```

---

# Troubleshooting

## SSH Key Ignored

Problem:

```text
Bad permissions
```

Solution:

```bash
chmod 600 id_rsa
```

---

## GPG Cannot Decrypt

Problem:

```text
No secret key
```

Solution:

```bash
gpg --import keyfile
```

---

## Certificate Warning

Problem:

```text
Browser distrusts certificate
```

Causes:

* Expired certificate
* Self-signed certificate
* Invalid trust chain

---

# Pentester Notes

## Enumeration

Look for:

```bash
~/.ssh
~/.gnupg
```

---

## Persistence

Add public key to:

```bash
authorized_keys
```

---

## Looting

Search for:

```text
id_rsa
id_ed25519
*.gpg
*.asc
```

---

## Common Findings

* Private SSH keys
* GPG key backups
* Weak certificates
* Reused RSA keys

---

# Key Takeaways

* Public key cryptography solves key distribution problems.
* RSA relies on the difficulty of integer factorization.
* Diffie-Hellman establishes shared secrets without transmitting them.
* SSH uses public/private keys for authentication.
* Digital signatures provide authenticity and integrity.
* Certificates establish trust on the Internet.
* GPG enables secure file and email encryption.

---

# Skills Gained

* Understanding RSA fundamentals
* Understanding Diffie-Hellman
* Working with SSH keys
* Understanding certificates and PKI
* Using GPG for decryption
* Recognizing public/private key workflows
* Understanding digital signatures

---

# Next Learning Path

```text
Cryptography Basics
        ↓
Public Key Cryptography Basics
        ↓
Hashing Basics
```

Upcoming concepts:

* Hash Functions
* SHA256
* MD5
* Salting
* Password Storage
* HMAC
* Integrity Verification

---

# References

* TryHackMe: Public Key Cryptography Basics
* OpenSSH Documentation
* GnuPG Documentation
* OpenPGP Standard
* RFC 8017 (RSA)
* RFC 3526 (Diffie-Hellman)

---

# Tags

```text
#TryHackMe
#Cryptography
#PublicKeyCryptography
#RSA
#DiffieHellman
#SSH
#TLS
#Certificates
#DigitalSignatures
#GPG
#PGP
#CyberSecurity
#CTF
#Writeup
```
