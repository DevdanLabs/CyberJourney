# Windows Shell Basics – PowerShell

## TryHackMe Writeup

---

# Room Information

**Room Name:** Windows Shell Basics – PowerShell
**Platform:** TryHackMe
**Module:** Command Line
**Difficulty:** Beginner

---

# Learning Objectives

In this room, we learned:

* What PowerShell is and why it was created
* The difference between CMD and PowerShell
* Object-oriented concepts in PowerShell
* Basic PowerShell cmdlets
* Navigating the file system
* Working with files and directories
* Piping, filtering, and sorting data
* Gathering system and network information
* Real-time system analysis
* Basic PowerShell scripting concepts
* Remote command execution using Invoke-Command

---

# What is PowerShell?

According to Microsoft:

> PowerShell is a cross-platform task automation solution consisting of a command-line shell, a scripting language, and a configuration management framework.

Unlike CMD, which works primarily with plain text, PowerShell works with **objects**.

---

## CMD vs PowerShell

### CMD

```cmd
tasklist
```

Output:

```text
chrome.exe
explorer.exe
notepad.exe
```

Only text is returned.

---

### PowerShell

```powershell
Get-Process
```

Output:

```text
Handles NPM(K) PM(K) WS(K) CPU(s) Id ProcessName
------- ------ ----- ----- ------ -- -----------
300     20     50    80    1.5    1234 chrome
```

PowerShell returns objects containing:

* Properties
* Methods
* Metadata

This makes filtering and automation much easier.

---

# Object-Oriented Approach

PowerShell was developed using an **Object-Oriented** approach.

Objects contain:

## Properties

Information about the object.

Example:

```text
ProcessName
Id
CPU
WorkingSet
```

---

## Methods

Actions that can be performed.

Example:

```text
Kill()
Refresh()
CloseMainWindow()
```

---

# Launching PowerShell

## From Command Prompt

```cmd
powershell
```

Output:

```powershell
PS C:\Users\captain>
```

Meaning:

* PowerShell session started
* Current directory is C:\Users\captain

---

# Basic Syntax: Verb-Noun

PowerShell cmdlets follow:

```text
Verb-Noun
```

Examples:

| Cmdlet       | Meaning              |
| ------------ | -------------------- |
| Get-Process  | Retrieve processes   |
| Get-Service  | Retrieve services    |
| Get-Content  | Read file contents   |
| Set-Location | Change directory     |
| Remove-Item  | Delete files/folders |

---

# Essential Cmdlets

---

## Get-Command

Lists all available commands.

```powershell
Get-Command
```

Output:

```text
CommandType Name
----------- ----
Cmdlet      Get-Process
Cmdlet      Get-Service
Alias       cd
Alias       dir
```

### Importance

Useful for discovering available functionality.

---

## Filtering Commands

Show only functions:

```powershell
Get-Command -CommandType Function
```

---

## Get-Help

Displays documentation.

```powershell
Get-Help Get-Date
```

Useful options:

```powershell
Get-Help Get-Date -Examples
Get-Help Get-Date -Detailed
Get-Help Get-Date -Full
Get-Help Get-Date -Online
```

Equivalent to Linux:

```bash
man
```

---

## Get-Alias

Lists aliases.

```powershell
Get-Alias
```

Examples:

| Alias | Actual Cmdlet |
| ----- | ------------- |
| dir   | Get-ChildItem |
| ls    | Get-ChildItem |
| cd    | Set-Location  |
| pwd   | Get-Location  |
| cat   | Get-Content   |

---

# File System Navigation

---

## Get-ChildItem

Equivalent:

```cmd
dir
```

Linux:

```bash
ls
```

Command:

```powershell
Get-ChildItem
```

Output:

```text
Mode LastWriteTime Length Name
---- ------------- ------ ----
d-r---              Desktop
d-r---              Documents
-a---- 1234         notes.txt
```

### Reading the Output

| Field  | Meaning   |
| ------ | --------- |
| d      | Directory |
| -a     | File      |
| Length | File size |
| Name   | Item name |

---

## Set-Location

Equivalent:

```cmd
cd
```

Command:

```powershell
Set-Location Documents
```

or

```powershell
cd Documents
```

---

# File Management

---

## New-Item

Create folders:

```powershell
New-Item -Path .\TestFolder -ItemType Directory
```

Create files:

```powershell
New-Item -Path .\notes.txt -ItemType File
```

---

## Remove-Item

Delete files/folders.

```powershell
Remove-Item notes.txt
```

---

## Copy-Item

Copy files.

```powershell
Copy-Item file1.txt file2.txt
```

---

## Move-Item

Move files.

```powershell
Move-Item notes.txt C:\Temp\
```

---

## Get-Content

Equivalent:

```bash
cat
```

Command:

```powershell
Get-Content notes.txt
```

Output:

```text
Hello World
```

---

# Piping, Filtering and Sorting

---

# Piping

Pipe operator:

```powershell
|
```

Used to pass objects between commands.

Example:

```powershell
Get-ChildItem | Sort-Object Length
```

Flow:

```text
Get files
↓
Pass objects
↓
Sort by Length
```

---

# Sort-Object

Sort output.

```powershell
Get-ChildItem | Sort-Object Length
```

---

# Where-Object

Filter objects.

Example:

```powershell
Get-ChildItem |
Where-Object Length -gt 100
```

Shows files larger than 100 bytes.

---

## Common Operators

| Operator | Meaning          |
| -------- | ---------------- |
| -eq      | Equal            |
| -ne      | Not Equal        |
| -gt      | Greater Than     |
| -ge      | Greater or Equal |
| -lt      | Less Than        |
| -le      | Less or Equal    |
| -like    | Pattern Match    |

---

## Pattern Matching

```powershell
Get-ChildItem |
Where-Object Name -like "*.txt"
```

---

# Select-Object

Choose displayed properties.

```powershell
Get-ChildItem |
Select-Object Name,Length
```

Output:

```text
Name            Length
----            ------
file1.txt       100
file2.txt       200
```

---

# Select-String

Equivalent:

```bash
grep
```

Command:

```powershell
Select-String -Path file.txt -Pattern password
```

Output:

```text
file.txt:5:password=admin123
```

---

# System Information

---

## Get-ComputerInfo

Displays system details.

```powershell
Get-ComputerInfo
```

Important fields:

```text
WindowsProductName
WindowsEditionId
WindowsBuildLabEx
```

Used for:

* OS identification
* Version checks
* Vulnerability assessment

---

## Get-LocalUser

Displays local users.

```powershell
Get-LocalUser
```

Output:

```text
Name          Enabled
----          -------
Administrator True
captain       True
Guest         False
```

---

## Get-NetIPConfiguration

Network configuration.

```powershell
Get-NetIPConfiguration
```

Output:

```text
IPv4Address        : 10.10.178.209
IPv4DefaultGateway : 10.10.0.1
DNSServer          : 10.0.0.2
```

---

## Get-NetIPAddress

Displays all IP addresses.

```powershell
Get-NetIPAddress
```

Useful for:

* Identifying interfaces
* Finding multiple networks
* Pivoting opportunities

---

# Real-Time Analysis

---

## Get-Process

Displays running processes.

```powershell
Get-Process
```

Important columns:

| Column      | Meaning      |
| ----------- | ------------ |
| ProcessName | Process name |
| Id          | PID          |
| CPU         | CPU usage    |
| WorkingSet  | Memory usage |

---

## Get-Service

Displays services.

```powershell
Get-Service
```

Output:

```text
Status Name
------ ----
Running BFE
Stopped AppIDSvc
```

---

## Get-NetTCPConnection

Displays active TCP connections.

```powershell
Get-NetTCPConnection
```

Important fields:

| Field         | Meaning          |
| ------------- | ---------------- |
| LocalAddress  | Local IP         |
| LocalPort     | Local port       |
| RemoteAddress | Remote IP        |
| RemotePort    | Remote port      |
| State         | Connection state |

Common states:

```text
Listen
Established
TimeWait
```

---

## Get-FileHash

Generates file hashes.

```powershell
Get-FileHash file.txt
```

Output:

```text
Algorithm : SHA256
Hash      : AABBCCDDEEFF...
```

Useful for:

* Malware analysis
* Integrity verification
* IOC comparison

---

## Alternate Data Streams (ADS)

View ADS:

```powershell
Get-Item file.txt -Stream *
```

Example:

```text
file.txt::$DATA
file.txt:hiddenstream
```

Useful during:

* Threat hunting
* Malware investigations

---

# Scripting

Scripting automates repetitive tasks.

Example:

enumeration.ps1

```powershell
Get-ComputerInfo
Get-LocalUser
Get-NetIPAddress
Get-Process
```

Execute:

```powershell
.\enumeration.ps1
```

---

# Invoke-Command

Execute commands remotely.

Run a script:

```powershell
Invoke-Command -FilePath script.ps1 -ComputerName Server01
```

Run a command remotely:

```powershell
Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Service }
```

---

# TryHackMe Tasks and Solutions

---

## Task: What do we call the advanced approach used to develop PowerShell?

Answer:

```text
Object-oriented
```

---

## Task: Retrieve items larger than 100 bytes

Command:

```powershell
Get-ChildItem | Where-Object Length -gt 100
```

Explanation:

* Get all items
* Filter by Length
* Show only items greater than 100

---

## Task: Find the Tampered Service

Scenario:

User:

```text
p1r4t3
```

Description:

```text
A merry life and a short one.
```

Steps:

View users:

```powershell
Get-LocalUser | Select-Object Name,Description
```

Find service with same DisplayName:

```powershell
Get-Service |
Where-Object DisplayName -eq "A merry life and a short one."
```

Result:

```text
p1r4t3-s-compass
```

Answer:

```text
p1r4t3-s-compass
```

---

## Task: Execute Get-Service on RoyalFortune

Command:

```powershell
Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }
```

Answer:

```powershell
Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }
```

---

# Problems Encountered and Solutions

---

## Problem 1

Command:

```powershell
Get-ChildItem | Where-Object Name,Length
```

Error:

```text
Cannot convert System.Object[] to System.String
```

Cause:

Where-Object is used for filtering, not selecting columns.

Solution:

```powershell
Get-ChildItem |
Select-Object Name,Length
```

---

## Problem 2

Typo in Property Name

Used:

```powershell
Length
```

Mistakenly typed:

```powershell
Lenght
```

Solution:

Use:

```powershell
Length
```

---

## Problem 3

Select-String Could Not Find File

Command:

```powershell
Select-String -Path .\captain-hat.txt -Pattern hat
```

Error:

```text
Cannot find path
```

Cause:

Current directory was:

```text
C:\Users\captain
```

while file was located in:

```text
C:\Users\captain\Documents\captain-cabin
```

Solution:

Navigate first:

```powershell
cd Documents
cd captain-cabin
```

Or use full path:

```powershell
Select-String -Path C:\Users\captain\Documents\captain-cabin\captain-hat.txt -Pattern hat
```

---

## Problem 4

Get-FileHash Output Appeared Incomplete

Observed:

```text
54D2EC3C12BF...[...]
```

Cause:

PowerShell truncated the table output.

Solution:

```powershell
Get-FileHash file.txt | Format-List
```

or

```powershell
(Get-FileHash file.txt).Hash
```

---

# Pentester Workflow Using PowerShell

Typical enumeration process:

```powershell
whoami
Get-ComputerInfo
Get-LocalUser
Get-NetIPConfiguration
Get-NetIPAddress
Get-Process
Get-Service
Get-NetTCPConnection
```

This reveals:

* User context
* Operating system
* Local users
* Network configuration
* Running processes
* Services
* Active connections

---

# Key Takeaways

PowerShell is significantly more powerful than CMD because it works with objects rather than text.

Most PowerShell workflows follow:

```text
Get
↓
Filter
↓
Sort
↓
Display
```

Example:

```powershell
Get-Process |
Where-Object CPU -gt 10 |
Sort-Object CPU |
Select-Object ProcessName,CPU
```

PowerShell is a fundamental skill for:

* System Administration
* Incident Response
* Threat Hunting
* Malware Analysis
* Active Directory Enumeration
* Penetration Testing
* Red Team Operations

Mastering PowerShell provides a strong foundation for advanced Windows and Active Directory security assessments.
