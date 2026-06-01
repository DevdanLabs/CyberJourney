# Windows Command Line Fundamentals - TryHackMe Writeup

## Overview

This room introduces the Microsoft Windows Command Prompt (`cmd.exe`) and demonstrates how command-line tools can be used to gather system information, troubleshoot network issues, manage files and directories, and inspect running processes.

For penetration testers, system administrators, and security analysts, mastering the command line is essential because it provides faster access to information and enables efficient system enumeration.

---

# Why Learn the Windows Command Line?

## Advantages of CLI

### Faster Workflow

Tasks that require multiple clicks in a graphical interface can often be completed with a single command.

Example:

```cmd
ipconfig
```

### Lower Resource Consumption

CLI applications consume fewer resources than GUI applications, making them ideal for:

* Servers
* Virtual Machines
* Cloud environments
* Remote systems

### Automation

Commands can be scripted and executed automatically through batch files and PowerShell scripts.

### Remote Administration

Command-line tools work extremely well over:

* SSH
* PowerShell Remoting
* Remote administration tools

---

# Initial Enumeration Workflow

One of the first actions a penetration tester performs after gaining access to a Windows machine is basic enumeration.

Common workflow:

```cmd
whoami
hostname
ver
systeminfo
ipconfig /all
netstat -abon
tasklist
```

These commands quickly reveal:

* Current user
* Computer name
* Operating system version
* Hardware specifications
* Network configuration
* Open ports
* Running processes

---

# Basic System Information

## Environment Variables

### Purpose

Displays all environment variables configured on the system.

### Syntax

```cmd
set
```

### Example Output

```text
OS=Windows_NT
PROCESSOR_ARCHITECTURE=AMD64
Path=C:\Windows\System32;C:\Windows;
```

### How to Read the Output

#### OS

```text
OS=Windows_NT
```

Shows the operating system family.

#### PROCESSOR_ARCHITECTURE

```text
PROCESSOR_ARCHITECTURE=AMD64
```

Indicates a 64-bit operating system.

#### PATH

```text
Path=C:\Windows\System32;C:\Windows;
```

Directories listed in PATH are searched when a command is executed.

### Pentesting Relevance

PATH misconfigurations may lead to:

* PATH Hijacking
* Unquoted Service Path vulnerabilities
* Privilege Escalation opportunities

### Linux Equivalent

```bash
env
```

---

## Windows Version

### Purpose

Displays the installed Windows version.

### Syntax

```cmd
ver
```

### Example Output

```text
Microsoft Windows [Version 10.0.17763.1821]
```

### How to Read the Output

```text
10.0.17763.1821
```

This is the Windows build version.

Different versions may contain:

* Different vulnerabilities
* Different patch levels
* Different exploit opportunities

### Pentesting Relevance

Useful for:

* Vulnerability assessment
* Exploit selection
* Patch verification

### Linux Equivalent

```bash
uname -a
```

---

## Detailed System Information

### Purpose

Displays comprehensive information about the operating system and hardware.

### Syntax

```cmd
systeminfo
```

### Example Output

```text
Host Name:                 WIN-SRV-2019
OS Name:                   Microsoft Windows Server 2019 Datacenter
OS Version:                10.0.17763 N/A Build 17763
System Type:               x64-based PC
Total Physical Memory:     4,095 MB
```

### How to Read the Output

#### Host Name

```text
WIN-SRV-2019
```

Computer name.

#### OS Name

```text
Microsoft Windows Server 2019 Datacenter
```

Installed operating system.

#### OS Version

```text
10.0.17763
```

Operating system build number.

#### System Type

```text
x64-based PC
```

Architecture of the operating system.

#### Total Physical Memory

```text
4095 MB
```

Installed RAM.

### Pentesting Relevance

One of the most important enumeration commands.

Useful for:

* OS fingerprinting
* Vulnerability research
* Privilege escalation
* Domain discovery

### Linux Equivalent

```bash
hostnamectl
free -h
lscpu
```

---

## Driver Enumeration

### Purpose

Displays installed device drivers.

### Syntax

```cmd
driverquery
```

### Example Output

```text
Module Name      Display Name
ACPI             Microsoft ACPI Driver
Beep             Beep
disk             Disk Driver
```

### How to Read the Output

#### Module Name

Internal driver name.

#### Display Name

Human-readable driver name.

### Pentesting Relevance

Vulnerable drivers are a common privilege escalation vector.

### Linux Equivalent

```bash
lsmod
```

---

## Help System

### Purpose

Displays help information for commands.

### Syntax

```cmd
help
```

or

```cmd
systeminfo /?
```

### Example Output

```text
For more information on a specific command,
type HELP command-name
```

### Pentesting Relevance

Helpful during engagements when syntax is forgotten.

---

## Clear Screen

### Purpose

Clears the terminal screen.

### Syntax

```cmd
cls
```

### Linux Equivalent

```bash
clear
```

---

# Network Configuration and Troubleshooting

## Display Network Configuration

### Purpose

Displays basic network settings.

### Syntax

```cmd
ipconfig
```

### Example Output

```text
IPv4 Address. . . . . . . . . . : 10.10.230.237
Subnet Mask . . . . . . . . . . : 255.255.0.0
Default Gateway . . . . . . . . : 10.10.0.1
```

### How to Read the Output

#### IPv4 Address

```text
10.10.230.237
```

The machine's IP address.

#### Subnet Mask

```text
255.255.0.0
```

Defines the network boundary.

#### Default Gateway

```text
10.10.0.1
```

Router used to communicate outside the local network.

### Pentesting Relevance

Useful for:

* Identifying network ranges
* Internal network mapping
* Pivoting opportunities

### Linux Equivalent

```bash
ip addr
```

---

## Detailed Network Configuration

### Purpose

Displays complete network information.

### Syntax

```cmd
ipconfig /all
```

### Example Output

```text
Physical Address . . . . . : 02-B7-DF-1D-0D-99
DHCP Enabled . . . . . . . : Yes
DNS Servers . . . . . . . : 10.0.0.2
```

### How to Read the Output

#### Physical Address

```text
02-B7-DF-1D-0D-99
```

MAC address of the network adapter.

#### DHCP Enabled

```text
Yes
```

IP address is assigned automatically.

#### DNS Servers

```text
10.0.0.2
```

Server used for name resolution.

### Pentesting Relevance

Provides valuable information about:

* Internal infrastructure
* DNS services
* DHCP services

---

## Connectivity Testing

### Purpose

Tests network connectivity.

### Syntax

```cmd
ping example.com
```

### Example Output

```text
Reply from 93.184.215.14:
bytes=32 time=78ms TTL=52
```

### How to Read the Output

#### Reply From

Target is reachable.

#### Time

```text
78ms
```

Round-trip latency.

#### TTL

```text
52
```

Remaining Time-To-Live value.

### Pentesting Relevance

Useful for:

* Host discovery
* Connectivity testing
* Troubleshooting

### Linux Equivalent

```bash
ping example.com
```

---

## Route Tracing

### Purpose

Displays the network path to a target.

### Syntax

```cmd
tracert example.com
```

### Example Output

```text
1  10.10.0.1
2  100.100.2.56
3  131.103.117.104
```

### How to Read the Output

Each numbered line represents a hop (router) between the source and destination.

### Pentesting Relevance

Useful for:

* Infrastructure mapping
* Network troubleshooting
* Route analysis

### Linux Equivalent

```bash
traceroute example.com
```

---

## DNS Resolution

### Purpose

Resolves domain names to IP addresses.

### Syntax

```cmd
nslookup example.com
```

### Example Output

```text
Server: 10.0.0.2

Name: example.com
Address: 93.184.215.14
```

### How to Read the Output

#### Server

DNS server that answered the query.

#### Address

Resolved IP address.

### Pentesting Relevance

Useful for:

* DNS enumeration
* Infrastructure discovery
* Troubleshooting

### Linux Equivalent

```bash
dig
host
nslookup
```

---

## Network Connections

### Purpose

Displays active network connections.

### Syntax

```cmd
netstat
```

### Example Output

```text
Proto Local Address     Foreign Address     State
TCP   10.10.230.237:22  10.11.81.126:53486  ESTABLISHED
```

### How to Read the Output

#### Local Address

The machine's IP and port.

#### Foreign Address

Remote endpoint.

#### State

Connection status.

Common states:

```text
LISTENING
ESTABLISHED
TIME_WAIT
```

### Pentesting Relevance

Useful for identifying:

* Active connections
* Open services
* Potential attack surfaces

### Linux Equivalent

```bash
netstat
ss
```

---

## Advanced Network Enumeration

### Purpose

Displays active connections, listening ports, executables, and process IDs.

### Syntax

```cmd
netstat -abon
```

### Example Output

```text
TCP    0.0.0.0:22      0.0.0.0:0      LISTENING     2116
[sshd.exe]
```

### How to Read the Output

#### Port

```text
22
```

Listening port.

#### PID

```text
2116
```

Associated process ID.

#### Executable

```text
sshd.exe
```

Program responsible for the connection.

### Pentesting Relevance

One of the most useful enumeration commands.

Maps:

```text
Port
↓
Process
↓
PID
```

### Linux Equivalent

```bash
ss -tulpn
netstat -tulpn
```

---

# File and Directory Management

## Current Directory

### Syntax

```cmd
cd
```

### Example Output

```text
C:\Users\strategos
```

### Meaning

Shows the current working directory.

### Linux Equivalent

```bash
pwd
```

---

## List Directory Contents

### Syntax

```cmd
dir
```

### Example Output

```text
Desktop
Documents
Downloads
Pictures
Videos
```

### Meaning

Displays files and folders inside the current directory.

### Linux Equivalent

```bash
ls
```

---

## Display Hidden Files

### Syntax

```cmd
dir /a
```

### Purpose

Shows hidden and system files.

### Linux Equivalent

```bash
ls -la
```

---

## Recursive Directory Listing

### Syntax

```cmd
dir /s
```

### Purpose

Displays files in all subdirectories.

### Linux Equivalent

```bash
ls -R
```

---

## Visualize Directory Structure

### Syntax

```cmd
tree
```

### Example Output

```text
C:
├── Desktop
├── Documents
├── Downloads
└── Pictures
```

### Linux Equivalent

```bash
tree
```

---

## Change Directory

### Syntax

```cmd
cd Documents
```

Move one level up:

```cmd
cd ..
```

### Linux Equivalent

```bash
cd
```

---

## Create Directory

### Syntax

```cmd
mkdir backup_files
```

### Example Output

```text
Directory created successfully.
```

### Linux Equivalent

```bash
mkdir backup_files
```

---

## Remove Directory

### Syntax

```cmd
rmdir backup_files
```

### Linux Equivalent

```bash
rmdir backup_files
```

---

## Read Text Files

### Syntax

```cmd
type file.txt
```

### Example Output

```text
Hello World
```

### Linux Equivalent

```bash
cat file.txt
```

---

## Copy Files

### Syntax

```cmd
copy file.txt backup.txt
```

### Example Output

```text
1 file(s) copied.
```

### Linux Equivalent

```bash
cp file.txt backup.txt
```

---

## Move Files

### Syntax

```cmd
move file.txt ..
```

### Example Output

```text
1 file(s) moved.
```

### Linux Equivalent

```bash
mv file.txt ..
```

---

## Delete Files

### Syntax

```cmd
del file.txt
```

or

```cmd
erase file.txt
```

### Linux Equivalent

```bash
rm file.txt
```

---

# Task and Process Management

## Display Running Processes

### Purpose

Lists all running processes.

### Syntax

```cmd
tasklist
```

### Example Output

```text
Image Name      PID      Mem Usage
lsass.exe       592      16,108 K
svchost.exe     704      23,432 K
```

### How to Read the Output

#### Image Name

Process executable name.

#### PID

Process ID.

#### Mem Usage

Memory consumed by the process.

### Pentesting Relevance

Useful for identifying:

* Antivirus solutions
* Databases
* Password managers
* VPN software
* Monitoring tools

### Linux Equivalent

```bash
ps aux
```

---

## Filter Processes

### Syntax

```cmd
tasklist /FI "imagename eq sshd.exe"
```

### Example Output

```text
sshd.exe    2116
sshd.exe    2712
```

### Meaning

Displays only processes matching the specified filter.

### Linux Equivalent

```bash
ps aux | grep ssh
```

---

## Terminate Process by PID

### Syntax

```cmd
taskkill /PID 4567
```

### Example Output

```text
SUCCESS: The process has been terminated.
```

### Linux Equivalent

```bash
kill 4567
```

---

## Force Terminate Process

### Syntax

```cmd
taskkill /PID 4567 /F
```

### Linux Equivalent

```bash
kill -9 4567
```

---

## Kill Process by Name

### Syntax

```cmd
taskkill /IM notepad.exe
```

or

```cmd
taskkill /IM notepad.exe /F
```

### Linux Equivalent

```bash
pkill notepad
```

---

# Windows vs Linux Quick Reference

| Linux      | Windows CMD |
| ---------- | ----------- |
| pwd        | cd          |
| ls         | dir         |
| ls -la     | dir /a      |
| ls -R      | dir /s      |
| tree       | tree        |
| cat        | type        |
| cp         | copy        |
| mv         | move        |
| rm         | del         |
| mkdir      | mkdir       |
| rmdir      | rmdir       |
| ps aux     | tasklist    |
| grep       | findstr     |
| kill       | taskkill    |
| ip addr    | ipconfig    |
| traceroute | tracert     |
| dig        | nslookup    |
| ss         | netstat     |
| clear      | cls         |
| env        | set         |
| uname -a   | ver         |

---

# Key Takeaways

The most valuable commands learned in this room for penetration testing are:

```cmd
whoami
hostname
systeminfo
ipconfig /all
netstat -abon
tasklist
```

Together, these commands provide a quick overview of the target's:

* Identity
* Operating system
* Hardware
* Network configuration
* Open ports
* Running services

These commands form the foundation of Windows enumeration and will be used repeatedly in future topics such as privilege escalation, Active Directory enumeration, lateral movement, and post-exploitation.
