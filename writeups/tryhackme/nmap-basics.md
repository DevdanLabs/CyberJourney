# Nmap: The Basics

## Executive Summary

This room introduced the fundamentals of Nmap, one of the most important network reconnaissance and enumeration tools used in cybersecurity. The room covered host discovery, port scanning, service and version detection, operating system fingerprinting, scan timing controls, output management, and reporting.

By the end of the room, we learned how to identify live hosts, discover open ports, enumerate running services, detect operating systems, optimize scan performance, and save scan results for later analysis. These skills form the foundation of network reconnaissance and are essential for both penetration testers and security defenders.

---

# Learning Objectives

After completing this room, you should be able to:

* Discover live hosts on a network
* Perform TCP and UDP port scans
* Understand the difference between Connect Scan and SYN Scan
* Detect service versions
* Perform OS fingerprinting
* Control scan speed and timing
* Generate and save scan reports
* Understand how Nmap fits into the penetration testing workflow

---

# Prerequisites

Before studying this room, it is recommended to understand:

* TCP/IP Model
* IPv4 Addressing
* Subnetting
* TCP Three-Way Handshake
* UDP Communication
* ICMP Protocol
* ARP Protocol
* Common Network Ports

Related TryHackMe Rooms:

* Networking Concepts
* Networking Essentials
* Networking Core Protocols
* Networking Secure Protocols

---

# What is Nmap?

Nmap (Network Mapper) is an open-source network scanning and reconnaissance tool first released in 1997.

Nmap allows security professionals to:

* Discover live hosts
* Identify open ports
* Detect running services
* Determine service versions
* Identify operating systems
* Perform advanced enumeration

Nmap is considered one of the most important tools in cybersecurity because almost every penetration test begins with reconnaissance and enumeration.

---

# Why Nmap Exists

Without Nmap, discovering hosts and services would require manually checking:

* Every IP address
* Every TCP port
* Every UDP port

This would be extremely slow and inefficient.

Nmap automates the process and provides fast, accurate network intelligence.

---

# Cybersecurity Relevance

## Red Team Perspective

Nmap helps attackers and pentesters:

* Discover targets
* Identify attack surfaces
* Detect services
* Find vulnerable software versions
* Gather intelligence before exploitation

## Blue Team Perspective

Nmap helps defenders:

* Inventory network assets
* Detect unauthorized devices
* Verify exposed services
* Audit firewall configurations

## Detection Perspective

Nmap scans can be detected through:

* Firewall Logs
* IDS Alerts
* IPS Systems
* SIEM Platforms

Examples:

* SYN Flood Detection
* Port Scan Detection
* ICMP Sweep Detection

---

# Nmap Target Specification

Nmap supports multiple target formats.

## IP Range

```bash
nmap 192.168.0.1-10
```

Scans:

```text
192.168.0.1
through
192.168.0.10
```

---

## CIDR Subnet

```bash
nmap 192.168.0.0/24
```

Equivalent to:

```text
192.168.0.0 - 192.168.0.255
```

---

## Hostname

```bash
nmap example.thm
```

Nmap resolves the hostname into an IP address before scanning.

---

# Host Discovery

## Objective

Identify which hosts are online.

## Command

```bash
sudo nmap -sn TARGET
```

---

## Purpose

Performs host discovery without scanning ports.

---

## Example

```bash
sudo nmap -sn 192.168.0.0/24
```

---

## Local Network Discovery

On local networks, Nmap uses:

```text
ARP Requests
```

Workflow:

```text
ARP Request
↓
ARP Reply
↓
Host Identified as Alive
```

---

## Remote Network Discovery

When scanning remote networks, Nmap cannot use ARP.

Instead it uses:

* ICMP Echo Requests
* ICMP Timestamp Requests
* TCP SYN Probes
* TCP ACK Probes

---

## List Scan

Lists targets without scanning them.

### Command

```bash
nmap -sL 192.168.0.0/24
```

### Purpose

Verify targets before scanning.

---

# Port Scanning

## Objective

Discover listening services.

---

# TCP Connect Scan

## Command

```bash
nmap -sT TARGET
```

---

## Purpose

Completes a full TCP Three-Way Handshake.

---

## TCP Handshake

```text
SYN
↓
SYN-ACK
↓
ACK
```

---

## Open Port Behavior

```text
SYN
↓
SYN-ACK
↓
ACK
```

Connection established.

---

## Closed Port Behavior

```text
SYN
↓
RST-ACK
```

Connection refused.

---

## Advantages

* Works without root privileges
* Reliable

---

## Disadvantages

* Generates more logs
* Easier to detect

---

# TCP SYN Scan (Stealth Scan)

## Command

```bash
sudo nmap -sS TARGET
```

---

## Purpose

Performs a half-open scan.

---

## Packet Flow

```text
SYN
↓
SYN-ACK
↓
RST
```

---

## Advantages

* Faster
* Fewer logs
* Preferred scan type

---

## Disadvantages

* Requires root privileges

---

# UDP Scan

## Command

```bash
sudo nmap -sU TARGET
```

---

## Purpose

Identify UDP services.

---

## Common UDP Services

| Port  | Service  |
| ----- | -------- |
| 53    | DNS      |
| 67/68 | DHCP     |
| 123   | NTP      |
| 161   | SNMP     |
| 5060  | SIP/VoIP |

---

## UDP Closed Port Response

```text
UDP Packet
↓
ICMP Port Unreachable
```

---

## Challenges

UDP scans are:

* Slower
* Less reliable
* More difficult to interpret

---

# Limiting Port Scans

## Fast Scan

### Command

```bash
nmap -F TARGET
```

### Purpose

Scans the 100 most common ports.

---

## Port Range Scan

### Example

```bash
nmap -p 1-1024 TARGET
```

Scans:

```text
1 through 1024
```

---

## Scan All Ports

### Command

```bash
nmap -p- TARGET
```

Equivalent to:

```bash
nmap -p1-65535 TARGET
```

---

# Service and Version Detection

## Command

```bash
nmap -sV TARGET
```

---

## Purpose

Identify:

* Service
* Software
* Version

---

## Example Output

```text
22/tcp open ssh OpenSSH 8.9p1 Ubuntu
```

---

## Why Version Detection Matters

Many vulnerabilities depend on specific versions.

Example:

```text
Apache 2.4.49
```

may be vulnerable to:

```text
CVE-2021-41773
```

---

# Operating System Detection

## Command

```bash
sudo nmap -O TARGET
```

---

## Purpose

Fingerprint the target operating system.

---

## Example Output

```text
Running: Linux 4.X|5.X
```

---

## How It Works

Nmap analyzes:

* TTL Values
* TCP Window Size
* TCP Options
* ICMP Responses
* Packet Behavior

---

## Limitations

OS detection is:

```text
An educated guess
```

Not guaranteed to be 100% accurate.

---

# Aggressive Scan

## Command

```bash
nmap -A TARGET
```

---

## Enables

* OS Detection
* Version Detection
* Traceroute
* Additional Enumeration

---

## Tradeoff

Produces more network traffic and is easier to detect.

---

# Forcing Scans

## Command

```bash
nmap -Pn TARGET
```

---

## Purpose

Treat all hosts as online.

---

## Useful When

Firewalls block:

* ICMP
* Discovery Probes

---

# Scan Timing

Nmap supports six timing templates.

| Template | Name       |
| -------- | ---------- |
| T0       | Paranoid   |
| T1       | Sneaky     |
| T2       | Polite     |
| T3       | Normal     |
| T4       | Aggressive |
| T5       | Insane     |

---

## Example

```bash
nmap -T4 TARGET
```

Equivalent to:

```bash
nmap -T aggressive TARGET
```

---

## Typical Usage

### Lab Environments

```bash
-T4
```

### Stealth Operations

```bash
-T0
-T1
```

---

# Parallelism

## Options

```bash
--min-parallelism
--max-parallelism
```

---

## Purpose

Control how many probes run simultaneously.

---

# Rate Control

## Options

```bash
--min-rate
--max-rate
```

---

## Purpose

Control packets per second.

---

## Example

```bash
nmap --min-rate 5000 TARGET
```

---

# Host Timeout

## Option

```bash
--host-timeout
```

---

## Purpose

Stop scanning slow hosts after a specified period.

---

# Verbose Output

## Command

```bash
nmap -v TARGET
```

---

## Purpose

Display additional scan progress information.

---

## Higher Verbosity

```bash
-vv
-vvv
-v4
```

---

# Debugging

## Command

```bash
nmap -d TARGET
```

---

## Purpose

Display internal Nmap debugging information.

---

## Higher Debug Levels

```bash
-d2
-d5
-d9
```

---

# Saving Scan Reports

---

## Normal Output

```bash
-oN scan.txt
```

Human-readable format.

---

## XML Output

```bash
-oX scan.xml
```

Useful for automation and importing into other tools.

---

## Grepable Output

```bash
-oG scan.gnmap
```

Useful with:

```bash
grep
awk
sed
```

---

## All Formats

```bash
-oA report
```

Creates:

```text
report.nmap
report.xml
report.gnmap
```

---

# Questions and Answers

## Question 1

### What is the last IP address that will be scanned when your scan target is 192.168.0.1/27?

### Explanation

```text
/27
=
32 addresses
```

Range:

```text
192.168.0.0
to
192.168.0.31
```

### Answer

```text
192.168.0.31
```

---

## Question 2

### How many TCP ports are open on MACHINE_IP?

### Method

```bash
nmap MACHINE_IP
```

### Answer

```text
6
```

---

## Question 3

### Find the web server and retrieve the flag.

### Method

```bash
nmap MACHINE_IP
```

Identify:

```text
80/tcp open
```

Open:

```text
http://MACHINE_IP
```

### Flag

```text
THM{SECRET_PAGE_38B9P6}
```

---

## Question 4

### What is the web server version?

### Method

```bash
nmap -sV MACHINE_IP
```

### Result

```text
lighttpd 1.4.74
```

---

## Question 5

### What is the non-numeric equivalent of -T4?

### Answer

```text
aggressive
```

---

## Question 6

### What option enables debugging?

### Answer

```bash
-d
```

---

## Question 7

### What scan is used by default when Nmap is run as a local user?

### Explanation

Without root privileges, Nmap cannot create raw SYN packets.

### Answer

```text
TCP Connect Scan
```

```bash
-sT
```

---

# Commands Reference

| Command  | Purpose                 |
| -------- | ----------------------- |
| nmap -sL | List targets            |
| nmap -sn | Host discovery          |
| nmap -sT | TCP Connect Scan        |
| nmap -sS | TCP SYN Scan            |
| nmap -sU | UDP Scan                |
| nmap -F  | Fast Scan               |
| nmap -p- | Scan all ports          |
| nmap -Pn | Skip host discovery     |
| nmap -sV | Version detection       |
| nmap -O  | OS detection            |
| nmap -A  | Aggressive scan         |
| nmap -T4 | Aggressive timing       |
| nmap -v  | Verbose output          |
| nmap -d  | Debug output            |
| nmap -oA | Save all report formats |

---

# Common Issues Encountered

## Host Appears Down

### Cause

ICMP blocked by firewall.

### Solution

```bash
nmap -Pn TARGET
```

---

## Missing Open Ports

### Cause

Only default 1000 ports scanned.

### Solution

```bash
nmap -p- TARGET
```

---

## SYN Scan Not Working

### Cause

Running without root privileges.

### Solution

```bash
sudo nmap -sS TARGET
```

---

# Pentester Notes

## Reconnaissance Value

⭐⭐⭐⭐⭐

Primary tool for discovering attack surfaces.

---

## Enumeration Value

⭐⭐⭐⭐⭐

Provides detailed information about services and operating systems.

---

## Exploitation Relevance

High.

Open ports and vulnerable services are common entry points.

---

## Detection Opportunities

Blue teams can detect:

* ICMP Sweeps
* SYN Scans
* Connect Scans
* UDP Scans
* Aggressive Enumeration

---

# Key Takeaways

* Nmap is much more than a port scanner.
* Host discovery should always come before port scanning.
* SYN scans are faster and stealthier than connect scans.
* UDP services should not be ignored.
* Version detection is critical for vulnerability research.
* OS detection relies on fingerprinting, not certainty.
* Timing templates affect speed, reliability, and stealth.
* Always save scan results for documentation and future analysis.
* Running Nmap with sudo unlocks its full capabilities.

---

# Skills Gained

* Host Discovery
* Port Scanning
* TCP Enumeration
* UDP Enumeration
* Service Detection
* Version Fingerprinting
* OS Fingerprinting
* Scan Optimization
* Scan Reporting
* Network Reconnaissance

---

# Future Learning Path

Recommended next topics:

1. Nmap NSE (Nmap Scripting Engine)
2. Advanced Port Scanning Techniques
3. Firewall Evasion
4. Packet Crafting
5. SMB Enumeration
6. Web Enumeration
7. Service Exploitation
8. Vulnerability Assessment

---

# Conclusion

Nmap is one of the most important tools in cybersecurity and serves as the foundation of reconnaissance and enumeration. This room introduced the core functionality required to identify live hosts, discover open ports, detect services, identify operating systems, optimize scan performance, and generate professional scan reports. Mastering these fundamentals provides the groundwork for advanced network enumeration, vulnerability assessment, and penetration testing workflows.
