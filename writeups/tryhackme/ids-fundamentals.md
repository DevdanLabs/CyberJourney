# IDS Fundamentals

## Executive Summary

Intrusion Detection Systems (IDS) are an essential component of modern cybersecurity defense strategies. While firewalls act as the first line of defense by filtering incoming and outgoing network traffic, they cannot detect every malicious activity that occurs after a connection has been established. IDS solutions fill this gap by continuously monitoring network or host activities, identifying suspicious behavior, and generating alerts for security teams.

In this TryHackMe room, I learned the fundamental concepts of Intrusion Detection Systems, including their purpose, deployment models, detection methodologies, and real-world applications. The room introduced both **Host-based Intrusion Detection Systems (HIDS)** and **Network-based Intrusion Detection Systems (NIDS)**, as well as the differences between **signature-based**, **anomaly-based**, and **hybrid** detection techniques.

The practical section focused on **Snort**, one of the most popular open-source Network Intrusion Detection Systems. I explored Snort's architecture, operational modes, directory structure, configuration files, and rule syntax. Additionally, I created a custom Snort rule, tested it against live traffic, and analyzed a packet capture (PCAP) file to investigate simulated malicious network activity.

By completing this room, I gained practical experience in writing detection rules, analyzing captured network traffic, understanding how IDS engines generate alerts, and appreciating the role of IDS within Security Operations Centers (SOC), Incident Response (IR), and Digital Forensics investigations.

---

# Learning Objectives

After completing this room, I was able to:

- Understand the purpose of Intrusion Detection Systems (IDS).
- Explain why IDS is necessary even when firewalls are deployed.
- Differentiate between HIDS and NIDS deployment models.
- Compare signature-based, anomaly-based, and hybrid detection techniques.
- Understand how Snort operates as an Intrusion Detection System.
- Explore Snort's configuration files and directory structure.
- Understand the syntax and components of Snort detection rules.
- Create and test custom Snort detection rules.
- Run Snort against live network traffic.
- Analyze historical PCAP files using Snort.
- Investigate IDS alerts generated from captured network traffic.
- Understand how IDS integrates into enterprise SOC environments.

---

# Prerequisites

Before starting this room, it is recommended to understand the following topics:

- Basic Networking Concepts
- TCP/IP Fundamentals
- Common Network Protocols (TCP, UDP, ICMP)
- Firewall Fundamentals
- Linux Command Line Basics
- Log Analysis Fundamentals
- Basic Security Operations Center (SOC) concepts

Recommended TryHackMe rooms:

- Network Concepts
- Firewall Fundamentals
- Logs Fundamentals
- SOC Fundamentals
- SIEM Fundamentals

---

# Room Information

| Information | Value |
|------------|-------|
| Platform | TryHackMe |
| Room | IDS Fundamentals |
| Category | Security Operations (SOC) |
| Difficulty | Easy |
| Focus | Intrusion Detection Systems (IDS) |
| Primary Tool | Snort |
| Detection Types | Signature-Based, Anomaly-Based, Hybrid |
| Deployment Types | HIDS, NIDS |

---

# What is an Intrusion Detection System (IDS)?

An **Intrusion Detection System (IDS)** is a security solution designed to monitor network traffic or host activities for malicious behavior, policy violations, or suspicious events. Unlike firewalls, which primarily control whether traffic is allowed or denied, an IDS continuously observes activities after communication has already been established.

The primary purpose of an IDS is **detection**, not prevention. When suspicious activity is identified, the IDS generates alerts that enable security analysts to investigate potential threats before they escalate into successful attacks.

Rather than blocking traffic directly, IDS solutions provide visibility into ongoing network activity, making them an essential component of enterprise monitoring and incident response.

---

# Why Do We Need an IDS?

Firewalls provide preventive security by filtering incoming and outgoing traffic according to predefined rules. However, attackers can still bypass firewalls using legitimate services, stolen credentials, compromised systems, or encrypted communication channels.

For example:

```text
Internet
     │
     ▼
 Firewall
     │
     ▼
 Internal Network
```

A firewall may allow HTTPS traffic because it is considered legitimate. If malware communicates with its command-and-control (C2) server over HTTPS, the firewall may not recognize the traffic as malicious.

Once an attacker gains access to the internal network, they may begin:

- Port scanning
- Lateral movement
- Privilege escalation
- Brute-force attacks
- Data exfiltration
- Malware deployment

These activities often occur after the firewall has already permitted the initial connection.

This is where an IDS becomes essential.

An IDS continuously monitors internal activities and generates alerts whenever suspicious behavior matches predefined detection rules or deviates from normal behavior.

---

# Firewall vs Intrusion Detection System

Although firewalls and IDS solutions both contribute to network security, they serve different purposes.

| Firewall | IDS |
|----------|-----|
| Prevents unauthorized access | Detects suspicious activities |
| Blocks or allows traffic | Generates alerts |
| Operates before a connection is established | Monitors activities after communication begins |
| Primarily protects network boundaries | Monitors internal network or host activities |
| Preventive security control | Detective security control |

These technologies complement each other rather than replace one another.

A typical enterprise deployment looks like this:

```text
                 Internet
                     │
                     ▼
               +------------+
               | Firewall   |
               +------------+
                     │
                     ▼
          +----------------------+
          | Internal Network     |
          +----------------------+
             │              │
             ▼              ▼
         Client          Server
             │              │
             └──────┬───────┘
                    ▼
                 IDS Sensor
                    │
                    ▼
              Security Alerts
                    │
                    ▼
               SIEM / SOC Team
```

The firewall controls which traffic may enter or leave the network, while the IDS continuously monitors internal communications and reports suspicious activities to the Security Operations Center (SOC) for investigation.

---

# Key Terminology

| Term | Description |
|------|-------------|
| IDS | Intrusion Detection System that monitors systems or networks for malicious activity. |
| Detection | The process of identifying suspicious or malicious behavior. |
| Alert | A notification generated when traffic matches a detection rule or anomaly. |
| Signature | A known attack pattern stored in the IDS database. |
| Baseline | The normal behavior profile used by anomaly-based detection systems. |
| HIDS | Host-based Intrusion Detection System that monitors individual hosts. |
| NIDS | Network-based Intrusion Detection System that monitors network traffic. |
| Snort | An open-source Network Intrusion Detection System (NIDS). |
| PCAP | Packet Capture file containing recorded network traffic for analysis. |

---

# Why This Topic Matters

Intrusion Detection Systems are widely deployed across enterprise environments, government organizations, cloud infrastructures, and Security Operations Centers (SOC). Modern security teams rely on IDS solutions to identify malicious activities that bypass preventive controls such as firewalls.

Understanding IDS technology is essential for careers in:

- SOC Analyst
- Blue Team Operations
- Incident Response
- Digital Forensics
- Network Security
- Security Engineering
- Threat Hunting

Learning how IDS solutions detect attacks also helps penetration testers understand how their activities are monitored, enabling them to evaluate defensive capabilities during security assessments.

# Types of Intrusion Detection Systems

Intrusion Detection Systems can be classified in two different ways:

1. **Deployment Mode** — Where the IDS is installed.
2. **Detection Method** — How the IDS identifies malicious activity.

Understanding both classifications helps security professionals choose the appropriate IDS solution based on the organization's infrastructure and security requirements.

```text
                    Intrusion Detection System
                              │
              ┌───────────────┴───────────────┐
              │                               │
      Deployment Mode                 Detection Method
              │                               │
      ┌───────┴────────┐         ┌────────────┼────────────┐
      │                │         │            │            │
     HIDS             NIDS   Signature    Anomaly      Hybrid
```

---

# Deployment Modes

Deployment mode describes **where the IDS is installed** and **what it monitors**.

There are two primary deployment models:

- Host-based Intrusion Detection System (HIDS)
- Network-based Intrusion Detection System (NIDS)

---

# Host-based Intrusion Detection System (HIDS)

A **Host-based Intrusion Detection System (HIDS)** is installed directly on an individual host such as a workstation, server, or endpoint.

Instead of monitoring the entire network, HIDS focuses exclusively on the activities occurring within its own operating system.

## How HIDS Works

HIDS continuously monitors system-level events, including:

- User logins
- Running processes
- File modifications
- Registry changes (Windows)
- Installed services
- Privilege escalation attempts
- System logs
- Malware activity

Because it operates inside the operating system, HIDS provides deep visibility into the behavior of a specific machine.

```text
           +-------------------+
           |      Server       |
           |-------------------|
           |      HIDS         |
           +-------------------+

           Detects:

           • File changes
           • New processes
           • Registry modifications
           • User activity
           • Privilege escalation
```

## Advantages

- Provides detailed host-level visibility.
- Detects file integrity violations.
- Monitors user activities.
- Detects privilege escalation attempts.
- Useful for protecting critical servers.

## Limitations

- Must be installed on every host.
- Consumes CPU and memory resources.
- More difficult to manage in large environments.
- Cannot monitor other hosts.

---

# Network-based Intrusion Detection System (NIDS)

A **Network-based Intrusion Detection System (NIDS)** monitors traffic flowing across an entire network instead of focusing on individual machines.

Rather than being installed on every endpoint, NIDS is typically deployed at strategic locations such as:

- Network gateways
- Core switches
- Data center switches
- Network taps
- SPAN (Mirror) ports

## How NIDS Works

NIDS analyzes packets moving through the network in real time.

It inspects protocols such as:

- HTTP
- HTTPS
- DNS
- FTP
- SSH
- SMB
- ICMP
- TCP
- UDP

Whenever network traffic matches a detection rule or appears abnormal, the IDS generates an alert.

```text
                Internet
                    │
                Firewall
                    │
                    ▼
            +----------------+
            |    Switch      |
            +----------------+
              │          │
              │          │
          Workstation   Server
              │          │
              └────┬─────┘
                   ▼
                 NIDS
                   │
             Security Alerts
```

## Advantages

- Monitors multiple devices simultaneously.
- Centralized deployment.
- Easier management.
- Detects network attacks.
- Ideal for enterprise environments.

## Limitations

- Cannot detect host-only activities.
- Encrypted traffic may reduce visibility.
- High-speed networks require powerful hardware.

---

# HIDS vs NIDS

| Feature | HIDS | NIDS |
|---------|------|------|
| Installation | Individual host | Network sensor |
| Visibility | Host activities | Network traffic |
| Coverage | Single machine | Entire network segment |
| Detects file changes | Yes | No |
| Detects process execution | Yes | No |
| Detects network attacks | Limited | Yes |
| Scalability | Lower | Higher |
| Management | Per endpoint | Centralized |

---

# Detection Methods

Deployment explains **where** the IDS operates.

Detection methods explain **how** the IDS determines whether an activity is malicious.

There are three major detection techniques:

- Signature-Based Detection
- Anomaly-Based Detection
- Hybrid Detection

---

# Signature-Based Detection

Signature-based IDS compares network traffic against a database of known attack signatures.

A **signature** is a unique pattern associated with a previously identified attack.

If incoming traffic matches one of these signatures, an alert is generated immediately.

```text
Incoming Packet
       │
       ▼
Compare Against
Signature Database
       │
       ▼
 Match?
   │
 ┌─┴─┐
 │   │
No  Yes
 │   │
Ignore Alert
```

Examples of attacks detected using signatures include:

- SQL Injection
- Port Scanning
- Known Malware
- SMB Exploits
- FTP Attacks
- SSH Brute Force
- Web Exploits

## Advantages

- Fast detection.
- High accuracy.
- Low false positive rate.
- Efficient resource usage.

## Limitations

- Cannot detect unknown attacks.
- Requires frequent signature updates.
- Ineffective against many zero-day attacks.

---

# Zero-Day Attacks

A **Zero-Day Attack** exploits a vulnerability that has not yet been publicly disclosed or patched.

Because no attack signature exists yet, traditional signature-based IDS solutions often fail to detect these threats.

```text
Unknown Attack
      │
No Existing Signature
      │
      ▼
No Detection
```

This limitation is one of the main reasons why anomaly-based detection was developed.

---

# Anomaly-Based Detection

Unlike signature-based systems, anomaly-based IDS does not rely on known attack patterns.

Instead, it learns what "normal" activity looks like by establishing a **baseline**.

Any significant deviation from this baseline is treated as suspicious.

```text
Normal Behavior
       │
Create Baseline
       │
       ▼
Current Activity
       │
Compare
       │
       ▼
Unusual?
   │
 ┌─┴─┐
 │   │
No  Yes
 │   │
Normal Alert
```

Examples include:

- Sudden traffic spikes
- Large data transfers
- Login attempts from unusual locations
- Unexpected protocol usage
- Unusual user behavior

## Advantages

- Can detect unknown attacks.
- Effective against zero-day threats.
- Identifies abnormal system behavior.
- Useful for insider threat detection.

## Limitations

- Higher false positive rate.
- Requires baseline training.
- More computationally intensive.
- Requires continuous tuning.

---

# False Positives and False Negatives

One important concept in intrusion detection is understanding detection accuracy.

## False Positive

A legitimate activity is incorrectly classified as malicious.

Example:

- A scheduled backup transfers several terabytes of data.
- The IDS incorrectly identifies it as data exfiltration.

```text
Legitimate Activity
        │
        ▼
Incorrect Alert
```

---

## False Negative

A real attack occurs but is not detected.

Example:

- A new malware variant bypasses existing detection rules.

```text
Real Attack
      │
      ▼
No Alert Generated
```

False negatives are generally considered more dangerous because attacks remain undetected.

---

# Hybrid IDS

A Hybrid IDS combines both signature-based and anomaly-based detection techniques.

It first attempts to match known attack signatures.

If no signature exists, it evaluates whether the observed behavior deviates significantly from the established baseline.

```text
Incoming Traffic
        │
        ▼
Signature Match?
    │
 ┌──┴───┐
 │      │
Yes     No
 │       │
Alert  Baseline Analysis
           │
           ▼
     Abnormal?
       │
   ┌───┴────┐
   │        │
  No      Yes
   │        │
Ignore   Alert
```

## Advantages

- Detects both known and unknown attacks.
- Improves detection coverage.
- Reduces blind spots.
- Better suited for modern threat landscapes.

## Limitations

- More complex to configure.
- Higher resource requirements.
- Requires ongoing maintenance and tuning.

---

# Comparison of Detection Methods

| Feature | Signature | Anomaly | Hybrid |
|---------|-----------|----------|---------|
| Detects Known Attacks | Yes | Yes | Yes |
| Detects Zero-Day Attacks | No | Yes | Yes |
| Detection Speed | Very Fast | Moderate | Moderate |
| False Positive Rate | Low | High | Medium |
| Requires Signature Updates | Yes | No | Yes |
| Requires Baseline Training | No | Yes | Yes |

---

# IDS in Enterprise Environments

Most enterprise organizations combine multiple IDS technologies to achieve layered security.

A common deployment includes:

- NIDS monitoring network traffic.
- HIDS protecting critical servers.
- Signature-based detection for known threats.
- Anomaly detection for identifying unknown attacks.
- SIEM collecting alerts from all IDS sensors.

```text
                  Internet
                      │
                  Firewall
                      │
             ┌────────┴────────┐
             │                 │
          NIDS Sensor      Network Switch
                                │
                   ┌────────────┴────────────┐
                   │                         │
               Server (HIDS)          Workstation
                   │                         │
                   └────────────┬────────────┘
                                ▼
                              SIEM
                                │
                                ▼
                           SOC Analysts
```

This layered architecture significantly improves an organization's ability to detect attacks at multiple stages of the cyber kill chain.

---

# Blue Team Perspective

For defenders, IDS provides continuous visibility into network and host activities. Security analysts rely on IDS alerts to detect reconnaissance, lateral movement, brute-force attacks, malware communication, and data exfiltration attempts before they become large-scale incidents.

However, IDS alerts should always be validated through additional evidence such as firewall logs, endpoint logs, authentication records, and SIEM correlation. An alert is an indicator of suspicious activity—not definitive proof of compromise.

---

# Red Team Perspective

Understanding IDS technologies is equally valuable for penetration testers.

Many offensive activities—including port scanning, vulnerability scanning, brute-force attacks, and exploit attempts—can trigger IDS alerts if they match existing detection rules or exhibit abnormal behavior.

Knowledge of IDS detection mechanisms helps Red Team operators evaluate defensive visibility, test detection capabilities during security assessments, and better understand how their activities may appear from the defender's perspective.

# Snort IDS

## Introduction to Snort

**Snort** is one of the world's most widely used **open-source Network Intrusion Detection Systems (NIDS)**. It was originally developed by **Martin Roesch** in **1998** and has become one of the most popular IDS solutions used by organizations, educational institutions, and security professionals.

Snort analyzes network traffic in real time and compares packets against a collection of predefined detection rules. Whenever network traffic matches one of these rules, Snort generates an alert for security analysts.

Over the years, Snort has evolved from a simple packet sniffer into a powerful intrusion detection engine capable of:

- Monitoring live network traffic
- Detecting known attacks
- Logging suspicious traffic
- Performing offline analysis of PCAP files
- Supporting custom detection rules
- Integrating with SIEM platforms

Although newer IDS solutions such as **Suricata** and **Zeek** are widely adopted today, Snort remains one of the best tools for learning IDS concepts because of its simplicity, flexibility, and extensive community support.

---

# How Snort Works

At a high level, Snort follows a straightforward workflow.

```text
             Network Traffic
                    │
                    ▼
             Packet Decoder
                    │
                    ▼
              Preprocessors
                    │
                    ▼
            Detection Engine
                    │
                    ▼
             Snort Rule Files
                    │
                    ▼
              Alert / Logging
```

Every packet flowing through the monitored interface passes through several processing stages before Snort decides whether it represents malicious activity.

---

# Snort Architecture

## 1. Packet Decoder

The Packet Decoder is responsible for reading incoming packets and identifying their protocol headers.

It extracts information such as:

- Ethernet Header
- IP Header
- TCP Header
- UDP Header
- ICMP Header
- Payload

Without decoding packets correctly, Snort would not understand what information is contained inside the network traffic.

---

## 2. Preprocessors

Before applying detection rules, Snort preprocesses packets.

Preprocessors normalize traffic and prepare packets for inspection.

Common preprocessing tasks include:

- IP Fragment Reassembly
- TCP Stream Reassembly
- HTTP Normalization
- Protocol Validation
- Traffic Normalization

For example, an attacker may fragment a malicious payload into multiple packets to evade detection.

Without preprocessing:

```text
Packet A

Packet B

Packet C
```

Nothing appears suspicious.

After reassembly:

```text
Malicious Payload
```

The complete payload can now be inspected by the detection engine.

---

## 3. Detection Engine

The Detection Engine is the core component of Snort.

Its job is to compare every packet against all enabled detection rules.

```text
Incoming Packet
       │
       ▼
Rule 1
 │
No Match
 │
 ▼
Rule 2
 │
Match
 │
 ▼
Generate Alert
```

If no rules match the traffic, Snort simply ignores the packet and continues processing the next one.

---

## 4. Output Modules

Once a rule matches, Snort produces output.

Depending on the configuration, Snort can:

- Display alerts on the console
- Save alerts into log files
- Export packet captures (PCAP)
- Send logs to Syslog
- Forward alerts to SIEM platforms

These alerts allow SOC analysts to investigate suspicious activity.

---

# Snort Directory Structure

After installation, Snort stores its configuration files and rule files inside its installation directory.

In this room, Snort is installed under:

```text
/etc/snort
```

Listing the directory:

```bash
ls /etc/snort
```

Example output:

```text
classification.config
community-sid-msg.map
gen-msg.map
reference.config
rules/
snort.conf
snort.lua
threshold.conf
unicode.map
```

---

# Important Snort Files

## snort.lua

The primary configuration file in Snort 3.

This file defines:

- Network variables
- Enabled rule files
- Detection settings
- Logging configuration
- Interface settings
- Output options

Think of this as the "main configuration file" that controls how Snort operates.

---

## rules/

This directory contains Snort detection rules.

Examples include:

```text
rules/

├── local.rules
├── malware.rules
├── ftp.rules
├── dns.rules
├── web.rules
```

These rule files tell Snort exactly what network activity should generate alerts.

---

## local.rules

This is the most important rule file for administrators.

Instead of modifying built-in rules, administrators usually create custom detection rules inside:

```text
rules/local.rules
```

This approach makes upgrades and maintenance much easier.

---

## classification.config

Defines alert classifications.

Examples:

- Attempted Administrator Privilege Gain
- Malware Activity
- Web Application Attack
- Information Leak

These classifications help SOC analysts prioritize alerts.

---

## threshold.conf

Controls alert suppression and thresholding.

Without thresholds, repeated attacks may generate thousands of identical alerts.

Example:

Instead of producing:

```text
1000 Port Scan Alerts
```

Snort can be configured to generate:

```text
1 Port Scan Alert
```

within a specified time window.

This reduces alert fatigue inside Security Operations Centers.

---

# Built-in Rules

One of Snort's greatest strengths is its extensive library of built-in detection rules.

These rules cover many common attacks, including:

- Port Scanning
- SQL Injection
- SMB Exploits
- FTP Attacks
- DNS Attacks
- Malware Communication
- Web Exploitation
- SSH Attacks
- ICMP Abuse

Instead of creating every rule manually, organizations can immediately benefit from thousands of community-maintained signatures.

---

# Custom Rules

Built-in rules cannot detect every organization's unique threats.

For this reason, Snort allows administrators to create their own rules.

Examples include detecting:

- Internal applications
- Sensitive servers
- Unauthorized services
- Company-specific protocols
- Restricted URLs
- Custom malware indicators

Custom rules are typically stored inside:

```text
rules/local.rules
```

This flexibility makes Snort highly adaptable to different environments.

---

# Why Rule Management Matters

Organizations rarely enable every available rule.

Why?

Because unnecessary rules may generate excessive alerts.

For example:

A company uses Dropbox every day.

If Snort contains a rule that flags all Dropbox traffic as suspicious, analysts may receive thousands of alerts that are completely legitimate.

This problem is known as **alert fatigue**.

To reduce unnecessary alerts, administrators often:

- Disable irrelevant rules.
- Modify existing rules.
- Create custom rules.
- Tune alert thresholds.

Proper rule management significantly improves IDS effectiveness.

---

# Operating Modes of Snort

Snort can operate in three different modes depending on the organization's requirements.

```text
                    Snort
                      │
        ┌─────────────┼─────────────┐
        │             │             │
 Packet Sniffer   Packet Logger   NIDS Mode
```

Each mode serves a different purpose.

---

# 1. Packet Sniffer Mode

Packet Sniffer Mode captures and displays network packets without performing intrusion detection.

```text
Network Traffic
        │
        ▼
      Snort
        │
        ▼
 Display Packets
```

Characteristics:

- Reads packets
- Displays packet information
- No intrusion detection
- No alert generation

Typical use cases include:

- Network troubleshooting
- Protocol analysis
- Connectivity testing
- Traffic observation

This mode is similar to tools such as **tcpdump** or **Wireshark** when simply capturing live traffic.

---

# 2. Packet Logging Mode

Packet Logging Mode captures network traffic and stores it for later analysis.

```text
Network Traffic
        │
        ▼
      Snort
        │
        ▼
    Save as PCAP
```

Instead of displaying packets only, Snort records traffic into log files or PCAP files.

This mode is especially useful during:

- Digital Forensics
- Incident Response
- Malware Investigations
- Root Cause Analysis

Investigators can replay captured traffic long after an incident has occurred.

---

# 3. Network Intrusion Detection System (NIDS) Mode

NIDS Mode is Snort's primary operating mode.

```text
Network Traffic
        │
        ▼
      Snort
        │
        ▼
 Compare Against Rules
        │
        ▼
Alert Generated
```

This mode continuously monitors live traffic and compares packets against enabled rule files.

Whenever traffic matches a rule, Snort immediately generates an alert.

Typical detections include:

- Port Scanning
- SSH Connections
- SQL Injection
- ICMP Activity
- Malware Communication
- Web Attacks
- Brute Force Attempts

This is the mode most commonly deployed in enterprise environments.

---

# Comparison of Snort Modes

| Mode | Detects Attacks | Generates Alerts | Saves Traffic | Primary Purpose |
|------|-----------------|-----------------|--------------|----------------|
| Packet Sniffer | No | No | Optional | Network monitoring and troubleshooting |
| Packet Logging | Limited | Optional | Yes | Packet capture for forensic analysis |
| NIDS Mode | Yes | Yes | Optional | Real-time intrusion detection |

---

# Enterprise Use Case

A typical enterprise deployment looks like this:

```text
                 Internet
                     │
                     ▼
                 Firewall
                     │
                     ▼
               Core Switch
                     │
        ┌────────────┴────────────┐
        │                         │
    Internal Hosts           Mirror Port
                                      │
                                      ▼
                                   Snort
                                      │
                                      ▼
                              Detection Rules
                                      │
                                      ▼
                                  Security Alert
                                      │
                                      ▼
                                   SIEM
                                      │
                                      ▼
                                SOC Analysts
```

Traffic flowing through the network is mirrored to the Snort sensor. Snort analyzes the packets in real time and sends alerts to the organization's SIEM platform, where SOC analysts investigate suspicious activity.

---

# Blue Team Perspective

Snort is widely used by Blue Teams because it provides continuous visibility into network activity. By combining built-in and custom rules, defenders can quickly detect reconnaissance, exploitation attempts, malware communication, brute-force attacks, and other suspicious behaviors.

Proper rule tuning, alert prioritization, and integration with SIEM platforms allow security teams to reduce false positives and respond more efficiently to incidents.

---

# Red Team Perspective

Understanding how Snort works helps penetration testers appreciate how their actions are monitored. Common activities such as network scanning, SSH enumeration, web exploitation, and known exploit payloads may trigger Snort alerts if corresponding detection rules exist.

Knowledge of Snort's rule-based detection enables Red Teams to evaluate defensive coverage, validate detection capabilities during engagements, and better understand how offensive actions appear from a defender's perspective.

# Understanding Snort Rules

The core functionality of Snort relies on **rules**. Every packet captured by Snort is compared against these rules to determine whether it matches a known attack pattern or suspicious activity.

If a rule matches the network traffic, Snort generates an alert based on the action defined within the rule.

A simple Snort rule looks like this:

```snort
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)
```

Although it appears complex at first glance, every part of the rule has a specific purpose.

---

# General Rule Structure

A Snort rule consists of two major sections:

1. Rule Header
2. Rule Options (Metadata)

```text
alert icmp any any -> $HOME_NET any
│      │    │   │      │       │
│      │    │   │      │       └── Destination Port
│      │    │   │      └────────── Destination IP
│      │    │   └───────────────── Direction
│      │    └───────────────────── Source Port
│      └────────────────────────── Source IP
└───────────────────────────────── Action + Protocol

(msg:"Ping Detected"; sid:10001; rev:1;)
```

---

# Rule Header Components

## 1. Action

The **Action** tells Snort what to do when the rule matches.

Example:

```snort
alert
```

This instructs Snort to generate an alert.

Common actions include:

| Action | Description |
|---------|-------------|
| alert | Generate an alert when the rule matches. |
| log | Log the matching packet. |
| pass | Ignore matching traffic. |
| drop* | Drop the packet (IPS mode). |

> **Note:** This room focuses on IDS functionality, so `alert` is the primary action used.

---

## 2. Protocol

The protocol field specifies which network protocol should be inspected.

Example:

```snort
icmp
```

Common protocols:

- TCP
- UDP
- ICMP
- IP

Example:

```snort
alert tcp ...
```

This rule would only inspect TCP traffic.

---

## 3. Source IP

Defines where the traffic originates.

Example:

```snort
any
```

Meaning:

```text
Any IP Address
```

It can also be a specific address or subnet.

Examples:

```snort
192.168.1.10
```

```snort
10.10.10.0/24
```

---

## 4. Source Port

Specifies the originating port.

Example:

```snort
any
```

or

```snort
22
80
443
```

---

## 5. Direction Operator

The direction operator defines the packet flow.

Example:

```snort
->
```

Meaning:

```text
Source  --------> Destination
```

Traffic moving in the opposite direction would not match this rule.

---

## 6. Destination IP

Defines the destination address.

Example:

```snort
$HOME_NET
```

`$HOME_NET` is a variable defined inside Snort's configuration file.

For example:

```text
HOME_NET = 192.168.1.0/24
```

Instead of hardcoding IP addresses into every rule, variables make configuration easier to maintain.

---

## 7. Destination Port

Specifies which destination port should trigger the rule.

Example:

```snort
any
```

or

```snort
80
22
3389
```

---

# Rule Metadata (Options)

Everything inside the parentheses represents the rule's metadata.

Example:

```snort
(msg:"Ping Detected"; sid:10001; rev:1;)
```

Metadata provides additional information about the alert.

---

## msg

The **msg** field specifies the alert message displayed when the rule matches.

Example:

```snort
msg:"Ping Detected"
```

Generated alert:

```text
Ping Detected
```

Good messages should clearly describe the activity being detected.

---

## sid (Signature ID)

Every Snort rule requires a unique **Signature ID (SID)**.

Example:

```snort
sid:10001
```

SID allows administrators to uniquely identify each rule.

Benefits include:

- Rule management
- Troubleshooting
- SIEM correlation
- Version tracking

No two custom rules should share the same SID.

---

## rev (Revision)

The revision number tracks modifications to a rule.

Example:

```snort
rev:1
```

If the rule is updated later:

```snort
rev:2
```

This helps administrators maintain version history.

---

# Creating a Custom Rule

In this room, a custom rule was added to the local rule file.

Open the file:

```bash
sudo nano /etc/snort/rules/local.rules
```

Append the following rule:

```snort
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)
```

This rule generates an alert whenever an ICMP packet (Ping) targets the local loopback interface.

Breaking it down:

| Component | Value |
|-----------|-------|
| Action | alert |
| Protocol | icmp |
| Source IP | any |
| Source Port | any |
| Destination IP | 127.0.0.1 |
| Destination Port | any |
| Message | Loopback Ping Detected |
| SID | 10003 |
| Revision | 1 |

---

# Why Use the Loopback Address?

The loopback address (`127.0.0.1`) always refers to the local machine itself.

Advantages:

- Always available
- No external network required
- Safe for testing
- Easy to reproduce

This makes it an excellent target for verifying custom detection rules.

---

# Running Snort on Live Traffic

After creating the rule, Snort can be started to monitor live traffic.

Command:

```bash
sudo snort -q -l /var/log/snort -i lo -A alert_fast -c /etc/snort/snort.lua
```

---

# Command Breakdown

## Purpose

Start Snort in IDS mode using the loopback interface and display alerts in a compact format.

---

## Syntax

```bash
sudo snort [options]
```

---

## Flag Breakdown

| Flag | Description |
|------|-------------|
| sudo | Execute Snort with administrative privileges. |
| -q | Quiet mode (reduces unnecessary console output). |
| -l /var/log/snort | Directory where logs are stored. |
| -i lo | Monitor the loopback interface (`lo`). |
| -A alert_fast | Display alerts using the fast alert format. |
| -c /etc/snort/snort.lua | Load the specified configuration file. |

---

# Expected Workflow

```text
Network Traffic
       │
       ▼
Loopback Interface (lo)
       │
       ▼
Snort
       │
       ▼
Load Rules
       │
       ▼
Traffic Matches?
       │
 ┌─────┴─────┐
 │           │
No          Yes
 │           │
Ignore     Generate Alert
```

---

# Testing the Rule

To trigger the rule, ping the loopback address.

Command:

```bash
ping 127.0.0.1
```

Every ICMP Echo Request sent to the loopback interface should trigger the custom rule.

Example output:

```text
07/24-10:46:52

[**]

Loopback Ping Detected

[**]

127.0.0.1 -> 127.0.0.1
```

Each ping packet produces a corresponding Snort alert, confirming that the rule is functioning correctly.

---

# Running Snort Against a PCAP File

Snort can also analyze previously captured network traffic.

Command:

```bash
sudo snort -q -l /var/log/snort -r Task.pcap -A alert_fast -c /etc/snort/snort.lua
```

---

# Flag Breakdown

| Flag | Description |
|------|-------------|
| -r Task.pcap | Read packets from a PCAP file instead of capturing live traffic. |

Everything else functions the same as live monitoring.

---

# Why Analyze PCAP Files?

PCAP analysis is one of the most valuable features for incident response and digital forensics.

Instead of monitoring traffic as it happens, investigators can replay historical traffic to identify malicious activity.

Typical workflow:

```text
Security Incident
        │
        ▼
Network Traffic Captured
        │
        ▼
Saved as PCAP
        │
        ▼
Snort Analysis
        │
        ▼
Alert Generation
        │
        ▼
Incident Investigation
```

This allows analysts to investigate attacks that occurred days, weeks, or even months earlier, provided packet captures are available.

---

# Blue Team Perspective

Writing custom Snort rules is an essential skill for SOC analysts and detection engineers. While community rules detect many common threats, organizations often create custom rules tailored to their own infrastructure, applications, and threat intelligence.

Proper rule management, regular updates, and careful tuning help reduce false positives while improving detection accuracy.

---

# Red Team Perspective

Understanding Snort rule syntax helps penetration testers recognize what types of traffic are likely to trigger alerts. Activities such as port scanning, ICMP probing, SSH enumeration, or known exploit payloads may easily match existing detection rules.

By understanding how defenders write detection logic, Red Team operators gain insight into defensive visibility and can better evaluate the effectiveness of security monitoring during authorized assessments.

# Practical Lab Walkthrough

## Scenario

In this practical exercise, I acted as a **third-party forensic investigator** hired to analyze a suspected security incident.

The company provided a packet capture (**PCAP**) file named:

```text
Intro_to_IDS.pcap
```

The objective was to analyze the captured network traffic using **Snort** and determine whether any malicious activities were present.

Unlike live monitoring, this investigation focused entirely on **historical network traffic**, simulating a real-world digital forensics or incident response investigation.

---

# Lab Objective

The goals of this exercise were to:

- Analyze a PCAP file using Snort.
- Detect suspicious network activities.
- Identify the source IP responsible for SSH communication.
- Determine which detection rules were triggered.
- Identify the Signature ID (SID) associated with the SSH detection rule.

---

# Step 1 — Navigate to the Snort Directory

The PCAP file provided by the lab is stored inside Snort's installation directory.

Command:

```bash
cd /etc/snort
```

## Purpose

Change the current working directory to the location where the PCAP file is stored.

## Verification

```bash
ls
```

Expected output includes:

```text
Intro_to_IDS.pcap
rules/
snort.lua
snort.conf
...
```

---

# Step 2 — Analyze the PCAP File

Run Snort against the provided PCAP.

Command:

```bash
sudo snort -q -l /var/log/snort -r Intro_to_IDS.pcap -A alert_fast -c /etc/snort/snort.lua
```

---

# Command Breakdown

| Option | Description |
|---------|-------------|
| sudo | Execute Snort with administrative privileges. |
| snort | Launch the Snort IDS engine. |
| -q | Quiet mode (reduces unnecessary console output). |
| -l /var/log/snort | Store logs in the specified directory. |
| -r Intro_to_IDS.pcap | Read packets from the PCAP file instead of capturing live traffic. |
| -A alert_fast | Display alerts using the fast alert format. |
| -c /etc/snort/snort.lua | Load the Snort configuration file. |

---

# What Happens Internally?

After executing the command, Snort processes the PCAP file packet by packet.

```text
Intro_to_IDS.pcap
        │
        ▼
Packet Decoder
        │
        ▼
Preprocessors
        │
        ▼
Detection Engine
        │
        ▼
Snort Rules
        │
        ▼
Matching Alert
```

Every packet inside the capture file is compared against the enabled Snort rule set.

---

# Detection Results

The analysis generated multiple alerts.

Among them were:

- SSH activity detected
- ICMP (Ping) activity detected

These alerts indicate that traffic inside the capture matched existing Snort detection rules.

---

# Question 1

## What is the IP address of the machine that tried to connect to the subject machine using SSH?

### Investigation

Snort generated an alert indicating SSH traffic.

By examining the alert output, the source IP attempting the SSH connection was identified.

### Answer

```text
10.11.90.211
```

---

# Explanation

This IP address represents the system initiating the SSH connection.

It is important to understand that this **does not automatically mean the host is malicious**.

Additional investigation would normally include:

- Authentication logs
- Successful or failed login attempts
- User accounts
- Firewall logs
- Endpoint logs

An IDS alert serves as an indicator that warrants further investigation rather than definitive proof of an attack.

---

# Question 2

## What other rule message besides the SSH message is detected?

### Investigation

In addition to the SSH alert, Snort detected another rule match involving ICMP traffic.

### Answer

```text
Ping Detected
```

---

# Explanation

The "Ping Detected" alert indicates that ICMP Echo Requests were observed during the packet capture.

This activity is often associated with:

- Host discovery
- Connectivity testing
- Network reconnaissance

While ping traffic is not inherently malicious, attackers frequently use ICMP during the reconnaissance phase to identify live hosts before launching further attacks.

---

# Question 3

## What is the SID of the rule that detects SSH?

### Investigation

Each Snort rule contains a unique **Signature ID (SID)**.

The alert references the SID associated with the rule responsible for detecting SSH activity.

### Answer

```text
1000002
```

---

# Why SID Matters

The Signature ID uniquely identifies every Snort rule.

SOC analysts use the SID to:

- Identify the triggered rule.
- Search documentation.
- Update detection logic.
- Correlate alerts inside SIEM platforms.
- Track rule revisions.

Without unique SIDs, managing thousands of detection rules would become extremely difficult.

---

# Lab Results Summary

| Question | Answer |
|----------|--------|
| SSH Source IP | **10.11.90.211** |
| Additional Alert | **Ping Detected** |
| SSH Rule SID | **1000002** |

---

# Investigation Workflow

The practical exercise follows a typical digital forensics workflow.

```text
Security Incident
        │
        ▼
Network Traffic Captured
        │
        ▼
PCAP File
        │
        ▼
Snort Analysis
        │
        ▼
Alert Generation
        │
        ▼
Identify Suspicious Activity
        │
        ▼
Further Investigation
```

This demonstrates how IDS tools can be used not only for live monitoring but also for analyzing historical network traffic during forensic investigations.

---

# Key Takeaways from the Lab

Throughout this exercise, I learned how to:

- Analyze offline packet captures using Snort.
- Identify suspicious network traffic through IDS alerts.
- Understand the relationship between Snort rules and generated alerts.
- Interpret Signature IDs (SIDs).
- Recognize SSH and ICMP activity within captured traffic.
- Perform basic forensic analysis using an IDS engine.

---

# Blue Team Perspective

This lab closely resembles a real-world incident response scenario.

Instead of monitoring live traffic, defenders receive a packet capture collected during an incident. By replaying the PCAP through Snort, analysts can quickly identify suspicious communications, determine which detection rules fired, and establish an initial timeline of attacker activity.

The generated alerts provide valuable starting points for deeper investigations involving SIEM logs, endpoint telemetry, firewall logs, and authentication records.

---

# Red Team Perspective

From an offensive security perspective, this exercise demonstrates that even basic reconnaissance techniques—such as ICMP pinging and SSH connection attempts—can trigger IDS alerts when appropriate detection rules are enabled.

Understanding how defensive tools identify these activities helps penetration testers evaluate detection coverage during authorized engagements and better appreciate the visibility available to Blue Teams.

# Skills Gained

By completing this room, I developed both theoretical knowledge and practical skills related to Intrusion Detection Systems (IDS).

## Technical Skills

- Understood the purpose of Intrusion Detection Systems (IDS).
- Differentiated IDS from traditional firewalls.
- Learned the differences between Host-based IDS (HIDS) and Network-based IDS (NIDS).
- Compared signature-based, anomaly-based, and hybrid detection methods.
- Explored the architecture and workflow of Snort.
- Learned the purpose of Snort configuration files.
- Understood Snort's directory structure.
- Created and modified custom Snort rules.
- Tested custom detection rules using live network traffic.
- Performed offline network traffic analysis using PCAP files.
- Investigated IDS alerts generated by Snort.
- Identified suspicious network activity through packet analysis.

---

# Key Concepts Learned

Throughout this room, several important cybersecurity concepts were introduced.

## Defense in Depth

No single security solution can protect an organization from every attack.

A layered security model combines multiple technologies:

```text
Internet
    │
    ▼
Firewall
    │
    ▼
Network
    │
    ▼
IDS
    │
    ▼
SIEM
    │
    ▼
SOC Analyst
    │
    ▼
Incident Response
```

Each component contributes a different layer of protection.

---

## Detection vs Prevention

One of the biggest takeaways from this room is understanding the difference between prevention and detection.

| Prevention | Detection |
|------------|-----------|
| Blocks attacks | Detects attacks |
| Firewall | IDS |
| Stops communication | Generates alerts |
| Preventive Control | Detective Control |

Modern organizations require both.

---

## Signature-Based Detection

Advantages:

- Fast
- Accurate
- Low false positives

Limitations:

- Cannot detect unknown attacks
- Requires regular signature updates

---

## Anomaly-Based Detection

Advantages:

- Detects zero-day attacks
- Detects abnormal behavior
- Learns network baseline

Limitations:

- Higher false positives
- Requires tuning
- More computationally expensive

---

## Hybrid Detection

Modern IDS solutions often combine:

- Signature detection
- Behavioral analysis

This provides broader detection coverage while reducing blind spots.

---

# Snort Workflow

The complete detection process inside Snort can be summarized as follows:

```text
Network Traffic
        │
        ▼
Packet Decoder
        │
        ▼
Preprocessors
        │
        ▼
Detection Engine
        │
        ▼
Rule Matching
        │
        ▼
Alert Generation
        │
        ▼
SOC Investigation
```

Understanding this workflow makes it easier to troubleshoot IDS deployments and develop custom detection rules.

---

# Real-World Applications

Intrusion Detection Systems are widely deployed across modern enterprise environments.

Typical use cases include:

- Enterprise network monitoring
- Security Operations Centers (SOC)
- Threat Hunting
- Incident Response
- Digital Forensics
- Malware investigations
- Compliance monitoring
- Critical infrastructure protection
- Cloud workload monitoring

Many organizations integrate IDS alerts into centralized SIEM platforms for automated correlation and incident response.

---

# Industry Relevance

IDS remains one of the fundamental technologies used in defensive cybersecurity.

Professionals who regularly interact with IDS include:

- SOC Analysts
- Security Engineers
- Detection Engineers
- Incident Responders
- Digital Forensics Analysts
- Threat Hunters
- Blue Team Operators
- Network Security Engineers

Understanding IDS is also valuable for Red Team members because it helps them understand how defensive monitoring works.

---

# Common Mistakes

When first learning IDS, beginners often misunderstand several concepts.

### "IDS blocks attacks."

❌ Incorrect

An IDS **detects** suspicious activity and generates alerts.

Blocking traffic is typically the responsibility of:

- Firewalls
- Intrusion Prevention Systems (IPS)
- Endpoint security solutions

---

### "Every alert means an attack."

❌ Incorrect

Alerts simply indicate suspicious activity.

Security analysts must investigate alerts before concluding that malicious activity has occurred.

---

### "Signature-based IDS detects everything."

❌ Incorrect

Signature-based IDS only detects attacks whose signatures already exist.

Unknown attacks may bypass signature detection.

---

### "Anomaly-based IDS is always better."

❌ Incorrect

Although anomaly detection can detect unknown attacks, it often generates significantly more false positives and requires ongoing tuning.

---

# Future Learning Path

This room provides an excellent introduction to intrusion detection.

Recommended next topics include:

### Network Security

- Intrusion Prevention Systems (IPS)
- Network Access Control (NAC)
- Zero Trust Networking
- Secure Network Architecture

### Security Operations

- Detection Engineering
- Threat Hunting
- Advanced SIEM
- Security Monitoring
- MITRE ATT&CK

### Digital Forensics

- PCAP Analysis
- Wireshark
- Zeek
- Network Forensics
- Memory Forensics

### Snort

- Advanced Rule Writing
- Rule Optimization
- Custom Detection Logic
- Community Rule Sets
- Snort Performance Tuning

---

# Personal Reflection

This room significantly strengthened my understanding of how organizations detect malicious activities after attackers bypass preventive security controls.

Before this room, I primarily viewed firewalls as the central component of network defense. After completing the exercises, I now understand that monitoring and detection are equally important. IDS solutions continuously observe network traffic, identify suspicious behavior, and provide security teams with the visibility needed to investigate potential incidents.

The practical exercises with Snort also introduced me to the fundamentals of detection engineering. Creating custom rules, testing them against live traffic, and analyzing historical PCAP files demonstrated how IDS technologies are used in real-world SOC operations and digital forensic investigations.

---

# References

## Official Documentation

- TryHackMe — IDS Fundamentals
- Snort Official Documentation
- Cisco Secure — Snort Documentation

## Additional Reading

- NIST SP 800-94 — *Guide to Intrusion Detection and Prevention Systems (IDPS)*
- MITRE ATT&CK Framework
- OWASP Security Monitoring Guidelines

---

# Room Summary

This room introduced the fundamental concepts of Intrusion Detection Systems (IDS) and demonstrated how they complement firewalls within a layered security architecture.

Key topics included:

- IDS fundamentals
- HIDS and NIDS deployment models
- Signature-based detection
- Anomaly-based detection
- Hybrid detection
- Snort architecture
- Snort operating modes
- Snort rule syntax
- Custom rule creation
- Live traffic monitoring
- Offline PCAP analysis
- Basic forensic investigation

By completing both the theoretical lessons and the practical lab, I gained a solid foundation in intrusion detection and learned how IDS technologies support Security Operations Centers (SOC), Incident Response (IR), and Digital Forensics workflows.

---

# Tags

```text
#TryHackMe
#CyberSecurity
#BlueTeam
#SOC
#IDS
#IntrusionDetectionSystem
#Snort
#DetectionEngineering
#IncidentResponse
#DigitalForensics
#NetworkSecurity
#PCAP
#Linux
#ThreatDetection
#SOCAnalyst
#CyberJourney
```