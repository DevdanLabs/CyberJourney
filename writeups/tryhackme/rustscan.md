# RustScan

## Executive Summary

RustScan is a modern, high-speed TCP port scanner designed to dramatically reduce the time required for port discovery during reconnaissance. Built with the Rust programming language, it leverages asynchronous networking and efficient resource management to scan thousands of ports in a fraction of the time required by traditional scanners.

Unlike Nmap, RustScan is **not intended to replace a full-featured network scanner**. Instead, it complements tools like Nmap by rapidly identifying open ports and automatically passing the results for deeper enumeration, including service detection, version identification, operating system fingerprinting, and NSE (Nmap Scripting Engine) execution.

In this room, I learned the fundamentals of RustScan, including its architecture, core features, installation, scanning techniques, scripting capabilities, adaptive optimization, and practical integration with Nmap during penetration testing.

---

# Learning Objectives

After completing this room, I was able to:

- Understand the purpose of RustScan and why it was created.
- Explain the differences between RustScan and Nmap.
- Install RustScan on Kali Linux.
- Perform high-speed TCP port scanning against target systems.
- Scan individual ports, port ranges, multiple hosts, CIDR ranges, and host files.
- Pass discovered ports directly to Nmap for automated enumeration.
- Understand the RustScan Scripting Engine (RSE).
- Explain RustScan's adaptive scanning and optimization features.
- Troubleshoot common issues such as `ulimit` warnings and file descriptor limitations.
- Interpret RustScan and Nmap scan results during reconnaissance.

---

# Prerequisites

Before completing this room, it is recommended to understand:

- Basic Networking
- TCP/IP Fundamentals
- TCP Three-Way Handshake
- Common Network Ports
- Basic Linux Commands
- Basic Nmap Usage
- Reconnaissance Methodology

---

# What is RustScan?

RustScan is a modern, high-performance TCP port scanner written in **Rust**.

Its primary objective is to discover open TCP ports as quickly as possible while delegating detailed service enumeration to tools such as Nmap.

Instead of trying to perform every reconnaissance task itself, RustScan focuses on doing one task extremely well:

> **Find open ports as fast as possible.**

Once the scan is complete, RustScan automatically launches Nmap (by default) using the discovered open ports, significantly reducing the overall reconnaissance time.

---

# Why Was RustScan Created?

Traditional scanners like Nmap are extremely powerful but can become slow when scanning all **65,535 TCP ports**, especially across multiple hosts or high-latency networks.

Typical workflow without RustScan:

```text
Nmap

↓

Scan every port

↓

Identify open ports

↓

Perform service detection

↓

Run NSE scripts
```

Although comprehensive, this approach may take several minutes depending on the target.

RustScan optimizes the first phase:

```text
RustScan

↓

Discover open ports

↓

Pass results to Nmap

↓

Service Detection

↓

Version Detection

↓

NSE Scripts
```

By separating **port discovery** from **service enumeration**, reconnaissance becomes significantly faster without sacrificing functionality.

---

# Where RustScan Fits in the Penetration Testing Methodology

RustScan is primarily used during the **Reconnaissance** and **Enumeration** phases of a penetration test.

```text
Reconnaissance
      │
      ▼
Host Discovery
      │
      ▼
RustScan
      │
      ▼
Open Ports Identified
      │
      ▼
Nmap Enumeration
      │
      ▼
Service Enumeration
      │
      ▼
Vulnerability Assessment
      │
      ▼
Exploitation
```

Rather than replacing existing tools, RustScan accelerates the early stages of information gathering.

---

# RustScan vs Nmap

| RustScan | Nmap |
|----------|-------|
| Extremely fast TCP port discovery | Comprehensive network scanner |
| Written in Rust | Written in C/C++ |
| Focuses on discovering open ports | Performs full service enumeration |
| Uses asynchronous networking | Supports numerous scan types |
| Automatically integrates with Nmap | Can operate independently |
| Lightweight | Feature-rich |
| No equivalent to NSE | Includes the powerful Nmap Scripting Engine (NSE) |

**Key Takeaway**

RustScan is **not a replacement for Nmap**.

Instead, the two tools complement each other:

```text
RustScan
      │
      ▼
Open Ports
      │
      ▼
Nmap
      │
      ▼
Service Detection
Version Detection
OS Detection
NSE Scripts
```

This workflow is widely adopted by penetration testers because it combines RustScan's speed with Nmap's extensive enumeration capabilities.

---

# Why RustScan Matters for Pentesters

During a penetration test, time is valuable.

When assessing multiple hosts, waiting several minutes for every full port scan can significantly slow down the engagement.

RustScan helps by:

- Rapidly identifying attack surfaces.
- Reducing reconnaissance time.
- Automating the transition to Nmap.
- Improving efficiency when scanning multiple targets.
- Integrating easily into automated enumeration workflows.

Its speed makes it particularly useful during:

- Internal Network Penetration Testing
- External Infrastructure Assessments
- Capture The Flag (CTF) competitions
- Hack The Box laboratories
- TryHackMe rooms
- Bug Bounty reconnaissance

---

# Skills Learned

- Understanding the purpose of RustScan.
- High-speed TCP port discovery.
- Reconnaissance workflow optimization.
- RustScan and Nmap integration.
- Modern port scanning methodology.
- Practical penetration testing reconnaissance.

---

# Key Takeaways

- RustScan is a **high-speed TCP port scanner** built using Rust.
- Its primary goal is to discover open ports as quickly as possible.
- RustScan complements rather than replaces Nmap.
- Separating port discovery from service enumeration significantly improves reconnaissance efficiency.
- Combining RustScan with Nmap creates a fast and effective workflow commonly used by penetration testers.

# Core Features of RustScan

One of the main reasons RustScan has become popular among penetration testers is that it focuses on **speed**, **automation**, and **extensibility** without sacrificing compatibility with existing tools like Nmap.

Rather than trying to replace traditional scanners, RustScan improves the reconnaissance workflow by making the initial port discovery phase significantly faster.

This room introduces four major features that define RustScan:

- Fast
- Accessible
- Extensible
- Adaptive

---

# Fast

The primary design goal of RustScan is speed.

Unlike many traditional scanners that perform multiple tasks during scanning, RustScan focuses solely on discovering open TCP ports before handing the results to Nmap for deeper analysis.

## Why is RustScan Fast?

Several design decisions contribute to its performance:

- Written in the Rust programming language
- Uses low-level kernel networking
- Implements asynchronous (async) networking instead of traditional multithreading
- Minimizes unnecessary processing during scanning

### Low-Level Networking

RustScan communicates efficiently with the operating system's networking stack to create TCP connections with minimal overhead.

```text
RustScan
      │
      ▼
Socket API
      │
      ▼
Kernel Networking Stack
      │
      ▼
Target
```

This allows RustScan to establish thousands of TCP connection attempts in a very short period.

---

### Asynchronous Scanning

Instead of waiting for each connection to complete one at a time, RustScan performs many connection attempts simultaneously.

Traditional approach:

```text
Port 22
↓

Wait

↓

Port 80

↓

Wait

↓

Port 443
```

RustScan:

```text
Port 22 ─┐
Port 80 ─┼──► Scan simultaneously
Port 443 ┘
```

Because asynchronous I/O avoids excessive thread creation and context switching, CPU resources are used much more efficiently.

---

### The Real Bottleneck

According to the RustScan developers, the scanner itself is rarely the limiting factor.

Scanning speed is usually limited by:

- Your computer
- Your network connection
- The target system

This means RustScan can generally scan as fast as the environment allows.

---

# Accessible (A11Y)

RustScan places a strong emphasis on **accessibility**, often abbreviated as **A11Y**.

The abbreviation comes from:

```text
A + 11 letters + Y
```

Accessibility means designing software so that it can be used by everyone, including users with disabilities.

Examples include:

- Screen reader compatibility
- Keyboard-only navigation
- Better terminal readability
- Improved usability for visually impaired users

The RustScan project continuously tests accessibility using:

- Continuous Integration (CI)
- Manual accessibility testing

The developers emphasize that cybersecurity should be accessible to everyone, regardless of physical ability.

---

# Extensible

One of RustScan's most powerful features is the **RustScan Scripting Engine (RSE)**.

Rather than stopping after discovering open ports, RustScan can automatically execute scripts or external tools.

Workflow:

```text
RustScan
      │
      ▼
Open Ports Found
      │
      ▼
RustScan Scripting Engine
      │
      ├──────────────┐
      ▼              ▼
Python Script     Bash Script
      │              │
      ▼              ▼
Gobuster         Nmap
```

---

## Supported Script Types

RustScan can execute:

- Python
- Shell (Bash)
- Perl
- Any executable available in the system's `$PATH`

This allows users to build custom automation pipelines for reconnaissance.

---

## Script Metadata

Each script contains metadata stored as comments.

Example:

```python
#!/usr/bin/python3

#tags = ["http"]
#trigger_port = "80"
#developer = ["example"]
#call_format = "python3 {{script}} {{ip}} {{port}}"
```

Important metadata fields include:

| Field | Purpose |
|--------|----------|
| `tags` | Categorizes scripts |
| `trigger_port` | Determines when the script should execute |
| `developer` | Identifies the script author |
| `call_format` | Specifies how RustScan passes arguments |

---

## Template Variables

RustScan automatically replaces template variables before executing the script.

Available variables include:

| Variable | Description |
|----------|-------------|
| `{{script}}` | Script filename |
| `{{ip}}` | Target IP address |
| `{{port}}` | Open port(s) |

Example:

```text
python3 example.py 10.10.10.5 80
```

---

## Automating Enumeration

One practical use of RSE is launching Nmap automatically.

Example shell script:

```bash
nmap -vvv -p {{port}} {{ip}}
```

The same approach can be used with other tools such as:

- Gobuster
- FFUF
- Nikto
- WhatWeb
- Custom Python scripts

This greatly reduces repetitive manual work during reconnaissance.

---

# Adaptive

RustScan includes an adaptive optimization system that adjusts its behavior according to both the scanning environment and the target.

Unlike static scanners that always use the same settings, RustScan attempts to optimize itself automatically.

---

## Adaptive SYN Timing

During scanning, RustScan observes how quickly the target responds.

Fast target:

```text
SYN

↓

Immediate SYN-ACK

↓

Increase scan speed
```

Slow target:

```text
SYN

↓

Slow response

↓

Reduce scan speed
```

This helps maximize performance without overwhelming slower systems.

---

## Custom Top Ports

Many scanners provide a predefined list of the "Top 1000 Ports."

However, those ports are based on Internet-wide statistics and may not represent every environment.

Examples:

- Corporate networks may expose proprietary services.
- CTF environments often use unusual ports.
- Internal applications frequently use custom port numbers.

RustScan can adapt to the ports that are commonly encountered in your own environment, making future scans more relevant.

---

## Operating System Adaptation

Different operating systems support different numbers of simultaneous open connections.

For example:

| Operating System | Approximate Open File Limit |
|------------------|-----------------------------|
| macOS | ~250 |
| Kali Linux | Tens of thousands |

RustScan adjusts its scanning behavior based on these system limitations to avoid unnecessary failures.

---

## Configuration File

Adaptive settings are stored in a configuration file.

This allows users to:

- Preserve optimized settings
- Share configurations with teammates
- Deploy consistent scanning behavior across a penetration testing team

---

# Feature Comparison

| Feature | Benefit |
|----------|---------|
| Fast | Extremely rapid TCP port discovery |
| Accessible | Designed for users of varying abilities |
| Extensible | Supports custom scripts and automation |
| Adaptive | Optimizes scanning for the system and target |

---

# Pentester Notes

RustScan is particularly valuable during reconnaissance because it minimizes the time spent identifying open ports.

Combined with the RustScan Scripting Engine, it can automatically launch enumeration tools based on discovered services, creating an efficient and repeatable workflow.

Example:

```text
RustScan
      │
      ▼
22, 80, 445
      │
      ▼
Nmap
      │
      ▼
Gobuster
      │
      ▼
Nikto
      │
      ▼
Manual Enumeration
```

This approach allows penetration testers to spend more time analyzing services and less time performing repetitive manual tasks.

---

# Blue Team Notes

From a defensive perspective, RustScan's high-speed scanning can generate noticeable network activity, including:

- Numerous TCP connection attempts
- Sequential or randomized port scanning
- Immediate follow-up enumeration using tools like Nmap

These behaviors may trigger:

- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Firewalls
- Security Information and Event Management (SIEM) platforms

Understanding how RustScan operates helps defenders recognize reconnaissance activity before an attacker progresses to later stages of an intrusion.

---

# Key Takeaways

- RustScan achieves its performance through asynchronous networking and efficient resource management.
- Accessibility is treated as a core design principle rather than an afterthought.
- The RustScan Scripting Engine (RSE) enables powerful automation by executing custom scripts after port discovery.
- Adaptive optimization allows RustScan to adjust its scanning behavior based on the operating system, target responsiveness, and previous scanning experience.
- Together, these features make RustScan a fast, flexible, and automation-friendly reconnaissance tool for modern penetration testing.

# Installing RustScan

Before using RustScan, it must be installed on the attack machine. In this room, RustScan was installed on a **Kali Linux** virtual machine using the official Debian package (`.deb`).

Although RustScan supports multiple operating systems, the installation process differs depending on the platform.

Supported platforms include:

- Debian / Kali Linux
- Ubuntu
- Arch Linux
- macOS
- Docker
- NixOS
- Other Linux distributions

The official installation guide can be found on the RustScan GitHub repository.

---

# Downloading RustScan

The official releases are available from GitHub.

Download the Debian package (`.deb`) that matches your system architecture.

Example:

```text
rustscan_1.8.0_amd64.deb
```

After downloading, the file is typically located in the **Downloads** directory.

---

# Installing on Kali Linux

Move to the directory containing the downloaded package.

```bash
cd ~/Downloads
```

Verify that the package exists.

```bash
ls
```

Example output:

```text
rustscan_1.8.0_amd64.deb
```

Install the package using **dpkg**.

```bash
sudo dpkg -i rustscan_1.8.0_amd64.deb
```

---

# Command Breakdown

## `dpkg`

**Purpose**

Install, remove, and manage Debian packages.

### Syntax

```bash
dpkg -i <package.deb>
```

### Flag Breakdown

| Flag | Description |
|------|-------------|
| `-i` | Install a package |

### Example

```bash
sudo dpkg -i rustscan_1.8.0_amd64.deb
```

---

# Handling Dependency Errors

Sometimes `dpkg` may report missing dependencies.

Example:

```text
dependency problems prevent configuration...
```

This can be fixed by allowing APT to install the missing packages.

```bash
sudo apt --fix-broken install
```

or

```bash
sudo apt-get install -f
```

After the dependencies are installed, rerun:

```bash
sudo dpkg -i rustscan_1.8.0_amd64.deb
```

---

# Verifying the Installation

To verify that RustScan has been installed correctly, simply execute:

```bash
rustscan
```

RustScan should display its help message or indicate that a target is required.

The installed version can also be checked with:

```bash
rustscan --version
```

Example:

```text
RustScan 1.8.0
```

---

# Alternative Installation Using APT

If RustScan is available in your distribution's repository, it can be installed directly through APT.

```bash
sudo apt update
```

```bash
sudo apt install rustscan
```

This approach automatically handles dependencies and simplifies future updates.

---

# Understanding `ulimit`

During the practical lab, RustScan displayed the following warning:

```text
[!] File limit is lower than default batch size.
```

This warning is related to **file descriptors**.

---

## What is a File Descriptor?

In Linux, almost every resource is represented as a file.

Examples include:

- Regular files
- Network sockets
- Pipes
- Terminal devices

Each TCP connection opened by RustScan consumes **one file descriptor**.

```text
RustScan
      │
      ▼
TCP Connection
      │
      ▼
File Descriptor
```

When thousands of connections are opened simultaneously, the operating system must provide enough file descriptors.

---

## Checking the File Descriptor Limit

The current limit can be viewed using:

```bash
ulimit -n
```

Example output:

```text
1024
```

This means the current shell is allowed to keep **1024 open file descriptors** at the same time.

---

# Increasing the Limit

RustScan allows the limit to be increased temporarily.

Example:

```bash
rustscan -a <TARGET-IP> --ulimit 2500
```

RustScan attempts to increase the available file descriptors before starting the scan.

---

# Why Did `--ulimit 5000` Fail?

During the lab, attempting to use:

```bash
rustscan -a <TARGET-IP> --ulimit 5000
```

produced the following error:

```text
Too many open files.
Please reduce batch size.
```

This occurred because the operating system refused to allocate that many file descriptors.

The problem was **not caused by RustScan itself**, but by the operating system's resource limits.

Reducing the value to:

```bash
--ulimit 2500
```

allowed RustScan to execute successfully.

---

# Why the Warning Still Appeared

Even after increasing the limit, RustScan still displayed:

```text
File limit is lower than default batch size.
```

This is only a **warning**, not an error.

RustScan compares:

- Current file descriptor limit
- Internal batch size

If the file descriptor limit is lower than RustScan's preferred batch size, it warns the user that maximum performance may not be achieved.

The scan will still continue normally.

---

# Practical Installation Verification

After installation, RustScan successfully identified the target's open ports.

Example:

```bash
rustscan -a 10.49.xxx.xxx
```

Output:

```text
Open 10.49.xxx.xxx:22
Open 10.49.xxx.xxx:80
```

After discovering the ports, RustScan automatically launched Nmap.

```text
[~] Starting Nmap

The Nmap command to be run is

nmap -sC -sV -vvv -p80,22 10.49.xxx.xxx
```

This demonstrates one of RustScan's primary design goals:

1. Discover open ports quickly.
2. Automatically pass them to Nmap for deeper enumeration.

---

# Troubleshooting

## Problem

```text
File limit is lower than default batch size.
```

### Cause

Insufficient file descriptor limit.

### Solution

Increase the limit using:

```bash
--ulimit <value>
```

or accept the warning if the scan completes successfully.

---

## Problem

```text
Too many open files.
```

### Cause

The requested file descriptor limit exceeds the operating system's allowed maximum.

### Solution

Reduce the requested value.

Example:

```bash
--ulimit 2500
```

or decrease the RustScan batch size.

---

## Problem

RustScan appears to stop after displaying the open ports.

### Cause

RustScan is waiting for the automatically launched Nmap scan to finish.

### Solution

Allow Nmap to complete before interrupting the process.

---

# Lessons Learned

- RustScan can be installed easily using the Debian package manager.
- APT can automatically resolve missing package dependencies.
- Linux limits the number of simultaneous file descriptors available to each process.
- RustScan relies heavily on file descriptors because every TCP connection consumes one.
- `ulimit` warnings are generally informational and do not prevent successful scans.
- After discovering open ports, RustScan automatically launches Nmap, creating a seamless reconnaissance workflow.

---

# Key Takeaways

- RustScan installation on Kali Linux is straightforward using either a `.deb` package or the APT package manager.
- File descriptor limits directly affect RustScan's scanning performance.
- `--ulimit` temporarily increases the number of available file descriptors for faster scanning.
- Warnings about low file limits do not necessarily indicate a failed scan.
- Successful installation can be verified by running RustScan and confirming that it discovers open ports and automatically hands them off to Nmap for further enumeration.

# RustScan Command Reference

This section covers the fundamental RustScan commands introduced in this room.

One of RustScan's strengths is its simple command-line interface. Although the syntax is lightweight, it provides enough flexibility to scan individual hosts, multiple targets, CIDR ranges, custom port ranges, and even automatically pass results to Nmap.

---

# Basic Command Syntax

The general syntax is:

```bash
rustscan -a <TARGET> -- <NMAP_ARGUMENTS>
```

Example:

```bash
rustscan -a 10.10.10.10 -- -sC -sV
```

RustScan first discovers open ports, then automatically executes:

```text
Nmap
      │
      ▼
Open Ports
      │
      ▼
Service Detection
Version Detection
NSE Scripts
```

---

# Understanding `--`

One of the most important parts of RustScan's syntax is:

```text
--
```

This separator tells RustScan:

> "Everything after this belongs to Nmap."

Example:

```bash
rustscan -a 10.10.10.10 -- -A -sC
```

RustScan interprets:

```text
RustScan Arguments
──────────────────
-a 10.10.10.10

Nmap Arguments
──────────────
-A
-sC
```

Internally, RustScan executes something similar to:

```bash
nmap -Pn -vvv -p <OPEN_PORTS> -A -sC 10.10.10.10
```

---

# Scanning a Single Host

## Purpose

Perform a complete TCP port scan against a single target.

### Syntax

```bash
rustscan -a <IP>
```

### Example

```bash
rustscan -a 10.10.10.10
```

### Example Output

```text
Open 10.10.10.10:22
Open 10.10.10.10:80
```

### Pentester Relevance

This is the most common command used during the reconnaissance phase.

---

# Scanning Multiple Hosts

RustScan accepts a comma-separated list of targets.

### Syntax

```bash
rustscan -a <IP1>,<IP2>,<IP3>
```

### Example

```bash
rustscan -a 10.10.10.10,10.10.10.20
```

### Use Cases

- Internal network assessments
- Multiple lab machines
- Bug bounty target lists

---

# Hostname Scanning

RustScan can resolve domain names automatically.

### Syntax

```bash
rustscan -a <HOSTNAME>
```

### Example

```bash
rustscan -a google.com
```

Workflow:

```text
Hostname

↓

DNS Resolution

↓

IP Address

↓

Port Scan
```

---

# CIDR Scanning

RustScan supports CIDR notation.

### Syntax

```bash
rustscan -a <NETWORK/CIDR>
```

### Example

```bash
rustscan -a 192.168.1.0/24
```

RustScan expands the subnet into individual IP addresses before scanning.

Example:

```text
192.168.1.0/24

↓

192.168.1.1

↓

192.168.1.2

↓

...

↓

192.168.1.254
```

### Pentester Relevance

Very useful after gaining access to an internal corporate network.

---

# Reading Targets from a File

RustScan can scan multiple targets stored inside a text file.

Example file:

```text
hosts.txt

10.10.10.10
10.10.10.20
google.com
192.168.1.0/30
```

### Syntax

```bash
rustscan -a hosts.txt
```

### Advantages

Useful for:

- Scope lists
- Host discovery results
- Bug bounty programs
- Large internal assessments

---

# Scanning a Single Port

## Purpose

Scan one specific TCP port.

### Syntax

```bash
rustscan -a <TARGET> -p <PORT>
```

### Example

```bash
rustscan -a 10.10.10.10 -p 80
```

Output:

```text
80
```

### Common Use Cases

- Verifying a known service
- Rechecking a previously discovered port

---

# Scanning Multiple Ports

A comma-separated list of ports can be specified.

### Syntax

```bash
rustscan -a <TARGET> -p <PORT1>,<PORT2>,<PORT3>
```

### Example

```bash
rustscan -a 10.10.10.10 -p 22,80,443
```

RustScan scans only the selected ports.

---

# Scanning a Port Range

Instead of scanning every TCP port, a range can be specified.

### Syntax

```bash
rustscan -a <TARGET> --range START-END
```

### Example

```bash
rustscan -a 10.10.10.10 --range 1-1000
```

Workflow:

```text
Port 1

↓

Port 2

↓

...

↓

Port 1000
```

### Benefits

- Faster than scanning all 65,535 ports.
- Useful when only common ports are required.

---

# Passing Arguments to Nmap

RustScan automatically launches Nmap by default.

Additional Nmap options can be supplied after the separator.

### Example

```bash
rustscan -a 10.10.10.10 -- -A -sC
```

or

```bash
rustscan -a 10.10.10.10 -- -sC -sV
```

Common Nmap arguments:

| Argument | Purpose |
|----------|----------|
| `-sC` | Run default NSE scripts |
| `-sV` | Detect service versions |
| `-A` | Enable aggressive scan (OS detection, version detection, NSE, traceroute) |

Workflow:

```text
RustScan

↓

Open Ports

↓

Nmap

↓

Version Detection

↓

NSE Scripts

↓

OS Detection
```

---

# Random Port Ordering

RustScan normally scans ports sequentially.

Example:

```text
22

↓

23

↓

24

↓

25
```

To randomize the order:

```bash
rustscan -a 10.10.10.10 --range 1-1000 --scan-order Random
```

Example order:

```text
443

↓

18

↓

9000

↓

53

↓

8080
```

### Why Randomize?

Some firewalls and intrusion detection systems monitor for sequential scanning.

Randomizing the scan order makes the traffic pattern less predictable.

> **Note:** This is **not** a stealth technique. Modern IDS/IPS solutions can still detect randomized scanning based on connection behavior and volume.

---

# Practical Commands Used During This Room

Basic scan:

```bash
rustscan -a 10.49.xxx.xxx
```

Scan with Nmap service detection:

```bash
rustscan -a 10.49.xxx.xxx -- -sC -sV
```

Increase file descriptor limit:

```bash
rustscan -a 10.49.xxx.xxx --ulimit 2500
```

Increase file descriptor limit while running Nmap:

```bash
rustscan -a 10.49.xxx.xxx --ulimit 2500 -- -sC -sV
```

---

# Practical Workflow

Typical reconnaissance workflow:

```text
Target
      │
      ▼
RustScan
      │
      ▼
22
80
445
      │
      ▼
Nmap
      │
      ▼
SSH Enumeration
HTTP Enumeration
SMB Enumeration
      │
      ▼
Manual Investigation
```

This minimizes the time spent discovering open ports while allowing Nmap to perform comprehensive service enumeration.

---

# Pentester Notes

RustScan is most effective when used as the **first step of active reconnaissance**.

A common workflow is:

1. Discover open ports with RustScan.
2. Automatically pass results to Nmap.
3. Enumerate identified services.
4. Investigate potential vulnerabilities.
5. Proceed to exploitation if appropriate.

This workflow is widely used in:

- Hack The Box
- TryHackMe
- Internal Penetration Testing
- External Infrastructure Assessments

---

# Blue Team Notes

RustScan typically generates:

- Numerous TCP connection attempts
- Rapid port discovery
- Immediate follow-up enumeration via Nmap

These activities can often be detected through:

- Firewall logs
- IDS/IPS alerts
- SIEM correlation rules
- Network monitoring platforms

Understanding RustScan's workflow helps defenders recognize reconnaissance activity before exploitation begins.

---

# Key Takeaways

- RustScan provides a simple yet powerful command-line interface for high-speed TCP port discovery.
- It supports scanning individual hosts, multiple targets, CIDR ranges, host files, individual ports, and custom port ranges.
- The `--` separator passes arguments directly to Nmap, allowing RustScan and Nmap to work together seamlessly.
- Randomized scanning changes the order of scanned ports but should not be considered a stealth technique.
- The combination of RustScan for discovery and Nmap for enumeration forms a fast and efficient reconnaissance workflow widely adopted in modern penetration testing.

# Practical Lab Walkthrough

In this section, I used RustScan against the provided vulnerable machine to perform rapid port discovery, followed by automatic service enumeration using Nmap.

The objective was to identify exposed services and understand how RustScan integrates with Nmap to streamline the reconnaissance process.

---

# Lab Information

| Item | Value |
|------|-------|
| Tool | RustScan |
| Target | THM Vulnerable Machine |
| Attack Machine | Kali Linux |
| Scan Type | TCP Port Discovery + Nmap Enumeration |

---

# Step 1 — Initial Port Discovery

The first scan was performed using RustScan's default settings.

```bash
rustscan -a 10.49.xxx.xxx
```

Example output:

```text
Open 10.49.xxx.xxx:22
Open 10.49.xxx.xxx:80
```

---

## Explanation

RustScan rapidly scanned the target and identified two open TCP ports:

| Port | Service (Expected) |
|------|--------------------|
| 22 | SSH |
| 80 | HTTP |

At this stage RustScan has **not yet identified the services**.

It only confirms that these ports accept TCP connections.

Workflow:

```text
Target
      │
      ▼
RustScan
      │
      ▼
22
80
```

---

# Step 2 — Understanding the Warning

During scanning, RustScan displayed the following warning:

```text
[!] File limit is lower than default batch size.
```

This warning indicates that the Linux operating system provides fewer file descriptors than RustScan's preferred batch size.

The scan still completed successfully.

This warning affects **performance**, not **accuracy**.

---

# Step 3 — Increasing the File Descriptor Limit

To improve performance, RustScan allows the file descriptor limit to be increased.

Command:

```bash
rustscan -a 10.49.xxx.xxx --ulimit 2500
```

RustScan responded with:

```text
Automatically increasing ulimit value to 2500.
```

The scan then continued normally.

---

## Why Not Use 5000?

Attempting:

```bash
rustscan -a 10.49.xxx.xxx --ulimit 5000
```

produced:

```text
Too many open files.
Please reduce batch size.
```

This occurred because the operating system refused to allocate that many file descriptors.

Reducing the requested value resolved the issue.

---

# Step 4 — Running Nmap Automatically

RustScan can automatically pass discovered ports to Nmap.

Command:

```bash
rustscan -a 10.49.xxx.xxx --ulimit 2500 -- -sC -sV
```

RustScan displayed:

```text
Starting Nmap

The Nmap command to be run is

nmap -sC -sV -vvv -p80,22 10.49.xxx.xxx
```

This demonstrates how RustScan automatically constructs an appropriate Nmap command using the discovered ports.

Workflow:

```text
RustScan
      │
      ▼
22
80
      │
      ▼
Nmap
      │
      ▼
Service Enumeration
```

---

# Step 5 — Service Detection Results

After Nmap completed its scan, the following services were identified.

## Port 22

```text
22/tcp open ssh
OpenSSH 6.6.1p1
```

### Findings

- SSH service is enabled.
- OpenSSH version **6.6.1p1** is running.
- Operating system appears to be Ubuntu Linux.

---

## Port 80

```text
80/tcp open http
Apache httpd 2.4.7
```

### Findings

- Apache HTTP Server version **2.4.7**
- Web application accessible through HTTP

---

# Step 6 — Website Identification

Nmap's default NSE scripts extracted additional information.

Example:

```text
http-title

Login :: Damn Vulnerable Web Application
```

This reveals that the hosted application is:

**Damn Vulnerable Web Application (DVWA)**

DVWA is a deliberately vulnerable web application designed for security training.

---

# Step 7 — Additional Enumeration

Several NSE scripts automatically gathered useful information.

## Supported HTTP Methods

```text
GET
HEAD
POST
OPTIONS
```

These methods indicate how clients may interact with the web server.

---

## robots.txt

Nmap discovered:

```text
robots.txt
```

This file often contains paths that administrators prefer search engines not to index.

Although not a security feature, it can reveal interesting directories during reconnaissance.

---

## HTTP Cookie Flags

Nmap reported:

```text
PHPSESSID

httponly flag not set
```

### Why Does This Matter?

The **HttpOnly** cookie attribute prevents client-side JavaScript from reading session cookies.

If a web application suffers from a Cross-Site Scripting (XSS) vulnerability, missing the **HttpOnly** flag may increase the risk of session theft.

Although this observation alone is **not proof of a vulnerability**, it represents a security weakness worth investigating further.

---

## HTTP Server Header

Nmap identified:

```text
Apache/2.4.7 (Ubuntu)
```

This reveals both:

- Web server software
- Operating system family

Banner information can assist attackers during vulnerability research.

---

# Step 8 — SSH Host Keys

Nmap also collected SSH host key fingerprints.

Example:

```text
ssh-hostkey
```

Several key types were identified:

- RSA
- DSA
- ECDSA
- ED25519

These fingerprints can later be used to verify server identity or detect changes to the SSH server.

---

# Step 9 — Reconnaissance Workflow

The complete workflow performed during this lab can be summarized as follows:

```text
Target
      │
      ▼
RustScan
      │
      ▼
Open Ports
22
80
      │
      ▼
Automatic Nmap Scan
      │
      ▼
SSH Enumeration
HTTP Enumeration
NSE Scripts
      │
      ▼
Attack Surface Identified
```

---

# Observations

The scan successfully identified:

| Category | Result |
|----------|--------|
| Open Ports | 22, 80 |
| SSH | OpenSSH 6.6.1p1 |
| HTTP Server | Apache 2.4.7 |
| Web Application | DVWA |
| HTTP Methods | GET, HEAD, POST, OPTIONS |
| robots.txt | Present |
| Cookie Security | HttpOnly not enabled |

---

# Pentester Notes

Even before attempting exploitation, the scan provided valuable reconnaissance information.

From only two open ports, it was possible to identify:

- Operating system family
- SSH implementation
- Web server software
- Web application name
- HTTP methods
- Cookie security settings
- Additional web resources
- SSH fingerprints

This information significantly narrows the scope of subsequent enumeration and vulnerability analysis.

---

# Lessons Learned

- RustScan rapidly identified the target's exposed TCP ports.
- Automatic integration with Nmap eliminated the need for manual port selection.
- Nmap's default scripts provided valuable reconnaissance data beyond simple service detection.
- Minor configuration warnings (such as `ulimit`) did not prevent successful scanning.
- Even a simple two-port scan can reveal extensive information about a target system.

---

# Key Takeaways

- RustScan excels at rapidly discovering open ports and seamlessly handing them off to Nmap for deeper analysis.
- Combining `-sC` and `-sV` enables automatic service detection and execution of useful NSE scripts.
- Enumeration results should be analyzed carefully, as they often reveal valuable information about the target's operating system, software versions, and potential attack surface.
- Effective reconnaissance is not only about finding open ports but also about extracting as much actionable information as possible before moving to the exploitation phase.

# Troubleshooting & Key Concepts

During the practical lab, several warnings and behaviors were encountered while using RustScan. Although these messages may appear alarming at first, most of them are normal and relate to Linux system limits or RustScan's internal optimizations.

This section explains the underlying concepts behind these observations and how to troubleshoot common issues.

---

# Understanding File Descriptors

One of the first warnings encountered during the scan was:

```text
[!] File limit is lower than default batch size.
```

To understand this warning, it is important to understand **file descriptors**.

In Linux, almost everything is treated as a file.

Examples include:

- Regular files
- Directories
- Network sockets
- Pipes
- Terminal devices

Whenever RustScan opens a TCP connection, the operating system allocates one file descriptor.

```text
RustScan
      │
      ▼
TCP Connection
      │
      ▼
File Descriptor
```

Scanning thousands of ports simultaneously requires thousands of file descriptors.

---

# What is `ulimit`?

Linux restricts how many file descriptors a process may open simultaneously.

The current limit can be checked using:

```bash
ulimit -n
```

Example:

```text
1024
```

This means a single process can only keep **1024 files or sockets** open at the same time.

---

# Why Did RustScan Display a Warning?

RustScan prefers to scan many ports simultaneously for maximum speed.

If the operating system's file descriptor limit is lower than RustScan's preferred batch size, the following warning appears:

```text
File limit is lower than default batch size.
```

This warning does **not** indicate that the scan failed.

It simply means RustScan cannot operate at its maximum speed.

---

# Increasing the File Descriptor Limit

RustScan provides the `--ulimit` option.

Example:

```bash
rustscan -a <TARGET> --ulimit 2500
```

RustScan attempts to temporarily increase the available file descriptors before scanning begins.

Example output:

```text
Automatically increasing ulimit value to 2500.
```

---

# Why Did `--ulimit 5000` Fail?

During the lab, using:

```bash
rustscan -a <TARGET> --ulimit 5000
```

produced:

```text
Too many open files.
Please reduce batch size.
```

This occurred because the operating system refused to provide that many file descriptors.

The issue was **not caused by RustScan**, but by the Linux resource limit.

Reducing the value to:

```bash
--ulimit 2500
```

allowed the scan to continue successfully.

---

# Why the Warning Still Appeared

Even after increasing the limit, RustScan continued displaying:

```text
File limit is lower than default batch size.
```

This is expected.

RustScan compares:

- Current file descriptor limit
- Internal batch size

If the batch size is still larger than the available limit, RustScan simply warns the user.

The scan itself remains fully functional.

---

# Batch Size

RustScan scans many ports simultaneously.

Instead of scanning ports one by one, it groups them into batches.

Example:

```text
Ports
1
2
3
...
1000

↓

Batch

↓

Scan Together
```

Larger batches generally increase scanning speed.

However, they also require more file descriptors.

Finding the right balance depends on:

- Operating system limits
- Available memory
- Network performance

---

# RustScan → Nmap Integration

One of RustScan's biggest advantages is its seamless integration with Nmap.

Workflow:

```text
RustScan
      │
      ▼
Port Discovery
      │
      ▼
Open Ports
22
80
      │
      ▼
Generate Nmap Command
      │
      ▼
Automatic Enumeration
```

Example output:

```text
Starting Nmap

The Nmap command to be run is

nmap -sC -sV -vvv -p80,22 <TARGET>
```

RustScan automatically builds the Nmap command using only the discovered open ports.

This eliminates unnecessary scanning and reduces overall reconnaissance time.

---

# Understanding TCP SYN-ACK

While examining the Nmap output, each open service included:

```text
syn-ack ttl 64
```

This refers to the TCP Three-Way Handshake.

```text
Scanner
      │
      │ SYN
      ▼
Target
      │
      │ SYN-ACK
      ▼
Scanner
      │
      │ ACK
      ▼
Connection Established
```

When Nmap receives the **SYN-ACK** response, it confirms that the TCP port is open.

---

# Understanding TTL

TTL stands for **Time To Live**.

It is a field inside the IP header that limits how long a packet may remain on the network.

Every router that forwards the packet decreases the TTL by one.

Example:

```text
TTL = 64

↓

Router

↓

TTL = 63

↓

Router

↓

TTL = 62
```

If TTL reaches zero, the packet is discarded.

---

# TTL in Nmap Output

Example:

```text
22/tcp open ssh syn-ack ttl 64
```

and

```text
80/tcp open http syn-ack ttl 64
```

The TTL shown here belongs to the **SYN-ACK packet** returned by the target service.

While TTL alone cannot accurately identify an operating system, it can provide useful clues during reconnaissance.

Common default values include:

| Operating System | Typical Initial TTL |
|------------------|---------------------|
| Linux / Unix | 64 |
| Windows | 128 |
| Many Network Devices | 255 |

Combined with service banners and OS fingerprinting, TTL becomes another useful indicator during target identification.

---

# Common Beginner Mistakes

## Mistake 1

Assuming RustScan replaces Nmap.

Reality:

RustScan complements Nmap by accelerating the port discovery phase.

---

## Mistake 2

Treating `ulimit` warnings as fatal errors.

Reality:

These warnings usually affect performance rather than functionality.

---

## Mistake 3

Interrupting the scan too early.

After displaying open ports, RustScan automatically launches Nmap.

If interrupted immediately, the enumeration phase never completes.

---

## Mistake 4

Ignoring Nmap output.

RustScan discovers open ports.

Most valuable reconnaissance information comes from the Nmap enumeration that follows.

---

# Pentester Notes

Efficient reconnaissance depends not only on using the right tools, but also on understanding how they interact.

RustScan dramatically accelerates port discovery, while Nmap provides detailed information about exposed services.

Understanding concepts such as:

- File descriptors
- `ulimit`
- Batch size
- TCP SYN-ACK
- TTL

helps troubleshoot scanning issues and interpret results more accurately during penetration tests.

---

# Lessons Learned

- Linux resource limits directly influence RustScan's performance.
- File descriptor warnings are generally informational rather than critical.
- RustScan automatically hands discovered ports to Nmap, creating an efficient reconnaissance workflow.
- TCP SYN-ACK packets confirm open ports during scanning.
- TTL values provide additional context that may assist with operating system fingerprinting.

---

# Key Takeaways

- File descriptors determine how many simultaneous TCP connections RustScan can create.
- `ulimit` controls the maximum number of open file descriptors available to a process.
- Increasing `ulimit` may improve performance but cannot exceed the operating system's configured limits.
- RustScan and Nmap work together to combine rapid port discovery with comprehensive service enumeration.
- Understanding the concepts behind scanning is just as important as knowing the commands themselves.

# Conclusion

RustScan is a modern TCP port scanner that prioritizes **speed**, **automation**, and **efficiency** during the reconnaissance phase of a penetration test.

Rather than attempting to replace traditional tools such as Nmap, RustScan enhances the reconnaissance workflow by rapidly identifying open ports and automatically passing the results to Nmap for detailed enumeration.

Throughout this room, I learned not only how to use RustScan, but also why it has become a popular tool among penetration testers. Understanding its design philosophy, asynchronous scanning model, scripting capabilities, and adaptive optimizations provided valuable insight into how modern reconnaissance tools are engineered for both performance and flexibility.

---

# Skills Gained

After completing this room, I gained practical experience with:

- Installing RustScan on Kali Linux.
- Performing high-speed TCP port discovery.
- Scanning individual hosts, multiple targets, CIDR ranges, and host files.
- Limiting scans to specific ports or custom port ranges.
- Passing discovered ports directly to Nmap.
- Using RustScan alongside Nmap for automated enumeration.
- Understanding the RustScan Scripting Engine (RSE).
- Troubleshooting `ulimit` warnings and Linux file descriptor limitations.
- Interpreting Nmap service detection results generated through RustScan.

---

# Concepts Strengthened

This room reinforced several important networking and penetration testing concepts:

- TCP port scanning
- Reconnaissance methodology
- Enumeration workflows
- TCP Three-Way Handshake
- Asynchronous networking
- Linux file descriptors
- `ulimit`
- Service fingerprinting
- Banner grabbing
- HTTP enumeration

These concepts form the foundation of effective network reconnaissance and will continue to appear throughout future penetration testing labs.

---

# Red Team Perspective

From an offensive security perspective, RustScan significantly reduces the time required to identify exposed services.

A typical workflow becomes:

```text
Target
      │
      ▼
RustScan
      │
      ▼
Open Ports
      │
      ▼
Nmap Enumeration
      │
      ▼
Service Analysis
      │
      ▼
Vulnerability Research
      │
      ▼
Exploitation
```

This allows penetration testers to spend less time discovering ports and more time analyzing the attack surface.

Because RustScan integrates seamlessly with Nmap, it naturally fits into modern penetration testing workflows and automation pipelines.

---

# Blue Team Perspective

Understanding how RustScan operates also benefits defenders.

High-speed port scanning often generates recognizable network activity, including:

- Numerous TCP connection attempts
- Sequential or randomized port scanning
- Immediate service enumeration using Nmap

These behaviors may be detected through:

- Firewall logs
- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Security Information and Event Management (SIEM) platforms
- Endpoint Detection and Response (EDR) solutions

Recognizing reconnaissance activity early can help defenders identify attackers before exploitation begins.

---

# Detection Opportunities

During a reconnaissance phase, defenders may observe:

- Large numbers of TCP SYN packets.
- Rapid connection attempts to multiple ports.
- Banner grabbing against discovered services.
- HTTP requests generated by NSE scripts.
- SSH enumeration attempts.
- Automated service fingerprinting.

Monitoring these behaviors can provide valuable early indicators of malicious activity.

---

# Real-World Relevance

RustScan has become increasingly popular across the cybersecurity community because of its ability to accelerate reconnaissance without sacrificing compatibility with existing tools.

It is commonly used in:

- Internal Penetration Testing
- External Infrastructure Assessments
- Red Team Operations
- Capture The Flag (CTF) competitions
- Hack The Box
- TryHackMe
- Bug Bounty reconnaissance

Its seamless integration with Nmap makes it particularly useful in professional penetration testing engagements.

---

# Challenges Encountered

During the practical lab, I encountered several issues that improved my understanding of RustScan and Linux networking.

## File Descriptor Warning

RustScan displayed:

```text
File limit is lower than default batch size.
```

This warning was caused by Linux file descriptor limits rather than a problem with RustScan itself.

---

## `--ulimit 5000` Failure

Attempting to increase the limit to 5000 resulted in:

```text
Too many open files.
```

Reducing the value to:

```text
--ulimit 2500
```

resolved the issue and allowed RustScan to execute successfully.

---

## Understanding the RustScan → Nmap Workflow

Initially, it appeared that RustScan stopped after discovering open ports.

Further investigation revealed that RustScan was automatically launching Nmap in the background and waiting for the enumeration phase to complete.

This reinforced the importance of understanding how multiple tools interact during a penetration test.

---

# Key Lessons Learned

The most important lesson from this room is that **speed alone is not enough**.

Finding open ports is only the beginning of reconnaissance.

The real value comes from combining fast port discovery with detailed service enumeration and careful analysis.

RustScan accelerates the first stage, while Nmap provides the depth required to understand the target's attack surface.

Together, they create an efficient and repeatable reconnaissance workflow.

---

# Future Learning Path

After mastering RustScan, the next logical topics include:

- Advanced Nmap techniques
- NSE (Nmap Scripting Engine)
- Banner Grabbing
- Web Enumeration
- SMB Enumeration
- SSH Enumeration
- Vulnerability Scanning
- Network Service Exploitation

These subjects build directly upon the reconnaissance skills introduced in this room.

---

# References

- RustScan Official GitHub Repository  
  https://github.com/RustScan/RustScan

- RustScan Documentation  
  https://github.com/RustScan/RustScan/wiki

- Nmap Official Documentation  
  https://nmap.org/docs.html

- TryHackMe — RustScan Room

---

# Tags

```text
TryHackMe
RustScan
Reconnaissance
Port Scanning
Nmap
Linux
Networking
TCP
Enumeration
Cybersecurity
Red Team
Blue Team
Penetration Testing
```