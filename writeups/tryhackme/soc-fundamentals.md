# SOC Fundamentals

> Learn the foundations of a Security Operations Center (SOC), understand its core responsibilities, explore the three pillars of SOC operations (People, Process, and Technology), and perform a practical alert triage exercise as a Level 1 SOC Analyst.

---

# Executive Summary

A Security Operations Center (SOC) is the defensive core of an organization's cybersecurity program. Rather than preventing every attack, a SOC continuously monitors systems, detects suspicious activities, investigates security events, and responds to incidents before they cause significant damage.

In this room, we explored the purpose of a SOC, its responsibilities, and the three fundamental pillars that enable an effective security operation: **People, Process, and Technology**. We also learned how SOC analysts perform alert triage using the **5 Ws** methodology and completed a practical exercise by investigating a port scanning alert through a simulated SIEM platform.

This room serves as an excellent introduction to Blue Team operations and establishes the knowledge required before learning SIEM, Threat Hunting, Detection Engineering, Incident Response, and Digital Forensics.

---

# Learning Objectives

By completing this room, I learned how to:

- Understand the purpose and responsibilities of a Security Operations Center (SOC).
- Explain the difference between **Detection** and **Response**.
- Identify the three pillars of a mature SOC environment.
- Understand the roles within a SOC team.
- Learn the alert triage workflow using the **5 Ws** methodology.
- Gain a high-level understanding of common SOC technologies such as SIEM, EDR, and Firewalls.
- Investigate a real-world alert as a SOC Level 1 Analyst.

---

# Prerequisites

Before taking this room, it is helpful to understand:

- Basic networking concepts
- TCP/IP fundamentals
- Common cyber attacks
- Linux and Windows basics
- Web application fundamentals

No previous Blue Team experience is required.

---

# What is a Security Operations Center (SOC)?

A **Security Operations Center (SOC)** is a dedicated team responsible for continuously monitoring, detecting, investigating, and responding to cybersecurity threats across an organization's infrastructure.

Unlike traditional IT teams that focus on maintaining system availability, a SOC focuses on protecting an organization's digital assets against cyber attacks.

SOC teams typically operate **24 hours a day, 7 days a week**, because cyber attacks can occur at any time.

The primary mission of a SOC is **not to prevent every attack**, but to:

- Detect attacks as quickly as possible.
- Investigate suspicious activities.
- Respond before attackers can cause significant damage.
- Improve the organization's security posture over time.

---

# Why Do Organizations Need a SOC?

Modern organizations rely heavily on digital infrastructure.

Examples include:

- Web applications
- Cloud services
- Email systems
- Active Directory
- Employee endpoints
- Internal servers
- Databases

Each of these systems continuously generates logs and events.

Without centralized monitoring, identifying malicious activity among millions of daily events would be nearly impossible.

A SOC helps organizations:

- Continuously monitor infrastructure
- Detect suspicious behavior
- Reduce attacker dwell time
- Coordinate incident response
- Improve overall cyber resilience

---

# Responsibilities of a SOC

The SOC is responsible for much more than simply monitoring dashboards.

Its primary responsibilities include:

- Detecting vulnerabilities
- Detecting unauthorized activities
- Detecting policy violations
- Detecting network and host intrusions
- Investigating suspicious alerts
- Supporting incident response activities
- Performing root cause analysis
- Continuously improving detection capabilities

---

# Detection vs Response

One of the most important concepts introduced in this room is understanding the difference between **Detection** and **Response**.

## Detection

Detection focuses on identifying abnormal or suspicious activities before they become major security incidents.

Examples include:

- Multiple failed login attempts
- Malware execution
- Port scanning
- Unauthorized privilege escalation
- Suspicious PowerShell execution
- Unusual outbound network connections

Detection answers questions such as:

- Is this activity suspicious?
- Should this alert be investigated?
- Does this indicate an attack?

---

## Response

Once malicious activity has been confirmed, the SOC moves into the response phase.

Typical response activities include:

- Isolating compromised hosts
- Blocking malicious IP addresses
- Containing attacker movement
- Removing malware
- Recovering affected systems
- Performing root cause analysis

The overall objective is to minimize business impact while restoring normal operations.

---

# High-Level SOC Workflow

A simplified SOC workflow looks like this:

```text
User Activity
        │
        ▼
Logs Generated
        │
        ▼
Security Monitoring
        │
        ▼
Detection
        │
        ▼
Alert Triage
        │
        ▼
Investigation
        │
        ▼
Incident Response
        │
        ▼
Recovery
```

Rather than reacting only after a compromise occurs, SOC teams continuously monitor systems throughout the entire attack lifecycle.

---

# Red Team vs Blue Team Perspective

## Red Team Perspective

From an attacker's perspective, every action generates artifacts that defenders may observe.

Examples include:

- Port scanning
- Failed logins
- Reverse shells
- PowerShell execution
- Lateral movement
- Data exfiltration

Understanding SOC operations helps penetration testers understand what activities are likely to trigger security alerts.

---

## Blue Team Perspective

The Blue Team uses logs, alerts, and threat intelligence to answer questions such as:

- What happened?
- When did it happen?
- Who performed the activity?
- Which systems were affected?
- Is the activity malicious?
- How should the organization respond?

The goal is to quickly distinguish legitimate activity from malicious behavior and respond appropriately.

---

# Key Terminology

| Term | Description |
|------|-------------|
| Event | Any recorded activity occurring on a system. |
| Alert | A suspicious event that matches one or more detection rules. |
| Incident | A confirmed security event requiring response actions. |
| Detection | The process of identifying suspicious activity. |
| Response | Actions taken after confirming a security incident. |
| SOC | Security Operations Center responsible for monitoring and responding to threats. |

---

# Key Takeaways

- A SOC is responsible for **continuous monitoring, detection, investigation, and incident response**.
- Detection and Response are the primary objectives of every SOC.
- Organizations rely on SOC teams because manual monitoring of modern infrastructure is impossible.
- Every cyber attack leaves evidence through logs and events that SOC analysts investigate.
- Understanding how defenders operate is essential knowledge for both Blue Team professionals and penetration testers.


**Next:** In Part 2, we'll explore the three pillars that make a Security Operations Center effective: **People, Process, and Technology**.

---

# The Three Pillars of a Security Operations Center

A mature Security Operations Center (SOC) is built upon three fundamental pillars:

- **People**
- **Process**
- **Technology**

These three pillars work together to create an effective security operation. Even if one pillar is weak, the SOC's ability to detect and respond to cyber threats will be significantly reduced.

The relationship between these pillars can be illustrated as follows:

```text
             PEOPLE
                ▲
               / \
              /   \
             /     \
      PROCESS ----- TECHNOLOGY
```

A successful SOC is not built by simply purchasing expensive security tools. It requires skilled professionals following standardized procedures while utilizing the appropriate technologies.

---

# Pillar 1 — People

People are the heart of every Security Operations Center.

Although modern security platforms use automation, artificial intelligence, and machine learning, humans are still responsible for making the final decisions during investigations.

Security tools generate alerts based on predefined rules or behavioral analysis, but they cannot fully understand business context.

For example, a PowerShell execution may be:

- A system administrator performing maintenance.
- A software deployment script.
- A malicious attacker executing malware.

A security solution alone cannot always distinguish between these scenarios.

SOC analysts investigate the evidence before deciding whether an alert is legitimate or malicious.

---

## Why People Are Important

Imagine a city's emergency center receiving thousands of fire alarms every day.

If every alarm immediately triggered a full emergency response, firefighters would waste enormous amounts of time responding to burnt food or harmless smoke.

SOC teams face the same challenge.

Security tools often generate thousands of alerts daily.

Many of these alerts are:

- False positives
- Expected administrative activity
- Scheduled vulnerability scans
- Automated software updates

SOC analysts filter these alerts and focus only on activities that represent genuine security risks.

---

# SOC Team Structure

A typical SOC consists of multiple roles, each with different responsibilities.

```text
                    SOC Manager
                         │
      ┌──────────────────┼──────────────────┐
      │                  │                  │
Detection Engineer   Security Engineer   SOC Level 3
                                              │
                                         SOC Level 2
                                              │
                                         SOC Level 1
```

---

## SOC Analyst Level 1

SOC Level 1 Analysts are the first responders.

Every alert generated by the SIEM or security platform is initially reviewed by Level 1 analysts.

Their responsibilities include:

- Monitoring security dashboards
- Performing initial alert triage
- Determining alert severity
- Collecting basic evidence
- Creating investigation tickets
- Escalating suspicious alerts

Level 1 analysts do **not** perform deep forensic investigations.

Instead, they determine whether an alert requires additional investigation.

---

## SOC Analyst Level 2

Level 2 analysts investigate complex alerts that cannot be resolved during initial triage.

Their responsibilities include:

- Correlating logs from multiple sources
- Investigating suspicious behavior
- Validating attack indicators
- Supporting incident response
- Assisting Level 1 analysts

Unlike Level 1, they perform much deeper technical investigations.

---

## SOC Analyst Level 3

Level 3 analysts are experienced security professionals responsible for handling the organization's most serious security incidents.

Typical responsibilities include:

- Threat Hunting
- Advanced investigations
- Malware analysis
- Root Cause Analysis
- Incident Response
- Detection improvement
- Mentoring junior analysts

Rather than waiting for alerts, Level 3 analysts often proactively search for hidden threats throughout the environment.

---

## Security Engineer

SOC analysts depend on security technologies.

Someone must install, configure, maintain, and update those systems.

This is the responsibility of the Security Engineer.

Typical tasks include:

- Deploying SIEM platforms
- Configuring EDR solutions
- Maintaining IDS/IPS sensors
- Managing security infrastructure
- Troubleshooting security tools

Without Security Engineers, analysts would have no visibility into the environment.

---

## Detection Engineer

Detection Engineers create the logic behind security alerts.

For example:

```text
IF

100 Failed Login Attempts

Within 5 Minutes

THEN

Generate Brute Force Alert
```

They continuously improve detection rules by studying:

- New attack techniques
- Threat intelligence
- MITRE ATT&CK techniques
- Previous incidents

This role is becoming increasingly important in modern SOC environments.

---

## SOC Manager

The SOC Manager oversees the entire security operations team.

Responsibilities include:

- Managing SOC operations
- Defining processes
- Reviewing KPIs
- Coordinating incident response
- Reporting to executive leadership
- Communicating with the CISO

Rather than investigating alerts, SOC Managers ensure that the entire SOC operates efficiently.

---

# Pillar 2 — Process

Having skilled analysts alone is not enough.

Without standardized procedures, every analyst may investigate incidents differently, resulting in inconsistent responses.

Processes ensure that every alert follows the same structured workflow.

---

## Alert Triage

Alert triage is usually the first activity performed after receiving an alert.

Its purpose is to determine:

- Is the alert legitimate?
- How severe is it?
- Does it require escalation?
- What evidence exists?
- What should happen next?

Alert triage helps analysts prioritize incidents based on risk.

---

## The 5 Ws Methodology

SOC analysts commonly use the **5 Ws** to investigate alerts.

| Question | Purpose |
|----------|---------|
| **What?** | What activity triggered the alert? |
| **When?** | When did the activity occur? |
| **Where?** | Which system or host was affected? |
| **Who?** | Which user or device performed the activity? |
| **Why?** | Why did the activity occur? Was it expected or malicious? |

Answering these questions provides enough context to determine whether an alert represents a legitimate threat.

---

## Reporting

After completing the investigation, analysts document their findings.

A proper security report typically includes:

- Alert summary
- Severity
- Timeline
- Evidence
- Investigation findings
- Impact assessment
- Recommended actions

These reports are often submitted through ticketing systems and forwarded to higher-level analysts when necessary.

---

## Incident Response

If an alert is confirmed to be malicious, it becomes a security incident.

The Incident Response team works to:

- Contain the attack
- Remove malicious artifacts
- Restore affected systems
- Prevent reinfection

A typical response workflow looks like this:

```text
Alert
   │
   ▼
Investigation
   │
   ▼
Incident Confirmed
   │
   ▼
Containment
   │
   ▼
Eradication
   │
   ▼
Recovery
```

---

## Digital Forensics

Some incidents require detailed forensic investigations.

Digital forensics aims to determine:

- How the attacker gained access.
- What actions were performed.
- Which systems were affected.
- Whether data was stolen.
- The root cause of the incident.

Artifacts commonly analyzed include:

- Event logs
- Memory dumps
- Registry changes
- Browser history
- Network captures
- Malware samples

---

# Pillar 3 — Technology (Overview)

Technology enables SOC teams to monitor thousands of systems efficiently.

Instead of manually checking every server, workstation, and firewall, security technologies collect, analyze, and correlate security events automatically.

Common technologies include:

| Technology | Primary Purpose |
|------------|-----------------|
| SIEM | Collects, correlates, and analyzes logs from multiple sources. |
| EDR | Detects and responds to threats on endpoints. |
| Firewall | Filters network traffic between trusted and untrusted networks. |
| IDS/IPS | Detects and prevents network-based attacks. |
| SOAR | Automates security workflows and incident response. |
| XDR | Extends detection and response across multiple security domains. |

The Technology pillar will be explored in greater detail in the next section.

---

# Why All Three Pillars Matter

Each pillar depends on the others.

```text
Security Tools
        │
        ▼
Generate Alerts
        │
        ▼
SOC Analysts
        │
        ▼
Follow Standard Processes
        │
        ▼
Investigate
        │
        ▼
Respond
```

If one pillar is missing:

- **Technology without People** results in thousands of unattended alerts.
- **People without Technology** cannot effectively monitor large environments.
- **People and Technology without Process** lead to inconsistent investigations and poor incident handling.

A mature SOC requires all three pillars to work together.

---

# Key Takeaways

- People make security decisions and investigate alerts.
- Processes ensure investigations follow standardized procedures.
- Technology provides visibility, detection, and response capabilities.
- SOC maturity depends on balancing all three pillars rather than relying on security tools alone.
- Effective cybersecurity is achieved through the combination of skilled professionals, well-defined processes, and modern security technologies.

---

**Next:** In Part 3, we'll take a deeper look at the technologies that power modern SOC operations, including **SIEM, EDR, Firewalls, IDS/IPS, XDR, and SOAR**.

---

# SOC Technologies

Technology is the third pillar of a Security Operations Center (SOC).

Even with highly skilled analysts and well-defined processes, monitoring thousands of endpoints and network devices manually would be impossible.

Security technologies provide centralized visibility, automate repetitive tasks, detect suspicious behavior, and assist analysts during investigations.

Rather than replacing analysts, these technologies significantly improve their efficiency and response time.

---

# Why SOC Needs Security Technologies

Modern enterprise environments consist of hundreds or even thousands of devices.

Examples include:

- Windows Servers
- Linux Servers
- Employee Workstations
- Active Directory
- Firewalls
- VPN Gateways
- Cloud Infrastructure
- Web Applications
- Email Servers
- Databases

Each device continuously generates logs.

A medium-sized organization may produce millions of security events every day.

Instead of manually reviewing every log, SOC teams rely on security platforms to collect, correlate, and analyze this information.

---

# Centralized Security Monitoring

Modern SOCs collect logs from multiple systems into a centralized platform.

Instead of investigating every device individually:

```text
Windows
Linux
Firewall
VPN
Email
Cloud
Database
```

Everything is forwarded to a central security platform.

```text
Windows Logs
        │
Linux Logs
        │
Firewall Logs
        │
VPN Logs
        │
Cloud Logs
        │
───────────────
        │
       SIEM
        │
Correlation
        │
Detection Rules
        │
Alerts
        │
SOC Analyst
```

Centralized monitoring allows analysts to investigate incidents from a single dashboard.

---

# Security Information and Event Management (SIEM)

The **Security Information and Event Management (SIEM)** platform is the backbone of most modern SOC environments.

Its primary responsibilities include:

- Collecting logs
- Normalizing log data
- Correlating events
- Running detection rules
- Generating alerts
- Supporting investigations

It is important to understand that **SIEM is primarily a detection platform**.

It helps analysts identify suspicious activities but typically does not perform automatic response actions.

---

## SIEM Workflow

A simplified SIEM workflow looks like this:

```text
Security Devices
        │
        ▼
Log Collection
        │
        ▼
Normalization
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
SOC Analyst
```

---

## Log Sources

A **Log Source** is any system capable of sending logs to the SIEM.

Common log sources include:

- Windows Event Logs
- Linux Syslog
- Active Directory
- Firewalls
- VPN Servers
- Web Servers
- Email Servers
- DNS Servers
- Proxy Servers
- Cloud Platforms
- Endpoint Security Solutions

The more log sources available, the better the visibility across the environment.

---

## Detection Rules

Detection rules define what activities should generate security alerts.

For example:

```text
IF

More than 100 Failed Login Attempts

Within 5 Minutes

THEN

Generate Brute Force Alert
```

Another example:

```text
IF

PowerShell

AND

EncodedCommand

THEN

Generate High Severity Alert
```

Detection Engineers continuously improve these rules based on:

- Threat Intelligence
- MITRE ATT&CK
- Previous incidents
- Emerging attack techniques

---

## Event Correlation

One of SIEM's strongest capabilities is **event correlation**.

Instead of analyzing logs individually, SIEM combines multiple data sources to identify suspicious patterns.

Example:

```text
Firewall
↓

Connection to Malicious IP

Windows
↓

PowerShell Execution

DNS
↓

Resolved Suspicious Domain
```

Individually, each event may appear harmless.

When correlated together, they strongly indicate malware activity.

---

## Modern SIEM Features

Modern SIEM platforms offer capabilities beyond traditional rule-based detection.

These include:

### User and Entity Behavior Analytics (UEBA)

UEBA establishes a baseline of normal user behavior and detects anomalies.

Example:

```text
Normal Behavior

User logs in
09:00
New York

↓

Abnormal Behavior

User logs in
03:00
Russia
```

Even though authentication succeeds, the login behavior appears suspicious.

---

### Threat Intelligence Integration

Threat Intelligence feeds contain information about known malicious infrastructure.

Examples include:

- Malicious IP addresses
- Command-and-control servers
- Malware domains
- File hashes

When SIEM detects communication with these indicators, it can automatically generate alerts.

---

### Machine Learning

Machine Learning helps identify previously unseen attack patterns.

Instead of relying only on predefined rules, the SIEM can detect unusual behaviors based on statistical analysis.

Examples include:

- Abnormal login frequency
- Unusual data transfers
- Rare administrative commands
- Suspicious user behavior

---

# Endpoint Detection and Response (EDR)

While SIEM provides visibility across the entire organization, **Endpoint Detection and Response (EDR)** focuses specifically on endpoints.

Endpoints include:

- Workstations
- Laptops
- Servers
- Virtual Machines

EDR provides both **Detection** and **Response** capabilities.

---

## Endpoint Visibility

EDR continuously monitors endpoint activities such as:

- Process creation
- File creation
- Registry modifications
- DLL loading
- Network connections
- User logins
- PowerShell execution

This allows analysts to reconstruct exactly what happened on an infected system.

---

## EDR Workflow

```text
Endpoint
      │
      ▼
EDR Agent
      │
      ▼
EDR Console
      │
      ▼
SOC Analyst
```

Unlike SIEM, EDR can perform automated response actions directly on the endpoint.

---

## Common EDR Response Actions

SOC analysts can perform actions such as:

- Isolate compromised devices
- Kill malicious processes
- Quarantine malware
- Delete malicious files
- Collect forensic artifacts

These actions help contain attacks before they spread further.

---

# Firewall

A firewall acts as the organization's first line of network defense.

It controls traffic entering and leaving the internal network.

```text
Internet
     │
     ▼
 Firewall
     │
     ▼
Internal Network
```

Firewalls inspect:

- Source IP
- Destination IP
- Ports
- Protocols
- Connection direction

Traffic is either allowed or blocked according to predefined security policies.

---

## Firewall Detection

Modern firewalls also provide basic detection capabilities.

Examples include:

- Port scanning
- Brute-force attempts
- Known exploit signatures
- Malicious IP addresses
- Denial-of-Service attacks

Many enterprise firewalls integrate IDS/IPS functionality directly into the platform.

---

# Intrusion Detection System (IDS)

An **Intrusion Detection System (IDS)** monitors network traffic for suspicious activities.

Unlike firewalls, IDS typically **does not block** traffic.

Instead, it generates alerts for SOC analysts.

Typical detection examples include:

- Port scans
- Malware communication
- Exploit attempts
- Policy violations

---

# Intrusion Prevention System (IPS)

An **Intrusion Prevention System (IPS)** extends IDS functionality by actively blocking malicious traffic.

Comparison:

```text
IDS

Detect
↓

Alert

---------------------

IPS

Detect
↓

Block
↓

Alert
```

IPS reduces attacker opportunities by preventing malicious traffic from reaching internal systems.

---

# Extended Detection and Response (XDR)

Traditional EDR focuses only on endpoints.

**Extended Detection and Response (XDR)** expands visibility across multiple security domains.

These may include:

- Endpoints
- Email
- Cloud services
- Identity platforms
- Network devices
- DNS
- Firewalls

XDR correlates information across these sources, allowing analysts to investigate incidents more efficiently.

---

# Security Orchestration, Automation and Response (SOAR)

SOAR platforms automate repetitive SOC tasks.

Instead of manually responding to every alert, SOAR executes predefined workflows.

Example automation:

```text
Alert Generated
        │
        ▼
Create Ticket
        │
        ▼
Notify Analyst
        │
        ▼
Isolate Endpoint
        │
        ▼
Collect Evidence
```

Automation significantly reduces Mean Time To Respond (MTTR).

---

# Technology Comparison

| Technology | Primary Function | Detection | Response |
|------------|------------------|-----------|----------|
| SIEM | Log collection, correlation, alerting | ✅ | ❌ |
| EDR | Endpoint monitoring and protection | ✅ | ✅ |
| Firewall | Network traffic filtering | ✅ | ✅ |
| IDS | Network intrusion detection | ✅ | ❌ |
| IPS | Network intrusion prevention | ✅ | ✅ |
| XDR | Cross-platform detection and response | ✅ | ✅ |
| SOAR | Security workflow automation | Supports | ✅ |

---

# How These Technologies Work Together

A modern SOC rarely depends on a single security solution.

Instead, multiple technologies complement one another.

```text
Endpoints
Servers
Firewalls
VPN
Cloud
Email
      │
      ▼
Log Collection
      │
      ▼
SIEM
      │
      ▼
Alert
      │
      ▼
SOC Analyst
      │
      ▼
EDR
      │
      ▼
Containment
      │
      ▼
SOAR
      │
      ▼
Automated Response
```

Each technology contributes unique capabilities, creating a layered defense strategy.

---

# Red Team Perspective

Understanding SOC technologies helps penetration testers anticipate how their activities may be detected.

Examples include:

- Port scanning detected by Firewalls or IDS.
- PowerShell execution detected by EDR.
- Lateral movement identified through SIEM correlation.
- Malware communication blocked by Firewalls or IPS.
- Suspicious endpoint behavior investigated using EDR telemetry.

Modern Red Team operations often focus not only on exploitation but also on minimizing detection across these technologies.

---

# Key Takeaways

- Technology enables SOC teams to monitor large environments efficiently.
- SIEM centralizes logs and generates alerts through event correlation.
- EDR provides deep endpoint visibility and response capabilities.
- Firewalls, IDS, and IPS secure network traffic from different perspectives.
- XDR extends visibility beyond endpoints by integrating multiple security domains.
- SOAR automates repetitive security operations, improving response time.
- No single technology can defend an organization alone; effective SOC operations rely on multiple integrated security solutions.

---

**Next:** In Part 4, we'll perform a practical SOC investigation by acting as a **Level 1 SOC Analyst**, analyzing a port scanning alert, answering the **5 Ws**, and determining whether the activity is malicious or legitimate.

---

# Practical Exercise — Investigating a Security Alert

The practical exercise in this room simulates one of the most common tasks performed by a **SOC Level 1 Analyst**.

Rather than exploiting systems or performing offensive security activities, the objective is to investigate an alert generated by the organization's SIEM platform and determine whether the detected activity represents a genuine security threat.

This exercise demonstrates how the three pillars of a SOC—**People, Process, and Technology**—work together during daily security operations.

---

# Scenario

You are working as a **SOC Level 1 Analyst**.

The organization's SIEM generates the following alert:

> **Port Scanning Activity Detected**

Your objective is to investigate the alert by reviewing the available logs and answering the **5 Ws** used during the alert triage process.

Before beginning the investigation, the Vulnerability Assessment Team informs the SOC that they are conducting scheduled vulnerability scanning from the following host:

```text
10.0.0.8
```

This information serves as important context during the investigation.

---

# Objective

The objective of this investigation is to determine:

- What activity triggered the alert?
- When did the activity occur?
- Which system was targeted?
- Who initiated the activity?
- Why did it occur?

Based on these answers, determine whether the alert represents malicious behavior or expected administrative activity.

---

# Investigation Workflow

The investigation follows a standard Level 1 SOC workflow.

```text
Alert Generated
        │
        ▼
Review SIEM Logs
        │
        ▼
Answer the 5 Ws
        │
        ▼
Determine Severity
        │
        ▼
Determine Legitimacy
        │
        ▼
Close or Escalate Alert
```

---

# Step 1 — Identify the Alert

The SIEM generated the following alert:

```text
Port Scanning Activity Detected
```

At this stage, analysts should avoid making assumptions.

A port scan is **not automatically malicious**.

Port scanning may be performed by:

- Attackers
- Vulnerability scanners
- Network administrators
- Security assessments
- Compliance audits

The analyst must investigate further before reaching any conclusions.

---

# Step 2 — Review the SIEM Logs

The SIEM provides detailed information about the alert.

The analyst reviews:

- Source host
- Destination host
- Timestamp
- Network communication
- Additional context

These logs provide the evidence required to answer the 5 Ws.

---

# Step 3 — Answer the 5 Ws

## What?

**Question**

> What activity triggered the alert?

**Answer**

```text
Port Scan
```

### Explanation

The SIEM detected a host attempting to connect to multiple ports on another system.

This behavior is commonly associated with reconnaissance activities.

However, port scanning alone does **not** confirm malicious intent.

---

## When?

**Question**

> When did the activity occur?

**Answer**

```text
June 12, 2024

17:24
```

### Explanation

Recording the exact timestamp allows analysts to:

- Build an attack timeline.
- Correlate additional events.
- Search surrounding logs.
- Determine whether other suspicious activities occurred before or after the scan.

---

## Where?

**Question**

> Which system was targeted?

**Answer**

```text
10.0.0.3
```

### Explanation

The destination IP identifies the host being scanned.

Knowing which asset is targeted helps analysts evaluate potential business impact.

For example, scanning a workstation is generally less critical than scanning a Domain Controller or production database server.

---

## Who?

**Question**

> Which host initiated the scan?

**Answer**

```text
Nessus
```

### Explanation

The source hostname is **Nessus**.

Nessus is one of the most widely used vulnerability assessment platforms in enterprise environments.

Rather than indicating an attacker, this strongly suggests that the activity originated from an authorized internal vulnerability scanner.

---

## Why?

**Question**

> Why did the activity occur?

**Answer**

```text
Intended
```

### Explanation

Before the investigation began, the Vulnerability Assessment Team informed the SOC that they would be performing scheduled scanning activities.

Therefore:

- The scan was expected.
- The activity was authorized.
- The alert represents legitimate administrative behavior rather than malicious activity.

This demonstrates why context is critical during SOC investigations.

---

# Additional Investigation

The analyst also reviews whether the target host responded to the scanner.

**Question**

```text
Has any response been sent back to the port scanner?
```

**Answer**

```text
Yes
```

This confirms that communication occurred between the scanner and the destination host during the assessment.

---

# Investigation Summary

| Investigation Question | Answer |
|-------------------------|--------|
| **What** | Port Scan |
| **When** | June 12, 2024 – 17:24 |
| **Where** | Destination IP: **10.0.0.3** |
| **Who** | Source Host: **Nessus** |
| **Why** | Intended vulnerability assessment |

---

# Final Assessment

After reviewing all available evidence, the analyst determines:

- The detected activity originated from the organization's authorized vulnerability scanner.
- The scan was scheduled by the Vulnerability Assessment Team.
- No indicators of malicious activity were identified.
- No escalation to Incident Response was required.

The alert can therefore be safely closed as a **benign security event**.

---

# Flag

```text
THM{000_INTRO_TO_SOC}
```

---

# Why This Exercise Is Important

This lab demonstrates one of the most important lessons in SOC operations:

> **Not every alert represents an attack.**

Security tools generate alerts based on predefined rules.

They cannot determine business context.

That responsibility belongs to SOC analysts.

Without proper investigation, analysts may mistakenly escalate legitimate administrative activities, resulting in wasted time and unnecessary incident response efforts.

---

# Red Team Perspective

From an attacker's perspective, port scanning is typically one of the earliest stages of an attack.

A common attack workflow is:

```text
Reconnaissance
        │
        ▼
Port Scanning
        │
        ▼
Service Enumeration
        │
        ▼
Vulnerability Discovery
        │
        ▼
Exploitation
```

Because of this, many organizations configure detection rules to alert whenever scanning behavior is observed.

---

# Blue Team Perspective

For defenders, the investigation extends beyond identifying the activity itself.

SOC analysts also verify:

- Was the scan authorized?
- Which system performed the scan?
- Was the activity scheduled?
- Does the source belong to an approved security tool?
- Did the scan lead to any follow-up malicious activity?

Only after answering these questions can an alert be classified as either **malicious** or **benign**.

---

# Skills Gained

By completing this exercise, I learned how to:

- Perform Level 1 SOC alert triage.
- Navigate a SIEM investigation workflow.
- Apply the **5 Ws** methodology.
- Interpret security logs.
- Distinguish legitimate administrative activity from malicious behavior.
- Make evidence-based security decisions rather than relying on assumptions.

---

**Next:** In Part 5, we'll explore the security concepts behind this exercise, including how port scanning is detected, common SOC false positives, Detection Engineering concepts, Red Team vs Blue Team perspectives, and real-world industry practices.

---

# Deep Dive and Pentester Notes

Although this room focuses on defensive security, understanding how SOC analysts investigate alerts is equally valuable for penetration testers.

Modern cybersecurity is no longer divided strictly into offensive and defensive roles. Successful security professionals understand both how attacks are performed and how they are detected.

This section connects the concepts learned in this room with real-world security operations.

---

# Understanding Port Scanning

Port scanning is the process of identifying open ports and services running on a target system.

Attackers perform port scanning during the **Reconnaissance** phase to gather information before attempting exploitation.

For example:

```text
Target Host

22   SSH
80   HTTP
135  RPC
139  NetBIOS
445  SMB
3389 RDP
```

Each open port reveals information about services that may contain vulnerabilities.

Common port scanning tools include:

- Nmap
- Masscan
- RustScan
- Nessus (during vulnerability assessments)

Although attackers frequently perform port scans, security teams also use them as part of routine vulnerability management.

This makes context extremely important during investigations.

---

# Port Scanning in the Cyber Kill Chain

Port scanning usually occurs very early in an attack.

```text
Reconnaissance
        │
        ▼
Port Scanning
        │
        ▼
Service Enumeration
        │
        ▼
Vulnerability Discovery
        │
        ▼
Exploitation
        │
        ▼
Privilege Escalation
        │
        ▼
Persistence
```

Detecting attackers during reconnaissance gives defenders the opportunity to stop attacks before exploitation occurs.

---

# How SOC Detects Port Scanning

SOC teams rarely identify port scanning manually.

Instead, detection rules automatically analyze network traffic.

A simplified detection rule might look like this:

```text
IF

One Source IP

Attempts Connections

To Multiple Ports

Within Short Time

THEN

Generate Port Scan Alert
```

Modern SIEM platforms may also correlate:

- Firewall logs
- IDS alerts
- Endpoint telemetry
- Network flow data

to increase confidence before generating an alert.

---

# Legitimate vs Malicious Port Scanning

One of the biggest lessons from this room is that **port scanning is not always malicious**.

## Legitimate Examples

- Internal vulnerability assessments
- Scheduled Nessus scans
- Compliance auditing
- Infrastructure inventory
- Network troubleshooting

---

## Malicious Examples

- External reconnaissance
- Target identification
- Service discovery
- Attack preparation
- Unauthorized internal reconnaissance

The activity itself may look identical.

The difference lies in the surrounding context.

---

# False Positives

One of the biggest challenges faced by SOC analysts is dealing with **False Positives**.

A False Positive occurs when a security tool incorrectly classifies legitimate activity as malicious.

Example:

```text
Scheduled Nessus Scan

↓

Port Scan Alert

↓

SOC Investigation

↓

Benign Activity
```

Although the alert was technically correct, it did not represent an actual security incident.

Too many False Positives lead to:

- Analyst fatigue
- Wasted investigation time
- Increased operational costs
- Missed real attacks

---

# False Negatives

False Negatives are even more dangerous.

A False Negative occurs when malicious activity is not detected.

Example:

```text
Attacker Performs Port Scan

↓

No Alert Generated

↓

Attack Continues

↓

Compromise
```

SOC teams constantly tune detection rules to reduce both False Positives and False Negatives.

---

# Detection Engineering Perspective

Detection Engineers continuously improve detection capabilities.

Typical responsibilities include:

- Creating new detection rules
- Improving existing detections
- Reducing False Positives
- Increasing detection accuracy
- Mapping detections to MITRE ATT&CK techniques

Detection engineering is an ongoing process because attacker techniques continuously evolve.

---

# Threat Hunting Perspective

Unlike SOC Level 1 analysts, Threat Hunters do not simply wait for alerts.

Instead, they proactively search for hidden threats.

Example hypothesis:

> "Has anyone performed network scanning during the past seven days?"

The Threat Hunter may query:

- Firewall logs
- IDS alerts
- Endpoint telemetry
- Network flows

to identify suspicious reconnaissance activity that automated rules failed to detect.

---

# Red Team Perspective

From a penetration tester's perspective, understanding SOC workflows is extremely valuable.

Every offensive action leaves evidence.

Examples include:

| Red Team Activity | Possible Detection |
|-------------------|-------------------|
| Port Scan | Firewall, IDS, SIEM |
| Brute Force | Authentication Logs |
| PowerShell | EDR |
| Reverse Shell | EDR, Firewall |
| SQL Injection | Web Server Logs |
| Privilege Escalation | Windows Event Logs |
| Lateral Movement | Authentication Logs, EDR |

Successful Red Teams attempt to reduce their detection footprint while accomplishing their objectives.

---

# Blue Team Perspective

Blue Teams approach investigations differently.

Instead of asking:

> "Can this system be compromised?"

They ask:

- What happened?
- Who performed the activity?
- When did it occur?
- Why did it happen?
- Is it expected?
- What evidence supports the conclusion?

Evidence—not assumptions—drives every decision.

---

# Common SOC Mistakes

New analysts often make similar mistakes during investigations.

Examples include:

- Assuming every alert is malicious.
- Ignoring business context.
- Escalating alerts without sufficient evidence.
- Failing to verify authorized maintenance activities.
- Overlooking historical logs.
- Closing alerts too quickly.

Following structured investigation procedures helps avoid these errors.

---

# Industry Relevance

SOC Fundamentals introduces concepts used daily by cybersecurity professionals.

The knowledge gained from this room directly applies to roles such as:

- SOC Analyst
- Security Analyst
- Incident Responder
- Detection Engineer
- Threat Hunter
- Security Engineer
- Blue Team Operator

Most organizations operating a Security Operations Center rely on these workflows regardless of the SIEM platform they use.

---

# Skills Gained

After completing this room, I strengthened my understanding of:

- Security Operations Center (SOC) fundamentals
- Detection vs Response
- The three pillars of SOC
- Alert triage
- The 5 Ws investigation methodology
- SIEM fundamentals
- Security alert analysis
- Port scan investigations
- SOC Level 1 responsibilities

---

# Knowledge Gaps

Although this room establishes a strong foundation, several advanced topics remain for future study.

These include:

- Windows Event Logs
- Sysmon
- Linux Logging
- Splunk
- Microsoft Sentinel
- Elastic Stack
- Detection Engineering
- Sigma Rules
- Threat Intelligence
- Threat Hunting
- Incident Response
- Digital Forensics
- Malware Analysis

These topics build directly upon the concepts introduced in this room.

---

# Interview Questions

### What is the primary goal of a Security Operations Center?

To continuously monitor, detect, investigate, and respond to cybersecurity threats while minimizing business impact.

---

### What are the three pillars of a SOC?

- People
- Process
- Technology

---

### What is the difference between Detection and Response?

Detection identifies suspicious activities, while Response contains, eradicates, and recovers from confirmed security incidents.

---

### What are the 5 Ws used during alert triage?

- What
- When
- Where
- Who
- Why

---

### Does every alert indicate an attack?

No.

Many alerts represent legitimate administrative activities, scheduled maintenance, or expected system behavior.

SOC analysts investigate alerts before determining whether they are malicious.

---

### What is the difference between SIEM and EDR?

SIEM primarily focuses on collecting logs, correlating events, and generating alerts across the environment.

EDR focuses on endpoint visibility and provides both detection and response capabilities directly on endpoint devices.

---

# Key Takeaways

- Port scanning is a reconnaissance technique, not necessarily an attack.
- Context is critical when investigating security alerts.
- Every alert should be validated before escalation.
- False Positives and False Negatives are major challenges in SOC operations.
- Effective security monitoring requires the combination of skilled analysts, standardized processes, and modern security technologies.
- Understanding both offensive and defensive perspectives improves overall cybersecurity knowledge.

---

# Conclusion

The **SOC Fundamentals** room provides an excellent introduction to the operational side of defensive cybersecurity. Rather than focusing on offensive techniques such as exploitation or privilege escalation, this room demonstrates how organizations detect, investigate, and respond to security events in real-world environments.

Throughout this room, we explored the responsibilities of a Security Operations Center (SOC), the importance of continuous monitoring, and the three pillars that enable an effective security operation: **People, Process, and Technology**.

By completing the practical exercise, we also experienced the day-to-day responsibilities of a **SOC Level 1 Analyst**, investigating a security alert through a structured alert triage process using the **5 Ws** methodology.

This room serves as a strong foundation for anyone beginning a Blue Team learning path.

---

# Key Lessons Learned

Throughout this room, the following concepts were introduced:

- The purpose and responsibilities of a Security Operations Center.
- The difference between **Detection** and **Response**.
- The importance of continuous security monitoring.
- The three pillars of a mature SOC:
  - People
  - Process
  - Technology
- The responsibilities of different SOC roles.
- The alert triage process.
- The **5 Ws** investigation methodology.
- The purpose of SIEM, EDR, and other security technologies.
- Basic incident investigation using a SIEM platform.

---

# Skills Gained

By completing this room, I gained practical knowledge of:

- Security Operations Center fundamentals
- SOC organizational structure
- Security monitoring concepts
- Detection vs Response
- Alert triage
- Security investigations
- The 5 Ws methodology
- SIEM fundamentals
- Endpoint Detection and Response (EDR)
- Firewall fundamentals
- Security event analysis
- SOC Level 1 Analyst responsibilities

---

# Commands and Tools Covered

Although this room is primarily conceptual, several important security technologies were introduced.

| Technology | Purpose |
|------------|---------|
| SIEM | Centralized log collection, event correlation, and alert generation. |
| EDR | Endpoint monitoring, detection, and response. |
| Firewall | Filters inbound and outbound network traffic based on security policies. |
| IDS | Detects suspicious network activity. |
| IPS | Detects and blocks malicious network traffic. |
| XDR | Extends detection and response across multiple security domains. |
| SOAR | Automates repetitive security operations and incident response workflows. |
| Nessus | Performs vulnerability assessments and authorized network scanning. |

---

# Real-World Applications

The concepts introduced in this room are widely used across enterprise environments.

Typical use cases include:

- Monitoring enterprise infrastructure
- Investigating suspicious alerts
- Detecting malware infections
- Monitoring authentication events
- Responding to security incidents
- Performing vulnerability assessments
- Supporting compliance requirements
- Threat hunting activities

These workflows are common regardless of whether an organization uses Splunk, Microsoft Sentinel, Elastic Security, QRadar, or other SIEM platforms.

---

# Career Relevance

SOC Fundamentals introduces concepts required for several cybersecurity roles, including:

- SOC Analyst (Tier 1–3)
- Security Operations Analyst
- Incident Responder
- Threat Hunter
- Detection Engineer
- Security Engineer
- Blue Team Operator

For aspiring cybersecurity professionals, understanding SOC operations is an essential first step before learning more advanced defensive technologies.

---

# Future Learning Path

This room lays the groundwork for more advanced Blue Team topics.

Recommended progression:

```text
SOC Fundamentals
        │
        ▼
Windows Event Logs
        │
        ▼
Sysmon
        │
        ▼
SIEM Platforms
(Splunk / Elastic / Sentinel)
        │
        ▼
Detection Engineering
        │
        ▼
Threat Intelligence
        │
        ▼
Threat Hunting
        │
        ▼
Incident Response
        │
        ▼
Digital Forensics
        │
        ▼
Malware Analysis
```

Each of these topics expands upon the concepts introduced in this room.

---

# Personal Reflection

This room reinforced an important lesson:

> **Not every alert represents an attack.**

One of the most valuable takeaways was learning how SOC analysts make evidence-based decisions rather than relying on assumptions.

The practical investigation demonstrated that even activities commonly associated with attackers—such as port scanning—may actually be legitimate administrative actions when proper business context is considered.

This highlights why security analysis is not simply about detecting suspicious activity, but about understanding the surrounding context before escalating or responding to an alert.

---

# References

## Official Resources

- TryHackMe — **SOC Fundamentals**
- Microsoft Learn — Security Operations
- MITRE ATT&CK Framework
- NIST Cybersecurity Framework (CSF)
- NIST SP 800-61 Revision 2 — Computer Security Incident Handling Guide

---

# Tags

```text
SOC
Blue Team
Security Operations Center
SOC Analyst
Detection
Response
Alert Triage
SIEM
EDR
Firewall
IDS
IPS
Threat Hunting
Incident Response
Digital Forensics
TryHackMe
Cyber Security
Blue Team Fundamentals
```

---

# Final Thoughts

SOC Fundamentals marks an important transition from learning **how attacks are performed** to understanding **how they are detected and investigated**.

For offensive security practitioners, this knowledge provides valuable insight into how security teams observe attacker behavior.

For aspiring Blue Team professionals, this room establishes the core concepts that will be expanded upon in future topics such as SIEM, Windows Event Logs, Detection Engineering, Threat Hunting, Incident Response, and Digital Forensics.

Mastering these fundamentals creates a strong foundation for developing practical defensive security skills and understanding how modern Security Operations Centers protect enterprise environments.