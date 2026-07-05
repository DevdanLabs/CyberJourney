# Introduction to SIEM

## Executive Summary

Security Information and Event Management (SIEM) is one of the most essential technologies used in modern Security Operations Centers (SOCs). Organizations generate millions of security events every day from endpoints, servers, firewalls, routers, web applications, and cloud services. Without a centralized solution, analyzing these scattered logs becomes extremely difficult and often results in delayed incident detection.

In this room, I learned the fundamental concepts behind SIEM, including how logs are collected from multiple sources, normalized into a common format, correlated to identify suspicious behavior, and analyzed using detection rules. The room also introduced the complete alert lifecycle—from log generation and rule matching to alert investigation and response.

Additionally, the practical lab demonstrated how a SOC analyst investigates a triggered alert by reviewing the related events, understanding the detection rule, identifying the affected user and host, and determining whether the alert represents a true positive or a false positive.

---

# Learning Objectives

After completing this room, I was able to:

- Understand the purpose of a Security Information and Event Management (SIEM) platform.
- Differentiate between host-centric and network-centric log sources.
- Explain why centralized log management is critical in modern enterprise environments.
- Understand how SIEM collects, parses, normalizes, and correlates security logs.
- Describe how detection rules generate security alerts.
- Understand the lifecycle of alert investigation within a Security Operations Center (SOC).
- Perform a basic SIEM investigation by identifying suspicious processes, affected users, and compromised hosts.
- Distinguish between true positive and false positive alerts.

---

# Room Information

| Information | Details |
|------------|---------|
| **Room** | Introduction to SIEM |
| **Platform** | TryHackMe |
| **Difficulty** | Easy |
| **Category** | Security Operations Center (SOC) |
| **Focus Areas** | SIEM Fundamentals, Log Sources, Log Ingestion, Detection Rules, Alert Investigation |

---

# Prerequisites

Before taking this room, it is recommended to understand the following topics:

- Basic Networking
- Windows Fundamentals
- Linux Fundamentals
- Log Fundamentals
- SOC Fundamentals
- Incident Response Fundamentals
- Digital Forensics Fundamentals

These topics provide the foundation needed to understand how security events are generated, collected, and analyzed by a SIEM platform.

---

# Why SIEM Matters

Modern enterprise environments contain hundreds or even thousands of devices.

Examples include:

- Windows Workstations
- Linux Servers
- Active Directory Domain Controllers
- Firewalls
- Routers
- VPN Gateways
- Web Servers
- Database Servers
- Cloud Services

Every one of these devices continuously generates security logs.

Without a SIEM, security analysts would need to manually access each device individually to review logs, making investigations slow, inefficient, and prone to human error.

A SIEM solves this problem by acting as a centralized platform that collects, normalizes, correlates, and analyzes logs from across the environment.

The overall workflow can be summarized as follows:

```text
                Log Sources
                     │
                     ▼
           Log Collection
                     │
                     ▼
          Parsing & Normalization
                     │
                     ▼
            Event Correlation
                     │
                     ▼
           Detection Rules Match
                     │
                     ▼
             Alert Generation
                     │
                     ▼
        SOC Analyst Investigation
                     │
                     ▼
      True Positive or False Positive
                     │
                     ▼
            Incident Response
```

---

# Skills Gained

By completing this room, I strengthened my understanding of:

- Security Information and Event Management (SIEM)
- Centralized Log Collection
- Log Parsing
- Log Normalization
- Event Correlation
- Detection Rules
- Alert Generation
- Alert Investigation
- Host-Centric Log Sources
- Network-Centric Log Sources
- SOC Investigation Workflow
- True Positive vs False Positive Analysis

---

# Real-World Relevance

SIEM platforms are among the most important technologies used by modern Security Operations Centers.

Common enterprise SIEM solutions include:

- Splunk Enterprise Security
- Microsoft Sentinel
- IBM QRadar
- Elastic Security
- Google Security Operations (Chronicle)
- Wazuh

Understanding SIEM fundamentals is an essential skill for roles such as:

- SOC Analyst (Tier 1)
- SOC Analyst (Tier 2)
- Incident Responder
- Threat Hunter
- Detection Engineer
- Security Engineer
- Blue Team Operator

This room serves as the foundation for learning enterprise SIEM platforms, writing detection rules, performing threat hunting, and conducting incident investigations.

# Core Concepts

## What is SIEM?

**Security Information and Event Management (SIEM)** is a centralized security platform that collects, stores, normalizes, correlates, and analyzes logs generated by devices, applications, and network infrastructure.

Its primary purpose is to help security analysts detect malicious activities quickly by transforming millions of individual log events into meaningful security alerts.

Instead of manually checking logs on every device, analysts can investigate security incidents from a single centralized platform.

The basic SIEM workflow is illustrated below:

```text
                Multiple Log Sources
                         │
                         ▼
                 Log Collection
                         │
                         ▼
                Parsing & Normalization
                         │
                         ▼
                 Event Correlation
                         │
                         ▼
                 Detection Rules
                         │
                         ▼
                   Security Alerts
                         │
                         ▼
              SOC Analyst Investigation
                         │
                         ▼
                Incident Response
```

---

# Why Do Organizations Need SIEM?

Every device connected to a corporate network continuously generates logs.

Examples include:

- Windows Workstations
- Linux Servers
- Web Servers
- Database Servers
- Firewalls
- Routers
- VPN Gateways
- Active Directory
- Cloud Platforms

Even a medium-sized company can generate millions of log events every day.

Without centralized monitoring, security analysts would face several major challenges.

---

## Challenge 1 — Numerous Log Sources

Every device stores its own logs.

```text
Windows
    │
    ├── Security Logs

Linux
    │
    ├── Authentication Logs

Firewall
    │
    ├── Connection Logs

Web Server
    │
    ├── HTTP Access Logs

Database
    │
    ├── Query Logs
```

Investigating every system individually would be extremely time-consuming.

---

## Challenge 2 — No Centralization

Without SIEM, analysts often need to:

- Connect to Windows using RDP
- Connect to Linux using SSH
- Log in to Firewall management interfaces
- Review Web Server logs manually
- Check Database logs separately

During an active security incident, this process wastes valuable time.

---

## Challenge 3 — Limited Context

Individual logs rarely tell the complete story.

For example:

```text
VPN Login
```

looks normal.

```text
PowerShell Execution
```

also looks normal.

```text
Large File Access
```

still appears normal.

However, when correlated together:

```text
VPN Login
      │
      ▼
PowerShell Execution
      │
      ▼
Sensitive File Access
      │
      ▼
Outbound Connection
```

the activity strongly suggests potential data exfiltration.

This ability to connect related events is known as **Event Correlation**.

---

## Challenge 4 — Manual Analysis

Large organizations generate thousands of events every second.

```text
Thousands of Devices
          │
          ▼
 Millions of Logs
          │
          ▼
Human Analysis?
      Impossible
```

SIEM automates this analysis using detection rules.

---

## Challenge 5 — Different Log Formats

Every vendor stores logs differently.

Example:

Windows

```text
EventID = 4624
```

Linux

```text
Accepted password for root
```

Apache

```text
GET /index.html HTTP/1.1
```

Firewall

```text
Allowed Connection
```

A SIEM converts all these different formats into a standardized structure through **Normalization**.

---

# Types of Log Sources

Log sources generally fall into two major categories.

---

## Host-Centric Log Sources

Host-centric logs record activities occurring inside a system.

Examples include:

- Windows Workstations
- Linux Servers
- Domain Controllers
- Application Servers

Typical events include:

- User Authentication
- File Access
- Process Execution
- Registry Modification
- PowerShell Execution
- Scheduled Tasks
- Service Creation

Example:

```text
User Login
      │
      ▼
Windows Event Log
```

---

## Network-Centric Log Sources

Network-centric logs record communication between systems.

Common sources include:

- Firewalls
- Routers
- IDS/IPS
- VPN Appliances
- Proxy Servers

Typical events include:

- SSH Connections
- VPN Sessions
- HTTP Requests
- FTP Transfers
- SMB File Sharing
- DNS Queries

Example:

```text
Client
    │
HTTP Request
    │
Web Server
```

---

# SIEM Core Features

Modern SIEM platforms perform several important tasks.

---

## 1. Centralized Log Collection

The first responsibility of a SIEM is collecting logs from multiple devices.

```text
Windows
Linux
Firewall
Router
Database
Web Server
      │
      ▼
      SIEM
```

This allows analysts to investigate incidents from one location instead of accessing every system individually.

---

## 2. Parsing

Raw logs are difficult to analyze.

Parsing extracts useful information into individual fields.

Example:

Raw log

```text
192.168.1.15 - - [05/Jul/2026] "GET /login" 200
```

Parsed log

```text
Source IP
192.168.1.15

HTTP Method
GET

URI
/login

Status Code
200
```

Each field can now be searched independently.

---

## 3. Normalization

Different vendors use different field names.

For example:

```text
Windows

SourceAddress
```

```text
Firewall

Src-IP
```

```text
Linux

RemoteHost
```

Normalization converts them into one common field.

```text
source_ip
```

This makes searching and writing detection rules significantly easier.

---

## 4. Event Correlation

One of the most powerful SIEM capabilities is correlating events across multiple log sources.

Example:

```text
VPN Login
      │
      ▼
File Access
      │
      ▼
PowerShell Execution
      │
      ▼
Outbound Internet Connection
```

Individually, each event appears harmless.

Together, they may indicate credential compromise followed by data exfiltration.

---

## 5. Detection Rules

SIEM platforms continuously compare incoming events against predefined rules.

Example:

```text
IF

Failed Login Attempts >= 5

within 10 seconds

THEN

Generate Alert
```

Another example:

```text
IF

EventID = 4688

AND

ProcessName contains "whoami"

THEN

Generate Alert
```

Detection rules automate the identification of suspicious behavior.

---

## 6. Alert Generation

Whenever a detection rule matches incoming events, SIEM generates an alert.

```text
Detection Rule
        │
Matched Conditions
        │
        ▼
Generate Alert
```

The alert is then assigned to a SOC analyst for investigation.

---

## 7. Dashboards

SIEM dashboards summarize security events visually.

Common dashboard widgets include:

- Top Event IDs
- Failed Login Attempts
- Source IP Addresses
- Destination IP Addresses
- Most Active Users
- Running Processes
- MITRE ATT&CK Techniques
- Alert Severity Distribution
- Event Volume Over Time

Dashboards allow analysts to quickly identify unusual behavior without reading thousands of raw log entries.

---

# Detection Rules Explained

Detection rules consist of logical conditions.

General structure:

```text
IF

Condition A

AND

Condition B

AND

Condition C

THEN

Generate Alert
```

Example from this room:

```text
IF

EventID = 4688

AND

ProcessName contains

"miner"

OR

"crypt"

THEN

Potential CryptoMiner Activity
```

The SIEM continuously evaluates these rules against incoming logs.

---

# Alert Investigation Workflow

Once an alert is generated, the SOC analyst follows a structured investigation process.

```text
Alert Triggered
        │
        ▼
Review Detection Rule
        │
        ▼
Locate Related Events
        │
        ▼
Review Timeline
        │
        ▼
Identify User
        │
        ▼
Identify Host
        │
        ▼
Analyze Process
        │
        ▼
Determine
True Positive
or
False Positive
        │
        ▼
Take Appropriate Action
```

---

# True Positive vs False Positive

One of the most important responsibilities of a SOC analyst is determining whether an alert represents a real security incident.

### True Positive

The alert correctly identifies malicious activity.

Example:

```text
Process:

cudominer.exe

Location:

Temp Folder

Detection Rule:

CryptoMiner
```

This represents an actual threat.

---

### False Positive

The alert is triggered even though no malicious activity occurred.

Example:

A legitimate administrative script accidentally matches a detection rule.

The analyst should tune the rule to reduce future false alerts while preserving detection accuracy.

---

# SIEM in the Security Operations Center

SIEM serves as the central investigation platform used by SOC analysts.

```text
Log Sources
      │
      ▼
SIEM Platform
      │
      ▼
Detection Rules
      │
      ▼
Security Alerts
      │
      ▼
SOC Analyst
      │
      ▼
Incident Response
```

Without SIEM, analysts would struggle to detect attacks across large enterprise environments.

With SIEM, millions of events can be collected, correlated, prioritized, and investigated efficiently, enabling faster detection and response to security incidents.

# Technologies, Log Sources, and SIEM Workflow

## Key Technologies Covered

Throughout this room, several important security technologies were introduced. Together, these technologies form the foundation of modern Security Operations Centers (SOC).

| Technology | Purpose |
|------------|---------|
| **SIEM** | Centralized platform for collecting, correlating, analyzing, and monitoring security logs. |
| **Windows Event Viewer** | Native Windows application used to view Windows Event Logs. |
| **Syslog** | Standard logging protocol commonly used by Linux systems and network devices. |
| **Splunk** | Enterprise SIEM platform used for log collection, searching, dashboards, and alerting. |
| **Detection Rules** | Logical conditions used to detect suspicious activities. |
| **Dashboards** | Visual representation of security events, alerts, and statistics. |

---

# Important Terminology

| Term | Description |
|------|-------------|
| **SIEM** | Security Information and Event Management platform. |
| **Log** | A record of an event generated by a device or application. |
| **Log Source** | Any system or device that generates logs. |
| **Host-Centric Logs** | Logs generated by endpoints or servers. |
| **Network-Centric Logs** | Logs generated by network devices. |
| **Parsing** | Extracting structured fields from raw log data. |
| **Normalization** | Converting logs from different formats into a standardized structure. |
| **Correlation** | Connecting related events across multiple log sources. |
| **Detection Rule** | Logical condition used to identify suspicious behavior. |
| **Alert** | Notification generated when a rule is triggered. |
| **Dashboard** | Visual interface displaying summarized security information. |
| **Log Ingestion** | Process of collecting logs into a SIEM platform. |
| **Event ID** | Numeric identifier representing a specific Windows event type. |
| **True Positive** | Alert correctly identifying malicious activity. |
| **False Positive** | Alert triggered by legitimate activity. |

---

# Common Log Sources

A SIEM can ingest logs from many different systems.

## Windows Logs

Generated by Windows Event Viewer.

Common categories include:

- Application Logs
- Security Logs
- System Logs
- Setup Logs
- Forwarded Events

Common security-related Event IDs:

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4672 | Special Privileges Assigned |
| 4688 | Process Creation |
| 4720 | User Account Created |
| 4726 | User Account Deleted |
| 104 | Event Log Cleared |

---

## Linux Logs

Most Linux logs are stored inside:

```text
/var/log/
```

Common log files:

| File | Purpose |
|------|---------|
| `/var/log/auth.log` | Authentication events |
| `/var/log/secure` | Authentication logs (RHEL-based systems) |
| `/var/log/httpd` | Apache Web Server logs |
| `/var/log/apache2` | Apache logs (Debian-based systems) |
| `/var/log/cron` | Scheduled task logs |
| `/var/log/kern.log` | Kernel events |
| `/var/log/syslog` | General system events |

---

## Web Server Logs

Web servers generate logs for every incoming request.

Typical information includes:

- Client IP Address
- HTTP Method
- Requested URI
- Status Code
- User-Agent
- Timestamp

Example:

```text
192.168.21.200 - - [21/Mar/2022:10:17:10]
"GET /cgi-bin/try/ HTTP/1.0"
200
Mozilla/5.0
```

These logs are essential for detecting:

- SQL Injection
- Directory Traversal
- Command Injection
- Brute Force Attacks
- Web Shell Uploads

---

## Network Device Logs

Network infrastructure also produces valuable security logs.

Examples include:

- Firewalls
- Routers
- Switches
- IDS/IPS
- VPN Appliances
- Proxy Servers

Common events:

- Allowed Connections
- Blocked Connections
- VPN Logins
- SSH Sessions
- DNS Queries
- SMB Connections

---

# Log Ingestion Methods

Before SIEM can analyze logs, it must first receive them.

This process is known as **Log Ingestion**.

General workflow:

```text
Endpoint
     │
Generate Logs
     │
     ▼
Log Ingestion
     │
     ▼
SIEM Platform
```

Several ingestion methods were introduced in this room.

---

## 1. Agent / Forwarder

An agent (or forwarder) is a lightweight application installed on endpoints.

Its responsibility is to continuously collect logs and forward them to the SIEM.

Workflow:

```text
Windows
     │
Universal Forwarder
     │
     ▼
Splunk Server
```

Advantages:

- Real-time log delivery
- Reliable
- Lightweight
- Easy to manage

Examples:

- Splunk Universal Forwarder
- Wazuh Agent

---

## 2. Syslog

Syslog is the industry-standard logging protocol for Linux systems and network devices.

Workflow:

```text
Firewall
     │
Syslog
     │
     ▼
SIEM
```

Common transport ports:

| Protocol | Port |
|----------|------|
| UDP | 514 |
| TCP | 514 |

Advantages:

- Vendor independent
- Widely supported
- Simple configuration

---

## 3. Manual Upload

Many SIEM solutions support importing offline log files.

Examples:

```text
apache.log
security.evtx
access.log
system.log
```

Common use cases:

- Incident Response
- Digital Forensics
- Malware Analysis
- Threat Hunting
- Security Training Labs

---

## 4. Port Forwarding

SIEM platforms can also listen on specific ports for incoming logs.

Workflow:

```text
Endpoint
      │
TCP Connection
      │
      ▼
Listening Port
      │
      ▼
SIEM
```

This method is commonly used when forwarding logs from remote systems.

---

# SIEM Processing Workflow

After logs arrive at the SIEM, several processing stages occur.

```text
Log Sources
      │
      ▼
Collection
      │
      ▼
Parsing
      │
      ▼
Normalization
      │
      ▼
Storage
      │
      ▼
Correlation
      │
      ▼
Detection Rules
      │
      ▼
Alert
      │
      ▼
SOC Investigation
```

Each stage plays an important role in the detection pipeline.

---

## Collection

Logs are gathered from multiple sources.

Examples:

- Windows
- Linux
- Firewalls
- Web Servers
- Databases
- Active Directory

---

## Parsing

Raw logs are divided into searchable fields.

Example:

Raw Log

```text
192.168.1.15 - - [05/Jul/2026]
"GET /login"
200
```

Parsed Fields

```text
Source IP
HTTP Method
URI
Status Code
Timestamp
```

---

## Normalization

Different vendors use different field names.

Example:

```text
Windows

SourceAddress
```

```text
Linux

RemoteHost
```

```text
Firewall

Src-IP
```

After normalization:

```text
source_ip
```

This allows analysts to search all log sources using the same field name.

---

## Correlation

Related events are connected together.

Example:

```text
VPN Login
      │
      ▼
PowerShell Execution
      │
      ▼
Sensitive File Access
      │
      ▼
Large Outbound Connection
```

Correlation transforms isolated events into meaningful attack scenarios.

---

## Detection Rules

Rules continuously inspect incoming events.

Example:

```text
IF

EventID = 4688

AND

ProcessName contains "miner"

THEN

Generate Alert
```

Detection rules automate threat identification.

---

## Alert Generation

When rule conditions are satisfied, an alert is created.

```text
Detection Rule
        │
Rule Match
        │
        ▼
Security Alert
```

The alert is then assigned to a SOC analyst for investigation.

---

# Alert Investigation Lifecycle

A SOC analyst follows a structured workflow when handling alerts.

```text
Alert Triggered
       │
       ▼
Review Detection Rule
       │
       ▼
Locate Related Event
       │
       ▼
Analyze User
       │
       ▼
Analyze Host
       │
       ▼
Analyze Process
       │
       ▼
Review Timeline
       │
       ▼
Determine

True Positive
or
False Positive
       │
       ▼
Take Action
```

Possible actions include:

- Close the alert
- Tune the detection rule
- Contact the asset owner
- Isolate the affected host
- Block the malicious IP address
- Escalate the incident to the Incident Response team

---

# Detection Rule Example

The room demonstrated a simple rule used to detect cryptocurrency miners.

```text
IF

EventID = 4688

AND

Log Source = WindowsEventLogs

AND

ProcessName contains

"miner"

OR

"crypt"

THEN

Generate Alert

"Potential CryptoMiner Activity"
```

This rule demonstrates how SIEM platforms continuously evaluate incoming events and automatically notify analysts whenever suspicious behavior matches predefined conditions.

# Lab Walkthrough

## Lab Objective

The objective of this lab was to simulate a basic Security Operations Center (SOC) investigation using a SIEM dashboard.

Instead of manually reviewing raw logs, the analyst monitors a centralized dashboard where alerts are automatically generated whenever a detection rule matches suspicious activity.

During the investigation, the analyst must:

- Review the generated alert.
- Identify the suspicious event.
- Determine which process triggered the alert.
- Identify the responsible user.
- Identify the affected host.
- Examine the detection rule.
- Decide whether the alert is a **True Positive** or **False Positive**.
- Select the appropriate response action.

This workflow closely resembles the daily responsibilities of a Tier 1 SOC Analyst.

---

# Investigation Workflow

The investigation followed the process below.

```text
Start Suspicious Activity
          │
          ▼
Detection Rule Triggered
          │
          ▼
Security Alert Generated
          │
          ▼
Open Alert Details
          │
          ▼
Locate Related Event
          │
          ▼
Identify User
          │
          ▼
Identify Host
          │
          ▼
Review Detection Rule
          │
          ▼
Determine Alert Type
          │
          ▼
Choose Response Action
```

---

# Task 1 — Identify the Suspicious Process

### Question

> After clicking on the **Start Suspicious Activity** button, which process caused the alert?

### Investigation

The lab simulated malicious activity that immediately triggered a SIEM alert.

Opening the alert displayed the process responsible for triggering the detection rule.

### Evidence

```text
Process Name

cudominer.exe
```

### Answer

```text
cudominer.exe
```

### Explanation

The suspicious executable was named **cudominer.exe**.

The filename itself strongly indicates cryptocurrency mining software because it contains the keyword **miner**, which matches the SIEM detection rule configured for crypto-mining activity.

---

# Task 2 — Identify the Responsible User

### Question

> Find the event that caused the alert and identify the user responsible for the process execution.

### Investigation

The related event contained information about the account that executed the suspicious process.

### Evidence

```text
Username

chris
```

### Answer

```text
chris
```

### Explanation

The event showed that the user **chris** launched the suspicious executable.

Identifying the responsible user is an important step because analysts often need to verify whether the activity was expected or potentially the result of a compromised account.

---

# Task 3 — Identify the Hostname

### Question

> What is the hostname of the suspect user?

### Investigation

The event details also included the hostname of the affected workstation.

### Evidence

```text
Hostname

HR_02
```

### Answer

```text
HR_02
```

### Explanation

The suspicious process was executed on the workstation named **HR_02**.

Knowing the hostname allows security teams to:

- Locate the affected asset.
- Contact the device owner.
- Isolate the machine if necessary.
- Continue forensic investigation.

---

# Task 4 — Examine the Detection Rule

### Question

> Examine the rule and the suspicious process; which term matched the rule that caused the alert?

### Investigation

The detection rule searched process names for specific keywords commonly associated with cryptocurrency mining malware.

The suspicious executable was:

```text
cudominer.exe
```

The matching keyword was:

```text
miner
```

### Answer

```text
miner
```

### Explanation

The SIEM rule did not specifically look for **cudominer.exe**.

Instead, it searched for generic indicators such as:

```text
miner
```

or

```text
crypt
```

Since **cudominer.exe** contains the word **miner**, the rule conditions were satisfied and the alert was generated.

This illustrates how keyword-based detection rules operate.

---

# Task 5 — Determine the Alert Classification

### Question

> Which option best represents the event?

Options:

- False Positive
- True Positive

### Investigation

The investigation revealed several suspicious indicators:

- Suspicious executable
- Crypto-mining detection rule matched
- Process execution event
- User activity recorded
- Host identified

### Answer

```text
True Positive
```

### Explanation

The alert correctly identified malicious activity.

Therefore, it represents a **True Positive**.

A **False Positive** would occur if a legitimate application accidentally matched the rule despite posing no security risk.

---

# Task 6 — Select the Appropriate Response

### Question

> Selecting the correct action reveals the flag.

### Investigation

Because the event was confirmed as malicious, the appropriate incident response action should be selected.

After choosing the correct response, the flag was displayed.

### Flag

```text
THM{000_SIEM_INTRO}
```

### Explanation

Once malicious activity has been confirmed, the analyst should take immediate action to reduce the impact of the incident.

Depending on organizational procedures, common actions include:

- Isolating the infected host
- Blocking malicious IP addresses
- Escalating the incident
- Contacting the asset owner
- Beginning forensic investigation

---

# Lab Summary

During this investigation, the following information was collected.

| Item | Result |
|------|--------|
| Suspicious Process | `cudominer.exe` |
| User | `chris` |
| Hostname | `HR_02` |
| Detection Keyword | `miner` |
| Alert Classification | True Positive |
| Flag | `THM{000_SIEM_INTRO}` |

---

# What I Learned

This practical exercise demonstrated the complete lifecycle of a SIEM investigation.

Rather than manually reviewing thousands of logs, the analyst follows a structured workflow:

1. Monitor the SIEM dashboard.
2. Review newly generated alerts.
3. Identify the event that triggered the detection.
4. Examine the responsible user and affected endpoint.
5. Understand why the detection rule was triggered.
6. Decide whether the alert is legitimate.
7. Take the appropriate response action.

Although simplified, this workflow accurately reflects the daily responsibilities of a SOC Analyst and illustrates how SIEM platforms help organizations rapidly detect and investigate security incidents.

# Troubleshooting

Although this room is introductory, understanding common SIEM issues is important because log collection and alerting are only effective when the entire pipeline functions correctly.

---

## Issue 1 — No Logs Appearing in the SIEM

### Symptoms

- Dashboard shows no events.
- Searches return no results.
- Alerts are never generated.

### Possible Causes

- Log source is offline.
- Agent/Forwarder is stopped.
- Syslog service is misconfigured.
- Firewall blocks communication between the endpoint and the SIEM.
- Incorrect ingestion configuration.

### Resolution

- Verify the endpoint is online.
- Check that the agent or forwarder service is running.
- Confirm network connectivity to the SIEM server.
- Verify ingestion settings.
- Review SIEM health dashboards.

---

## Issue 2 — Detection Rules Never Trigger

### Symptoms

- Logs are successfully ingested.
- Searches work correctly.
- Expected alerts never appear.

### Possible Causes

- Detection rule is disabled.
- Incorrect field names.
- Incorrect Event ID.
- Rule conditions are too restrictive.
- Log normalization failed.

### Resolution

- Verify the rule is enabled.
- Confirm normalized field names.
- Test the detection logic with sample events.
- Validate Event IDs.
- Review parsing and normalization.

---

## Issue 3 — Too Many False Positives

### Symptoms

- Large number of alerts.
- Most alerts are legitimate administrator activity.
- Analysts waste time investigating harmless events.

### Possible Causes

- Rules are too broad.
- No user exclusions.
- No trusted process exclusions.
- Poor threshold configuration.

### Resolution

- Tune detection rules.
- Exclude trusted administrative accounts.
- Add process allowlists.
- Increase thresholds where appropriate.
- Continuously improve detection logic.

---

## Issue 4 — Missing Important Events

### Symptoms

- Suspicious activity occurred.
- SIEM generated no alert.
- Investigation shows missing logs.

### Possible Causes

- Required logs are not collected.
- Logging policy is incomplete.
- Agent failed to send logs.
- Incorrect filtering.

### Resolution

- Verify required log sources.
- Confirm audit logging is enabled.
- Check agent health.
- Review ingestion filters.

---

# Detection Opportunities

Throughout this room, several activities were identified as valuable detection opportunities.

| Activity | Detection Method |
|----------|------------------|
| Multiple Failed Logins | Failed authentication threshold |
| Successful Login after Failed Attempts | Correlation rule |
| Event Log Clearing | Windows Event ID 104 |
| Process Creation | Windows Event ID 4688 |
| USB Device Connected | Device monitoring |
| Large Outbound Traffic | Network monitoring |
| Crypto Miner Execution | Process name matching |
| PowerShell Execution | Process monitoring |

These activities frequently appear in enterprise detection rules because they are commonly associated with attacker behavior.

---

# Pentester Notes (Red Team Perspective)

Understanding SIEM is valuable not only for defenders but also for penetration testers.

A successful attack is not only about gaining access—it is also about understanding what security controls may detect the attack.

## Reconnaissance

Reconnaissance activities may generate:

- Port Scan Logs
- IDS Alerts
- Firewall Events
- Network Flow Records

Large scans are often detected quickly by SIEM correlation rules.

---

## Enumeration

Enumeration activities commonly leave traces such as:

- LDAP Queries
- SMB Connections
- DNS Requests
- Authentication Attempts

Repeated enumeration can trigger abnormal behavior alerts.

---

## Exploitation

Successful exploitation often generates:

- New Process Creation
- Service Creation
- PowerShell Execution
- Command Prompt Activity

Many SIEM detection rules monitor these events.

---

## Privilege Escalation

Common indicators include:

- Administrator Logins
- Group Membership Changes
- Privileged Token Assignment
- Account Creation

Windows Event Logs provide valuable evidence during privilege escalation attempts.

---

## Lateral Movement

Techniques such as:

- RDP
- PsExec
- WinRM
- SMB
- Remote PowerShell

produce logs on multiple systems that SIEM platforms can correlate.

---

## Persistence

Persistence mechanisms often generate logs involving:

- Scheduled Tasks
- Registry Changes
- Startup Folder Modifications
- Service Installation

These activities are frequently monitored by security teams.

---

## Defense Evasion

Attackers sometimes attempt to hide their activity.

Examples include:

- Clearing Event Logs
- Disabling Security Software
- Stopping Logging Agents

Ironically, many of these actions themselves generate security events that can trigger alerts.

---

# Blue Team Notes (SOC Analyst Perspective)

A SIEM platform is one of the primary tools used by SOC analysts.

A typical investigation follows this workflow:

```text
Alert Generated
        │
        ▼
Review Alert
        │
        ▼
Locate Related Events
        │
        ▼
Review Detection Rule
        │
        ▼
Build Timeline
        │
        ▼
Determine Impact
        │
        ▼
True Positive?
        │
   ┌────┴─────┐
   │          │
   ▼          ▼
Close      Escalate
Alert      Incident
```

SOC analysts should always answer several important questions during an investigation:

- What happened?
- Who performed the activity?
- Which host was affected?
- When did it happen?
- Why did the rule trigger?
- Is the activity malicious?
- What action should be taken?

---

# Security Best Practices

To maximize the effectiveness of a SIEM deployment, organizations should follow several best practices.

## Centralize Logging

Collect logs from every critical asset, including:

- Endpoints
- Servers
- Firewalls
- Network Devices
- Active Directory
- Cloud Services

---

## Enable Comprehensive Logging

Configure systems to record important security events such as:

- Authentication
- Process Creation
- PowerShell Execution
- File Access
- Network Connections

Without proper logging, attacks may go undetected.

---

## Continuously Tune Detection Rules

Detection rules should evolve as environments change.

Goals include:

- Reduce false positives.
- Improve detection accuracy.
- Detect new attack techniques.
- Adapt to changing business requirements.

---

## Monitor SIEM Health

A SIEM is only useful if it continuously receives logs.

Regularly verify:

- Agent status
- Log ingestion rate
- Storage utilization
- Parsing success
- Detection rule health

---

# Lessons Learned

This room demonstrates that collecting logs alone is not enough.

The real value of a SIEM comes from its ability to:

- Centralize logs from multiple systems.
- Normalize different log formats.
- Correlate seemingly unrelated events.
- Detect suspicious activity automatically.
- Assist analysts during investigations.
- Improve incident response efficiency.

Without SIEM, security analysts would spend most of their time manually reviewing logs instead of investigating real security incidents.

This room serves as an excellent introduction to the workflows and concepts that form the foundation of modern Security Operations Centers.

# Key Takeaways

This room introduced the fundamental concepts behind **Security Information and Event Management (SIEM)** and demonstrated why SIEM has become a core technology in modern Security Operations Centers (SOCs).

The biggest lesson is that logs alone provide limited value when they are scattered across different systems. A SIEM transforms these isolated logs into actionable security intelligence by collecting, normalizing, correlating, and analyzing events from across an organization's infrastructure.

Some of the most important concepts learned include:

- Every device generates valuable security logs.
- Logs should be centralized to improve visibility.
- Different log formats must be parsed and normalized before analysis.
- Event correlation provides context that individual logs cannot.
- Detection rules automatically identify suspicious activities.
- Alerts must always be investigated before taking action.
- SOC analysts determine whether alerts are **True Positives** or **False Positives**.
- Continuous rule tuning is necessary to reduce false positives while maintaining detection effectiveness.

---

# Skills Gained

By completing this room, I developed practical knowledge in the following areas:

## SIEM Fundamentals

- Understanding the purpose of SIEM
- Understanding centralized logging
- Understanding enterprise security monitoring

---

## Log Management

- Host-centric log sources
- Network-centric log sources
- Windows Event Logs
- Linux Logs
- Web Server Logs
- Network Device Logs

---

## Log Processing

- Log Collection
- Log Ingestion
- Parsing
- Normalization
- Event Correlation

---

## Detection Engineering Basics

- Detection Rules
- Event Matching
- Process Monitoring
- Event ID Analysis
- Alert Generation

---

## SOC Operations

- Dashboard Monitoring
- Alert Investigation
- Event Analysis
- Host Identification
- User Identification
- Process Investigation
- Incident Classification

---

## Incident Response

- Identifying suspicious activity
- Determining True Positives
- Determining False Positives
- Selecting appropriate response actions

---

# Knowledge Strengthened

This room reinforced concepts learned in previous TryHackMe rooms.

| Previous Room | Reinforced Concepts |
|--------------|--------------------|
| Networking Fundamentals | Network communication and log generation |
| Windows Fundamentals | Windows Event Logs |
| Linux Fundamentals | Linux logging system |
| Logs Fundamentals | Log analysis and interpretation |
| SOC Fundamentals | Security monitoring workflow |
| Incident Response Fundamentals | Alert handling and investigation |
| Digital Forensics Fundamentals | Using logs as investigative evidence |

Rather than introducing completely new topics, this room connected those concepts into a complete SOC monitoring workflow.

---

# Real-World SOC Workflow

A modern Security Operations Center typically follows the workflow below.

```text
Users & Systems
        │
        ▼
Generate Security Logs
        │
        ▼
Log Sources
(Windows, Linux, Firewall, VPN, Web Server)
        │
        ▼
SIEM Platform
        │
        ▼
Parsing & Normalization
        │
        ▼
Event Correlation
        │
        ▼
Detection Rules
        │
        ▼
Alert Generated
        │
        ▼
SOC Analyst Investigation
        │
        ▼
True Positive?
        │
   ┌────┴────┐
   │         │
   ▼         ▼
Close      Incident
Alert      Response
```

Understanding this workflow is essential for anyone pursuing a career in Blue Team operations.

---

# Career Relevance

SIEM knowledge is required for many cybersecurity roles.

Examples include:

- SOC Analyst Tier 1
- SOC Analyst Tier 2
- Detection Engineer
- Incident Responder
- Threat Hunter
- Security Engineer
- Blue Team Analyst
- Cyber Defense Analyst

Most enterprise environments rely heavily on SIEM platforms to monitor and investigate security events.

---

# Future Learning Path

This room serves as an introduction to SIEM concepts. Recommended next topics include:

## SIEM Platforms

- Splunk Enterprise Security
- Microsoft Sentinel
- IBM QRadar
- Elastic Security
- Wazuh

---

## Detection Engineering

- Writing detection rules
- Sigma Rules
- YARA
- Behavioral detection
- MITRE ATT&CK mapping

---

## Threat Hunting

- IOC Hunting
- Log Searching
- Timeline Analysis
- Threat Intelligence Integration

---

## Windows Event Analysis

Study common Windows Event IDs such as:

- 4624
- 4625
- 4688
- 4672
- 4720
- 4726
- 7045

Understanding these events is essential for Windows security monitoring.

---

## Advanced Log Analysis

Continue learning how to investigate:

- PowerShell Logs
- Sysmon Logs
- DNS Logs
- Firewall Logs
- Proxy Logs
- Active Directory Logs
- Cloud Logs

---

# References

## Official Documentation

- TryHackMe — Introduction to SIEM Room
- Microsoft Windows Event Logging Documentation
- Splunk Documentation
- Microsoft Sentinel Documentation
- Elastic Security Documentation
- Wazuh Documentation

---

# Tags

```text
#TryHackMe
#SOC
#BlueTeam
#SIEM
#Splunk
#WindowsLogs
#LinuxLogs
#EventViewer
#Syslog
#DetectionRules
#AlertInvestigation
#LogAnalysis
#ThreatDetection
#IncidentResponse
#SecurityMonitoring
#CyberSecurity
#CyberJourney
```

---

# Personal Reflection

This room marked an important milestone in my cybersecurity learning journey because it connected many topics that I had previously studied separately.

Earlier rooms introduced networking, operating systems, log analysis, SOC operations, incident response, and digital forensics. This room demonstrated how those concepts come together inside a SIEM platform to enable centralized monitoring and efficient security investigations.

Although the lab was intentionally simple, it accurately reflected the basic workflow of a SOC analyst:

- Monitor dashboards.
- Investigate alerts.
- Review related events.
- Understand why a detection rule triggered.
- Identify the affected user and host.
- Determine whether the activity is malicious.
- Recommend an appropriate response.

This foundational knowledge will make it much easier to learn enterprise SIEM platforms such as Splunk, Microsoft Sentinel, and Wazuh, as well as more advanced topics like detection engineering, threat hunting, and incident response automation.