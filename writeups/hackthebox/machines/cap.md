# Hack The Box - Cap

| Machine Information | |
|---------------------|------------------------------------------------|
| Platform | Hack The Box |
| Machine Name | Cap |
| Difficulty | Easy |
| Operating System | Linux |
| Author | Hack The Box |
| Tags | Linux, Web, IDOR, PCAP, FTP, Wireshark, Linux Capabilities, Privilege Escalation |

---

# Executive Summary

**Cap** is an Easy Linux machine from Hack The Box that demonstrates how multiple seemingly minor security weaknesses can be chained together into a complete system compromise.

The attack begins with web application enumeration, where an administrative dashboard exposes a **Security Snapshot** feature. Further analysis reveals an **Insecure Direct Object Reference (IDOR)** vulnerability, allowing access to another user's packet capture (PCAP) file by manipulating predictable object identifiers.

Analysis of the downloaded packet capture using **Wireshark** reveals plaintext FTP credentials because the FTP protocol transmits authentication data without encryption. These credentials are then successfully reused to obtain an SSH foothold on the target system.

During local enumeration, no sudo privileges are available. However, inspection of Linux capabilities reveals that the Python interpreter possesses the **CAP_SETUID** capability. This Linux capability allows Python to change its effective user ID to root, resulting in successful privilege escalation and complete compromise of the machine.

This machine highlights an important lesson in penetration testing:

> Critical compromises often result from chaining several low-to-medium severity weaknesses rather than exploiting a single critical vulnerability.

---

# Learning Objectives

After completing this machine, you should understand how to:

- Perform systematic Linux enumeration using Nmap.
- Enumerate web applications to identify administrative functionality.
- Identify and exploit an Insecure Direct Object Reference (IDOR) vulnerability.
- Enumerate predictable object identifiers using FFUF.
- Analyze packet capture (PCAP) files with Wireshark.
- Extract plaintext credentials from insecure network protocols.
- Understand the risks of FTP authentication over unencrypted channels.
- Perform credential reuse against other exposed services.
- Enumerate Linux systems after gaining an initial foothold.
- Discover Linux capabilities using `getcap`.
- Understand how Linux capabilities differ from SUID binaries.
- Abuse the `CAP_SETUID` capability to escalate privileges.

---

# Skills Gained

## Web Application Security

- HTTP Enumeration
- Resource Discovery
- IDOR Identification
- Authorization Bypass
- Web Application Analysis

## Network Analysis

- Packet Capture Analysis
- Wireshark Fundamentals
- FTP Traffic Inspection
- Credential Harvesting

## Linux

- Linux Enumeration
- SSH Access
- File Permission Analysis
- Linux Capabilities Enumeration
- Privilege Escalation

## Penetration Testing Methodology

- Enumeration
- Vulnerability Validation
- Credential Access
- Lateral Thinking
- Attack Chaining
- Privilege Escalation

---

# Prerequisites

To fully understand this walkthrough, readers should already be familiar with:

- Basic Linux command line usage
- TCP/IP networking fundamentals
- Common network services (FTP, SSH, HTTP)
- Basic Nmap usage
- Basic web application concepts
- Wireshark fundamentals
- Linux permissions
- SSH authentication

---

# Attack Chain

```text
                     Internet
                         │
                         ▼
                 Nmap Enumeration
                         │
                         ▼
            Security Dashboard Discovery
                         │
                         ▼
               Security Snapshot Feature
                         │
                         ▼
               Insecure Direct Object Reference
                         │
                         ▼
            Download Another User's PCAP
                         │
                         ▼
            Wireshark Packet Analysis
                         │
                         ▼
          Plaintext FTP Credential Discovery
                         │
                         ▼
               SSH Credential Reuse
                         │
                         ▼
               Initial User Foothold
                         │
                         ▼
              Linux Local Enumeration
                         │
                         ▼
          Python CAP_SETUID Capability
                         │
                         ▼
              Privilege Escalation
                         │
                         ▼
                     Root Access
```

---

# Technologies Covered

| Technology | Purpose |
|------------|---------|
| HTTP | Web application communication |
| FTP | File transfer protocol used to expose plaintext credentials |
| SSH | Secure remote shell access |
| PCAP | Network packet capture format |
| Wireshark | Packet capture analysis |
| FFUF | Resource and object enumeration |
| Nmap | Network enumeration |
| Linux Capabilities | Fine-grained privilege management |
| Python | Capability abuse for privilege escalation |

---

# Terminology

| Term | Description |
|------|-------------|
| Enumeration | The process of collecting as much information as possible about a target before attempting exploitation. |
| IDOR | Insecure Direct Object Reference, a vulnerability where an application exposes internal object identifiers without proper authorization checks. |
| Object Identifier | A unique identifier used by an application to reference resources such as files, users, or database records. |
| PCAP | Packet Capture file containing recorded network traffic for later analysis. |
| Packet Sniffing | Capturing and inspecting packets traversing a network. |
| Plaintext Credentials | Usernames and passwords transmitted without encryption. |
| Credential Harvesting | Extracting authentication information from captured data or compromised systems. |
| Credential Reuse | Attempting discovered credentials against multiple services or systems. |
| Foothold | The first successful access obtained on a target machine. |
| Linux Capabilities | A Linux security feature that divides root privileges into smaller, individually assignable capabilities. |
| CAP_SETUID | A Linux capability allowing a process to change its effective user ID. |
| Privilege Escalation | The process of gaining higher privileges after initial system access. |
| Effective UID (EUID) | The user identity the kernel uses to determine a process's permissions. |

---

# Machine Objectives

During this machine, the following objectives were completed:

- Enumerate exposed network services.
- Analyze the web application.
- Discover an IDOR vulnerability.
- Enumerate downloadable resources.
- Obtain another user's packet capture.
- Extract FTP credentials from captured traffic.
- Gain SSH access using credential reuse.
- Enumerate local Linux privileges.
- Abuse Linux capabilities.
- Obtain root privileges.
- Capture both `user.txt` and `root.txt`.

---

**Difficulty:** ⭐ Easy

Although rated as an Easy machine, **Cap** introduces several real-world concepts that appear frequently during penetration tests, including web application authorization flaws, packet capture analysis, credential harvesting, Linux capability abuse, and privilege escalation. It serves as an excellent introduction to attack chaining, demonstrating how multiple low-impact weaknesses can combine into a complete system compromise.

# Part 2 — Enumeration

Enumeration is the foundation of every penetration test. Before attempting any exploitation, information about the target system must be collected to identify potential attack surfaces.

The primary objective during this phase is to identify:

- Running services
- Open ports
- Service versions
- Operating system indicators
- Potential entry points
- Web application functionality

Throughout this machine, no vulnerability scanning or exploit frameworks were used. Instead, the enumeration process relied on understanding how exposed services interact and how they may introduce security weaknesses.

---

# Network Enumeration

The first step was performing a TCP port scan using **Nmap**.

```bash
nmap -sC -sV 10.129.54.123
```

## Command Breakdown

| Option | Description |
|----------|-------------|
| `nmap` | Network discovery and security auditing tool |
| `-sC` | Executes Nmap's default NSE (Nmap Scripting Engine) scripts |
| `-sV` | Performs service version detection |
| `10.129.54.123` | Target machine IP address |

---

# Scan Results

```text
PORT   STATE SERVICE VERSION

21/tcp open  ftp     vsftpd 3.0.3

22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu

80/tcp open  http    Gunicorn
```

Only three TCP services were exposed.

---

# Service Analysis

## FTP (Port 21)

```
21/tcp
vsftpd 3.0.3
```

### Purpose

FTP (File Transfer Protocol) provides file transfer functionality between systems.

### Initial Assessment

Although FTP itself was not immediately exploitable, its presence was important because:

- FTP commonly handles authentication.
- Older FTP implementations transmit credentials in plaintext.
- Captured FTP traffic may reveal usernames and passwords.

At this stage, no assumptions were made about FTP. It was simply documented as a potentially valuable service.

---

## SSH (Port 22)

```
22/tcp
OpenSSH 8.2p1 Ubuntu
```

### Purpose

SSH provides encrypted remote administration.

### Initial Assessment

SSH generally becomes useful **after credentials have been obtained**.

Since no valid usernames or passwords had yet been discovered, SSH was not considered the initial attack vector.

Instead, it was documented as a likely destination for credential reuse if authentication data could later be recovered.

---

## HTTP (Port 80)

```
80/tcp
Gunicorn
```

The HTTP service appeared significantly more interesting than the other exposed services.

The webpage title was identified as:

```
Security Dashboard
```

This immediately suggested that the web application contained administrative functionality.

Unlike FTP or SSH, which typically require valid credentials, web applications often expose functionality that can be abused through logical flaws rather than software vulnerabilities.

Because of this, the HTTP service became the primary focus of further enumeration.

---

# Initial Web Enumeration

Opening the web application revealed a simple dashboard containing several navigation options.

One feature immediately stood out:

```
Security Snapshot
```

The name strongly suggested that the application was capable of generating or displaying network captures.

This observation aligned closely with the machine's overall theme involving network traffic analysis.

Instead of immediately searching for exploits, the application's available functionality was explored first.

This follows a common penetration testing methodology:

> Understand what an application is designed to do before attempting to break it.

---

# Discovering Application Behavior

Running a **Security Snapshot** caused the application to redirect the browser to a URL similar to:

```
/data/1
```

Several important observations were made:

- The application referenced resources using numeric identifiers.
- The identifier appeared predictable.
- No random token or UUID was used.
- The URL directly exposed an internal object identifier.

At this stage, no vulnerability had been confirmed.

However, predictable identifiers often warrant further investigation because they may indicate an **Insecure Direct Object Reference (IDOR)** vulnerability.

Rather than immediately assuming the application was vulnerable, the next step was to validate this hypothesis through controlled testing.

---

# Initial IDOR Hypothesis

A penetration tester should always question predictable identifiers.

Questions that naturally arise include:

- Can the identifier be changed?
- Does another identifier return another user's data?
- Does the application verify object ownership?
- Is authorization checked before serving the requested resource?

These questions formed the basis of the next phase of testing.

Initially, manually modifying the identifier produced inconsistent results, suggesting that additional application functionality needed to be investigated.

Further examination of the generated report revealed an option to download the associated packet capture.

This observation shifted the investigation from the report page itself toward the downloadable capture resource, which ultimately became the real attack surface exploited during the foothold phase.

---

# Enumeration Summary

At the end of the enumeration phase, the following information had been collected:

| Finding | Result |
|----------|--------|
| FTP | vsftpd 3.0.3 |
| SSH | OpenSSH 8.2p1 |
| HTTP | Gunicorn Web Application |
| Interesting Feature | Security Dashboard |
| Administrative Function | Security Snapshot |
| Predictable Resource | `/data/<id>` |
| Initial Hypothesis | Possible IDOR |
| Next Investigation | Downloadable packet capture |

---

# Key Takeaways

This phase demonstrates an important penetration testing principle:

> Enumeration is not about finding exploits—it is about understanding the target.

Rather than immediately searching for public vulnerabilities, careful observation of normal application behavior revealed a predictable resource identifier. This ultimately led to the discovery of the application's authorization flaw during the next stage of the assessment.

# Part 3 — Initial Foothold

After completing the initial enumeration, the investigation shifted toward understanding how the **Security Dashboard** managed generated snapshots.

Rather than attacking exposed services directly, the objective became validating whether the application properly enforced authorization when accessing generated resources.

This phase ultimately resulted in discovering an **Insecure Direct Object Reference (IDOR)** vulnerability, obtaining another user's packet capture, extracting plaintext credentials, and gaining an SSH foothold.

---

# Exploring the Security Snapshot Feature

Running a **Security Snapshot** generated a report and redirected the browser to a URL similar to:

```
/data/1
```

At first glance, this appeared to simply display the generated scan.

However, one detail immediately stood out.

The application referenced the report using a predictable numeric identifier.

```
/data/1
```

Instead of using random UUIDs or long unpredictable tokens, the application relied entirely on sequential IDs.

Predictable object identifiers are often worth investigating because applications sometimes validate that an object exists, but fail to verify whether the current user is actually authorized to access it.

This class of vulnerability is known as **Insecure Direct Object Reference (IDOR).**

---

# Initial IDOR Testing

The first hypothesis was straightforward:

> If the identifier is predictable, can another identifier expose another user's report?

Initially, different values were tested manually.

```
/data/2
/data/3
/data/4
```

Most requests redirected back to the dashboard, indicating that simply modifying the report identifier was not sufficient.

Although this did not immediately confirm an IDOR vulnerability, it demonstrated an important penetration testing principle:

> When one resource appears protected, continue examining related functionality rather than abandoning the hypothesis.

---

# Discovering the Download Functionality

Closer inspection of the generated report revealed an additional option:

```
Download PCAP
```

Unlike the report page itself, this feature downloaded the network capture associated with the generated snapshot.

Inspecting the request revealed a new endpoint:

```
/download/1
```

This endpoint became significantly more interesting than `/data/<id>` because it directly referenced downloadable files.

---

# Enumerating Downloadable Objects

Rather than manually testing dozens of identifiers, the endpoint was enumerated using **FFUF**.

A simple list of numeric IDs was created:

```bash
seq 0 20 > ids.txt
```

The download endpoint was then fuzzed.

```bash
ffuf \
-u http://10.129.54.123/download/FUZZ \
-w ids.txt
```

## Command Breakdown

| Option | Description |
|----------|-------------|
| `ffuf` | Fast web fuzzer |
| `-u` | Target URL containing the `FUZZ` keyword |
| `-w` | Wordlist used during enumeration |
| `FUZZ` | Placeholder replaced with each value from the wordlist |

---

# Enumeration Results

The results immediately revealed something unusual.

```
/download/0
Status: 200
Size: 9935

/download/1
Status: 200
Size: 108
```

The difference in response size was significant.

| Endpoint | Size |
|----------|------|
| `/download/0` | 9935 bytes |
| `/download/1` | 108 bytes |

This strongly suggested that the downloaded resources were different.

Since `/download/1` corresponded to the newly generated capture, the much larger file returned by `/download/0` was likely another user's packet capture.

This confirmed the presence of an **IDOR vulnerability**.

The application failed to verify ownership before allowing access to downloadable resources.

---

# Downloading Another User's Capture

The larger packet capture was downloaded for analysis.

```bash
wget http://10.129.54.123/download/0
```

The downloaded file was identified as a packet capture (PCAP) suitable for analysis using Wireshark.

---

# Packet Capture Analysis

The PCAP file was opened with Wireshark.

Instead of blindly browsing packets, attention focused on protocols that commonly expose authentication information.

One protocol immediately stood out:

```
FTP
```

Because FTP does not encrypt authentication traffic, following the TCP stream revealed the complete login sequence.

```
USER nathan

331 Please specify the password.

PASS Buck3tH4TF0RM3!

230 Login successful.
```

The packet capture exposed valid credentials in plaintext.

This occurred because FTP transmits usernames and passwords without encryption, making them visible to anyone capable of capturing the traffic.

---

# Credential Harvesting

The recovered credentials were:

| Username | Password |
|----------|----------|
| nathan | Buck3tH4TF0RM3! |

This phase demonstrates **credential harvesting**, where authentication information is extracted from captured network traffic rather than stolen directly from a system.

No password cracking was required.

No brute force attack was performed.

The credentials were simply exposed because an insecure protocol was used.

---

# Credential Reuse

The next question became:

> Are these credentials reused elsewhere?

Reviewing the original Nmap scan showed that SSH was exposed.

```
22/tcp
OpenSSH
```

Rather than reconnecting to FTP, the credentials were tested against SSH.

```bash
ssh nathan@10.129.54.123
```

Password:

```
Buck3tH4TF0RM3!
```

Authentication succeeded immediately.

This is a classic example of **credential reuse**, where the same username and password combination is valid across multiple services.

---

# Obtaining the Initial Foothold

Successful SSH authentication provided the initial shell on the target system.

Initial verification confirmed access as the user:

```bash
whoami
```

Output:

```
nathan
```

Additional enumeration confirmed the current privileges.

```bash
id
```

Output:

```
uid=1001(nathan)
gid=1001(nathan)
groups=1001(nathan)
```

At this point, an initial foothold had been established.

The objective now shifted from gaining access to escalating privileges in order to obtain full administrative control of the system.

---

# Attack Chain So Far

```text
Nmap Enumeration
        │
        ▼
Security Dashboard
        │
        ▼
Security Snapshot
        │
        ▼
Predictable Resource Identifier
        │
        ▼
Download Endpoint Discovery
        │
        ▼
IDOR Validation
        │
        ▼
Download Another User's PCAP
        │
        ▼
Wireshark Analysis
        │
        ▼
FTP Plaintext Credentials
        │
        ▼
Credential Reuse
        │
        ▼
SSH Foothold
```

---

# Key Takeaways

Several important security concepts were demonstrated during this phase.

- Predictable object identifiers should always be investigated for authorization flaws.
- IDOR vulnerabilities often expose sensitive resources without requiring authentication bypass.
- Packet captures may contain valuable authentication data when insecure protocols are used.
- FTP transmits usernames and passwords in plaintext and should not be used on untrusted networks.
- Credentials obtained from one service should always be evaluated for reuse against other exposed services.
- Careful observation and logical attack chaining can be more effective than searching for software exploits.

Rather than relying on a single critical vulnerability, the initial compromise resulted from combining multiple weaknesses into a complete attack chain.

# Part 4 — Privilege Escalation

After obtaining an initial foothold through SSH, the next objective was to determine whether the compromised user possessed a path to elevated privileges.

Rather than immediately searching for public exploits, a structured local enumeration was performed to identify common privilege escalation vectors.

---

# Local Enumeration

The first step after gaining shell access was to identify the current user.

```bash
whoami
```

Output:

```text
nathan
```

Current user information was then verified.

```bash
id
```

Output:

```text
uid=1001(nathan)
gid=1001(nathan)
groups=1001(nathan)
```

This confirmed that the current shell was running with standard user privileges.

---

# System Information

The hostname was identified to verify the target system.

```bash
hostname
```

Output:

```text
cap
```

The current working directory was also confirmed.

```bash
pwd
```

Output:

```text
/home/nathan
```

---

# Home Directory Inspection

The contents of Nathan's home directory were inspected.

```bash
ls -la
```

Among the normal shell configuration files, two symbolic links immediately stood out.

```text
.bash_history -> /dev/null

.viminfo -> /dev/null
```

### Analysis

Normally, `.bash_history` stores previously executed shell commands.

However, in this system it pointed to:

```text
/dev/null
```

As a result, no shell history would be preserved.

This is sometimes configured to reduce forensic artifacts or prevent sensitive commands from being stored.

Although interesting, it did not provide a privilege escalation path.

---

# Checking Sudo Privileges

One of the first privilege escalation checks on Linux is determining whether the current user has sudo permissions.

```bash
sudo -l
```

Output:

```text
Sorry, user nathan may not run sudo on cap.
```

### Analysis

This confirmed that Nathan had **no sudo privileges**, eliminating one of the most common privilege escalation techniques.

The investigation therefore continued with further local enumeration.

---

# Enumerating Linux Capabilities

Unlike many beginner Linux machines that rely on SUID binaries, this machine required inspecting **Linux Capabilities**.

Capabilities were enumerated using:

```bash
getcap -r / 2>/dev/null
```

## Command Breakdown

| Option | Description |
|----------|-------------|
| `getcap` | Displays Linux capabilities assigned to files |
| `-r` | Recursively search directories |
| `/` | Search the entire filesystem |
| `2>/dev/null` | Suppress permission denied errors |

---

# Enumeration Results

```text
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip

/usr/bin/ping = cap_net_raw+ep

/usr/bin/traceroute6.iputils = cap_net_raw+ep

/usr/bin/mtr-packet = cap_net_raw+ep
```

Most capabilities appeared completely normal.

For example:

```
ping
```

requires:

```
CAP_NET_RAW
```

to create raw sockets.

However, one entry was highly unusual.

```text
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
```

---

# Understanding CAP_SETUID

Linux Capabilities divide the traditional "all-or-nothing" root privilege into smaller, individually assignable privileges.

One of these capabilities is:

```
CAP_SETUID
```

This capability allows a process to change its effective user ID (EUID).

Normally, only root is permitted to change its UID to another user.

However, assigning the `CAP_SETUID` capability to a binary allows that program to perform this action without being executed as root.

Because Python possessed this capability, it became a potential privilege escalation vector.

---

# Why Python Became Dangerous

Python includes the built-in **os** module, which provides direct access to many operating system functions.

One of these functions is:

```python
os.setuid()
```

Normally, executing:

```python
os.setuid(0)
```

as a regular user would fail.

On this machine, however, Python had already been granted the `CAP_SETUID` capability.

As a result, the kernel allowed Python to change its effective UID to:

```
0
```

which corresponds to the **root** user.

---

# Privilege Escalation

A short Python one-liner was used to change the effective UID to root and spawn a new shell.

```bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

## Command Breakdown

| Component | Purpose |
|-----------|---------|
| `python3 -c` | Execute Python code directly from the command line |
| `import os` | Import Python's operating system module |
| `os.setuid(0)` | Change the effective user ID to root |
| `os.system("/bin/bash")` | Spawn a new Bash shell |

---

# Verifying Root Access

The new shell was verified.

```bash
whoami
```

Output:

```text
root
```

The effective user ID was also confirmed.

```bash
id
```

Output:

```text
uid=0(root)
gid=1001(nathan)
groups=1001(nathan)
```

The privilege escalation was successful.

---

# Obtaining the Root Flag

Although the shell now had root privileges, the current working directory remained:

```text
/home/nathan
```

This is expected behavior because spawning a new shell does **not** automatically change the current working directory.

The root user's home directory was accessed manually.

```bash
cd /root
```

Listing its contents revealed the final flag.

```bash
ls
```

Output:

```text
root.txt
```

The flag was then read.

```bash
cat root.txt
```

At this point, the machine was fully compromised.

---

# Privilege Escalation Flow

```text
SSH Foothold
        │
        ▼
Linux Enumeration
        │
        ▼
No Sudo Privileges
        │
        ▼
Enumerate Linux Capabilities
        │
        ▼
Python3.8 Has CAP_SETUID
        │
        ▼
Abuse os.setuid(0)
        │
        ▼
Spawn Root Shell
        │
        ▼
Access /root
        │
        ▼
Retrieve root.txt
```

---

# Key Takeaways

This privilege escalation did **not** rely on:

- Kernel exploits
- Buffer overflows
- Public CVEs
- SUID binaries
- Misconfigured sudo rules

Instead, it abused a legitimate Linux security feature that had been incorrectly configured.

Several important lessons can be drawn from this phase:

- Linux Capabilities should always be included in local privilege escalation enumeration.
- Granting powerful capabilities such as `CAP_SETUID` to general-purpose binaries like Python introduces severe security risks.
- Enumeration is often more valuable than immediately searching for exploits.
- Legitimate operating system features can become powerful attack vectors when misconfigured.

This machine demonstrates that successful privilege escalation often comes from understanding how Linux security mechanisms work internally rather than relying solely on known exploits.

# Part 5 — Concepts Deep Dive

Unlike many beginner machines that focus on a single vulnerability, **Cap** demonstrates how several seemingly harmless weaknesses can be chained together into a complete system compromise.

Understanding **why** each step worked is far more valuable than simply reproducing the attack.

---

# Insecure Direct Object Reference (IDOR)

## What is IDOR?

An **Insecure Direct Object Reference (IDOR)** is an access control vulnerability where an application exposes internal object identifiers without verifying whether the current user is authorized to access the requested object.

For example, consider the following request:

```
GET /download/15
```

The application retrieves object **15**.

If the server only checks whether object **15** exists—but never verifies whether the current user owns it—an attacker may simply change the identifier:

```
/download/14
/download/13
/download/12
```

and gain access to resources belonging to other users.

This is exactly what happened on the Cap machine.

---

## Why Did It Happen?

The application used predictable numeric identifiers.

```
/download/0
/download/1
```

Instead of checking:

> "Does this file belong to the current user?"

the server effectively asked only:

> "Does this file exist?"

This missing authorization check resulted in unauthorized access to another user's packet capture.

---

## Security Impact

IDOR vulnerabilities may expose:

- Personal information
- Medical records
- Financial documents
- Source code
- Internal reports
- Configuration files
- Network captures

Unlike many vulnerabilities, IDOR usually does **not** require exploiting software bugs.

Instead, it exploits **broken authorization logic**.

---

## Red Team Perspective

When a penetration tester encounters URLs such as:

```
/download/15
/profile/5
/invoice/20
/report/8
```

the immediate question becomes:

> Can another identifier be accessed?

Testing sequential object identifiers is a standard part of web application assessments.

---

## Blue Team Perspective

Applications should never trust client-supplied identifiers.

Every request should verify:

- Does the object exist?
- Does the authenticated user own the object?
- Is the current user authorized to access it?

Proper authorization checks eliminate IDOR vulnerabilities.

---

# Packet Capture (PCAP)

## What is a PCAP?

A **Packet Capture (PCAP)** file stores recorded network traffic.

Every packet transmitted across a network can be captured and later analyzed.

PCAP files commonly contain:

- HTTP traffic
- DNS requests
- FTP sessions
- SSH handshakes
- TCP connections
- ICMP packets

Security analysts frequently use PCAP files during:

- Incident response
- Malware analysis
- Network troubleshooting
- Penetration testing

---

## Why Was the PCAP Valuable?

The downloaded capture represented another user's network session.

Rather than attacking the target directly, the packet capture provided historical evidence of network activity.

Analyzing this evidence revealed authentication data transmitted over FTP.

---

# FTP and Plaintext Authentication

## What is FTP?

FTP (File Transfer Protocol) is one of the oldest Internet protocols for transferring files.

Although still encountered in legacy environments, FTP provides **no encryption** by default.

During authentication, the client sends:

```
USER username

PASS password
```

directly across the network.

Anyone capable of observing the traffic can read the credentials.

---

## Why Is This Dangerous?

Unlike HTTPS or SSH, FTP provides neither:

- Confidentiality
- Integrity

As a result:

- Usernames are exposed.
- Passwords are exposed.
- Commands are exposed.
- Directory listings are exposed.

Attackers only need access to captured network traffic to recover credentials.

---

## Secure Alternatives

Modern environments should replace FTP with:

- SFTP (SSH File Transfer Protocol)
- FTPS (FTP over TLS)

These protocols encrypt authentication traffic before transmission.

---

# Credential Harvesting

## Definition

Credential harvesting is the process of obtaining usernames and passwords from various sources rather than cracking them.

Common sources include:

- Packet captures
- Configuration files
- Browser databases
- Password managers
- Log files
- Backup files

On this machine, credentials were harvested directly from captured FTP traffic.

No brute-force attack or password cracking was necessary.

---

# Credential Reuse

## What is Credential Reuse?

Many users reuse identical passwords across multiple services.

For example:

| Service | Username | Password |
|----------|----------|----------|
| FTP | nathan | Buck3tH4TF0RM3! |
| SSH | nathan | Buck3tH4TF0RM3! |

Once credentials were recovered from FTP traffic, they were successfully reused to authenticate via SSH.

Credential reuse remains one of the most common causes of successful compromises in real-world environments.

---

## Defensive Measures

Organizations should enforce:

- Unique passwords
- Password managers
- Multi-factor authentication (MFA)
- Account monitoring
- Password rotation policies

---

# Linux Capabilities

## Why Were Linux Capabilities Created?

Originally, Linux used a simple privilege model.

A process was either:

- Root
- Non-root

There was no middle ground.

Linux Capabilities were introduced to divide root privileges into smaller, individually assignable permissions.

Instead of granting complete administrative access, a process can receive only the capabilities it actually requires.

---

## Common Capabilities

| Capability | Purpose |
|------------|---------|
| CAP_NET_RAW | Create raw sockets |
| CAP_NET_BIND_SERVICE | Bind to privileged ports |
| CAP_SYS_TIME | Modify system time |
| CAP_SYS_ADMIN | Perform various administrative operations |
| CAP_SETUID | Change effective user ID |

Capabilities reduce the need to execute programs as full root.

---

# CAP_SETUID

## Purpose

`CAP_SETUID` allows a process to change its effective user ID.

Normally, only root may execute:

```
setuid(0)
```

However, if a binary possesses the `CAP_SETUID` capability, the kernel permits that binary to perform the operation.

---

## Why Was Python Dangerous?

Python is a general-purpose interpreter capable of executing arbitrary code.

Because it possessed:

```
CAP_SETUID
```

Python was able to execute:

```python
os.setuid(0)
```

and successfully become root.

The problem was **not Python itself**.

The problem was assigning a powerful capability to an interpreter capable of executing arbitrary user-supplied code.

---

## Why This Is Considered Misconfiguration

Capabilities should be granted only to applications that genuinely require them.

Assigning `CAP_SETUID` to:

- Python
- Perl
- Ruby
- Bash

is extremely dangerous because these interpreters allow unrestricted code execution.

An attacker gaining access to such a binary can often elevate privileges immediately.

---

# Red Team Perspective

From an attacker's perspective, Cap demonstrates the importance of chaining findings together.

Each discovery enabled the next stage of the attack.

```
Enumeration
        │
        ▼
IDOR
        │
        ▼
Packet Capture
        │
        ▼
Credential Harvesting
        │
        ▼
Credential Reuse
        │
        ▼
SSH Foothold
        │
        ▼
Linux Enumeration
        │
        ▼
Capability Abuse
        │
        ▼
Root
```

No single vulnerability alone provided complete system compromise.

Instead, the attacker combined multiple weaknesses into one successful attack chain.

---

# Blue Team Perspective

Several defensive improvements would have completely prevented this compromise.

## Web Application

- Implement proper authorization checks.
- Prevent IDOR vulnerabilities.
- Use unpredictable resource identifiers where appropriate.

## Network Security

- Replace FTP with SFTP or FTPS.
- Encrypt all authentication traffic.
- Restrict access to packet captures.

## Authentication

- Enforce unique passwords.
- Prevent credential reuse.
- Deploy Multi-Factor Authentication.

## Linux Hardening

- Audit Linux capabilities regularly.
- Avoid granting dangerous capabilities to scripting interpreters.
- Follow the Principle of Least Privilege.

---

# Detection Opportunities

Security teams could detect this attack through several indicators.

## Web Layer

- Sequential requests to multiple object identifiers.
- Enumeration of downloadable resources.
- Abnormal access to other users' files.

## Authentication

- Successful SSH login immediately following FTP activity.
- Logins from unusual IP addresses.
- Authentication using previously exposed credentials.

## Host Monitoring

- Execution of:

```bash
getcap -r /
```

- Python invoking:

```python
os.setuid(0)
```

- Unexpected root shell spawned by Python.

These behaviors may be detected through endpoint monitoring, audit logs, or Endpoint Detection and Response (EDR) solutions.

---

# Key Lessons Learned

Cap demonstrates that successful attacks rarely rely on a single vulnerability.

Instead, attackers combine multiple weaknesses to progressively increase their level of access.

The complete compromise resulted from the following chain:

1. Broken authorization (IDOR)
2. Exposure of another user's packet capture
3. Plaintext FTP credentials
4. Credential reuse
5. Linux capability misconfiguration

Each weakness individually might appear minor.

Together, they resulted in complete root compromise of the target system.

This machine serves as an excellent introduction to **attack chaining**, reinforcing the importance of systematic enumeration, logical reasoning, and understanding how different technologies interact within a real-world penetration test.

# Part 6 — Conclusion

The **Cap** machine demonstrates how multiple low-severity weaknesses can be combined into a complete system compromise.

Rather than exploiting a vulnerable service or a known CVE, the attack relied entirely on careful enumeration, logical analysis, and understanding how different technologies interact.

The attack began by identifying a **Security Dashboard** exposed through the web application. Further investigation revealed an **Insecure Direct Object Reference (IDOR)** vulnerability that allowed access to another user's packet capture.

Analyzing the PCAP file with **Wireshark** exposed plaintext FTP credentials because FTP transmits authentication data without encryption. Those credentials were then successfully reused to gain SSH access.

After obtaining an initial foothold, systematic Linux enumeration revealed that the Python interpreter possessed the **CAP_SETUID** Linux capability. This legitimate Linux security feature had been misconfigured, allowing Python to change its effective user ID to root and ultimately providing full administrative access to the machine.

Cap is an excellent example of **attack chaining**, showing that a complete compromise often results from combining several smaller weaknesses rather than exploiting a single critical vulnerability.

---

# Security Recommendations

## Web Application Security

- Implement proper authorization checks for every protected resource.
- Never rely solely on predictable object identifiers.
- Validate resource ownership before returning sensitive data.
- Perform regular authorization testing to identify IDOR vulnerabilities.

---

## Network Security

- Replace FTP with encrypted alternatives such as SFTP or FTPS.
- Encrypt all authentication traffic.
- Restrict access to packet capture files.
- Protect administrative functionality behind authentication and authorization.

---

## Authentication Security

- Enforce unique passwords across services.
- Implement Multi-Factor Authentication (MFA).
- Prevent credential reuse through password policies.
- Monitor authentication logs for suspicious login activity.

---

## Linux Hardening

- Periodically audit Linux capabilities.

```bash
getcap -r / 2>/dev/null
```

- Avoid assigning dangerous capabilities such as `CAP_SETUID` to scripting interpreters.
- Apply the Principle of Least Privilege.
- Regularly review privileged binaries and system permissions.

---

# Commands Reference

| Command | Purpose |
|----------|---------|
| `nmap -sC -sV <IP>` | Enumerate open ports and service versions |
| `ffuf -u URL/FUZZ -w wordlist` | Enumerate predictable object identifiers |
| `wget URL` | Download remote files |
| `ssh user@IP` | Connect to the target via SSH |
| `whoami` | Display the current user |
| `id` | Display user and group information |
| `hostname` | Display the system hostname |
| `pwd` | Display the current working directory |
| `ls -la` | List files with permissions and hidden entries |
| `sudo -l` | Check sudo privileges |
| `getcap -r / 2>/dev/null` | Enumerate Linux capabilities |
| `python3 -c '...'` | Execute Python code directly from the command line |
| `cat user.txt` | Read the user flag |
| `cat root.txt` | Read the root flag |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Reconnaissance | Active Scanning | T1595 |
| Discovery | Network Service Discovery | T1046 |
| Initial Access | Exploit Public-Facing Application (IDOR) | T1190 |
| Credential Access | Network Sniffing | T1040 |
| Credential Access | Unsecured Credentials | T1552 |
| Lateral Movement / Initial Access | Valid Accounts | T1078 |
| Discovery | System Owner/User Discovery | T1033 |
| Discovery | File and Directory Discovery | T1083 |
| Privilege Escalation | Abuse Elevation Control Mechanism (Linux Capabilities) | T1548 |

> **Note:** While MITRE ATT&CK does not include a technique specifically for Linux Capabilities, abusing `CAP_SETUID` is generally categorized under **T1548 – Abuse Elevation Control Mechanism**, as it involves exploiting privilege management mechanisms to elevate access.

---

# Skills Gained

After completing this machine, the following skills were strengthened:

### Web Security

- Web enumeration
- Identifying IDOR vulnerabilities
- Understanding authorization flaws
- Enumerating predictable object identifiers

### Networking

- Packet capture analysis
- Wireshark usage
- FTP protocol analysis
- Credential harvesting

### Linux

- SSH access
- Local enumeration
- Linux capability enumeration
- Privilege escalation through capability abuse

### Penetration Testing

- Enumeration methodology
- Attack chaining
- Credential reuse
- Logical vulnerability validation
- Post-exploitation enumeration

---

# Lessons Learned

Throughout this machine, several important penetration testing principles became clear.

- Enumeration should always come before exploitation.
- Understanding application behavior is often more valuable than searching for public exploits.
- Predictable object identifiers should always be investigated for authorization flaws.
- Packet captures can expose sensitive information when insecure protocols are used.
- Credentials recovered from one service should always be evaluated against other exposed services.
- Linux Capabilities are an important privilege escalation vector that should never be overlooked during local enumeration.
- Small security weaknesses can combine into a critical compromise when chained together.

---

# Future Learning Path

This machine provides a strong foundation for several advanced topics.

Recommended next areas of study include:

- Broken Access Control
- Advanced IDOR exploitation
- Authentication and session management vulnerabilities
- API security testing
- Linux privilege escalation
- GTFOBins
- Linux Capabilities
- SUID binaries
- Windows privilege escalation
- Active Directory attack paths

---

# References

## Hack The Box

- https://app.hackthebox.com/

## OWASP

- https://owasp.org/www-community/attacks/Insecure_Direct_Object_Reference

- https://owasp.org/Top10/A01_2021-Broken_Access_Control/

## Linux Capabilities

- https://man7.org/linux/man-pages/man7/capabilities.7.html

## Wireshark

- https://www.wireshark.org/

## Nmap

- https://nmap.org/

## FFUF

- https://github.com/ffuf/ffuf

## MITRE ATT&CK

- https://attack.mitre.org/

---

# Tags

```text
Hack The Box
HTB
Cap
Linux
Easy
Web Security
Broken Access Control
IDOR
OWASP Top 10
Packet Capture
PCAP
Wireshark
FTP
Credential Harvesting
Credential Reuse
SSH
Linux Enumeration
Linux Capabilities
CAP_SETUID
Privilege Escalation
Nmap
FFUF
Attack Chain
CyberJourney
Penetration Testing
```

---

# Final Thoughts

Although **Cap** is categorized as an **Easy** machine, it introduces several concepts that are frequently encountered during real-world penetration tests.

Instead of relying on software vulnerabilities or public exploits, the compromise was achieved by chaining together multiple weaknesses:

- Broken authorization (IDOR)
- Sensitive information disclosure
- Plaintext credential exposure
- Credential reuse
- Linux capability misconfiguration

This machine reinforces one of the most important lessons in penetration testing:

> **Successful attackers rarely rely on a single vulnerability. They succeed by connecting multiple weaknesses into a complete attack chain.**

Understanding *why* each step works—not simply reproducing the commands—is what transforms a beginner into a capable penetration tester.