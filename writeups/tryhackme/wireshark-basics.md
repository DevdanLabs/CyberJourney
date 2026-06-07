# Wireshark Basics — Complete Study Notes & Writeup

## Executive Summary

This room introduced the fundamentals of Wireshark, one of the most widely used packet analysis tools in networking, cybersecurity, incident response, digital forensics, and penetration testing.

The room covered:

* Wireshark interface overview
* Packet capture file analysis
* Packet dissection and OSI layers
* Packet navigation techniques
* Packet filtering
* Exporting transferred files
* Stream reconstruction
* Expert information analysis

By the end of the room, we learned how to inspect packet captures, identify important packet attributes, extract transferred files, reconstruct network conversations, and efficiently filter large packet datasets.

---

# Learning Objectives

* Understand what Wireshark is and why it is used.
* Learn how packet captures (PCAP/PCAPNG) work.
* Navigate the Wireshark interface.
* Analyze packet structures through protocol dissection.
* Use filtering to reduce noise.
* Search, mark, comment, and export packets.
* Reconstruct application-layer communications.
* Understand basic blue team and red team applications.

---

# Prerequisites

Recommended knowledge:

* OSI Model
* TCP/IP Model
* Basic Networking
* HTTP
* TCP & UDP
* IP Addressing
* Ports & Services

---

# What is Wireshark?

Wireshark is an open-source network protocol analyzer used to capture and inspect network traffic.

Think of Wireshark as:

> An X-ray machine for network traffic.

It allows analysts to inspect packets moving through a network and understand:

* Who is communicating
* What protocol is used
* What data is transferred
* Whether communication is normal or suspicious

---

# Why Does Wireshark Exist?

Without packet analysis, troubleshooting becomes guesswork.

Example:

A website is not loading.

Possible causes:

* DNS failure
* TCP connection failure
* Routing issue
* Firewall blocking traffic
* Server-side problem

Wireshark allows us to observe exactly what happens on the wire.

---

# Wireshark Use Cases

## Network Troubleshooting

Identify:

* Connectivity issues
* Packet loss
* Routing problems
* DNS failures
* TCP retransmissions

---

## Security Analysis

Identify:

* Rogue hosts
* Malware communications
* Suspicious protocols
* Unusual ports
* Unauthorized data transfers

---

## Protocol Analysis

Learn how protocols work:

* HTTP
* DNS
* FTP
* SMB
* SMTP
* DHCP

---

# Important Note

Wireshark is NOT an IDS.

Wireshark:

```text
Observes traffic
```

IDS:

```text
Detects attacks automatically
```

Examples of IDS:

* Snort
* Suricata
* Zeek

Wireshark requires human analysis.

---

# Wireshark Interface Overview

## Main Components

### Toolbar

Provides shortcuts for:

* Open
* Save
* Capture
* Export
* Statistics
* Analysis

---

### Display Filter Bar

Used to filter visible traffic.

Example:

```text
http
```

```text
dns
```

```text
ip.addr == 10.10.10.10
```

---

### Recent Files

Displays recently opened captures.

---

### Capture Interfaces

Available network interfaces:

Examples:

```text
eth0
wlan0
lo
```

---

### Status Bar

Displays:

* Total packets
* Displayed packets
* Current profile

---

# Packet Visualization

Wireshark displays packets using three panes.

---

## Packet List Pane

Provides summary information:

* Packet Number
* Time
* Source
* Destination
* Protocol
* Information

---

## Packet Details Pane

Shows protocol breakdown.

Example:

```text
Frame
Ethernet
IPv4
TCP
HTTP
```

---

## Packet Bytes Pane

Displays raw hexadecimal data.

Example:

```text
45 00 00 30
```

---

# Packet Coloring

Wireshark uses colors to identify traffic types.

Examples:

## Green

Normal TCP traffic

---

## Red

Errors

Example:

```text
TCP Reset
```

---

## Black

TCP Problems

Examples:

* Retransmissions
* Duplicate ACKs
* Packet Loss

---

# Packet Dissection

Packet dissection is the process of decoding packet contents into human-readable information.

---

## Encapsulation

Data is wrapped as it moves down the network stack.

Example:

```text
HTTP
↓
TCP
↓
IP
↓
Ethernet
```

---

## De-encapsulation

Receiving systems reverse the process.

```text
Ethernet
↓
IP
↓
TCP
↓
HTTP
```

---

# Packet Layers

## Layer 1 — Frame

Contains:

* Frame number
* Arrival time
* Packet size

---

## Layer 2 — Ethernet

Contains:

* Source MAC
* Destination MAC

---

## Layer 3 — IP

Contains:

* Source IP
* Destination IP
* TTL

---

## Layer 4 — Transport

Contains:

* TCP
* UDP
* Ports

Example:

```text
Source Port
Destination Port
```

---

## Protocol Errors

Examples:

```text
TCP Retransmission
```

```text
TCP Segment Reassembled
```

---

## Application Protocol

Examples:

```text
HTTP
DNS
FTP
SMB
```

---

## Application Data

Actual transferred content.

Examples:

* HTML
* XML
* Credentials
* File contents

---

# Markup Languages

A markup language adds structure and meaning to data.

Examples:

## HTML

```html
<h1>Hello</h1>
```

---

## XML

```xml
<user>
    <name>Devdan</name>
</user>
```

---

## Markdown

```markdown
# Title
```

---

# Packet Navigation

## Go To Packet

Navigate directly to a packet number.

Menu:

```text
Go
→ Go To Packet
```

---

## Find Packet

Search packet contents.

Methods:

* String
* Regex
* Hex
* Display Filter

---

## Mark Packet

Temporarily highlight packets.

Menu:

```text
Edit
→ Mark Packet
```

---

## Packet Comments

Add permanent notes to packets.

Useful during investigations.

---

# Exporting Data

## Export Packets

Save selected packets into a separate capture.

---

## Export Objects

Recover files transferred through:

* HTTP
* SMB
* TFTP

Menu:

```text
File
→ Export Objects
```

---

# Expert Information

Menu:

```text
Analyze
→ Expert Information
```

Categories:

| Severity | Meaning           |
| -------- | ----------------- |
| Chat     | Informational     |
| Note     | Interesting Event |
| Warn     | Warning           |
| Error    | Serious Issue     |

---

# Packet Filtering

Filtering is the most important Wireshark skill.

Without filtering:

```text
Millions of packets
```

With filtering:

```text
Only relevant packets
```

---

# Capture Filter vs Display Filter

## Capture Filter

Filters before capture.

```text
Traffic
↓
Filter
↓
Saved
```

---

## Display Filter

Filters after capture.

```text
Traffic
↓
Saved
↓
Displayed
```

---

# Apply As Filter

Right-click any field.

```text
Apply as Filter
```

Wireshark creates the filter automatically.

---

# Conversation Filter

Displays only packets belonging to a specific conversation.

---

# Colourise Conversation

Highlights related packets without filtering them out.

---

# Prepare As Filter

Creates a filter but does not execute it.

Useful for building complex queries.

---

# Apply As Column

Adds packet fields as columns.

Useful for:

* User-Agent
* Hostname
* URI
* Protocol-specific fields

---

# Follow Stream

Reconstructs communication.

Menu:

```text
Follow
→ TCP Stream
```

or

```text
Follow
→ HTTP Stream
```

Useful for:

* Credentials
* Requests
* Responses
* Chat Messages

---

# Common Display Filters

## HTTP

```text
http
```

---

## DNS

```text
dns
```

---

## TCP

```text
tcp
```

---

## UDP

```text
udp
```

---

## ARP

```text
arp
```

---

## Filter by Port

```text
tcp.port == 80
```

```text
tcp.port == 443
```

```text
tcp.port == 445
```

---

## Filter by IP

```text
ip.addr == 192.168.1.2
```

---

# Blue Team Relevance

Wireshark is heavily used for:

* Incident Response
* Malware Analysis
* Threat Hunting
* Forensics
* Network Troubleshooting

Examples:

* Detect C2 traffic
* Identify exfiltration
* Investigate malware downloads
* Analyze suspicious DNS traffic

---

# Red Team Relevance

Wireshark helps understand:

* TCP handshakes
* HTTP requests
* DNS queries
* SMB communication
* Reverse shells
* Exploit traffic

Useful during:

* Reconnaissance
* Enumeration
* Exploitation validation
* Tool development

---

# Practical Questions & Solutions

---

## Question 1

### Read the capture file comments. What is the flag?

Answer:

```text
TryHackMe_Wireshark_Demo
```

### Steps

1. Open Exercise.pcapng
2. Navigate to:

```text
Statistics
→ Capture File Properties
```

3. Locate:

```text
Capture File Comments
```

4. Read the value.

---

## Question 2

### Total number of packets?

Answer:

```text
58620
```

### Steps

1. Open Exercise.pcapng
2. Check Status Bar
3. Or:

```text
Statistics
→ Capture File Properties
```

---

## Question 3

### SHA256 hash?

Answer:

```text
f446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb
```

### Steps

1. Open:

```text
Statistics
→ Capture File Properties
```

2. Locate SHA256 value.

---

## Question 4

### Packet 38 — Markup language?

Answer:

```text
eXtensible Markup Language
```

### Steps

1. Go To Packet:

```text
38
```

2. Expand HTTP layer.
3. Locate:

```text
Content-Type: text/xml
```

or XML content.

---

## Question 5

### Arrival Date?

Answer:

```text
05/13/2004
```

### Steps

1. Packet 38
2. Expand:

```text
Frame
```

3. Read:

```text
Arrival Time
```

---

## Question 6

### TTL Value?

Answer:

```text
47
```

### Steps

1. Packet 38
2. Expand:

```text
IPv4
```

3. Locate:

```text
Time To Live
```

---

## Question 7

### TCP Payload Size?

Answer:

```text
424
```

### Steps

1. Packet 38
2. Expand TCP
3. Locate:

```text
TCP Segment Length
```

---

## Question 8

### E-Tag Value?

Answer:

```text
9a01a-4696-7e354b00
```

### Steps

1. Packet 38
2. Expand HTTP
3. Locate:

```text
ETag
```

---

## Question 9

### Search "r4w"

Answer:

```text
r4w8173
```

### Steps

1. Edit
2. Find Packet
3. Type:

```text
r4w
```

4. Search Packet Details.

---

## Question 10

### Packet 12 Comment

Answer:

```text
911cd574a42865a956ccde2d04495ebf
```

### Steps

1. Go To Packet 12
2. View Packet Comments

---

## Question 11

### Alien Name

Answer:

```text
PACKETMASTER
```

### Steps

1. Navigate:

```text
File
→ Export Objects
→ HTTP
```

2. Use filter:

```text
.txt
```

3. Select:

```text
note.txt
```

4. Save file
5. Open file
6. Read ASCII art.

### Personal Troubleshooting Note

Initially, the wrong HTTP object was exported because the `.txt` filter was not used in the Export Objects window.

This led to analyzing the wrong file and missing the answer.

Resolution:

```text
Export Objects
→ Filter ".txt"
→ Export note.txt
```

---

## Question 12

### Number of Warnings

Answer:

```text
1636
```

### Steps

1. Open:

```text
Analyze
→ Expert Information
```

2. Read Warning count.

---

## Question 13

### Apply HTTP as Filter

Answer:

```text
http
```

### Steps

1. Go To Packet 4
2. Expand:

```text
Hypertext Transfer Protocol
```

3. Right-click
4. Apply as Filter

---

## Question 14

### Displayed Packets

Answer:

```text
1089
```

### Steps

1. Apply HTTP filter
2. Check status bar

---

## Question 15

### Number of Artists

Answer:

```text
3
```

### Steps

1. Go To Packet:

```text
33790
```

2. Follow HTTP Stream
3. Read server response
4. Count artists.

---

## Question 16

### Second Artist

Answer:

```text
Blad3
```

### Steps

1. Stay in HTTP Stream
2. Read artist list
3. Identify second entry.

---

# Troubleshooting & Lessons Learned

## Issue: Exported Wrong Object

Problem:

Wrong HTTP object was exported from Export Objects.

Cause:

The `.txt` filter was not used.

Impact:

The alien name could not be found.

Resolution:

```text
File
→ Export Objects
→ HTTP
→ Filter ".txt"
→ Export note.txt
```

Lesson:

Always verify that the artifact you are analyzing matches the indicator given in the question.

---

# Skills Gained

* Wireshark Navigation
* Packet Dissection
* OSI Layer Analysis
* Packet Searching
* Stream Reconstruction
* File Extraction
* Display Filtering
* Expert Information Analysis

---

# Key Takeaways

* Wireshark is a packet analysis tool, not an IDS.
* Filtering is the most important Wireshark skill.
* Every packet can be broken into multiple protocol layers.
* Follow Stream reconstructs application-level communication.
* Export Objects allows recovery of transferred files.
* Expert Info helps identify anomalies quickly.
* Metadata and packet comments often contain valuable clues.
* Accurate artifact selection is critical during investigations.

---

# Conclusion

This room provided a strong foundation in packet analysis using Wireshark. We learned how to inspect packet captures, navigate large datasets, extract transferred files, reconstruct application-layer conversations, and efficiently filter traffic.

These skills form the basis for more advanced topics such as malware traffic analysis, network forensics, incident response, Active Directory investigations, and threat hunting. Mastering these fundamentals is essential before moving into protocol-specific analysis such as DNS, SMB, Kerberos, HTTP authentication, and command-and-control traffic investigations.
