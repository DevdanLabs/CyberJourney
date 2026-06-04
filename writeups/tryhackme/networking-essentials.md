# TryHackMe - Networking Essentials

## Comprehensive Learning Notes & Writeup

---

# Introduction

Before this room, I learned the basic networking concepts:

* OSI Model
* TCP/IP Model
* IP Addressing
* MAC Addresses
* Ethernet
* TCP

However, knowing those concepts alone does not explain how devices actually communicate in real life.

For example:

* How does my laptop automatically get an IP address when connecting to WiFi?
* How does my computer find the MAC address of another device?
* How can my entire house access the Internet using only one public IP address?
* How do routers know where to send packets?
* How does ping work?
* How does traceroute reveal every router along the path?

This room answers those questions.

---

# Big Picture

Whenever I open a website, the following things happen behind the scenes:

```text
Connect to WiFi
↓
DHCP
↓
Receive IP Address

Need Gateway MAC
↓
ARP

Need Route
↓
Routing

Need Internet Access
↓
NAT

Need Troubleshooting
↓
ICMP / Ping / Traceroute
```

Understanding this flow helped me connect all networking concepts together.

---

# DHCP (Dynamic Host Configuration Protocol)

## The Problem DHCP Solves

Imagine walking into a coffee shop.

You connect to WiFi.

A few seconds later:

```text
Internet works
```

Yet you never manually entered:

```text
IP Address
Subnet Mask
Gateway
DNS Server
```

How did your laptop know all of this?

Answer:

```text
DHCP
```

---

## Simple Analogy

Imagine entering a hotel.

You approach reception and ask:

> "Can I have a room?"

Reception checks available rooms and replies:

> "Room 203 is available."

You accept.

Reception records:

> "Room 203 now belongs to this guest."

This is exactly how DHCP works.

---

## What DHCP Provides

DHCP automatically provides:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

Without DHCP, every device would require manual configuration.

---

## DHCP Ports

| Device | Protocol | Port |
| ------ | -------- | ---- |
| Server | UDP      | 67   |
| Client | UDP      | 68   |

---

## DHCP DORA Process

One of the most important things in this room.

DORA stands for:

```text
Discover
Offer
Request
Acknowledge
```

---

### Step 1 - Discover

The client has nothing.

No IP.

No gateway.

No DNS.

The client basically shouts:

> "Is there any DHCP server here?"

Because it has no IP address yet, it sends:

```text
Source IP:
0.0.0.0

Destination IP:
255.255.255.255
```

This destination is a broadcast address.

Meaning:

> "Everyone on the network, listen."

---

### Step 2 - Offer

The DHCP server replies:

> "I have an available address."

For example:

```text
192.168.66.133
```

Along with:

```text
Gateway
DNS
Subnet Mask
```

---

### Step 3 - Request

The client says:

> "I accept that address."

---

### Step 4 - Acknowledge

The server replies:

> "Confirmed. That IP now belongs to you."

Only after this step can the client officially use the address.

---

## Packet Capture Analysis

Example:

```text
DHCP Discover
DHCP Offer
DHCP Request
DHCP ACK
```

One interesting observation:

During Discover and Request:

```text
Source IP = 0.0.0.0
```

because the client still does not own an address.

---

## DHCP Lease

The IP is not assigned forever.

The address is leased.

Example:

```text
24-hour lease
```

After some time, the client requests renewal.

---

## Pentesting Perspective

### Rogue DHCP

An attacker creates a fake DHCP server.

Instead of giving:

```text
Gateway = Router
```

the attacker gives:

```text
Gateway = Attacker
```

Result:

```text
Victim
↓
Attacker
↓
Internet
```

Man-in-the-Middle becomes possible.

---

# ARP (Address Resolution Protocol)

## The Problem ARP Solves

Suppose my laptop knows:

```text
192.168.66.1
```

But Ethernet doesn't use IP addresses.

Ethernet uses:

```text
MAC Addresses
```

The question becomes:

> "Who owns 192.168.66.1?"

ARP answers this question.

---

## Simple Analogy

Think of IP addresses as house addresses.

Think of MAC addresses as the people living inside.

You know the house location.

But you still need to find the person.

ARP is basically a phone book.

```text
IP
↓
MAC
```

---

## ARP Request

A host broadcasts:

> "Who has 192.168.66.1?"

The frame is sent to:

```text
ff:ff:ff:ff:ff:ff
```

which means:

```text
Everyone
```

---

## ARP Reply

The target responds:

> "192.168.66.1 is at 44:df:65:d8:fe:6c"

Now communication can begin.

---

## Key Learning

ARP translates:

```text
Layer 3 Address
(IP)
↓
Layer 2 Address
(MAC)
```

This is the most important sentence in the entire ARP section.

---

## Common Misconception

At first I thought:

> If devices already know each other's IP addresses, why do they need MAC addresses?

Answer:

Because Ethernet cannot deliver frames using IP addresses.

It requires MAC addresses.

ARP acts as the translator.

---

## Useful Commands

Windows:

```cmd
arp -a
```

Linux:

```bash
ip neigh
```

---

## Pentesting Perspective

ARP is heavily abused.

### ARP Spoofing

The attacker lies:

> "I am the gateway."

Victim believes it.

Traffic becomes:

```text
Victim
↓
Attacker
↓
Router
```

This allows packet sniffing and credential theft.

---

# ICMP (Internet Control Message Protocol)

## Purpose

ICMP is not used for web browsing.

ICMP is mainly used for:

```text
Diagnostics
Troubleshooting
Error Reporting
```

---

## Simple Analogy

TCP is the delivery truck.

ICMP is the traffic police.

It tells you:

```text
Road closed
Destination unreachable
Route too long
Host alive
```

---

# Ping

## How Ping Works

Ping sends:

```text
ICMP Echo Request
(Type 8)
```

The target replies:

```text
ICMP Echo Reply
(Type 0)
```

---

## Human Translation

My computer asks:

> "Are you alive?"

Target replies:

> "Yes."

---

## Example

```bash
ping 192.168.11.1 -c 4
```

Output:

```text
Reply from 192.168.11.1
ttl=63
time=11ms
```

---

## Understanding TTL

TTL means:

```text
Time To Live
```

It prevents packets from looping forever.

Example:

```text
TTL=64
↓
Router
TTL=63
↓
Router
TTL=62
```

Eventually:

```text
TTL=0
```

Packet is destroyed.

---

## RTT

Round Trip Time.

Measures how long it takes for:

```text
Request
↓
Target
↓
Reply
```

to complete.

Example:

```text
11 ms
```

---

# Traceroute

## Goal

Traceroute answers:

> "Which routers did my packet pass through?"

---

## How It Works

Traceroute manipulates TTL.

---

### Packet 1

```text
TTL=1
```

First router decreases it:

```text
1 → 0
```

Packet dies.

Router sends:

```text
ICMP Time Exceeded
(Type 11)
```

Traceroute records Router #1.

---

### Packet 2

```text
TTL=2
```

Second router becomes visible.

---

### Packet 3

```text
TTL=3
```

Third router becomes visible.

---

This continues until the destination is reached.

---

## Why Some Hops Show * * *

Possible reasons:

```text
Firewall
ICMP blocked
Security policy
Rate limiting
```

---

## Important ICMP Types

| Type | Meaning                 |
| ---- | ----------------------- |
| 0    | Echo Reply              |
| 3    | Destination Unreachable |
| 8    | Echo Request            |
| 11   | Time Exceeded           |

---

# Routing

## The Problem

Suppose:

```text
Network A
```

wants to reach:

```text
Network B
```

How does the packet know where to go?

Answer:

```text
Routing
```

---

## Simple Analogy

Routing is basically Google Maps for packets.

The router asks:

> "Which road should I take?"

and forwards packets accordingly.

---

## Routing Table

Every router maintains a routing table.

Example:

```text
Destination      Next Hop
192.168.1.0/24   Router A
10.0.0.0/8       Router B
```

---

## Routing Protocols

### RIP

Chooses:

```text
Fewest Hops
```

---

### OSPF

Creates a full map of the network.

Calculates optimal paths.

Common in enterprise networks.

---

### EIGRP

Cisco protocol.

Uses:

```text
Bandwidth
Delay
Reliability
```

---

### BGP

The most important protocol here.

BGP is the routing protocol that powers the Internet itself.

Google, Cloudflare, AWS, ISPs and large organizations all exchange routes using BGP.

---

# NAT (Network Address Translation)

## The Problem

IPv4 supports:

2^{32}=4294967296

About 4.3 billion addresses.

This sounded huge decades ago.

Today it isn't.

---

## NAT Solution

Instead of giving every device a public IP:

```text
Laptop
Phone
TV
CCTV
```

all devices share:

```text
One Public IP
```

---

## Simple Analogy

Think of NAT as a receptionist.

Employees:

```text
Room 101
Room 102
Room 103
```

have private internal numbers.

Outside callers only know:

```text
Main Company Number
```

The receptionist keeps track of who called whom.

This is exactly what NAT does.

---

## NAT Translation Example

Laptop:

```text
192.168.0.129:15401
```

gets translated into:

```text
212.3.4.5:19273
```

The web server never sees:

```text
192.168.0.129
```

It only sees:

```text
212.3.4.5
```

---

## Why Ports Matter

At first I wondered:

> If everyone shares the same public IP, how does the router know who receives the reply?

Answer:

Ports.

The router assigns different source ports.

Example:

```text
Laptop
212.3.4.5:10000

Phone
212.3.4.5:10001

TV
212.3.4.5:10002
```

---

## Question I Asked Myself

### If one IP only has 65,536 ports, can NAT only support 65,536 devices?

No.

This was my biggest misconception.

The limitation applies to:

```text
Connections
```

not:

```text
Devices
```

---

Example:

```text
1000 employees
```

Each employee creates:

```text
50 TCP connections
```

Total:

```text
50,000 active sessions
```

Still manageable.

---

## Another Question

### If I leave 20 browser tabs open all day, do they keep consuming TCP connections?

Not necessarily.

Many TCP sessions are created only while loading content.

After downloading:

```text
TCP
↓
FIN
↓
Closed
```

The NAT entry disappears.

---

However:

Applications such as:

```text
Discord
WhatsApp Web
Gmail
```

may keep long-lived connections alive using WebSockets.

---

## Another Question

### Why don't home routers run out of NAT ports?

Because NAT port exhaustion is rarely the bottleneck.

The real bottlenecks are usually:

```text
CPU
RAM
WiFi Capacity
Bandwidth
```

Long before NAT reaches 65,000 active sessions.

---

## Security Perspective

NAT hides internal devices.

External scans usually reveal only:

```text
Router
Firewall
```

while internal systems remain invisible.

This is why internal and external pentests often produce very different results.

---

# Final Summary

The entire room can be summarized as:

```text
DHCP
↓
Get an IP address

ARP
↓
Find MAC addresses

Routing
↓
Find the path

NAT
↓
Share one public IP

ICMP
↓
Troubleshoot connectivity

Ping
↓
Check if a host is alive

Traceroute
↓
Discover the path
```

This room connected many concepts from the Networking Concepts room and explained what actually happens behind the scenes whenever a device joins a network and accesses the Internet.
