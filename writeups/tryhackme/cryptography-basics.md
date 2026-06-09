# Cryptography Basics - TryHackMe Writeup

## Executive Summary

This room introduces the fundamental concepts of cryptography, the science of securing information against unauthorized access and modification. The room covers the importance of cryptography in modern computing, essential cryptographic terminology, historical ciphers such as the Caesar Cipher, symmetric and asymmetric encryption, and foundational mathematical concepts including XOR and modulo operations.

Cryptography is one of the foundational pillars of cybersecurity. It enables secure communication, protects sensitive information, verifies authenticity, and ensures data integrity. Nearly every modern security technology—including HTTPS, SSH, VPNs, digital certificates, secure messaging, wireless security, and cryptocurrencies—relies heavily on cryptographic principles.

---

# Learning Objectives

By completing this room, I learned:

- Core cryptographic terminology
- Why cryptography is essential in modern systems
- How Caesar Cipher works
- Differences between symmetric and asymmetric encryption
- Common cryptographic algorithms
- Basic cryptographic mathematics
- XOR operation
- Modulo arithmetic
- Cryptography relevance in cybersecurity

---

# Prerequisites

- Basic Linux knowledge
- Basic networking understanding
- Familiarity with binary numbers (helpful but not required)

---

# What is Cryptography?

Cryptography is the practice and study of techniques used to secure information and communication in the presence of adversaries.

Its primary goal is to protect data from:

- Unauthorized disclosure
- Unauthorized modification
- Impersonation
- Tampering

Modern cryptography provides:

| Security Property | Purpose |
|------------------|----------|
| Confidentiality | Prevent unauthorized access |
| Integrity | Prevent unauthorized modification |
| Authentication | Verify identities |
| Non-Repudiation | Prevent denial of actions |

---

# Why Cryptography Matters

Without cryptography:

```text
Username: devdan
Password: secret123
```

would travel across the internet in plain text.

Anyone capable of intercepting network traffic could read:

- Passwords
- Banking information
- Emails
- Medical records
- Corporate secrets

Cryptography enables secure communication over untrusted networks.

---

# Real-World Uses of Cryptography

## HTTPS

Protects website communication.

Used for:

- Logins
- Payments
- Secure browsing

---

## SSH

Encrypts remote administrative sessions.

Example:

```bash
ssh user@server
```

---

## VPN

Creates encrypted tunnels between systems.

---

## Wi-Fi Security

WPA2/WPA3 use cryptography to secure wireless traffic.

---

## Online Banking

Uses certificates and encryption to verify identity and protect transactions.

---

## Software Downloads

Hashes verify file integrity.

---

# Cryptography Terminology

---

## Plaintext

Original readable data before encryption.

Examples:

```text
Hello World
```

```text
Password123
```

```text
Credit Card Information
```

---

## Ciphertext

Encrypted unreadable version of data.

Example:

```text
A8F92C81D0E7...
```

---

## Cipher

Algorithm that performs encryption and decryption.

Examples:

- Caesar Cipher
- AES
- RSA
- ECC

---

## Key

Secret value used by cryptographic algorithms.

Important:

```text
Cipher = Public
Key = Secret
```

Modern cryptography assumes attackers know the algorithm.

Security relies on protecting the key.

---

## Encryption

Process of converting plaintext into ciphertext.

```text
Plaintext
    ↓
Encrypt
    ↓
Ciphertext
```

---

## Decryption

Process of converting ciphertext back into plaintext.

```text
Ciphertext
    ↓
Decrypt
    ↓
Plaintext
```

---

# Encryption Workflow

## Encryption

```text
Plaintext + Key
        ↓
    Encryption
        ↓
    Ciphertext
```

---

## Decryption

```text
Ciphertext + Key
         ↓
     Decryption
         ↓
      Plaintext
```

---

# Historical Ciphers

Cryptography dates back thousands of years.

Examples include:

- Caesar Cipher
- Vigenère Cipher
- Enigma Machine
- One-Time Pad

---

# Caesar Cipher

One of the oldest encryption techniques.

Attributed to Julius Caesar.

---

## How It Works

Each letter is shifted by a fixed number.

Example:

```text
Key = 3
```

Shift letters three positions right.

---

### Example

Plaintext:

```text
TRYHACKME
```

Key:

```text
3
```

Encryption:

```text
T → W
R → U
Y → B
H → K
A → D
C → F
K → N
M → P
E → H
```

Ciphertext:

```text
WUBKDFNPH
```

---

# Decryption

Shift left by the same amount.

Ciphertext:

```text
WUBKDFNPH
```

Key:

```text
3
```

Recovered plaintext:

```text
TRYHACKME
```

---

# Weakness of Caesar Cipher

Only 25 possible keys exist.

An attacker can try all keys.

Example:

```text
Key 1
Key 2
Key 3
...
Key 25
```

This is called:

## Brute Force Attack

Trying every possible key until the correct plaintext appears.

---

# Example Challenge

Ciphertext:

```text
XRPCTCRGNEI
```

Recovered plaintext:

```text
ICANENCRYPT
```

---

# Cryptography and Key Space

The larger the key space:

```text
More possible keys
=
More difficult brute force
```

---

## Caesar Cipher

```text
25 keys
```

---

## AES-128

```text
2^128 keys
```

Approximately:

```text
340,282,366,920,938,463,463,374,607,431,768,211,456
```

possible keys.

Brute forcing becomes practically impossible.

---

# Types of Encryption

Two primary categories exist:

1. Symmetric Encryption
2. Asymmetric Encryption

---

# Symmetric Encryption

Uses the same key for:

- Encryption
- Decryption

---

## Workflow

```text
Alice
  ↓
Encrypt
  ↓
Shared Secret Key
  ↓
Ciphertext
  ↓
Bob
  ↓
Decrypt
  ↓
Shared Secret Key
```

---

## Formula

```text
Encryption Key
=
Decryption Key
```

---

# Advantages

- Very fast
- Efficient
- Suitable for large data

---

# Disadvantages

## Key Distribution Problem

How do two parties securely share the secret key?

Example:

Sending:

```text
secret.zip
```

and

```text
password
```

through the same channel defeats security.

---

# Common Symmetric Algorithms

---

## DES

Data Encryption Standard

- Adopted: 1977
- Key Size: 56-bit

No longer secure.

---

## 3DES

Triple DES.

- Effective Security: 112-bit

Deprecated in 2019.

---

## AES

Advanced Encryption Standard.

Current industry standard.

Key sizes:

- AES-128
- AES-192
- AES-256

Used in:

- HTTPS
- VPNs
- WPA2
- BitLocker
- VeraCrypt

---

# Asymmetric Encryption

Uses two keys.

---

## Public Key

Shared publicly.

---

## Private Key

Kept secret.

---

## Workflow

```text
Alice
    ↓
Encrypt
    ↓
Bob's Public Key
    ↓
Ciphertext
    ↓
Bob
    ↓
Decrypt
    ↓
Bob's Private Key
```

---

# Advantages

Solves the key distribution problem.

Public key can be shared openly.

---

# Disadvantages

Slower than symmetric encryption.

---

# Common Asymmetric Algorithms

---

## RSA

Typical key sizes:

- 2048-bit
- 3072-bit
- 4096-bit

---

## Diffie-Hellman

Used primarily for:

```text
Key Exchange
```

---

## ECC

Elliptic Curve Cryptography.

Provides strong security with smaller keys.

Example:

```text
ECC 256-bit
≈
RSA 3072-bit
```

---

# Why HTTPS Uses Both

HTTPS combines:

## Asymmetric Cryptography

Used for:

```text
Secure Key Exchange
```

---

## Symmetric Cryptography

Used for:

```text
Encrypting Data
```

because it is much faster.

---

## Simplified HTTPS Flow

```text
Client
   ↓
RSA/ECC Handshake
   ↓
Shared Secret
   ↓
AES Session Key
   ↓
Encrypted Communication
```

---

# Cryptographic Mathematics

Modern cryptography relies heavily on mathematics.

This room introduced:

- XOR
- Modulo

---

# XOR Operation

XOR = Exclusive OR

Symbol:

```text
⊕
```

or

```text
^
```

---

## XOR Truth Table

| A | B | A ⊕ B |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

---

# XOR Example

```text
1010
1100
----
0110
```

Result:

```text
0110
```

---

# Important XOR Properties

---

## Identity

```text
A ⊕ 0 = A
```

---

## Self Cancellation

```text
A ⊕ A = 0
```

---

## Commutative

```text
A ⊕ B = B ⊕ A
```

---

## Associative

```text
(A ⊕ B) ⊕ C
=
A ⊕ (B ⊕ C)
```

---

# XOR as Encryption

Let:

```text
P = Plaintext
K = Key
```

Encryption:

```text
C = P ⊕ K
```

Decryption:

```text
P = C ⊕ K
```

Because:

```text
K ⊕ K = 0
```

---

# Red Team Relevance

XOR is commonly used in:

- Malware obfuscation
- Payload encoding
- Shellcode hiding
- Reverse engineering challenges

---

# Modulo Operation

Modulo returns the remainder after division.

Notation:

```text
%
```

or

```text
mod
```

---

# Examples

---

## Example 1

```text
25 % 5 = 0
```

Because:

```text
25 = 5 × 5 + 0
```

---

## Example 2

```text
23 % 6 = 5
```

Because:

```text
23 = 3 × 6 + 5
```

---

## Example 3

```text
23 % 7 = 2
```

Because:

```text
23 = 3 × 7 + 2
```

---

# Modulo and Circular Arithmetic

Think of a clock.

```text
11 + 3 = 14
```

But on a clock:

```text
14 mod 12 = 2
```

---

# Why Modulo Matters

Modulo is heavily used in:

- RSA
- Diffie-Hellman
- ECC
- Digital Signatures

---

# Important Property

If:

```text
x % 5 = 4
```

Possible values include:

```text
4
9
14
19
24
...
```

Therefore:

```text
Modulo is not reversible
```

---

# Cybersecurity Relevance

---

# Red Team Perspective

Understanding cryptography helps identify:

- Weak TLS configurations
- Weak cipher suites
- Deprecated algorithms
- Poor key management
- Weak certificates

Tools:

```bash
nmap --script ssl-enum-ciphers
```

```bash
openssl s_client
```

```bash
testssl.sh
```

---

# Blue Team Perspective

Responsibilities include:

- Strong encryption deployment
- Certificate management
- Secure key storage
- Key rotation
- TLS hardening

---

# Detection Perspective

Security analysts frequently monitor:

- Expired certificates
- Self-signed certificates
- Weak TLS versions
- Deprecated ciphers
- Certificate mismatches

---

# Pentester Notes

## Enumeration Value

Identify:

- Supported cipher suites
- TLS versions
- Certificate details

---

## Exploitation Relevance

Weak cryptography can enable:

- MITM attacks
- Traffic decryption
- Authentication bypass

---

## Persistence Relevance

Compromised keys may provide long-term access.

---

## Detection Opportunities

Indicators include:

- TLS downgrade attempts
- Invalid certificates
- Legacy encryption protocols

---

# Skills Gained

- Cryptography fundamentals
- Encryption concepts
- Caesar Cipher analysis
- Symmetric encryption understanding
- Asymmetric encryption understanding
- XOR operations
- Modulo arithmetic
- Cryptographic terminology
- Security implications of cryptography

---

# Key Takeaways

- Cryptography protects confidentiality, integrity, and authenticity.
- Security depends on protecting keys, not hiding algorithms.
- Caesar Cipher demonstrates encryption concepts but is insecure.
- Symmetric encryption uses one shared key.
- Asymmetric encryption uses public/private key pairs.
- HTTPS combines asymmetric and symmetric cryptography.
- XOR is a fundamental cryptographic operation.
- Modulo arithmetic is heavily used in modern cryptography.
- AES is the modern symmetric encryption standard.
- RSA and ECC are foundational asymmetric algorithms.

---

# Conclusion

Cryptography is one of the most important foundations of cybersecurity. It enables secure communication across untrusted networks, protects sensitive information, and provides mechanisms for authentication and integrity verification. This room introduced the essential terminology, historical development, encryption categories, and mathematical foundations required to understand modern cryptographic systems. These concepts form the basis for more advanced topics such as public key cryptography, digital signatures, TLS/HTTPS, hashing, VPN technologies, and secure communications used throughout the cybersecurity industry.

---

# Tags

```text
#TryHackMe
#Cryptography
#Encryption
#AES
#RSA
#ECC
#DES
#3DES
#XOR
#Modulo
#Cybersecurity
#BlueTeam
#RedTeam
#Networking
#TLS
#HTTPS
#SecurityFundamentals
```