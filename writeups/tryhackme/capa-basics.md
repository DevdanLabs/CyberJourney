# CAPA: The Basics

## Executive Summary

Understanding what a suspicious executable is capable of doing is one of the first and most important steps in malware analysis. While manually reverse engineering a binary provides the most detailed insight, it requires significant expertise and can be extremely time-consuming. To bridge this gap, security analysts often rely on automated analysis tools such as **CAPA**.

**CAPA (Common Analysis Platform for Artifacts)**, developed by Mandiant, is a static malware analysis tool that automatically identifies the capabilities of executable files without executing them. Instead of determining which malware family a sample belongs to, CAPA focuses on identifying **what the program can do**, such as communicating over HTTP, creating files, launching processes, performing process injection, establishing persistence, or evading analysis.

In this room, I learned how CAPA analyzes binaries using rule-based detection, how to interpret its output through frameworks like **MITRE ATT&CK**, **MAEC**, and **Malware Behavior Catalogue (MBC)**, and how **Namespaces**, **Capabilities**, and **Rule YAML** work together to describe malware behavior. I also explored the **CAPA Web Explorer**, which provides an interactive way to investigate why a capability was detected by examining the evidence that matched each rule.

Rather than replacing reverse engineering, CAPA serves as a powerful triage tool that rapidly provides an overview of a program's potential behavior, enabling malware analysts, incident responders, and threat hunters to prioritize investigations more efficiently.

---

## Room Information

| Information | Details |
|------------|---------|
| **Room** | CAPA: The Basics |
| **Platform** | TryHackMe |
| **Category** | Malware Analysis |
| **Difficulty** | Easy |
| **Tool** | CAPA (Common Analysis Platform for Artifacts) |
| **Analysis Type** | Static Analysis |

---

# Learning Objectives

After completing this room, I was able to:

- Understand the purpose of CAPA in malware analysis.
- Differentiate between static and dynamic malware analysis.
- Perform basic malware capability analysis using CAPA.
- Interpret CAPA output and understand its structure.
- Understand MITRE ATT&CK, MAEC, and Malware Behavior Catalogue (MBC) mappings.
- Learn how CAPA organizes detections using Namespaces and Capabilities.
- Investigate Rule YAML files and understand how CAPA detects malware behaviors.
- Use CAPA Web Explorer to inspect evidence behind matched rules.

---

# Prerequisites

Before taking this room, it is recommended to understand:

- Basic malware analysis concepts
- Windows Portable Executable (PE) format
- Static vs Dynamic Analysis
- Basic understanding of the MITRE ATT&CK Framework
- Basic familiarity with PowerShell

---

# What is CAPA?

**CAPA (Common Analysis Platform for Artifacts)** is an open-source static malware analysis tool developed by **Mandiant (formerly FireEye)** that automatically identifies the capabilities of executable files.

Unlike antivirus software that attempts to determine whether a file is malicious, CAPA focuses on identifying **what the executable is capable of doing** by matching thousands of behavioral rules against the binary.

Instead of answering:

> *"What malware is this?"*

CAPA answers:

> *"What can this program do?"*

Examples of capabilities identified by CAPA include:

- HTTP communication
- File creation and deletion
- Registry manipulation
- Process creation
- Process injection
- PowerShell execution
- Persistence mechanisms
- Anti-VM detection
- Data encoding (Base64, XOR)
- Command and Control (C2) communication

This capability-focused approach allows analysts to quickly understand a suspicious binary without manually reverse engineering thousands of assembly instructions.

---

# Static Analysis vs Dynamic Analysis

Before understanding CAPA, it is important to distinguish between the two major approaches used in malware analysis.

## Static Analysis

Static analysis examines an executable **without running it**.

Instead of executing the malware, analysts inspect various parts of the binary, including:

- PE Headers
- Import Address Table (IAT)
- Strings
- Metadata
- Assembly Instructions
- Functions
- Resources

### Advantages

- Safe to perform
- No risk of infecting the analysis machine
- Fast initial triage
- Does not require an isolated sandbox

### Limitations

- Packed or obfuscated malware may hide functionality
- Runtime behavior cannot be observed
- Cannot reveal decrypted payloads loaded during execution

---

## Dynamic Analysis

Dynamic analysis involves **executing the malware** inside a controlled environment such as a virtual machine or sandbox while monitoring its behavior.

Analysts observe:

- File system activity
- Registry modifications
- Network traffic
- Process creation
- Memory allocation
- API calls

### Advantages

- Reveals actual runtime behavior
- Detects unpacked code
- Shows network communications
- Captures persistence mechanisms

### Limitations

- Requires an isolated environment
- Malware may detect sandboxes or virtual machines
- Higher operational risk if isolation fails

---

# Why CAPA Matters

Traditional malware analysis often requires manually reversing an executable using tools like:

- Ghidra
- IDA Pro
- Binary Ninja

This process may take hours or even days depending on the malware's complexity.

CAPA significantly reduces the initial analysis time by automatically applying thousands of expert-written behavioral rules against the executable.

Instead of manually identifying every API call and assembly instruction, CAPA immediately summarizes what the binary is capable of doing.

For incident responders, this means:

- Faster malware triage
- Better incident prioritization
- Earlier understanding of attacker behavior
- Improved threat hunting

---

# How CAPA Works

CAPA performs static analysis by examining the executable and matching its contents against thousands of predefined behavioral rules.

The overall workflow is illustrated below.

```text
Executable File
        │
        ▼
Read Binary Structure
        │
        ▼
Disassemble Functions
        │
        ▼
Analyze Instructions
        │
        ▼
Match Rule YAML Files
        │
        ▼
Generate Capabilities
        │
        ▼
Produce Human-Readable Report
```

Rather than relying on signatures or malware names, CAPA searches for behavioral patterns.

For example:

```text
OpenProcess()

+

WriteProcessMemory()

+

CreateRemoteThread()

↓

Process Injection Capability
```

Similarly,

```text
InternetOpen()

+

HttpOpenRequest()

+

InternetReadFile()

↓

HTTP Communication Capability
```

This rule-based approach enables CAPA to recognize behaviors commonly found across many malware families.

---

# CAPA Analysis Workflow

The typical malware triage workflow using CAPA looks like this:

```text
Suspicious Executable
        │
        ▼
Hash Verification
        │
        ▼
String Analysis
        │
        ▼
CAPA Static Analysis
        │
        ▼
Capability Assessment
        │
        ▼
Reverse Engineering (Ghidra / IDA)
        │
        ▼
Dynamic Analysis
        │
        ▼
Final Malware Report
```

CAPA is not intended to replace reverse engineering.

Instead, it provides analysts with a rapid overview of malware functionality before investing time in deeper analysis.

---

# Pentester Relevance

Although CAPA is primarily designed for malware analysts and DFIR professionals, understanding its functionality is also valuable for penetration testers.

## Red Team Perspective

A Red Team operator can use CAPA to:

- Analyze custom payloads before deployment.
- Understand techniques used by existing malware.
- Evaluate whether payloads expose recognizable behaviors.
- Learn common detection patterns used by defenders.

---

## Blue Team Perspective

Blue Team analysts can leverage CAPA to:

- Quickly triage suspicious executables.
- Map malware behavior to MITRE ATT&CK techniques.
- Support incident response investigations.
- Improve malware classification.
- Prioritize reverse engineering efforts.

---

## Detection Opportunities

Information provided by CAPA can be transformed into detection content such as:

- Sigma Rules
- YARA Rules
- EDR detection logic
- SIEM correlation rules
- Threat hunting hypotheses

---

## Key Takeaway

CAPA dramatically accelerates the early stages of malware analysis by transforming complex reverse engineering knowledge into automated behavioral detection. Rather than identifying malware families, it focuses on revealing **what an executable is capable of doing**, allowing analysts to rapidly understand potential threats, prioritize investigations, and build more effective detection and response strategies.

# Lab Overview

In this room, the CAPA tool is already installed on the provided Windows virtual machine, allowing learners to experiment with different command-line options without performing any installation. Since analyzing malware samples can take several minutes, TryHackMe also provides several pre-generated output files to ensure the learning process focuses on interpreting the results rather than waiting for the analysis to complete.

The following files are located in:

```text
C:\Users\Administrator\Desktop\capa
```

| File | Description |
|------|-------------|
| `cryptbot.bin` | Sample executable analyzed throughout the room |
| `cryptbot.txt` | Standard CAPA output |
| `cryptbot_vv.txt` | Very Verbose output |
| `cryptbot_vv.json` | JSON output used by CAPA Web Explorer |

---

# Running CAPA

Running CAPA is straightforward. The simplest command analyzes an executable using the default output format.

```powershell
capa.exe .\cryptbot.bin
```

### Command Breakdown

| Component | Description |
|----------|-------------|
| `capa.exe` | Launches the CAPA executable. |
| `.\cryptbot.bin` | Specifies the executable to analyze. `.\` refers to the current directory. |

Internally, CAPA performs several analysis stages before generating the report.

```text
Executable
      │
      ▼
Identify File Format
      │
      ▼
Disassemble Binary
      │
      ▼
Analyze Instructions
      │
      ▼
Apply Rule YAML Files
      │
      ▼
Generate Capability Report
```

Unlike sandbox-based analysis, CAPA **never executes the malware**. It only inspects the binary and compares it against thousands of predefined behavioral rules.

---

# CAPA Command-Line Options

CAPA provides several useful parameters that control how much information is displayed.

## Help Option (`-h`)

Displays all available command-line options.

### Syntax

```powershell
capa -h
```

### Purpose

Useful when learning the tool or checking supported parameters.

---

## Verbose Mode (`-v`)

Produces a more detailed report than the default output.

### Syntax

```powershell
capa.exe .\cryptbot.bin -v
```

### Purpose

Verbose mode provides additional information regarding detected capabilities, making it easier to understand why CAPA generated specific findings.

---

## Very Verbose Mode (`-vv`)

Produces the most detailed output available.

### Syntax

```powershell
capa.exe .\cryptbot.bin -vv
```

### Purpose

Very Verbose mode exposes:

- matched rules
- evidence
- matched features
- rule conditions
- additional metadata

Because of the amount of information generated, execution takes noticeably longer and may produce thousands of lines of output.

---

## JSON Output (`-j`)

Exports CAPA results in JSON format.

### Syntax

```powershell
capa.exe -j -vv .\cryptbot.bin > cryptbot_vv.json
```

### Purpose

Instead of displaying text in the terminal, CAPA stores structured results inside a JSON file that can later be opened using **CAPA Web Explorer** for interactive investigation.

---

# Understanding CAPA's Initial Output

After completing the analysis, CAPA first displays general information about the executable.

Example:

```text
md5
sha1
sha256
analysis
os
format
arch
path
```

Each field provides useful context before capability analysis begins.

---

## MD5

Example:

```text
3b9d26d2e7433749f2c32edb13a2b0a2
```

### Purpose

MD5 is a 128-bit cryptographic hash commonly used to uniquely identify files.

Although it is no longer considered secure for cryptographic purposes due to collision attacks, it remains widely used as an identifier within malware repositories.

Typical use cases include:

- VirusTotal searches
- MalwareBazaar lookups
- IOC documentation
- File integrity verification

---

## SHA-1

Example:

```text
969437df8f4ad08542ce8fc9831fc49a7765b7c5
```

SHA-1 produces a 160-bit hash.

While stronger than MD5, SHA-1 is also considered deprecated for cryptographic security. However, many malware databases still reference SHA-1 hashes for historical compatibility.

---

## SHA-256

Example:

```text
ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c
```

SHA-256 is currently the standard identifier used in malware analysis.

Security researchers commonly use SHA-256 to search malware samples in services such as:

- VirusTotal
- Hybrid Analysis
- MalwareBazaar
- ANY.RUN

---

## Analysis

Example:

```text
analysis
static
```

This field indicates that CAPA performed **Static Analysis**.

The executable was **not executed**.

Instead, CAPA inspected:

- PE headers
- assembly instructions
- imported APIs
- metadata
- embedded strings

---

## Operating System

Example:

```text
windows
```

Indicates that the detected capabilities are intended for the Windows operating system.

---

## File Format

Example:

```text
pe
```

PE stands for **Portable Executable**, the standard executable format used by Windows.

Examples include:

- `.exe`
- `.dll`
- `.sys`

CAPA also supports other executable formats, including ELF binaries, .NET assemblies, shellcode, and sandbox reports.

---

## Architecture

Example:

```text
i386
```

Indicates that the executable targets the **32-bit x86 architecture**.

Knowing the architecture is important when selecting reverse engineering tools, debuggers, and analysis environments.

---

## File Path

Example:

```text
/home/strategos/Room-CAPA/cryptbot.bin
```

Displays the location of the analyzed executable.

Although it does not influence malware behavior, documenting the analyzed file path improves reproducibility and investigation tracking.

---

# Why Does CAPA Take Time?

When CAPA starts, the terminal displays messages similar to:

```text
loading: 100%
analyzing program...
```

Loading the rule database is usually fast.

Most of the processing time is spent:

- disassembling functions
- analyzing assembly instructions
- evaluating thousands of Rule YAML files
- matching behavioral patterns

Larger executables typically require more processing time because they contain more code, functions, and possible behaviors.

---

# Using Pre-Generated Reports

To avoid waiting several minutes for every analysis, the room provides pre-generated reports.

The standard report can be viewed directly using PowerShell.

```powershell
Get-Content .\cryptbot.txt
```

This command displays the same information that would normally appear after running CAPA against the sample executable.

---

# Practical Notes

At this stage, CAPA has not yet identified whether the executable belongs to a specific malware family.

Instead, it has collected essential metadata and prepared the behavioral analysis that will be explored in the following sections through:

- MITRE ATT&CK mappings
- MAEC classification
- Malware Behavior Catalogue (MBC)
- Namespaces
- Capabilities

# Understanding CAPA Results

After the initial file information, CAPA begins presenting the behavioral analysis of the executable. Instead of simply listing detected APIs or imported functions, CAPA enriches its findings using several well-known cybersecurity frameworks.

These include:

- MITRE ATT&CK
- Malware Attribute Enumeration and Characterization (MAEC)
- Malware Behavior Catalogue (MBC)

Together, these frameworks help analysts understand **what the malware is trying to accomplish**, **how it behaves**, and **why those behaviors are important**.

---

# MITRE ATT&CK Mapping

One of CAPA's most valuable features is its ability to automatically map detected behaviors to the **MITRE ATT&CK Framework**.

The **MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge)** framework is a globally recognized knowledge base that documents techniques used by threat actors throughout the cyber kill chain.

Instead of simply stating:

> "This malware runs PowerShell."

CAPA provides additional context by mapping the behavior to its official ATT&CK technique.

Example:

```text
EXECUTION
Command and Scripting Interpreter::PowerShell [T1059.001]
```

This immediately tells the analyst:

- **Tactic:** Execution
- **Technique:** Command and Scripting Interpreter
- **Sub-technique:** PowerShell
- **Technique ID:** T1059.001

This standardized mapping makes malware reports easier to understand and allows security teams worldwide to communicate using the same terminology.

---

# Understanding ATT&CK Structure

CAPA displays ATT&CK results using two common formats.

## Technique

```text
ATT&CK Tactic
        ↓
ATT&CK Technique
        ↓
Technique ID
```

Example:

```text
Defense Evasion

↓

Obfuscated Files or Information

↓

T1027
```

---

## Sub-Technique

Some ATT&CK techniques contain additional detail.

```text
ATT&CK Tactic
        ↓
Technique
        ↓
Sub-Technique
        ↓
Technique ID
```

Example:

```text
Defense Evasion

↓

Obfuscated Files or Information

↓

Indicator Removal from Tools

↓

T1027.005
```

This additional level of detail helps analysts understand **how** a particular technique is implemented.

---

# Examples Found in Cryptbot

The sample analyzed throughout this room demonstrated several ATT&CK techniques.

Examples include:

| ATT&CK Tactic | Technique |
|--------------|-----------|
| Defense Evasion | Obfuscated Files or Information |
| Defense Evasion | Virtualization/Sandbox Evasion |
| Discovery | File and Directory Discovery |
| Execution | PowerShell |
| Persistence | Scheduled Tasks |
| Impact | Resource Hijacking |

These mappings immediately provide an overview of the malware's potential behavior before any manual reverse engineering begins.

---

# Why ATT&CK Mapping Matters

Mapping malware behavior to ATT&CK allows analysts to:

- understand attacker objectives
- accelerate incident response
- improve threat hunting
- create detection rules
- communicate findings consistently

Rather than describing malware using informal terminology, analysts can reference standardized ATT&CK techniques that are recognized throughout the cybersecurity community.

---

# Malware Attribute Enumeration and Characterization (MAEC)

CAPA also classifies malware using **MAEC (Malware Attribute Enumeration and Characterization)**.

Unlike ATT&CK, which focuses on attacker techniques, MAEC focuses on describing malware itself.

It acts as a standardized language for documenting:

- malware categories
- behaviors
- relationships
- characteristics

In the Cryptbot sample, CAPA identified:

```text
malware-category

↓

launcher
```

---

# Common MAEC Categories

## Launcher

A **Launcher** is malware whose primary purpose is to trigger additional malicious activity.

Typical behaviors include:

- launching secondary payloads
- establishing persistence
- communicating with command-and-control servers
- executing embedded components

Launchers are often used during the first stage of an infection chain.

---

## Downloader

Another common MAEC category is **Downloader**.

A downloader focuses on retrieving additional components from external sources.

Examples include:

- downloading payloads
- retrieving configuration files
- fetching updates
- executing downloaded malware

Unlike Launchers, Downloaders generally require network communication to perform their tasks.

---

# Malware Behavior Catalogue (MBC)

While ATT&CK describes attacker techniques, the **Malware Behavior Catalogue (MBC)** describes malware behavior itself.

MBC provides standardized terminology for documenting:

- malware objectives
- malware behaviors
- implementation methods
- low-level behaviors

It complements ATT&CK rather than replacing it.

---

# MBC Structure

CAPA presents MBC information using the following hierarchy.

```text
Objective
      ↓
Behavior
      ↓
Method (optional)
      ↓
Identifier
```

Example:

```text
ANTI-STATIC ANALYSIS

↓

Executable Code Obfuscation

↓

Argument Obfuscation

↓

B0032.020
```

Not every behavior contains a Method.

For example:

```text
COMMUNICATION

↓

HTTP Communication

↓

C0002
```

---

# MBC Objectives

Objectives describe the primary purpose behind a malware behavior.

Some common objectives include:

| Objective | Description |
|-----------|-------------|
| Anti-Behavioral Analysis | Attempts to evade sandboxes and behavioral analysis. |
| Anti-Static Analysis | Attempts to complicate reverse engineering. |
| Discovery | Collects information about the system. |
| Execution | Executes commands or code. |
| Persistence | Maintains long-term access. |
| Impact | Damages or abuses system resources. |
| Credential Access | Attempts to steal credentials. |
| Exfiltration | Transfers stolen data outside the system. |

These objectives closely resemble ATT&CK tactics but are tailored specifically for malware characterization.

---

# Micro-Objectives

Not every action performed by malware is inherently malicious.

For this reason, MBC introduces **Micro-Objectives**, which represent low-level operations commonly performed by software.

Examples include:

- PROCESS
- MEMORY
- DATA
- COMMUNICATION

Although these activities are normal in legitimate software, malware frequently abuses them.

For example:

```text
MEMORY

↓

Allocate Memory
```

Memory allocation itself is harmless.

However, malware may allocate executable memory before performing process injection or unpacking itself.

---

# Behaviors

Behaviors describe the specific actions performed by malware.

Examples detected in the Cryptbot sample include:

| Behavior | Description |
|----------|-------------|
| Lab Machine Detection | Detects virtual machines or sandboxes. |
| Executable Code Obfuscation | Makes static analysis more difficult. |
| HTTP Communication | Communicates over HTTP. |
| Encode Data | Encodes information using Base64 or XOR. |
| Read File | Reads files from disk. |
| Write File | Writes files to disk. |
| Delete File | Deletes files. |
| Create Process | Launches new processes. |

Together, these behaviors provide a much clearer picture of malware functionality.

---

# Methods

Some behaviors include additional implementation details called **Methods**.

Methods explain **how** a particular behavior is performed.

Examples include:

| Behavior | Method |
|----------|--------|
| Encode Data | Base64 |
| Encode Data | XOR |
| Executable Code Obfuscation | Stack Strings |
| Executable Code Obfuscation | Argument Obfuscation |
| HTTP Communication | Read Header |

Methods provide additional context without requiring manual reverse engineering.

---

# Practical Example

Suppose CAPA reports the following:

```text
DATA

↓

Encode Data::Base64

↓

C0026.001
```

This can be interpreted as:

- **Objective:** Data manipulation
- **Behavior:** Encode Data
- **Method:** Base64
- **Identifier:** C0026.001

From this single result, we can conclude that the executable is capable of encoding data using Base64, which may be used to conceal configuration data, URLs, commands, or payloads.

---

# Why MBC Is Valuable

MBC provides significantly more detail than a simple capability list.

Instead of merely stating that malware communicates over HTTP, MBC describes:

- the objective
- the behavior
- the implementation method
- the standardized identifier

This structured representation helps malware analysts produce more consistent reports while enabling defenders to better understand malware functionality.

---

# Practical Takeaway

The combination of **MITRE ATT&CK**, **MAEC**, and **MBC** transforms CAPA from a simple capability scanner into a comprehensive malware triage tool.

Rather than manually analyzing thousands of assembly instructions, analysts immediately gain insight into:

- attacker techniques
- malware categories
- behavioral objectives
- implementation methods
- potential impact

These frameworks provide the context necessary to prioritize investigations before moving on to deeper reverse engineering.

# Namespaces

After introducing MITRE ATT&CK, MAEC, and the Malware Behavior Catalogue (MBC), CAPA presents another important section called **Namespaces**.

Namespaces organize CAPA's thousands of detection rules into logical categories, making the output easier to understand and navigate.

Without namespaces, every matched rule would appear as an unorganized list.

Instead, CAPA groups related rules according to their purpose.

The hierarchy looks like this:

```text
Top-Level Namespace (TLN)
        │
        ▼
Namespace
        │
        ▼
Rule YAML
        │
        ▼
Capability
```

This organization helps analysts immediately understand **what area of malware behavior** a capability belongs to.

---

# Top-Level Namespace (TLN)

A **Top-Level Namespace (TLN)** is the highest category used to organize CAPA rules.

Each TLN groups together namespaces that share a similar purpose.

Some of the most commonly encountered TLNs include:

| Top-Level Namespace | Purpose |
|--------------------|---------|
| anti-analysis | Detects anti-debugging, anti-VM, packing, and obfuscation techniques. |
| communication | Detects network communication behaviors such as HTTP and DNS. |
| data-manipulation | Detects encoding, encryption, and data transformation. |
| executable | Detects executable-specific attributes such as PE sections. |
| host-interaction | Detects interactions with the operating system. |
| persistence | Detects techniques used to maintain long-term access. |
| impact | Detects behaviors capable of damaging or abusing systems. |
| linking | Detects runtime loading of libraries and APIs. |
| load-code | Detects code loading and execution techniques. |
| malware-family | Detects characteristics associated with known malware families. |
| runtime | Detects runtime environments and programming platforms. |

---

# Namespace Hierarchy

Each TLN contains one or more namespaces.

For example:

```text
anti-analysis
        │
        ├── anti-vm
        │       └── vm-detection
        │
        └── obfuscation
                └── string
                        └── stackstring
```

Similarly,

```text
host-interaction
        │
        ├── file-system
        │       ├── create
        │       ├── read
        │       ├── write
        │       └── delete
        │
        └── process
                ├── create
                └── inject
```

This hierarchical structure makes it much easier to locate related capabilities during an investigation.

---

# Understanding Namespace Examples

Below are several namespaces identified in the Cryptbot sample.

## Anti-Analysis

```text
anti-analysis/anti-vm/vm-detection
```

This namespace contains rules that detect malware attempting to identify whether it is running inside a virtual machine.

Typical indicators include:

- VMware registry keys
- VMware Tools
- VirtualBox artifacts
- Virtual machine drivers

Malware commonly performs these checks to evade automated analysis.

---

## Communication

```text
communication/http
```

This namespace contains rules related to HTTP communication.

Examples include:

- HTTP requests
- User-Agent strings
- HTTP response codes
- Command-and-Control communication

---

## Data Manipulation

```text
data-manipulation/encoding/base64
```

This namespace contains rules related to data encoding techniques.

Examples include:

- Base64
- XOR
- String encoding

Encoding is frequently used to hide configuration data or evade simple signature detection.

---

## Executable

```text
executable/pe/section/tls
```

This namespace focuses on executable-specific structures.

In the Cryptbot sample, CAPA detected the presence of a **Thread Local Storage (TLS)** section.

Although TLS is a legitimate Windows feature, malware sometimes abuses it to execute code before the program reaches its main entry point.

---

## Host Interaction

```text
host-interaction/file-system
```

This namespace contains rules describing interactions with the operating system.

Examples include:

- Create Directory
- Read File
- Write File
- Delete File

These capabilities indicate that malware can manipulate the victim's file system.

---

## Impact

```text
impact/cryptocurrency
```

This namespace focuses on behaviors capable of causing harm.

In the Cryptbot sample, CAPA detected cryptocurrency-related strings, suggesting possible cryptomining functionality or interaction with cryptocurrency-related resources.

---

## Linking

```text
linking/runtime-linking
```

This namespace detects programs that dynamically resolve or load functions during execution.

Runtime linking is commonly used by malware to avoid exposing imported APIs within the Import Address Table (IAT), making static analysis more difficult.

---

## Load Code

```text
load-code/pe
```

This namespace detects behaviors associated with loading or parsing executable code.

Examples include:

- Parsing PE headers
- Resolving exported functions
- Loading executable code
- Running PowerShell expressions

---

## Persistence

```text
persistence/scheduled-tasks
```

This namespace detects persistence mechanisms.

Examples include:

- Scheduled Tasks
- schtasks.exe
- at.exe

These techniques allow malware to survive system reboots.

---

# Capability

The **Capability** section is the most important part of CAPA's output.

A capability represents a behavior that CAPA successfully detected within the analyzed executable.

Rather than identifying malware families, CAPA identifies **what the executable is capable of doing**.

Examples include:

- Create Process
- HTTP Communication
- Process Injection
- Base64 Encoding
- File Deletion
- Scheduled Tasks
- Anti-VM Detection

Every capability displayed by CAPA is backed by one or more detection rules.

---

# Relationship Between Capabilities and Rules

One of the key concepts introduced in this room is that **every capability originates from a Rule YAML file**.

The relationship is straightforward:

```text
Rule YAML
        │
        ▼
Rule Match
        │
        ▼
Capability
```

For example,

Capability:

```text
reference anti-VM strings
```

Originates from:

```text
reference-anti-vm-strings.yml
```

Similarly,

Capability:

```text
schedule task via schtasks
```

Originates from:

```text
schedule-task-via-schtasks.yml
```

The capability name is essentially the rule filename with spaces replacing hyphens.

---

# Rule YAML Files

CAPA stores every detection rule inside a YAML file.

A typical rule contains:

- metadata
- namespace
- ATT&CK mapping
- MBC mapping
- references
- detection features

When an executable satisfies the conditions defined in the rule, CAPA reports the corresponding capability.

---

# Rule Matching Process

Internally, CAPA performs the following workflow:

```text
Executable
        │
        ▼
Analyze Binary
        │
        ▼
Evaluate Rule YAML Files
        │
        ▼
Rule Conditions Satisfied?
        │
   ┌────┴────┐
   │         │
  Yes        No
   │
   ▼
Generate Capability
```

This explains why CAPA results are deterministic rather than heuristic—every capability is supported by evidence from a matching rule.

---

# Example: Anti-VM Detection

Capability:

```text
reference anti-VM strings
```

Namespace:

```text
anti-analysis/anti-vm/vm-detection
```

Rule:

```text
reference-anti-vm-strings.yml
```

Interpretation:

CAPA detected evidence suggesting that the executable searches for VMware or VirtualBox artifacts.

Malware commonly performs these checks to determine whether it is running inside a virtual machine before executing its malicious payload.

---

# Example: Scheduled Tasks

Capability:

```text
schedule task via schtasks
```

Namespace:

```text
persistence/scheduled-tasks
```

Rule:

```text
schedule-task-via-schtasks.yml
```

Interpretation:

The executable appears capable of creating Windows Scheduled Tasks using **schtasks.exe**, allowing it to automatically execute after system startup.

This is a common persistence technique used by many malware families.

---

# Example: Base64 Encoding

Capability:

```text
reference Base64 string
```

Namespace:

```text
data-manipulation/encoding/base64
```

Rule:

```text
reference-base64-string.yml
```

Interpretation:

CAPA detected evidence that the executable uses Base64 encoding.

Although Base64 itself is not malicious, malware frequently uses it to:

- conceal configuration data
- encode commands
- hide URLs
- evade simple string-based detection

---

# Nursery Rules

One interesting exception discussed in this room is the **Nursery** namespace.

Normally, a capability belongs to its corresponding TLN.

However, some rules are still under development and are stored inside:

```text
nursery
```

This acts as a staging area for rules that have not yet reached production quality.

For example:

```text
reference cryptocurrency strings
```

Although logically related to the **Impact** namespace, its rule currently resides inside the Nursery folder.

---

# Why Namespaces and Capabilities Matter

Together, Namespaces and Capabilities transform CAPA from a simple rule engine into an effective malware triage platform.

Instead of manually reviewing thousands of assembly instructions, analysts immediately learn:

- what behaviors exist,
- where those behaviors belong,
- why they matter,
- and which detection rule identified them.

This structured organization greatly accelerates malware analysis while providing valuable context for reverse engineers, incident responders, and threat hunters.

# Investigating CAPA Results with Very Verbose Mode

The default CAPA output provides a concise summary of the detected capabilities. While this is sufficient for initial malware triage, analysts often need to understand **why** a capability was detected.

CAPA addresses this by providing **Very Verbose Mode (`-vv`)**, which exposes the evidence and rule conditions responsible for every detection.

Rather than simply showing:

```text
Capability

↓

schedule task via schtasks
```

Very Verbose Mode explains:

- which rule matched,
- what conditions were satisfied,
- what strings or APIs were identified,
- and why CAPA concluded that the executable possesses that capability.

This level of detail is extremely valuable during malware investigations.

---

# Very Verbose Mode (`-vv`)

CAPA supports two verbose options:

| Option | Description |
|---------|-------------|
| `-v` | Displays a more detailed report. |
| `-vv` | Displays the most detailed report, including rule evidence and matching conditions. |

Example:

```powershell
capa.exe -vv .\cryptbot.bin
```

Internally, CAPA performs the same analysis as before but retains significantly more information about every matched rule.

The resulting report may exceed several thousand lines depending on the complexity of the executable.

---

# Why Very Verbose Output Is Difficult to Read

The Cryptbot sample generates an output containing **more than 3,000 lines**.

Viewing this directly in PowerShell:

```powershell
Get-Content .\cryptbot_vv.txt
```

or opening the text file in a standard editor quickly becomes overwhelming.

Finding a single capability or understanding the evidence behind a rule match is both time-consuming and inefficient.

For this reason, CAPA provides another output format better suited for detailed investigations.

---

# Exporting Results as JSON

CAPA can export structured results using the `-j` parameter.

Example:

```powershell
capa.exe -j -vv .\cryptbot.bin > cryptbot_vv.json
```

### Command Breakdown

| Component | Description |
|----------|-------------|
| `-j` | Exports results as JSON. |
| `-vv` | Generates the most detailed analysis. |
| `>` | Redirects terminal output into a file. |
| `cryptbot_vv.json` | Stores structured analysis results. |

Unlike plain text, JSON preserves the complete relationship between:

- capabilities
- namespaces
- matched rules
- evidence
- metadata
- rule conditions

This structured format can then be analyzed using CAPA Web Explorer.

---

# CAPA Web Explorer

CAPA Web Explorer is a web-based interface designed specifically for exploring JSON reports generated by CAPA.

Instead of reading thousands of lines manually, analysts can browse the report interactively.

The workflow is simple:

```text
Executable
        │
        ▼
CAPA Analysis
        │
        ▼
JSON Output
        │
        ▼
CAPA Web Explorer
        │
        ▼
Interactive Investigation
```

The room provides both:

- an online version of CAPA Web Explorer
- an offline HTML version included inside the virtual machine

---

# Uploading a Report

To begin an investigation:

1. Open CAPA Web Explorer.
2. Select **Upload from Local**.
3. Choose:

```text
cryptbot_vv.json
```

After loading the report, the interface presents all detected capabilities in a searchable and expandable format.

Compared to reading a text file, the Web Explorer dramatically improves usability and investigation speed.

---

# Understanding Rule YAML Files

One of the most valuable features of CAPA Web Explorer is the ability to inspect the **Rule YAML** responsible for every capability.

Each rule contains two primary sections:

```yaml
rule:

meta:

features:
```

---

## Meta Section

The **meta** section describes the rule itself.

Typical fields include:

- Rule name
- Namespace
- Authors
- ATT&CK mapping
- MBC mapping
- References
- Example samples

Example:

```yaml
meta:

name:

namespace:

authors:

att&ck:

mbc:

references:

examples:
```

This metadata provides useful context about why the rule exists and where it originated.

---

## Features Section

The **features** section is the heart of every CAPA rule.

It defines **what conditions must be satisfied** before CAPA reports a capability.

Features may include:

- strings
- API calls
- instruction patterns
- imported functions
- logical operators
- nested conditions

When all required conditions are met, CAPA considers the rule matched.

---

# Example: VMware Detection Rule

Consider the capability:

```text
reference anti-VM strings targeting VMware
```

Its corresponding Rule YAML contains feature definitions similar to:

```yaml
string: /VMWare/i

string: /VMTools/i

string: /vmtoolsd.exe/i
```

CAPA searches the executable for these strings.

If one or more of them are found, the rule is considered satisfied.

This indicates that the executable may be attempting to detect whether it is running inside a VMware virtual machine.

Such behavior is commonly associated with malware attempting to evade sandbox analysis.

---

# Regular Expressions (Regex)

Notice that CAPA rules do not simply contain plain text.

Instead, they often use **Regular Expressions (Regex)**.

Example:

```yaml
/VMWare/i
```

Breaking this expression down:

| Component | Meaning |
|----------|---------|
| `/.../` | Defines a regular expression. |
| `VMWare` | Text pattern being searched. |
| `i` | Case-insensitive matching. |

Because of the `i` flag, all of the following satisfy the rule:

```text
VMWare

vmware

VMWARE

VmWaRe
```

Regular expressions make CAPA rules significantly more flexible than simple string matching.

---

# Logical Operators

Rule YAML files frequently combine multiple conditions using logical operators.

## OR

Example:

```yaml
or:

VMWare

VMTools

vmtoolsd.exe
```

Only **one** of these conditions must match for the rule to succeed.

---

## AND

Example:

```yaml
and:

schtasks

/create
```

Both conditions must be present before CAPA reports the capability.

This allows CAPA to reduce false positives while accurately identifying malware behavior.

---

# Example: Scheduled Task Detection

The capability:

```text
schedule task via schtasks
```

is produced by a rule containing logic similar to:

```yaml
match:

host-interaction/process/create

AND

string: /schtasks/i

AND

string: /\/create/i
```

CAPA therefore verifies:

- the executable creates a process,
- the string **schtasks** exists,
- and the **/create** argument is present.

Only after all required conditions are satisfied does CAPA conclude that the malware is capable of creating Scheduled Tasks.

---

# Rule Matching Workflow

The overall rule evaluation process can be summarized as follows:

```text
Rule YAML
        │
        ▼
Read Features
        │
        ▼
Evaluate Conditions
        │
        ▼
Evidence Found?
        │
   ┌────┴────┐
   │         │
  Yes        No
   │
   ▼
Capability Reported
```

This demonstrates that CAPA capabilities are based on concrete evidence rather than assumptions.

---

# Global Search

CAPA Web Explorer also provides a **Global Search** feature.

This search box allows analysts to instantly locate:

- capabilities
- rules
- strings
- namespaces
- ATT&CK techniques

For example, searching for:

```text
PowerShell
```

immediately displays every capability related to PowerShell execution.

Similarly, searching for:

```text
Base64
```

reveals all encoding-related capabilities.

This greatly simplifies investigations involving very large reports.

---

# Why CAPA Web Explorer Matters

Very Verbose reports contain an enormous amount of technical information.

Without CAPA Web Explorer, analysts would need to manually search through thousands of lines of text.

Using the interactive interface instead provides several advantages:

- Rapid capability filtering
- Interactive rule exploration
- Easy evidence inspection
- Faster malware triage
- Better understanding of rule logic

For malware analysts, this significantly reduces investigation time while providing greater confidence in CAPA's findings.

---

# Practical Takeaway

The combination of **Very Verbose Mode**, **JSON export**, and **CAPA Web Explorer** transforms CAPA from a simple command-line utility into a powerful malware investigation platform.

Instead of merely reporting detected capabilities, CAPA allows analysts to inspect the exact rules, evidence, and conditions responsible for every finding.

This transparency not only improves confidence in automated analysis but also provides valuable insight into how malware behaviors are recognized, making CAPA an excellent companion for reverse engineering, malware triage, and detection engineering.

# Lab Questions

## Task 1

**Question**

> What is the SHA256 hash of `cryptbot.bin`?

**Answer**

```text
ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c
```

---

**Question**

> What is the Technique Identifier of Obfuscated Files or Information?

**Answer**

```text
T1027
```

---

**Question**

> What is the Sub-Technique Identifier of Obfuscated Files or Information::Indicator Removal from Tools?

**Answer**

```text
T1027.005
```

---

**Question**

> When CAPA tags a file with this MAEC value, it indicates that it demonstrates behaviour similar to, but not limited to, Activating persistence mechanisms?

**Answer**

```text
launcher
```

---

**Question**

> When CAPA tags a file with this MAEC value, it indicates that the file demonstrates behaviour similar to, but not limited to, Fetching additional payloads or resources from the internet?

**Answer**

```text
Downloader
```

---

## Task 2

**Question**

> Which feature of this CAPA Web Explorer allows you to filter options or results?

**Answer**

```text
Global Search box
```

---

# Key Takeaways

Throughout this room, I learned that CAPA is far more than a simple malware scanning tool. It provides a structured and automated approach to static malware analysis by identifying **capabilities** rather than attempting to classify malware families.

Some of the most important concepts covered include:

- Understanding the difference between **Static Analysis** and **Dynamic Analysis**.
- Using CAPA to quickly identify malware capabilities without executing the sample.
- Interpreting CAPA output using **MITRE ATT&CK**, **MAEC**, and **Malware Behavior Catalogue (MBC)**.
- Understanding how **Namespaces**, **Capabilities**, and **Rule YAML** work together.
- Learning how CAPA rules rely on evidence such as API usage, strings, and regular expressions.
- Using **CAPA Web Explorer** to investigate why a capability was detected.
- Appreciating the importance of automated malware triage before performing manual reverse engineering.

---

# Skills Gained

After completing this room, I gained practical knowledge in:

- Static Malware Analysis
- Malware Capability Analysis
- CAPA Command-Line Usage
- Malware Triage
- Rule-Based Detection
- MITRE ATT&CK Mapping
- Malware Behavior Catalogue (MBC)
- MAEC Classification
- Rule YAML Interpretation
- Regular Expression (Regex) Matching
- Interactive Malware Investigation using CAPA Web Explorer

---

# Pentester Notes

Although CAPA is primarily intended for malware analysis, understanding its workflow is valuable from both offensive and defensive perspectives.

## Red Team Perspective

Red Team operators can use CAPA to:

- Analyze custom payloads before deployment.
- Understand how malware capabilities are identified.
- Evaluate whether payloads expose recognizable behaviors.
- Study common anti-analysis and persistence techniques.
- Learn how defenders inspect suspicious binaries.

---

## Blue Team Perspective

Blue Team analysts can leverage CAPA to:

- Perform rapid malware triage.
- Prioritize reverse engineering efforts.
- Map malware behavior to MITRE ATT&CK.
- Improve malware documentation.
- Accelerate incident response investigations.

---

## Detection Opportunities

Information provided by CAPA can be converted into practical detection content, including:

- Sigma Rules
- YARA Rules
- SIEM correlation rules
- EDR detection logic
- Threat Hunting queries

Examples of behaviors worth monitoring include:

- PowerShell execution
- Scheduled Task creation
- HTTP communications
- Process injection
- RWX memory allocation
- Base64 encoding
- Anti-VM checks
- Runtime API resolution

---

# Common Mistakes

Beginners often misunderstand several aspects of CAPA.

### CAPA identifies malware families.

❌ Incorrect.

CAPA identifies **capabilities**, not malware families.

---

### Every detected capability is malicious.

❌ Incorrect.

Many capabilities, such as Base64 encoding or HTTP communication, are legitimate system behaviors.

Context always matters.

---

### Static analysis completely replaces dynamic analysis.

❌ Incorrect.

Static analysis provides an excellent starting point, but dynamic analysis is still required to observe runtime behavior.

---

### CAPA executes malware.

❌ Incorrect.

CAPA performs **static analysis only**.

The executable is never run during analysis.

---

# Future Learning Path

This room serves as an excellent foundation for more advanced malware analysis topics.

Recommended next steps include:

## Malware Analysis

- Windows Internals
- Portable Executable (PE) Format
- Process Injection
- DLL Injection
- Windows API

---

## Reverse Engineering

- Ghidra
- IDA Pro
- Binary Ninja
- x64dbg

---

## Detection Engineering

- YARA Rules
- Sigma Rules
- Sysmon
- Microsoft Defender for Endpoint
- Elastic Security

---

## Digital Forensics & Incident Response (DFIR)

- Memory Forensics
- Volatility
- Procmon
- Process Explorer
- Wireshark

---

## Threat Hunting

- MITRE ATT&CK
- IOC Hunting
- Behavioral Detection
- Malware Triage
- Incident Response

---

# Conclusion

In this room, I learned how **CAPA (Common Analysis Platform for Artifacts)** simplifies malware analysis by automatically identifying the capabilities of executable files through static analysis. Instead of relying on manual reverse engineering alone, CAPA leverages thousands of expertly crafted Rule YAML files to detect common malware behaviors and present them in a structured, human-readable format.

I also learned how to interpret CAPA's output using industry-standard frameworks such as **MITRE ATT&CK**, **MAEC**, and the **Malware Behavior Catalogue (MBC)**, providing valuable context about malware objectives, behaviors, and techniques. Additionally, understanding **Namespaces**, **Capabilities**, and **Rule YAML** helped me appreciate how CAPA organizes its detections and why specific capabilities are reported.

Finally, I explored **CAPA Web Explorer**, which makes it significantly easier to investigate rule matches, inspect evidence, and understand the reasoning behind each detected capability. This transparency makes CAPA not only an excellent malware triage tool but also a valuable learning platform for aspiring malware analysts, DFIR practitioners, detection engineers, and threat hunters.

Overall, CAPA dramatically accelerates the early stages of malware analysis by transforming complex reverse engineering knowledge into automated behavioral analysis, enabling security professionals to investigate suspicious executables more efficiently and make informed decisions during incident response.

---

# References

- TryHackMe — **CAPA: The Basics**
- Mandiant CAPA Documentation
- CAPA GitHub Repository
- MITRE ATT&CK Framework
- Malware Behavior Catalogue (MBC)
- Malware Attribute Enumeration and Characterization (MAEC)

---

# Tags

```text
#TryHackMe
#CAPA
#MalwareAnalysis
#StaticAnalysis
#DFIR
#ThreatHunting
#DetectionEngineering
#ReverseEngineering
#MITREATTACK
#MBC
#MAEC
#PowerShell
#Windows
#PortableExecutable
#CyberSecurity
```