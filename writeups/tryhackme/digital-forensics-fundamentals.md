# Digital Forensics Fundamentals

> Learn the core principles of Digital Forensics, evidence acquisition, Windows forensics, metadata analysis, and basic forensic investigation techniques.

---

# Executive Summary

Digital Forensics is a specialized field of cybersecurity that focuses on identifying, collecting, preserving, analyzing, and presenting digital evidence after a cyber incident or criminal investigation. Whether investigating a ransomware attack, insider threat, malware infection, or cybercrime, digital forensics enables investigators to reconstruct events and determine what happened, when it happened, how it happened, and who was responsible.

In this TryHackMe room, I learned the fundamental concepts of digital forensics, including the four phases defined by the National Institute of Standards and Technology (NIST), different categories of digital forensics, evidence acquisition procedures, Windows forensic artifacts, and practical metadata analysis.

The room also introduced several industry-standard forensic tools such as **FTK Imager**, **Autopsy**, **DumpIt**, **Volatility**, **pdfinfo**, and **ExifTool**, demonstrating how investigators analyze documents and images to recover hidden metadata that may reveal valuable evidence.

Finally, I completed a practical investigation by analyzing a ransom letter and an attached image to identify the document author, the camera model used to capture the image, and the exact street where the photograph was taken using embedded GPS coordinates.

---

# Learning Objectives

After completing this room, I was able to:

- Understand the purpose of Digital Forensics.
- Explain the four phases of the NIST Digital Forensics methodology.
- Differentiate between multiple types of digital forensics.
- Understand proper evidence acquisition procedures.
- Explain the importance of Chain of Custody.
- Understand why Write Blockers are essential.
- Differentiate between Disk Images and Memory Images.
- Recognize common Windows forensic artifacts.
- Understand the purpose of FTK Imager, Autopsy, DumpIt, and Volatility.
- Analyze PDF metadata using `pdfinfo`.
- Analyze image EXIF metadata using `exiftool`.
- Perform a simple digital forensic investigation using metadata.

---

# Room Information

| Platform | TryHackMe |
|----------|-----------|
| Room | Digital Forensics Fundamentals |
| Difficulty | Easy |
| Category | Digital Forensics |
| Tools Used | pdfinfo, exiftool |
| Operating System | Linux (AttackBox) |
| Skills Learned | Evidence Acquisition, Metadata Analysis, Windows Forensics |

---

# Prerequisites

Before studying this room, it is helpful to understand:

- Basic operating system concepts
- Windows fundamentals
- Linux command line basics
- Files and directories
- Basic cybersecurity concepts

---

# What is Digital Forensics?

Digital Forensics is the branch of forensic science that focuses on identifying, preserving, collecting, examining, analyzing, and reporting digital evidence from electronic devices.

Unlike traditional forensics, which investigates physical evidence such as fingerprints or DNA, digital forensics investigates artifacts left behind on computers, smartphones, storage devices, networks, cloud infrastructure, and other digital systems.

Its primary objective is to reconstruct digital events while maintaining the integrity of the evidence so that the findings can be used during legal proceedings or incident response activities.

---

# Why Does Digital Forensics Exist?

Almost every action performed on a digital device leaves traces.

Examples include:

- Opening a document
- Browsing a website
- Downloading files
- Plugging in a USB drive
- Connecting to Wi-Fi
- Running malware
- Sending emails
- Installing software

These activities generate artifacts that investigators can analyze to answer questions such as:

- Who performed the activity?
- What happened?
- When did it happen?
- How did it happen?
- Which files were involved?
- What was affected?
- Can the evidence be presented in court?

---

# Digital Forensics Workflow

```text
Incident

      │
      ▼

+----------------+
|  Collection    |
+----------------+
        │
        ▼
+----------------+
| Examination    |
+----------------+
        │
        ▼
+----------------+
|   Analysis     |
+----------------+
        │
        ▼
+----------------+
|   Reporting    |
+----------------+
```

This standardized workflow is defined by the **National Institute of Standards and Technology (NIST)** and forms the foundation of most digital forensic investigations.

---

# The Four Phases of Digital Forensics

## 1. Collection

The first phase focuses on identifying and securely collecting digital evidence.

Typical evidence includes:

- Desktop computers
- Laptops
- Smartphones
- USB flash drives
- External hard drives
- Memory cards
- Network devices

The investigator must ensure that no evidence is modified during collection.

---

## 2. Examination

After collection, investigators often face massive amounts of data.

The examination phase filters the evidence to identify data relevant to the investigation.

Examples include filtering files by:

- User
- Date
- File extension
- Keyword
- Folder
- File type

This significantly reduces the amount of data requiring detailed analysis.

---

## 3. Analysis

During analysis, investigators correlate evidence from multiple sources to reconstruct the sequence of events.

Typical activities include:

- Building timelines
- Recovering deleted files
- Identifying suspicious activity
- Correlating logs
- Determining attacker actions

The goal is to understand exactly what happened.

---

## 4. Reporting

The final phase documents every step of the investigation.

A forensic report generally includes:

- Executive Summary
- Investigation methodology
- Evidence collected
- Findings
- Timeline
- Conclusions
- Recommendations

The report must be technically accurate while remaining understandable to both technical and non-technical audiences.

---

# Types of Digital Forensics

Digital forensics consists of several specialized fields.

| Type | Purpose |
|-------|----------|
| Computer Forensics | Investigates desktops, laptops, storage devices, and operating systems. |
| Mobile Forensics | Investigates smartphones, tablets, call logs, messages, GPS data, and mobile applications. |
| Network Forensics | Analyzes network traffic, packet captures, firewall logs, and IDS/IPS logs. |
| Database Forensics | Investigates databases for unauthorized access, data modification, or data theft. |
| Cloud Forensics | Examines evidence stored in cloud environments and cloud infrastructure. |
| Email Forensics | Investigates phishing emails, spoofing attempts, malicious attachments, and email headers. |

---

# Red Team Perspective

Understanding digital forensics helps penetration testers recognize what artifacts are left behind after an attack.

Examples include:

- Browser history
- Event logs
- Registry entries
- Network connections
- USB activity
- Process execution
- PowerShell logs

Knowledge of these artifacts helps security professionals better understand attacker visibility and detection opportunities.

---

# Blue Team Perspective

Digital forensics is one of the most important skills for Incident Response (IR) and Security Operations Center (SOC) teams.

It enables defenders to:

- Investigate security incidents
- Determine attack timelines
- Recover deleted evidence
- Identify attacker techniques
- Support legal investigations
- Improve future defenses

---

# Key Concepts Learned

- Digital Forensics investigates digital evidence after cyber incidents.
- The NIST methodology consists of Collection, Examination, Analysis, and Reporting.
- Every digital action leaves artifacts that investigators can analyze.
- Different investigation scenarios require different forensic specializations.
- Proper forensic procedures ensure evidence remains admissible and trustworthy.

---

# Skills Gained (Part 1)

- Understanding Digital Forensics fundamentals
- Understanding the NIST forensic methodology
- Identifying different forensic disciplines
- Understanding the purpose of forensic investigations
- Connecting digital evidence to cybersecurity incident response

---

# Evidence Acquisition

Evidence acquisition is one of the most critical stages of any digital forensic investigation. If digital evidence is collected improperly, it may become unreliable or even inadmissible in court.

Unlike traditional file copying, forensic investigators must preserve every bit of information exactly as it exists on the original storage device. Any modification—even an automatic timestamp update performed by the operating system—can compromise the integrity of the evidence.

The objective of evidence acquisition is to create an exact copy of the original evidence while ensuring the original device remains untouched.

---

# Why Evidence Acquisition Matters

Digital evidence can easily be altered.

Examples include:

- Connecting a hard drive to another computer
- Opening a document
- Booting a suspect's operating system
- Mounting a storage device in read-write mode

Even background operating system processes may modify:

- File timestamps
- Metadata
- Log files
- Temporary files
- File system journals

For this reason, forensic investigators follow strict procedures to preserve the originality of digital evidence.

---

# Principles of Digital Evidence

Digital evidence should always be:

```text
Authentic
        │
        ▼
Reliable
        │
        ▼
Untampered
        │
        ▼
Repeatable
```

This ensures another investigator can perform the same examination and obtain identical results.

---

# Proper Authorization

Before collecting any digital evidence, investigators must receive authorization from the appropriate authority.

Depending on the investigation, authorization may come from:

- Search warrants
- Court orders
- Internal company approval
- Incident response authorization
- Consent from the device owner

Without proper authorization, evidence may not be legally admissible.

---

## Why Authorization Matters

Imagine an investigator copies data from an employee's laptop without company approval.

Even if the laptop contains proof of insider threats, the evidence may become legally questionable because proper procedures were not followed.

Digital investigations must always operate within legal and organizational boundaries.

---

# Chain of Custody

One of the most important concepts in digital forensics is the **Chain of Custody**.

A Chain of Custody is a formal document that records every individual who handles digital evidence from collection until the investigation is complete.

Its primary purpose is to prove that the evidence has remained unchanged throughout the investigation.

---

## Typical Chain of Custody Information

A Chain of Custody document generally records:

- Evidence ID
- Evidence description
- Device type
- Serial number
- Collection date and time
- Collection location
- Investigator name
- Storage location
- Access history
- Transfer history

---

## Example Chain of Custody

| Evidence ID | Device | Collected By | Date | Storage Location |
|-------------|--------|--------------|------|------------------|
| E001 | Dell Laptop | Investigator A | 29 Jun 2026 | Evidence Locker A |
| E002 | USB Flash Drive | Investigator B | 29 Jun 2026 | Evidence Locker B |
| E003 | Mobile Phone | Investigator C | 29 Jun 2026 | Evidence Locker C |

Every transfer of evidence must be documented.

---

## Why Chain of Custody Is Important

Without documentation, investigators cannot prove:

- Who collected the evidence
- Who accessed it
- Whether it was modified
- Where it was stored

This may result in evidence being challenged during legal proceedings.

---

# Write Blockers

A **Write Blocker** is a hardware or software mechanism that allows investigators to read data from a storage device while preventing any write operations.

Instead of accessing the suspect's drive directly, investigators connect the drive through a write blocker.

```text
Suspect HDD
      │
      ▼
Write Blocker
      │
      ▼
Forensic Workstation
```

The workstation can read every file but cannot modify anything on the original drive.

---

## Why Write Blockers Are Necessary

Modern operating systems automatically perform background operations whenever a storage device is connected.

Examples include:

- Updating timestamps
- Creating temporary files
- Generating thumbnails
- Updating file system metadata
- Indexing files

These automatic changes may unintentionally alter evidence.

A write blocker prevents all write operations, preserving the original state of the storage device.

---

# Hardware vs Software Write Blockers

| Hardware Write Blocker | Software Write Blocker |
|------------------------|------------------------|
| Physical device | Software configuration |
| Highest level of protection | Depends on operating system |
| Widely accepted in legal investigations | Useful in laboratory environments |
| Blocks all write operations | Mounts storage as read-only |

Hardware write blockers are generally preferred during official forensic investigations.

---

# Forensic Imaging

Instead of analyzing the original storage device, investigators create a **forensic image**.

A forensic image is a bit-by-bit copy of an entire storage device.

Unlike copying individual files, forensic imaging copies:

- Existing files
- Deleted files
- File system metadata
- Free space
- Partition information
- Hidden data

Everything on the storage medium is duplicated.

---

## Forensic Imaging Workflow

```text
Original Storage Device
            │
            ▼
      Write Blocker
            │
            ▼
   Forensic Imaging Tool
            │
            ▼
      Forensic Image
            │
            ▼
 Investigation & Analysis
```

The original evidence is preserved while investigators work on the forensic image.

---

# Disk Images

A **Disk Image** contains every bit stored on a hard disk or solid-state drive.

Disk images contain **non-volatile** data, meaning the data remains available even after the computer is powered off.

Typical evidence found inside a disk image includes:

- Documents
- Pictures
- Videos
- Browser history
- Registry files
- Event logs
- Installed applications
- Deleted files
- Recycle Bin
- User profiles

---

# Memory Images

A **Memory Image** captures the contents of a computer's RAM.

Unlike disk storage, RAM is **volatile**, meaning its contents disappear when power is removed.

Memory images may contain:

- Running processes
- Active malware
- Network connections
- Open files
- Encryption keys
- User sessions
- Clipboard contents

Because volatile data disappears after shutdown, memory acquisition should always occur before powering off the system whenever possible.

---

# Disk Image vs Memory Image

| Disk Image | Memory Image |
|------------|--------------|
| Captures HDD/SSD | Captures RAM |
| Non-volatile | Volatile |
| Survives shutdown | Lost after shutdown |
| Stores permanent files | Stores live system activity |
| Used for file analysis | Used for live process analysis |

---

# Evidence Acquisition Workflow

```text
Incident Detected
        │
        ▼
Obtain Authorization
        │
        ▼
Document Chain of Custody
        │
        ▼
Connect Write Blocker
        │
        ▼
Acquire Memory Image (if system is live)
        │
        ▼
Acquire Disk Image
        │
        ▼
Verify Image Integrity
        │
        ▼
Begin Investigation
```

---

# Red Team Perspective

Understanding evidence acquisition helps penetration testers recognize what investigators may recover after an engagement.

Potential evidence includes:

- Malware binaries
- Browser artifacts
- PowerShell history
- USB activity
- Command execution history
- Downloaded tools
- Registry modifications

Knowing what artifacts remain on a compromised system helps security professionals better understand attacker visibility and forensic footprints.

---

# Blue Team Perspective

For Incident Response and DFIR teams, evidence acquisition is the foundation of every investigation.

Following proper acquisition procedures allows defenders to:

- Preserve evidence integrity
- Maintain legal admissibility
- Perform accurate forensic analysis
- Reproduce investigation results
- Build reliable incident timelines

---

# Key Takeaways

- Digital evidence must never be modified during acquisition.
- Proper authorization is required before collecting evidence.
- Chain of Custody documents every individual who handles evidence.
- Write Blockers prevent accidental modification of storage devices.
- Investigators analyze forensic images instead of original evidence.
- Memory images should be collected before shutting down a live system.
- Disk images preserve non-volatile data, while memory images capture volatile data.

---

# Skills Gained (Part 2)

- Understanding forensic evidence acquisition
- Understanding legal considerations during investigations
- Maintaining Chain of Custody
- Understanding Write Blockers
- Differentiating Disk Images and Memory Images
- Understanding forensic imaging workflows
- Recognizing best practices for preserving digital evidence

---

# Windows Forensics

Windows is the most widely used desktop operating system in both personal and enterprise environments. Because of its popularity, Windows systems frequently become sources of digital evidence during cybercrime investigations, incident response activities, and malware analysis.

A forensic investigation on Windows systems focuses on collecting and analyzing artifacts that reveal user activities, executed programs, network connections, deleted files, system events, and much more.

Unlike ordinary troubleshooting, forensic investigations must preserve the integrity of the evidence while reconstructing events as accurately as possible.

---

# Windows Forensic Images

Before investigators begin analyzing a Windows system, they first acquire forensic images.

There are two primary types of forensic images:

```text
Windows Computer
        │
        ├──────────────┐
        │              │
        ▼              ▼
 Disk Image      Memory Image
```

Each image captures different types of evidence.

---

# Disk Image

A **Disk Image** is a bit-by-bit copy of an entire storage device, including every sector on the disk.

Unlike copying files using Windows Explorer, forensic imaging duplicates everything stored on the drive.

This includes:

- Existing files
- Deleted files
- Hidden files
- File system metadata
- Free space
- Partition information
- System files

Because storage devices retain data after shutdown, disk images contain **non-volatile evidence**.

---

## Common Evidence Found in Disk Images

Investigators can recover various artifacts from disk images, including:

- User documents
- Downloads
- Browser history
- Installed applications
- Windows Registry
- Event Logs
- USB device history
- Recycle Bin contents
- Deleted files
- Email databases

These artifacts help investigators reconstruct user activity over time.

---

# Memory Image

A **Memory Image** is a snapshot of the computer's RAM (Random Access Memory).

Unlike storage devices, RAM only stores temporary information while the operating system is running.

Memory acquisition captures the current state of the computer before it is powered off.

---

## Common Evidence Found in Memory Images

Memory analysis can reveal information that may never be written to disk.

Examples include:

- Running processes
- Active malware
- Network connections
- Logged-in users
- Encryption keys
- Open files
- Clipboard contents
- Loaded DLLs
- Command history

Since RAM is volatile, investigators should always prioritize memory acquisition when investigating a live system.

---

# Why Memory Acquisition Comes First

Once a computer is shut down, all information stored in RAM disappears.

```text
Running System
       │
       ▼
Acquire Memory
       │
       ▼
Shutdown System
       │
       ▼
Acquire Disk
```

If investigators shut down the system before capturing memory, valuable evidence may be permanently lost.

---

# Disk Image vs Memory Image

| Disk Image | Memory Image |
|------------|--------------|
| HDD / SSD | RAM |
| Non-volatile | Volatile |
| Data survives shutdown | Data disappears after shutdown |
| Permanent files | Live system activity |
| Registry | Running processes |
| Browser history | Active network connections |
| Deleted files | Encryption keys |

Both image types complement each other and provide a complete picture of the investigated system.

---

# Windows Forensic Tools

Several specialized forensic tools are used throughout the acquisition and analysis process.

Each tool serves a different purpose.

---

# FTK Imager

## Overview

**FTK Imager** is one of the most widely used forensic acquisition tools in the industry.

It allows investigators to create forensic disk images while preserving evidence integrity.

The tool also provides basic forensic analysis capabilities, allowing investigators to browse the contents of acquired images without modifying the original evidence.

---

## Primary Features

- Disk image acquisition
- Evidence preview
- File export
- Metadata viewing
- Hash verification
- Support for multiple forensic image formats

---

## Typical Workflow

```text
Suspect Drive
      │
      ▼
Write Blocker
      │
      ▼
FTK Imager
      │
      ▼
Forensic Image
```

---

## Industry Usage

FTK Imager is commonly used during the **Collection** and **Examination** phases of digital forensics.

---

# Autopsy

## Overview

**Autopsy** is a free and open-source digital forensic platform used for investigating disk images.

Rather than creating forensic images, Autopsy focuses on analyzing the evidence after acquisition.

It provides investigators with a graphical interface for exploring large datasets efficiently.

---

## Main Features

- Keyword searching
- Deleted file recovery
- File metadata analysis
- Browser history analysis
- Timeline analysis
- Hash analysis
- File carving
- Extension mismatch detection

---

## Example Investigation

```text
Disk Image
      │
      ▼
Autopsy
      │
      ▼
Keyword Search
Deleted Files
Browser History
Timeline
```

Autopsy automatically indexes evidence, making investigations much faster than manually browsing files.

---

# Extension Mismatch Detection

Attackers sometimes disguise malicious files by changing their extensions.

Example:

```text
invoice.pdf.exe
```

or

```text
holiday.jpg
```

where the file is actually an executable.

Autopsy compares the actual file signature with the file extension and alerts investigators when they do not match.

---

# DumpIt

## Overview

**DumpIt** is a lightweight command-line utility used to capture the contents of system memory.

Unlike FTK Imager, which focuses on storage devices, DumpIt acquires RAM images.

---

## Purpose

DumpIt is primarily used during live incident response investigations.

```text
Running Windows System
          │
          ▼
       DumpIt
          │
          ▼
Memory Image
```

The generated memory image can later be analyzed using dedicated memory forensic tools.

---

# Volatility

## Overview

**Volatility** is one of the most popular open-source memory forensic frameworks.

Instead of collecting memory, Volatility analyzes memory images produced by tools such as DumpIt.

It supports multiple operating systems, including:

- Windows
- Linux
- macOS
- Android

---

## Plugin-Based Architecture

Volatility uses plugins to analyze different artifacts inside memory images.

Common plugins include:

| Plugin | Purpose |
|---------|----------|
| pslist | Lists running processes |
| pstree | Displays parent-child process relationships |
| netscan | Shows active network connections |
| dlllist | Lists loaded DLL files |
| filescan | Searches for file objects |
| cmdline | Displays process command-line arguments |

Each plugin extracts specific forensic artifacts from RAM.

---

# Typical Memory Analysis Workflow

```text
Running Computer
        │
        ▼
DumpIt
        │
        ▼
Memory Image
        │
        ▼
Volatility
        │
        ▼
Processes
Connections
DLLs
Commands
Malware
```

---

# Investigation Workflow

A typical Windows forensic investigation follows a structured process.

```text
Security Incident
        │
        ▼
Acquire Memory Image
        │
        ▼
Acquire Disk Image
        │
        ▼
Analyze Disk (Autopsy)
        │
        ▼
Analyze Memory (Volatility)
        │
        ▼
Correlate Evidence
        │
        ▼
Generate Report
```

This workflow helps investigators preserve evidence while reconstructing attacker activities.

---

# Comparison of Windows Forensic Tools

| Tool | Primary Purpose | Evidence Type | Category |
|------|-----------------|---------------|----------|
| FTK Imager | Disk acquisition and preview | Disk Image | Collection & Examination |
| Autopsy | Disk analysis | Disk Image | Examination & Analysis |
| DumpIt | Memory acquisition | Memory Image | Collection |
| Volatility | Memory analysis | Memory Image | Analysis |

---

# Red Team Perspective

Understanding Windows forensics allows penetration testers to recognize which artifacts remain after executing attacks.

Examples include:

- Process execution history
- Browser artifacts
- Registry modifications
- PowerShell logs
- USB device history
- Network connections
- Persistence mechanisms

Awareness of forensic artifacts helps security professionals better understand detection opportunities and improve operational security during authorized engagements.

---

# Blue Team Perspective

Windows forensics plays a major role in Incident Response and Digital Forensics & Incident Response (DFIR).

Security teams use these forensic artifacts to:

- Investigate malware infections
- Identify persistence mechanisms
- Detect lateral movement
- Recover deleted evidence
- Build attack timelines
- Determine attacker objectives

These findings help organizations improve detection capabilities and strengthen future defenses.

---

# Key Takeaways

- Windows systems are one of the most common sources of digital evidence.
- Disk images preserve non-volatile evidence such as files, logs, and registry data.
- Memory images capture volatile evidence such as running processes and active network connections.
- Memory should be acquired before shutting down a live system.
- FTK Imager is commonly used for disk acquisition.
- Autopsy performs comprehensive disk image analysis.
- DumpIt captures RAM images.
- Volatility analyzes memory images using specialized plugins.

---

# Skills Gained (Part 3)

- Understanding Windows forensic investigations
- Differentiating disk and memory evidence
- Understanding live memory acquisition
- Recognizing common Windows forensic artifacts
- Understanding the purpose of FTK Imager
- Understanding the capabilities of Autopsy
- Understanding DumpIt memory acquisition
- Understanding Volatility memory analysis
- Connecting forensic tools to the NIST investigation workflow

---

# Practical Investigation: Metadata Analysis

One of the fundamental principles of digital forensics is that **every digital action leaves traces**.

Whenever we create a document, edit a file, take a photograph, or browse the internet, our devices automatically generate additional information known as **metadata**.

During forensic investigations, metadata often provides valuable evidence without requiring investigators to examine the file's actual contents.

In this practical exercise, the objective was to investigate a ransom letter and an attached image to recover hidden metadata that could help identify the kidnapper.

---

# Investigation Scenario

In this scenario, our cat **Gado** has been kidnapped.

The kidnapper sent a ransom letter in Microsoft Word format. For convenience, the document was also provided as a PDF along with the extracted image.

Our goal was to analyze these files and identify useful forensic evidence hidden within their metadata.

---

# Understanding Metadata

Metadata is commonly described as **"data about data."**

Instead of storing the actual contents of a file, metadata describes characteristics of that file.

For example:

```text
File Name: ransom-letter.pdf

Metadata
──────────────
Author
Creator
Creation Date
Modification Date
Software Used
Page Count
File Size
```

Metadata is automatically generated by operating systems, applications, cameras, smartphones, and many other devices.

---

# Why Metadata Matters

Metadata can reveal information that is not immediately visible to the user.

Examples include:

- Document author
- Software used to create the file
- Creation timestamp
- Last modification time
- Camera model
- GPS coordinates
- Device manufacturer
- Image resolution

Although these details may seem insignificant, they often become valuable evidence during investigations.

---

# PDF Metadata

Modern document editors such as Microsoft Word embed metadata inside documents.

Even after exporting a document as a PDF, much of this metadata is often preserved.

Examples include:

- Author
- Creator
- Producer
- Creation Date
- Modification Date
- Page Count
- PDF Version

This allows investigators to determine when the document was created and which software generated it.

---

# Tool: pdfinfo

The **pdfinfo** utility displays metadata stored inside PDF files.

It provides investigators with a quick overview of important document properties without opening the document in a PDF reader.

---

## Purpose

Display metadata from PDF documents.

---

## Syntax

```bash
pdfinfo <PDF_FILE>
```

---

## Example

```bash
pdfinfo ransom-letter.pdf
```

---

## Example Output

```text
Title:          Ransom Letter
Author:         Ann Gree Shepherd
Creator:        Microsoft Word for Office 365
Producer:       Microsoft Word for Office 365
CreationDate:   Wed Oct 10 21:47:53
Pages:          1
Encrypted:      No
```

---

## Output Interpretation

| Field | Description |
|--------|-------------|
| Title | Document title |
| Author | Person who created the document |
| Creator | Software used to create the document |
| Producer | Software that generated the PDF |
| CreationDate | Original creation timestamp |
| Pages | Number of pages |
| Encrypted | Indicates whether the document is password protected |

---

# Why PDF Metadata Is Useful

Document metadata may reveal:

- The identity of the document creator
- The application used
- The document timeline
- Signs of editing or modification

Even if the visible content contains very little information, metadata may provide valuable investigative leads.

---

# EXIF Metadata

Unlike PDF documents, digital photographs store metadata using the **Exchangeable Image File Format (EXIF)** standard.

Whenever a photo is taken using a smartphone or digital camera, the device automatically records technical information about the image.

---

# Typical EXIF Information

Depending on the device, EXIF metadata may include:

- Camera manufacturer
- Camera model
- Lens information
- Date and time
- Exposure settings
- ISO
- Focal length
- Image dimensions
- GPS coordinates

Not every image contains all of these fields, but many modern smartphones automatically record GPS information if location services are enabled.

---

# Why GPS Metadata Is Important

GPS coordinates embedded inside an image can reveal the exact location where the photograph was taken.

For investigators, this information may help determine:

- Crime scene location
- Suspect location
- Victim location
- Travel routes
- Timeline of events

Sometimes a single photograph can reveal more information than its visible contents.

---

# Tool: ExifTool

**ExifTool** is one of the most widely used forensic utilities for reading and writing metadata from various file formats.

It supports hundreds of file types, including:

- JPEG
- PNG
- TIFF
- PDF
- DOCX
- MP4
- MP3

For digital forensics, ExifTool is primarily used to extract metadata from images.

---

## Purpose

Read metadata embedded inside digital files.

---

## Syntax

```bash
exiftool <IMAGE_FILE>
```

---

## Example

```bash
exiftool ransom-image.jpg
```

---

## Example Output

```text
Camera Model Name : Canon EOS R6
Create Date       : 2026:06:29 15:10:24
GPS Position      : 51 deg 31' 4.00" N, 0 deg 5' 48.30" W
```

---

## Output Interpretation

| Field | Description |
|--------|-------------|
| Camera Model Name | Device used to capture the image |
| Create Date | Date and time the photo was taken |
| GPS Position | Geographic location where the photo was captured |

---

# GPS Coordinate Investigation

The extracted GPS coordinates can be searched using online mapping services such as:

- Google Maps
- Bing Maps
- OpenStreetMap

Example:

```text
51°31'4.00"N 0°5'48.30"W
```

After entering the coordinates into a mapping service, investigators can determine the exact location where the photograph was taken.

This technique is frequently used in both criminal investigations and OSINT (Open Source Intelligence).

---

# Practical Investigation Workflow

```text
Received Evidence
        │
        ▼
Analyze PDF Metadata
(pdfinfo)
        │
        ▼
Identify Document Author
        │
        ▼
Analyze Image Metadata
(exiftool)
        │
        ▼
Recover GPS Coordinates
        │
        ▼
Search Coordinates
Using Online Maps
        │
        ▼
Identify Investigation Location
```

---

# Security Implications

Metadata can unintentionally expose sensitive information.

Examples include:

- Employee names
- Company names
- Internal usernames
- Device models
- GPS locations
- Software versions
- Creation timestamps

Organizations should review and remove unnecessary metadata before publicly sharing documents or images.

---

# Red Team Perspective

Metadata is an excellent source of **Open Source Intelligence (OSINT)**.

During reconnaissance, attackers may collect publicly available documents and images to discover:

- Employee names
- Internal software versions
- Organizational structure
- Geographic locations
- Device information

This information can later be used to perform phishing attacks, social engineering, or targeted reconnaissance.

---

# Blue Team Perspective

Security teams should regularly sanitize documents before publication.

Recommended practices include:

- Removing document metadata
- Stripping EXIF information from images
- Disabling unnecessary GPS tagging
- Reviewing publicly shared documents
- Using metadata removal tools before publishing files

Reducing metadata exposure decreases the amount of information available to attackers.

---

# Key Takeaways

- Metadata provides valuable evidence without modifying file contents.
- PDF metadata can reveal authors, software, and document timelines.
- EXIF metadata contains detailed information about digital photographs.
- GPS coordinates embedded in images can identify the exact capture location.
- `pdfinfo` is used to analyze PDF metadata.
- `ExifTool` is used to analyze image metadata.
- Metadata analysis plays an important role in digital forensic investigations, OSINT, and incident response.

---

# Skills Gained (Part 4)

- Understanding metadata analysis
- Using `pdfinfo` to inspect PDF metadata
- Using `exiftool` to inspect EXIF metadata
- Understanding GPS evidence in digital investigations
- Performing basic metadata-based forensic analysis
- Recognizing metadata as both a forensic asset and a potential information disclosure risk

---

# Practical Challenge Walkthrough

The final challenge of this room focused on applying basic digital forensic techniques to investigate evidence provided by a fictional kidnapper.

Instead of exploiting a system or recovering deleted files, the investigation relied entirely on **metadata analysis**.

The provided evidence consisted of:

- A PDF ransom letter
- An extracted image from the original Microsoft Word document

The objective was to recover hidden information embedded within these files and answer several forensic questions.

---

# Investigation Objectives

The investigation aimed to identify:

1. The author of the ransom letter.
2. The street where the attached photograph was taken.
3. The model of the camera used to capture the image.

These answers were obtained by examining metadata rather than the visible contents of the files.

---

# Challenge Workflow

```text
Evidence Received
        │
        ▼
Analyze PDF Metadata
(pdfinfo)
        │
        ▼
Recover Document Author
        │
        ▼
Analyze Image Metadata
(exiftool)
        │
        ▼
Recover GPS Coordinates
        │
        ▼
Search Coordinates
on Google Maps
        │
        ▼
Identify Street Name
        │
        ▼
Recover Camera Model
```

---

# Task 1 — Identify the Document Author

## Objective

Determine who created the ransom letter.

---

## Tool Used

- pdfinfo

---

## Command

```bash
pdfinfo ransom-letter.pdf
```

---

## Purpose

Display metadata embedded inside the PDF document.

---

## Syntax

```bash
pdfinfo <PDF_FILE>
```

---

## Flag Breakdown

This command does not require additional flags.

- `pdfinfo` → Reads PDF metadata.
- `ransom-letter.pdf` → Target document.

---

## Relevant Output

```text
Author: Ann Gree Shepherd
```

---

## Answer

```text
Ann Gree Shepherd
```

---

## Explanation

The PDF metadata contained the **Author** field, which identifies the user account or individual who originally created the Microsoft Word document before it was exported as a PDF.

This demonstrates why metadata is often more valuable than the visible document contents.

---

# Task 2 — Identify the Photo Location

## Objective

Determine where the attached image was taken.

---

## Tool Used

- ExifTool

---

## Command

```bash
exiftool ransom-image.jpg
```

---

## Purpose

Extract EXIF metadata from the image.

---

## Syntax

```bash
exiftool <IMAGE_FILE>
```

---

## Relevant Output

```text
GPS Position :
51°31'4.00"N
0°5'48.30"W
```

---

## Investigation

The GPS coordinates were copied into an online mapping service such as Google Maps.

The location resolved to:

```text
Milk Street
```

---

## Answer

```text
Milk Street
```

---

## Explanation

Many smartphones and digital cameras automatically record GPS coordinates whenever location services are enabled.

These coordinates can reveal the exact location where a photograph was captured, making them extremely valuable during investigations.

---

# Task 3 — Identify the Camera Model

## Objective

Determine which camera was used to capture the attached image.

---

## Tool Used

- ExifTool

---

## Command

```bash
exiftool ransom-image.jpg
```

---

## Relevant Output

```text
Camera Model Name :
Canon EOS R6
```

---

## Answer

```text
Canon EOS R6
```

---

## Explanation

EXIF metadata stores information about the camera or smartphone used to capture the photograph.

Investigators can use this information to:

- Identify the recording device.
- Correlate evidence across multiple photographs.
- Link images captured by the same camera.
- Support ownership investigations.

---

# Commands Used

| Command | Purpose |
|----------|---------|
| `pdfinfo ransom-letter.pdf` | Display PDF metadata |
| `exiftool ransom-image.jpg` | Display EXIF metadata from the image |

---

# Investigation Results

| Investigation Question | Answer |
|-------------------------|--------|
| Document Author | **Ann Gree Shepherd** |
| Photo Location | **Milk Street** |
| Camera Model | **Canon EOS R6** |

---

# Evidence Summary

| Evidence | Information Recovered |
|----------|-----------------------|
| PDF Metadata | Author information |
| Image EXIF Metadata | GPS coordinates |
| Image EXIF Metadata | Camera model |

---

# Lessons Learned

This practical investigation demonstrates that valuable forensic evidence is often hidden within metadata rather than the visible contents of a file.

Throughout the exercise, I learned that:

- Documents preserve metadata even after being converted to PDF.
- Images often contain extensive EXIF metadata.
- GPS coordinates can reveal precise physical locations.
- Camera information can identify the device used to capture an image.
- Metadata analysis is a fast and effective investigation technique.

---

# Troubleshooting

## Problem

`pdfinfo: command not found`

### Cause

The Poppler utilities package is not installed.

### Solution

```bash
sudo apt install poppler-utils
```

---

## Problem

`exiftool: command not found`

### Cause

ExifTool is not installed.

### Solution

```bash
sudo apt install libimage-exiftool-perl
```

---

## Problem

GPS coordinates do not appear.

### Possible Causes

- GPS tagging was disabled.
- Metadata was removed before sharing.
- The image was edited using software that strips EXIF data.

---

# Pentester Notes

Although this room focuses on digital forensics, the same metadata can also be valuable during reconnaissance.

Examples include:

- Employee names inside document metadata.
- Microsoft Office versions.
- Camera or smartphone models.
- Geographic locations.
- Device manufacturers.
- Internal usernames.

Attackers frequently collect public documents and images to gather information before launching phishing or social engineering campaigns.

---

# DFIR Notes

For Digital Forensics and Incident Response (DFIR) teams, metadata analysis is one of the quickest methods for extracting investigative leads.

Common use cases include:

- Identifying document authors.
- Building investigation timelines.
- Recovering GPS evidence.
- Determining software used to create files.
- Correlating evidence from multiple devices.

Metadata analysis is often performed early in an investigation because it is fast, non-destructive, and may immediately reveal critical evidence.

---

# Skills Demonstrated

- PDF metadata analysis
- EXIF metadata analysis
- GPS coordinate investigation
- Evidence interpretation
- Basic forensic investigation workflow
- Command-line forensic utilities
- Metadata-based evidence collection

---

# Conclusion

The **Digital Forensics Fundamentals** room provided a solid introduction to one of the most important disciplines in cybersecurity. Unlike offensive security, where the focus is on exploiting systems, digital forensics focuses on understanding what happened after an incident by preserving, collecting, examining, analyzing, and reporting digital evidence.

Throughout this room, I learned how investigators follow the **NIST Digital Forensics Methodology**, why evidence integrity is critical, and how proper evidence acquisition ensures that forensic findings remain legally admissible.

I also gained hands-on experience using forensic tools such as **pdfinfo** and **ExifTool** to analyze document and image metadata, demonstrating how seemingly harmless metadata can reveal valuable investigative information such as document authors, camera models, and GPS coordinates.

Finally, I explored the basics of Windows forensics, forensic imaging, memory acquisition, and industry-standard forensic tools including FTK Imager, Autopsy, DumpIt, and Volatility.

This room serves as an excellent foundation for future learning in Digital Forensics and Incident Response (DFIR).

---

# Key Takeaways

- Every digital action leaves traces.
- Digital forensics follows a structured methodology defined by NIST.
- Evidence integrity is essential throughout the investigation.
- Chain of Custody ensures accountability and legal admissibility.
- Write Blockers prevent accidental modification of evidence.
- Memory acquisition should be prioritized because RAM is volatile.
- Disk images preserve long-term evidence, while memory images capture live system activity.
- Metadata can reveal valuable information without inspecting file contents.
- PDF and image metadata are valuable sources of forensic evidence.
- Windows forensic investigations rely on specialized forensic acquisition and analysis tools.

---

# Skills Gained

After completing this room, I gained experience in:

## Digital Forensics

- Understanding forensic investigation principles
- Applying the NIST Digital Forensics methodology
- Understanding evidence acquisition procedures

## Evidence Handling

- Proper Authorization
- Chain of Custody
- Write Blockers
- Forensic Imaging

## Windows Forensics

- Disk Image concepts
- Memory Image concepts
- Volatile vs Non-Volatile evidence
- Windows forensic workflow

## Metadata Analysis

- PDF metadata analysis
- EXIF metadata analysis
- GPS coordinate investigation
- Camera metadata analysis

## Forensic Tools

- pdfinfo
- ExifTool
- FTK Imager
- Autopsy
- DumpIt
- Volatility

---

# Real-World Applications

The knowledge gained from this room can be applied in various cybersecurity roles.

Examples include:

- Digital Forensics Investigator
- DFIR Analyst
- SOC Analyst
- Incident Response Analyst
- Malware Analyst
- Threat Hunter
- Security Consultant
- Law Enforcement Cybercrime Investigator

Digital forensics is also an important component of ransomware investigations, insider threat investigations, corporate incident response, and legal evidence collection.

---

# Common Interview Questions

## What is Digital Forensics?

Digital Forensics is the process of identifying, preserving, collecting, examining, analyzing, and reporting digital evidence to investigate cyber incidents or criminal activity.

---

## What are the four phases of the NIST Digital Forensics process?

- Collection
- Examination
- Analysis
- Reporting

---

## What is the purpose of a Chain of Custody?

A Chain of Custody documents every individual who handles evidence, ensuring its integrity and legal admissibility throughout the investigation.

---

## Why are Write Blockers important?

Write Blockers prevent investigators or operating systems from modifying the original storage device during evidence acquisition.

---

## What is the difference between a Disk Image and a Memory Image?

A Disk Image captures non-volatile data stored on HDDs or SSDs, while a Memory Image captures volatile data stored in RAM, such as running processes and active network connections.

---

## Why should memory be acquired before shutting down a system?

RAM is volatile and loses its contents when power is removed. Acquiring memory first preserves valuable evidence such as active malware, encryption keys, and running processes.

---

## What is EXIF metadata?

EXIF (Exchangeable Image File Format) metadata stores information about digital photographs, including camera details, timestamps, and GPS coordinates.

---

## What tools were introduced in this room?

- FTK Imager
- Autopsy
- DumpIt
- Volatility
- pdfinfo
- ExifTool

---

# Future Learning Path

This room provides a strong foundation for more advanced Digital Forensics and DFIR topics.

Recommended next topics include:

- Windows Event Logs
- Windows Registry Forensics
- Browser Forensics
- Memory Forensics with Volatility
- Disk Forensics with Autopsy
- File System Forensics (NTFS)
- Malware Analysis
- Incident Response
- Log Analysis
- Threat Hunting

---

# References

## Official Documentation

- National Institute of Standards and Technology (NIST)
- FTK Imager Documentation
- Autopsy Documentation
- Volatility Foundation
- ExifTool Documentation
- Poppler Utilities Documentation

## Learning Platform

- TryHackMe — Digital Forensics Fundamentals

---

# Tags

```text
#TryHackMe
#DigitalForensics
#DFIR
#IncidentResponse
#WindowsForensics
#MemoryForensics
#DiskForensics
#Metadata
#EXIF
#PDFMetadata
#ChainOfCustody
#EvidenceAcquisition
#CyberSecurity
#BlueTeam
#SOC
#Autopsy
#FTKImager
#Volatility
#ExifTool
#CyberJourney
```

---

# Personal Reflection

This room introduced me to the defensive side of cybersecurity by demonstrating how investigators analyze digital evidence after an incident occurs. Unlike penetration testing, which focuses on identifying and exploiting vulnerabilities, digital forensics emphasizes preserving evidence, reconstructing events, and understanding attacker activity without altering the original data.

One of the most interesting parts of the room was learning that metadata alone can reveal significant information about documents and images. I also developed a better understanding of why forensic procedures such as Chain of Custody, Write Blockers, and forensic imaging are critical for maintaining evidence integrity.

Overall, this room strengthened my understanding of Digital Forensics and provided an excellent starting point for exploring more advanced DFIR topics in the future.

---