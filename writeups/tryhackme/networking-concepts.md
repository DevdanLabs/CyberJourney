# TryHackMe - Networking Concepts

## Room Information

| Category | Details |
|----------|----------|
| Room Name | Networking Concepts |
| Platform | TryHackMe |
| Difficulty | Beginner |
| Focus Areas | OSI Model, TCP/IP Model, IP Addressing, Subnetting, TCP, UDP, Encapsulation, Telnet |
| Objective | Learn the fundamental networking concepts used in modern computer networks and cybersecurity |

---

# Introduction

This room introduces the fundamental concepts of computer networking. Understanding these concepts is essential for cybersecurity professionals because almost every attack, defense mechanism, and security tool relies on networking protocols.

The room covers:

- OSI Model
- TCP/IP Model
- IPv4 Addressing
- Subnetting
- Routing
- TCP and UDP
- Encapsulation
- Using Telnet to communicate with TCP services

---

# 1. OSI Model

The Open Systems Interconnection (OSI) Model is a conceptual framework created by ISO to standardize network communications.

The OSI Model contains seven layers.

| Layer | Name |
|---------|---------|
| 7 | Application |
| 6 | Presentation |
| 5 | Session |
| 4 | Transport |
| 3 | Network |
| 2 | Data Link |
| 1 | Physical |

---

## Mnemonic

To remember the layers from bottom to top:

```text
Please
Do
Not
Throw
Spinach
Pizza
Away
```

---

## Layer 1 – Physical

Responsible for transmitting bits over a physical medium.

Examples:

- Ethernet cables
- Fiber optic cables
- WiFi radio waves

---

## Layer 2 – Data Link

Provides communication between devices on the same network segment.

Examples:

- Ethernet (802.3)
- WiFi (802.11)

Important concept:

```text
MAC Address
```

Example:

```text
00:1A:2B:3C:4D:5E
```

---

## Layer 3 – Network

Responsible for:

- Logical addressing
- Routing
- Packet forwarding

Protocols:

- IP
- ICMP
- IPSec

---

## Layer 4 – Transport

Provides communication between applications.

Protocols:

- TCP
- UDP

---

## Layer 5 – Session

Responsible for establishing and maintaining communication sessions.

Examples:

- RPC
- NFS

---

## Layer 6 – Presentation

Responsible for:

- Encoding
- Compression
- Encryption

Examples:

- Unicode
- MIME
- JPEG
- PNG

---

## Layer 7 – Application

Provides services directly to applications.

Examples:

- HTTP
- HTTPS
- FTP
- DNS
- SMTP
- IMAP

---

# Why OSI Matters for Pentesters

Many security tools operate at specific OSI layers.

| Tool | Layer |
|--------|--------|
| Burp Suite | 7 |
| Wireshark | All |
| Nmap | 3 & 4 |
| Responder | 2 |
| FFUF | 7 |

---

# 2. TCP/IP Model

Unlike the OSI Model, the TCP/IP Model is actually implemented on the Internet.

---

## TCP/IP Layers

| TCP/IP Layer | Corresponding OSI Layers |
|--------------|--------------------------|
| Application | 5, 6, 7 |
| Transport | 4 |
| Internet | 3 |
| Link | 1, 2 |

---

## Layer Mapping

```text
OSI                  TCP/IP

Application     \
Presentation      > Application
Session         /

Transport        > Transport

Network          > Internet

Data Link       \
Physical         > Link
```

---

## Why TCP/IP Was Created

TCP/IP was designed by the U.S. Department of Defense (DoD) to create resilient communication networks.

A major advantage:

```text
Network can continue functioning even if parts fail.
```

---

# 3. IPv4 Addresses

Every device on a network requires a unique identifier.

Example:

```text
192.168.1.10
```

---

## IPv4 Structure

IPv4 contains:

```text
4 Octets
```

Each octet:

```text
0 - 255
```

Example:

```text
192.168.1.10
```

Contains:

```text
192
168
1
10
```

---

## IPv4 Size

IPv4 uses:

```text
32 bits
```

Maximum theoretical addresses:

```text
2^32
≈ 4.29 billion
```

---

# Network Address and Broadcast Address

Example subnet:

```text
192.168.1.0/24
```

Network Address:

```text
192.168.1.0
```

Broadcast Address:

```text
192.168.1.255
```

Valid Hosts:

```text
192.168.1.1
to
192.168.1.254
```

---

# Private IP Addresses

Memorize these ranges.

## Range 1

```text
10.0.0.0/8
```

---

## Range 2

```text
172.16.0.0/12
```

Equivalent:

```text
172.16.x.x
to
172.31.x.x
```

---

## Range 3

```text
192.168.0.0/16
```

---

# Public vs Private IP

Private:

```text
192.168.1.10
10.10.10.10
172.20.1.5
```

Public:

```text
8.8.8.8
1.1.1.1
104.16.132.229
```

---

# Routing

Routers operate at:

```text
Layer 3
```

Responsibilities:

- Reading destination IP addresses
- Selecting routes
- Forwarding packets

---

# 4. TCP and UDP

Both operate at:

```text
Layer 4
```

---

# Port Numbers

Ports identify applications running on a host.

Example:

```text
10.10.10.10:80
```

Meaning:

```text
Host = 10.10.10.10
Port = 80
```

---

## Port Range

Port numbers use:

```text
16 bits
```

Range:

```text
1 - 65535
```

Port 0 is reserved.

---

# UDP

User Datagram Protocol.

Characteristics:

- Connectionless
- Fast
- No delivery guarantee

Examples:

- DNS
- VoIP
- Streaming
- Gaming

---

## UDP Data Unit

```text
Datagram
```

Encapsulation:

```text
Application Data
↓
UDP Header
↓
UDP Datagram
```

---

# TCP

Transmission Control Protocol.

Characteristics:

- Reliable
- Ordered delivery
- Error detection
- Retransmission

Examples:

- HTTP
- HTTPS
- SSH
- FTP

---

## TCP Data Unit

```text
Segment
```

---

# TCP Three-Way Handshake

Step 1:

```text
SYN
```

Client requests connection.

---

Step 2:

```text
SYN-ACK
```

Server accepts request.

---

Step 3:

```text
ACK
```

Client confirms.

---

Result:

```text
Connection Established
```

---

# TCP vs UDP

| TCP | UDP |
|-------|-------|
| Reliable | Not Reliable |
| Connection-Oriented | Connectionless |
| Uses ACK | No ACK |
| Slower | Faster |
| Web Traffic | Streaming |

---

# 5. Encapsulation

Encapsulation is the process of adding protocol headers at each layer.

---

## Step 1

Application creates data.

```text
Hello
```

---

## Step 2

TCP adds its header.

```text
[TCP Header]
Hello
```

Result:

```text
TCP Segment
```

---

## Step 3

IP adds its header.

```text
[IP Header]
[TCP Header]
Hello
```

Result:

```text
IP Packet
```

---

## Step 4

Ethernet adds its header and trailer.

```text
[MAC Header]
[IP Header]
[TCP Header]
Hello
[Trailer]
```

Result:

```text
Frame
```

---

## Data Units by Layer

| Layer | Data Unit |
|---------|---------|
| Application | Data |
| TCP | Segment |
| UDP | Datagram |
| IP | Packet |
| Data Link | Frame |
| Physical | Bits |

---

# Life of a Packet

Example:

User visits:

```text
https://tryhackme.com
```

Process:

```text
Browser
↓
HTTP Request
↓
TCP Segment
↓
IP Packet
↓
Ethernet Frame
↓
Router
↓
Internet
↓
Destination Server
```

The reverse process happens on the receiving side.

---

# 6. Telnet

TELNET allows communication with any TCP service.

Command:

```bash
telnet IP PORT
```

---

# Echo Server

Port:

```text
7
```

Command:

```bash
telnet MACHINE_IP 7
```

Example:

```text
Hi
Hi

Hello
Hello
```

The server returns everything received.

---

# Daytime Server

Port:

```text
13
```

Command:

```bash
telnet MACHINE_IP 13
```

Output:

```text
Thu Jun 20 12:36:32 PM UTC 2024
```

Purpose:

Returns current date and time.

---

# HTTP Server

Port:

```text
80
```

Connection:

```bash
telnet MACHINE_IP 80
```

Request:

```http
GET / HTTP/1.1
Host: telnet.thm

```

Note:

Press Enter twice after the Host header.

---

# Room Task

## Question 1

Use telnet to connect to:

```text
10.48.173.178
```

Determine the web server software and version.

---

## Solution

Connect:

```bash
telnet 10.48.173.178 80
```

Send:

```http
GET / HTTP/1.1
Host: telnet.thm

```

Output:

```http
HTTP/1.1 200 OK
Content-Type: text/html
ETag: "2920831920"
Last-Modified: Thu, 20 Jun 2024 12:39:38 GMT
Content-Length: 20
Accept-Ranges: bytes
Date: Wed, 03 Jun 2026 10:39:17 GMT
Server: lighttpd/1.4.63

THM{TELNET_MASTER}
```

---

## Reading the Output

### Status Line

```http
HTTP/1.1 200 OK
```

Meaning:

- HTTP/1.1 = protocol version
- 200 = success
- OK = request succeeded

---

### Content-Type

```http
Content-Type: text/html
```

Meaning:

Response contains HTML.

---

### Content-Length

```http
Content-Length: 20
```

Meaning:

Response size is 20 bytes.

---

### Server Header

```http
Server: lighttpd/1.4.63
```

Meaning:

- Web Server = lighttpd
- Version = 1.4.63

This is an example of:

```text
Banner Grabbing
```

which is commonly used during reconnaissance.

---

## Answer 1

```text
lighttpd/1.4.63
```

---

## Answer 2

```text
THM{TELNET_MASTER}
```

---

# Challenges Encountered

## 1. Understanding Private vs Public IP Addresses

Problem:

Initially assumed that any IP ending with values between 0 and 255 was private.

Reality:

All IPv4 octets range from:

```text
0-255
```

Private IP addresses are only:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

Everything else is generally public.

---

## 2. Understanding 172.16.x.x to 172.31.x.x

Problem:

Why does the range stop at 31?

Solution:

Because RFC1918 reserves:

```text
172.16.0.0/12
```

which translates to:

```text
172.16.x.x
through
172.31.x.x
```

---

## 3. Forgetting the Blank Line in HTTP Requests

Problem:

Server does not respond.

Cause:

Missing empty line after:

```http
Host: telnet.thm
```

Solution:

Press Enter twice.

---

# Key Takeaways

- OSI is a conceptual model with 7 layers.
- TCP/IP is the practical model used on the Internet.
- IPv4 uses 32-bit addresses.
- Memorize private IP ranges.
- TCP provides reliable communication.
- UDP provides faster connectionless communication.
- Encapsulation wraps data with headers at each layer.
- Telnet can be used to manually communicate with TCP services.
- HTTP requests can be crafted manually through Telnet.
- Banner grabbing reveals service names and versions.
- Understanding networking fundamentals is essential for enumeration and penetration testing.