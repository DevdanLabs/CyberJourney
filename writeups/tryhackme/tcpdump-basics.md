# Tcpdump: The Basics — Complete Writeup

## Executive Summary

This room introduced the fundamentals of **Tcpdump**, one of the most widely used command-line packet capture and network analysis tools. Throughout this room, we learned how to capture network traffic, save and read packet capture files (PCAP), apply filtering expressions, analyze protocols, inspect packet contents, and use advanced filters based on TCP flags and packet lengths.

By the end of this room, we were able to:

* Capture packets from specific network interfaces
* Save packet captures to `.pcap` files
* Read packet captures offline
* Filter traffic by host, port, protocol, packet size, and TCP flags
* Display packets in different formats (brief, ASCII, hexadecimal)
* Analyze ARP, DNS, ICMP, and TCP traffic
* Apply packet analysis techniques commonly used by network engineers, SOC analysts, incident responders, and penetration testers

---

# Learning Objectives

* Understand what Tcpdump is and why it is important
* Capture packets from a network interface
* Save and load packet captures
* Filter traffic effectively
* Analyze packet contents
* Use advanced filtering expressions
* Understand TCP flag analysis
* Investigate network traffic using packet captures

---

# Prerequisites

Recommended prerequisite knowledge:

* TCP/IP Model
* Networking Concepts
* Networking Essentials
* Networking Core Protocols
* Networking Secure Protocols

---

# What is Tcpdump?

Tcpdump is a command-line packet analyzer used to capture and inspect network traffic.

Think of Tcpdump as a CCTV camera for network traffic.

```text
Network Traffic
       │
       ▼
    Tcpdump
       │
       ▼
 Packet Analysis
```

Instead of seeing only applications and websites, Tcpdump allows us to observe the actual packets being exchanged between hosts.

---

# Why Tcpdump Exists

Without packet captures, troubleshooting networking problems becomes guesswork.

Example:

```text
Website not loading
```

Possible causes:

* DNS failure
* TCP handshake failure
* TLS negotiation failure
* Firewall blockage
* Routing issues

Tcpdump provides direct visibility into network communications.

---

# Tcpdump and Wireshark

Relationship:

```text
libpcap
 │
 ├── Tcpdump
 │
 └── Wireshark
```

Typical workflow:

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

Then:

```text
Open capture.pcap in Wireshark
```

---

# Core Concepts

## Network Interface

A network interface is a communication endpoint.

Examples:

```text
lo      Loopback
eth0    Ethernet
ens5    Ethernet
wlan0   WiFi
```

View interfaces:

```bash
ip a s
```

Example output:

```text
1: lo
2: ens5
```

---

# Basic Packet Capture

## Capture Traffic from a Specific Interface

### Command

```bash
sudo tcpdump -i ens5
```

### Breakdown

| Option | Purpose           |
| ------ | ----------------- |
| -i     | Specify interface |
| ens5   | Interface name    |

### Example Output

```text
IP 10.10.1.10 > 8.8.8.8: DNS Query
```

### Interpretation

Traffic from:

```text
10.10.1.10
```

to:

```text
8.8.8.8
```

---

## Capture on All Interfaces

```bash
sudo tcpdump -i any
```

---

# Saving Captures

## Save Packets to File

### Command

```bash
sudo tcpdump -i ens5 -w capture.pcap
```

### Purpose

Stores captured packets for later analysis.

### Output

No scrolling packet output appears because Tcpdump writes directly to the file.

---

# Reading Captures

## Read Existing PCAP Files

### Command

```bash
tcpdump -r capture.pcap
```

### Purpose

Analyze previously captured traffic.

### Example

```bash
tcpdump -r traffic.pcap
```

---

# Limiting Packet Count

## Capture Only Specific Number of Packets

### Command

```bash
tcpdump -c 10
```

### Purpose

Stop automatically after 10 packets.

---

# Numeric Output

## Disable DNS Resolution

```bash
tcpdump -n
```

Example:

Instead of:

```text
dns.google
```

Shows:

```text
8.8.8.8
```

---

## Disable DNS and Port Resolution

```bash
tcpdump -nn
```

Instead of:

```text
https
```

Shows:

```text
443
```

---

# Verbose Output

## More Details

```bash
tcpdump -v
```

More verbose:

```bash
tcpdump -vv
```

Maximum verbosity:

```bash
tcpdump -vvv
```

---

# Packet Filtering

Filtering is the most important Tcpdump skill.

---

# Filtering by Host

## Any Traffic To/From Host

```bash
tcpdump host example.com
```

or

```bash
tcpdump host 192.168.1.10
```

---

## Source Host

```bash
tcpdump src host 192.168.1.10
```

---

## Destination Host

```bash
tcpdump dst host 192.168.1.10
```

---

# Filtering by Port

## Port 53 (DNS)

```bash
tcpdump port 53
```

---

## Source Port

```bash
tcpdump src port 53
```

---

## Destination Port

```bash
tcpdump dst port 53
```

---

# Filtering by Protocol

## TCP

```bash
tcpdump tcp
```

---

## UDP

```bash
tcpdump udp
```

---

## ICMP

```bash
tcpdump icmp
```

---

## IPv4

```bash
tcpdump ip
```

---

## IPv6

```bash
tcpdump ip6
```

---

# Logical Operators

## AND

```bash
tcpdump host 1.1.1.1 and tcp
```

---

## OR

```bash
tcpdump udp or icmp
```

---

## NOT

```bash
tcpdump not tcp
```

---

# Advanced Filtering

## Packet Length

### Larger Than

```bash
tcpdump greater 1500
```

---

### Smaller Than

```bash
tcpdump less 100
```

---

# Binary Operations

Tcpdump supports binary operators:

```text
&
|
!
```

Used heavily when analyzing TCP flags.

---

# TCP Flags

Common TCP flags:

| Flag | Meaning          |
| ---- | ---------------- |
| SYN  | Start connection |
| ACK  | Acknowledge      |
| FIN  | Close connection |
| RST  | Reset connection |
| PSH  | Push data        |

---

# Filtering TCP Flags

## Only SYN

```bash
tcpdump "tcp[tcpflags] == tcp-syn"
```

---

## Has SYN Set

```bash
tcpdump "tcp[tcpflags] & tcp-syn != 0"
```

---

## Has SYN or ACK

```bash
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"
```

---

# Packet Display Options

## Quick Output

### Command

```bash
tcpdump -q
```

### Output

```text
104.18.12.149.https > g5000.45248: tcp 25
```

---

# Display Ethernet Header

### Command

```bash
tcpdump -e
```

### Shows

```text
Source MAC
Destination MAC
```

Example:

```text
52:54:00:7c:d3:5b > ff:ff:ff:ff:ff:ff
```

---

# Display ASCII Data

### Command

```bash
tcpdump -A
```

Useful for plaintext protocols.

Example:

```http
GET / HTTP/1.1
Host: example.com
```

---

# Display Hexadecimal Data

### Command

```bash
tcpdump -xx
```

Example:

```text
0x0000: 0800 4500
```

---

# Display Hex + ASCII

### Command

```bash
tcpdump -X
```

Provides both raw bytes and readable characters.

---

# Task Walkthroughs

---

## Task: Basic Packet Capture

### Question

What option can you add to your command to display addresses only in numeric format?

### Analysis

Review section:

```text
Don't Resolve IP Addresses
```

### Answer

```text
-n
```

---

## Task: Filtering Expressions

### Question 1

How many packets use ICMP?

### Method

Filter ICMP packets.

### Command

```bash
tcpdump -r traffic.pcap icmp -n | wc
```

### Output

```text
26
```

### Answer

```text
26
```

---

### Question 2

What host requested the MAC address of 192.168.124.137?

### Method

Analyze ARP traffic.

### Command

```bash
tcpdump -r traffic.pcap arp -n
```

Locate:

```text
Who has 192.168.124.137?
Tell 192.168.124.148
```

### Answer

```text
192.168.124.148
```

---

### Question 3

What hostname appears in the first DNS query?

### Command

```bash
tcpdump -r traffic.pcap port 53 -n
```

### Result

```text
mirrors.rockylinux.org
```

### Answer

```text
mirrors.rockylinux.org
```

---

## Task: Advanced Filtering

### Question 1

How many packets have only the TCP RST flag set?

### Command

```bash
tcpdump -r traffic.pcap "tcp[tcpflags] == tcp-rst" -n | wc
```

### Output

```text
57
```

### Answer

```text
57
```

---

### Question 2

What host sent packets larger than 15000 bytes?

### Command

```bash
tcpdump -r traffic.pcap greater 15000 -n
```

### Analysis

Observed:

```text
185.117.80.53.80 > 192.168.124.137
```

### Answer

```text
185.117.80.53
```

---

## Task: Displaying Packets

### Question

What is the MAC address of the host that sent an ARP request?

### Command

```bash
tcpdump -r traffic.pcap arp -e -n
```

### Output

```text
52:54:00:7c:d3:5b > ff:ff:ff:ff:ff:ff
ARP Request who-has 192.168.124.137
```

### Answer

```text
52:54:00:7c:d3:5b
```

---

# Troubleshooting

## Issue 1

### Problem

```bash
tcpdump host 192.168.124.137 ARP
```

Produced:

```text
Operation not permitted
```

### Cause

Tcpdump attempted live capture on interface.

User lacked permissions.

---

### Resolution

Use:

```bash
tcpdump -r traffic.pcap
```

instead of live capture.

---

## Issue 2

### Problem

```bash
tcpdump "tcp[tcpflags] == tcp-rst"
```

Never stopped running.

### Cause

Tcpdump was listening on a live interface.

---

### Resolution

Use:

```bash
tcpdump -r traffic.pcap "tcp[tcpflags] == tcp-rst"
```

---

# Cybersecurity Relevance

## Red Team

Tcpdump can be used for:

* Internal network enumeration
* DNS discovery
* Protocol identification
* Credential interception (unencrypted protocols)
* Lateral movement analysis
* Traffic reconnaissance

---

## Blue Team

Tcpdump is heavily used for:

* Incident response
* Threat hunting
* Malware investigations
* Network troubleshooting
* Packet forensics
* Security monitoring

---

## Detection Opportunities

Tcpdump can reveal:

* Port scans
* ARP spoofing
* DNS tunneling
* Beaconing malware
* Data exfiltration
* Suspicious communications

---

# Key Points

* Tcpdump is the foundation of packet analysis.
* PCAP files enable offline investigation.
* Filtering is essential for large captures.
* ARP reveals IP-to-MAC relationships.
* DNS traffic exposes hostname lookups.
* TCP flags reveal connection behavior.
* Packet size can indicate file transfers or exfiltration.
* MAC addresses are visible using `-e`.

---

# Key Takeaways

* Always use filters to reduce noise.
* Learn to read packet direction:

```text
SOURCE > DESTINATION
```

* Understand common protocols:

```text
ARP
DNS
ICMP
TCP
UDP
```

* Mastering Tcpdump makes Wireshark easier.
* Most real-world investigations start with packet filtering.

---

# Skills Gained

* Packet capture
* Offline packet analysis
* Protocol filtering
* DNS analysis
* ARP analysis
* TCP flag analysis
* Packet size analysis
* Traffic investigation

---

# Future Learning Path

Recommended next topics:

1. Wireshark Fundamentals
2. Network Traffic Analysis
3. ARP Spoofing and Detection
4. DNS Security
5. HTTP/HTTPS Analysis
6. Incident Response
7. Malware Traffic Analysis
8. Network Forensics

---

# Conclusion

Tcpdump is one of the most important networking and cybersecurity tools because it exposes the conversations occurring behind applications and user interfaces. Throughout this room, we learned how to capture traffic, inspect PCAP files, apply filters, analyze protocols, inspect packet contents, and perform investigations using real packet captures.

The room demonstrated that packet analysis is not about memorizing commands but about understanding communications between hosts. Whether working as a network engineer, SOC analyst, incident responder, or penetration tester, Tcpdump provides direct visibility into network behavior and remains one of the most valuable troubleshooting and forensic tools available on Unix-like systems.
