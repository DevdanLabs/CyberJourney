# Networking Secure Protocols - Writeup

## Executive Summary

This room introduced the fundamental technologies used to secure network communications. In previous networking rooms, protocols such as HTTP, SMTP, POP3, IMAP, and TELNET were studied in their original plaintext form. While these protocols successfully deliver data, they do not provide confidentiality, integrity, or authenticity.

This room explored three primary approaches used to secure network traffic:

1. **TLS (Transport Layer Security)** – Adds encryption and authentication to existing protocols such as HTTP, SMTP, POP3, and IMAP.
2. **SSH (Secure Shell)** – Provides secure remote administration, file transfer, tunneling, and graphical application forwarding.
3. **VPN (Virtual Private Network)** – Creates encrypted tunnels across untrusted networks, allowing secure communication between remote users, offices, and internal networks.

The room concluded with a practical Wireshark challenge involving TLS decryption using a browser-generated SSL key log file.

---

# Learning Objectives

After completing this room, I was able to:

* Understand the purpose of SSL and TLS
* Explain confidentiality, integrity, and authenticity
* Differentiate HTTP and HTTPS
* Understand certificate authorities and digital certificates
* Explain SMTPS, POP3S, and IMAPS
* Understand SSH and how it replaced TELNET
* Explain SFTP and FTPS
* Understand VPN concepts and use cases
* Decrypt TLS traffic in Wireshark using SSL session keys

---

# Prerequisites

* Networking Concepts
* Networking Essentials
* Networking Core Protocols

---

# Concepts Covered

## Confidentiality

Ensures that only authorized parties can read transmitted data.

Example:

```text
Username
Password
Credit Card Number
```

Without encryption, anyone monitoring the network can read this information.

---

## Integrity

Ensures data is not modified during transmission.

Example:

```text
£100 payment
```

could be altered to:

```text
£800 payment
```

if integrity protection does not exist.

---

## Authenticity

Ensures that we are communicating with the legitimate server and not an attacker.

Example:

```text
bank.com
```

must actually be the bank's server.

---

# SSL and TLS

## What is TLS?

TLS (Transport Layer Security) is a cryptographic protocol that provides:

* Confidentiality
* Integrity
* Authentication

for network communications.

TLS operates between:

```text
Application Layer
        ↑
       TLS
        ↑
Transport Layer (TCP)
```

---

## Evolution

| Year | Protocol |
| ---- | -------- |
| 1995 | SSL 2.0  |
| 1996 | SSL 3.0  |
| 1999 | TLS 1.0  |
| 2018 | TLS 1.3  |

TLS is the modern replacement for SSL.

---

## Why TLS Exists

Without TLS:

```text
Client
   |
Internet
   |
Server
```

Attackers can:

* Read traffic
* Modify traffic
* Impersonate servers

TLS prevents these attacks.

---

# Certificates and Certificate Authorities

## Certificate Signing Request (CSR)

A server generates a:

```text
CSR
```

and submits it to a Certificate Authority.

---

## Certificate Authority (CA)

A CA verifies ownership and signs certificates.

Examples:

* DigiCert
* GlobalSign
* Let's Encrypt

---

## Let's Encrypt

Provides free TLS certificates.

Benefits:

* Free
* Automated
* Widely trusted

---

## Self-Signed Certificates

Can encrypt traffic but cannot prove authenticity.

Problem:

```text
Anyone can create one.
```

No trusted third party validates it.

---

# HTTP vs HTTPS

## HTTP

Default Port:

```text
80
```

Communication flow:

```text
TCP Handshake
      ↓
HTTP Request
      ↓
HTTP Response
```

Everything is plaintext.

Example:

```http
GET / HTTP/1.1
Host: example.com
```

Visible to attackers.

---

## HTTPS

Default Port:

```text
443
```

Communication flow:

```text
TCP Handshake
      ↓
TLS Handshake
      ↓
Encrypted HTTP Traffic
```

---

## Wireshark Observation

### HTTP

Visible:

```text
GET
POST
Cookies
Passwords
HTML
```

---

### HTTPS

Visible:

```text
TLSv1.3 Application Data
```

Contents hidden.

---

# TLS Decryption in Wireshark

Normally:

```text
HTTPS
```

appears as encrypted application data.

However, if TLS session keys are available:

```text
ssl-key.log
```

Wireshark can decrypt traffic.

---

## Browser SSL Key Logging

Chromium was launched using:

```bash
chromium --ssl-key-log-file=~/ssl-key.log
```

This saves TLS session secrets.

---

## Configuring Wireshark

Navigate to:

```text
Edit
 └── Preferences
      └── Protocols
           └── TLS
```

Set:

```text
(Pre)-Master-Secret log filename
```

to:

```text
~/Documents/ssl-key.log
```

---

## Result

Encrypted traffic becomes readable.

Visible:

```http
POST /login
email=user@example.com
password=secret
```

---

# Secure Email Protocols

Adding TLS works exactly like HTTPS.

---

## Default Ports

| Protocol | Port |
| -------- | ---- |
| HTTP     | 80   |
| SMTP     | 25   |
| POP3     | 110  |
| IMAP     | 143  |

---

## Secure Versions

| Protocol | Port      |
| -------- | --------- |
| HTTPS    | 443       |
| SMTPS    | 465 / 587 |
| POP3S    | 995       |
| IMAPS    | 993       |

---

## Benefits

* Confidentiality
* Integrity
* Authentication

---

# SSH (Secure Shell)

## Why SSH Exists

TELNET sends:

```text
Username
Password
Commands
```

in plaintext.

Attackers can capture everything.

---

## SSH Features

### Secure Authentication

Supports:

* Passwords
* Public Keys
* Multi-Factor Authentication

---

### Confidentiality

Traffic is encrypted.

---

### Integrity

Traffic tampering is detected.

---

### Tunneling

Allows secure forwarding of other protocols.

---

### X11 Forwarding

Run graphical applications remotely.

Example:

```bash
ssh 192.168.124.148 -X
```

Then:

```bash
wireshark
```

runs remotely while displaying locally.

---

## SSH Default Port

```text
22
```

---

## TELNET Default Port

```text
23
```

---

# SFTP vs FTPS

Many beginners confuse these.

---

## SFTP

### Meaning

```text
SSH File Transfer Protocol
```

### Port

```text
22
```

### Based On

```text
SSH
```

### Example

```bash
sftp user@host
```

Download:

```bash
get file.txt
```

Upload:

```bash
put file.txt
```

---

## FTPS

### Meaning

```text
FTP Secure
```

### Port

```text
990
```

### Based On

```text
TLS
```

### Requirements

* TLS Certificate
* FTP Infrastructure

More complex than SFTP.

---

## Comparison

| Feature              | SFTP | FTPS     |
| -------------------- | ---- | -------- |
| Based on             | SSH  | TLS      |
| Port                 | 22   | 990      |
| Certificate Required | No   | Yes      |
| Firewall Friendly    | Yes  | Often No |

---

# VPN (Virtual Private Network)

## What is a VPN?

VPN stands for:

```text
Virtual Private Network
```

---

## Virtual

No dedicated physical connection is required.

Uses:

```text
Internet
```

as transport.

---

## Private

Traffic is encrypted.

Others cannot read the contents.

---

# Site-to-Site VPN

Connects:

```text
Network ↔ Network
```

Example:

```text
Head Office
     |
Internet
   /   \
Branch1 Branch2
```

All offices behave like one network.

---

# Remote Access VPN

Connects:

```text
Device ↔ Network
```

Example:

```text
Laptop
   |
 VPN
   |
Company Network
```

Used by remote workers.

---

# VPN Tunnel

Traffic flow:

```text
Client
   ↓
Encrypt
   ↓
Internet
   ↓
Decrypt
   ↓
Destination
```

---

# OpenVPN Example

Command:

```bash
sudo openvpn file.ovpn
```

---

Output:

```text
Initialization Sequence Completed
```

Meaning:

```text
VPN Tunnel Established
```

---

## Virtual Interface

OpenVPN creates:

```text
tun0
```

Check:

```bash
ip a
```

Example:

```text
tun0
10.80.0.30
```

---

# DNS Leak

A common VPN issue.

Example:

```text
Web Traffic → VPN
DNS Queries → ISP
```

The ISP can still see visited domains.

Use DNS leak tests to verify protection.

---

# Challenge Walkthrough

## Objective

Recover login credentials from encrypted HTTPS traffic.

---

## Step 1

Open:

```text
randy-chromium.pcapng
```

---

## Step 2

Load:

```text
ssl-key.log
```

through:

```text
Edit
 └── Preferences
      └── TLS
```

---

## Step 3

Apply filter:

```text
http2
```

---

## Step 4

Locate:

```text
POST /login
```

packet.

Observed:

```text
POST /login/?privacy_mutation_token=...
```

---

## Step 5

Follow HTTP2 Stream.

Observed credentials:

```text
email=strategos@networking.thm
pass=THM%7BB8WM6P%7D
```

---

## Step 6

Decode URL Encoding:

```text
%7B = {
%7D = }
```

Result:

```text
THM{B8WM6P}
```

---

# Challenge Answer

```text
THM{B8WM6P}
```

---

# Commands Used

## SSH

```bash
ssh user@host
```

Purpose:

Secure remote administration.

---

## SSH with X11 Forwarding

```bash
ssh host -X
```

Purpose:

Run graphical applications remotely.

---

## SFTP

```bash
sftp user@host
```

Purpose:

Secure file transfer.

---

## OpenVPN

```bash
sudo openvpn file.ovpn
```

Purpose:

Establish VPN connection.

---

## IP Address Information

```bash
ip a
```

Purpose:

Display network interfaces.

---

# Pentester Notes

## TLS

### Reconnaissance

Limited visibility due to encryption.

### Detection

TLS inspection can reveal:

* SNI
* Certificates
* Metadata

### Common Misconfiguration

* Self-signed certificates
* Weak cipher suites
* Expired certificates

---

## SSH

### Enumeration

```bash
nmap -sV -p22 target
```

reveals SSH version.

### Misconfigurations

* Weak passwords
* Password authentication enabled
* Outdated OpenSSH

---

## VPN

### Internal Pentesting

Organizations often provide VPN access before an assessment.

After VPN access:

```bash
nmap
enum4linux
bloodhound
crackmapexec
```

become possible.

---

# Skills Gained

* TLS fundamentals
* Certificate validation
* HTTPS analysis
* TLS decryption in Wireshark
* Secure email protocols
* SSH administration
* SFTP and FTPS comparison
* VPN concepts
* OpenVPN usage
* Traffic analysis

---

# Key Takeaways

* TLS secures existing protocols without modifying TCP/IP.
* HTTPS is simply HTTP running over TLS.
* SSH replaces insecure TELNET.
* SFTP and FTPS are different technologies.
* VPN creates encrypted tunnels across untrusted networks.
* Possession of TLS session keys allows HTTPS traffic decryption.
* Endpoint security remains critical even when encryption is used.

---

# Room Completion

```text
Networking Concepts
        ↓
Networking Essentials
        ↓
Networking Core Protocols
        ↓
Networking Secure Protocols ✓
```

---

# References

* RFC 8446 (TLS 1.3)
* OpenSSH Documentation
* OpenVPN Documentation
* Wireshark User Guide
* Let's Encrypt Documentation
* TryHackMe Networking Secure Protocols

---

# Tags

```text
#Networking
#TLS
#HTTPS
#SSL
#SSH
#SFTP
#FTPS
#VPN
#OpenVPN
#Wireshark
#NetworkSecurity
#TryHackMe
#CyberSecurity
```
