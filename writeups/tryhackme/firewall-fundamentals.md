# Firewall Fundamentals

## Executive Summary

Firewalls are one of the most fundamental security controls in modern computer networks. They act as the first line of defense by monitoring, filtering, and controlling network traffic based on predefined security rules. Whether protecting a personal computer, an enterprise network, or cloud infrastructure, firewalls help prevent unauthorized access while allowing legitimate communications to pass through.

In this TryHackMe room, I learned the core concepts behind firewalls, including why they exist, how they inspect network traffic, and the different types of firewalls used in real-world environments. The room also introduced firewall rules, explaining how administrators create policies based on source addresses, destination addresses, ports, protocols, traffic direction, and actions such as allowing, denying, or forwarding traffic.

The practical section focused on two major operating systems. On Windows, I explored **Windows Defender Firewall**, learned about network profiles, and created custom inbound and outbound firewall rules using the Advanced Security console. On Linux, I was introduced to the **Netfilter** framework and several firewall management utilities, including **iptables**, **nftables**, **firewalld**, and **UFW (Uncomplicated Firewall)**. Through simple command-line exercises, I learned how to enable a firewall, configure default policies, create filtering rules, and manage existing rules.

By completing this room, I gained a solid understanding of firewall technologies from both offensive and defensive security perspectives. This knowledge forms an essential foundation for networking, system administration, SOC operations, penetration testing, and security engineering.

---

## Room Information

| Information | Details |
|------------|---------|
| Platform | TryHackMe |
| Room | Firewall Fundamentals |
| Category | Defensive Security |
| Difficulty | Easy |
| Focus Areas | Firewalls, Network Security, Windows Defender Firewall, Linux Firewall, UFW, Netfilter, Firewall Rules |

---

# Learning Objectives

After completing this room, I was able to:

- Understand the purpose of firewalls in modern networks.
- Explain how firewalls inspect and filter network traffic.
- Differentiate between various firewall technologies.
- Understand where different firewalls operate within the OSI model.
- Explain the differences between stateless, stateful, proxy, and next-generation firewalls.
- Understand firewall rule components and traffic filtering logic.
- Configure firewall actions such as Allow, Deny, and Forward.
- Distinguish between inbound, outbound, and forwarding rules.
- Navigate and configure Windows Defender Firewall.
- Create custom inbound and outbound firewall rules in Windows.
- Understand Linux's Netfilter framework.
- Use UFW to configure firewall policies and rules on Linux.
- Understand how firewall technologies are used in enterprise environments.

---

# Prerequisites

Before starting this room, learners should be familiar with:

- Basic computer networking
- IP addressing
- TCP/IP fundamentals
- Common network protocols
- Port numbers
- Client-server communication
- Basic Windows navigation
- Basic Linux command-line usage

Recommended TryHackMe rooms:

- Networking Concepts
- Networking Essentials
- Windows Fundamentals
- Linux Fundamentals

---

# Why This Room Matters

Firewalls are among the most widely deployed security technologies in the world. Nearly every organization—regardless of size—uses firewalls to protect endpoints, servers, cloud environments, and entire enterprise networks.

Understanding firewall concepts is critical for many cybersecurity roles, including:

- SOC Analyst
- Security Analyst
- Security Engineer
- Network Engineer
- System Administrator
- Penetration Tester
- Incident Responder
- Cloud Security Engineer

Without understanding how firewalls operate, it becomes difficult to:

- Secure network services
- Troubleshoot connectivity issues
- Perform network segmentation
- Investigate blocked connections
- Detect malicious traffic
- Understand attack paths during penetration tests

For both defenders and attackers, firewalls represent one of the most important layers in a defense-in-depth security strategy.

---

# Skills Gained

By completing this room, I strengthened the following skills:

### Networking

- Understanding network traffic flow
- Working with TCP and UDP ports
- Understanding network communication paths
- Traffic filtering concepts

### Defensive Security

- Firewall configuration
- Traffic filtering
- Network access control
- Security policy implementation
- Attack surface reduction

### Windows Administration

- Windows Defender Firewall
- Advanced Security console
- Inbound Rules
- Outbound Rules
- Network Profiles
- Custom firewall rules

### Linux Administration

- Netfilter framework
- UFW management
- Basic firewall administration
- Linux packet filtering

### Cybersecurity

- Network hardening
- Access control
- Security monitoring concepts
- Enterprise firewall architecture

---

# Key Terminology

| Term | Description |
|------|-------------|
| Firewall | A security device or software that monitors and filters network traffic. |
| Rule | A policy that determines whether traffic should be allowed, blocked, or redirected. |
| Packet | The basic unit of data transmitted across a network. |
| Inbound Traffic | Network traffic entering a device or network. |
| Outbound Traffic | Network traffic leaving a device or network. |
| Forwarding | Redirecting traffic to another destination. |
| Stateful Inspection | Tracking active network connections before making filtering decisions. |
| Stateless Filtering | Filtering packets individually without remembering previous connections. |
| Proxy Firewall | A firewall that inspects application-layer traffic by acting as an intermediary. |
| NGFW | A Next-Generation Firewall providing advanced threat detection and deep packet inspection. |
| Netfilter | The Linux kernel framework responsible for packet filtering and NAT. |
| UFW | Uncomplicated Firewall, a user-friendly frontend for Linux firewall configuration. |
| iptables | A command-line utility for configuring Netfilter rules. |
| nftables | The modern successor to iptables. |
| Network Profile | A Windows firewall configuration applied based on the connected network type. |

---

# Technologies Covered

- Windows Defender Firewall
- Windows Defender Firewall with Advanced Security
- Netfilter
- iptables
- nftables
- firewalld
- UFW (Uncomplicated Firewall)
- TCP
- UDP
- IPv4
- Firewall Policies
- Packet Filtering
- Deep Packet Inspection (DPI)
- Intrusion Prevention Concepts

---

# Red Team Perspective

From an offensive security perspective, firewalls determine what services are exposed to attackers.

A penetration tester commonly interacts with firewalls by:

- Performing port scanning
- Identifying filtered services
- Enumerating accessible ports
- Discovering firewall restrictions
- Testing firewall rule bypass techniques
- Identifying unnecessary exposed services

Understanding firewall behavior helps attackers determine the network's attack surface and identify potential entry points.

---

# Blue Team Perspective

From a defensive perspective, firewalls are one of the first security controls used to reduce risk.

Security teams use firewalls to:

- Restrict unnecessary services
- Limit administrative access
- Segment internal networks
- Block malicious traffic
- Enforce security policies
- Monitor suspicious connections
- Reduce the attack surface
- Protect critical assets

Proper firewall configuration significantly reduces the likelihood of successful network attacks.

---

# Key Takeaways

- Firewalls inspect and control network traffic based on predefined security rules.
- Different firewall technologies provide different levels of visibility and protection.
- Firewall rules define exactly how traffic should be handled.
- Windows Defender Firewall provides powerful built-in firewall management through graphical tools.
- Linux offers multiple firewall utilities built on the Netfilter framework.
- UFW simplifies Linux firewall management while leveraging Netfilter internally.
- Firewalls are essential components of enterprise security architectures and play a critical role in both network defense and penetration testing.

# What Is a Firewall?

## Definition

A **firewall** is a network security system that monitors, filters, and controls incoming and outgoing network traffic based on predefined security rules.

Its primary objective is to prevent unauthorized access while allowing legitimate communication between trusted and untrusted networks.

A firewall can be implemented as:

- Hardware
- Software
- Virtual appliance
- Cloud service

Regardless of its implementation, the firewall acts as a security barrier between different network zones.

---

## Why Do Firewalls Exist?

Whenever a computer is connected to a network—especially the Internet—it becomes accessible to other devices.

Without any protection:

- Attackers could scan open ports.
- Malware could communicate with command-and-control (C2) servers.
- Unauthorized users could access exposed services.
- Sensitive systems could become reachable from untrusted networks.

Firewalls were developed to solve these problems by controlling which network traffic is permitted and which should be blocked.

Instead of allowing unrestricted communication, every packet is evaluated according to security policies before reaching its destination.

---

## Real-World Analogy

A firewall can be compared to a **security guard** stationed at the entrance of a building.

Everyone who enters or leaves must first pass through the guard.

The guard checks whether the visitor is authorized.

- Authorized visitors are allowed to enter.
- Unauthorized visitors are denied access.

Similarly, every network packet entering or leaving a device must pass through the firewall.

```text
                Internet
                    │
                    │
           ┌─────────────────┐
           │    Firewall     │
           │ Security Guard  │
           └─────────────────┘
             │             │
         Allow         Block
             │             │
      Internal Network
```

---

# How Firewalls Work

Whenever network traffic reaches a firewall, it is compared against configured firewall rules.

A simplified workflow is shown below:

```text
Incoming Packet
        │
        ▼
Firewall Checks Rules
        │
        ├───────────────┐
        │               │
     Match          No Match
        │               │
        ▼               ▼
 Apply Action      Continue Checking
        │
        ▼
Allow / Deny / Forward
```

Firewall decisions are entirely based on the configured rules and security policies.

---

# Benefits of Firewalls

Firewalls provide multiple security benefits, including:

- Preventing unauthorized access
- Reducing attack surface
- Restricting unnecessary network services
- Monitoring network connections
- Controlling inbound traffic
- Controlling outbound traffic
- Supporting network segmentation
- Enforcing organizational security policies

Modern firewalls also include advanced security features beyond simple packet filtering.

---

# Firewall Deployment

Firewalls can be deployed in several locations depending on the organization's requirements.

## Host-Based Firewall

A host-based firewall protects an individual computer or server.

Examples:

- Windows Defender Firewall
- UFW
- firewalld

```text
Internet
     │
Firewall
     │
Windows PC
```

---

## Network Firewall

A network firewall protects an entire network by filtering traffic entering or leaving the organization.

```text
Internet
     │
Network Firewall
     │
Switch
     │
Multiple Devices
```

This deployment is common in enterprise environments.

---

# Firewall Types

As network threats evolved, firewall technologies also became more sophisticated.

Each generation introduced additional security capabilities while addressing the limitations of previous designs.

The evolution can be summarized as:

```text
Stateless Firewall
        │
        ▼
Stateful Firewall
        │
        ▼
Proxy Firewall
        │
        ▼
Next-Generation Firewall (NGFW)
```

---

# Firewall Types and OSI Layers

Different firewall technologies inspect different parts of network communication.

| Firewall Type | OSI Layer |
|--------------|-----------|
| Stateless Firewall | Layer 3 – Layer 4 |
| Stateful Firewall | Layer 3 – Layer 4 |
| Proxy Firewall | Layer 7 |
| Next-Generation Firewall | Layer 3 – Layer 7 |

Generally, the higher the OSI layer inspected, the more visibility and control the firewall has over network traffic.

---

# Stateless Firewall

## Overview

A Stateless Firewall is the simplest firewall implementation.

It filters every packet independently without remembering previous network connections.

Each packet is evaluated solely against predefined filtering rules.

---

## How It Works

When a packet arrives, the firewall examines information such as:

- Source IP Address
- Destination IP Address
- Protocol
- Port Number

If the packet matches an allowed rule, it passes.

Otherwise, it is blocked.

Once the decision is made, the firewall forgets the packet completely.

The next packet is treated as an entirely new connection.

```text
Packet
   │
   ▼
Firewall
   │
Compare Rules
   │
Allow / Deny
```

---

## Advantages

- Very fast packet processing
- Low memory usage
- Simple implementation
- Suitable for high-speed environments

---

## Limitations

- Does not track connections
- Cannot recognize legitimate sessions
- More susceptible to spoofed traffic
- Limited policy flexibility

---

# Stateful Firewall

## Overview

A Stateful Firewall improves upon stateless filtering by maintaining information about active network connections.

Instead of evaluating every packet independently, it tracks communication sessions using a **State Table**.

---

## State Table

A simplified state table may look like:

| Source | Destination | Status |
|---------|-------------|--------|
| 10.0.0.5 | 8.8.8.8:443 | Established |
| 10.0.0.5 | 1.1.1.1:53 | Established |

Future packets belonging to an existing session can be processed much more efficiently.

---

## Advantages

- Recognizes established connections
- More secure than stateless filtering
- Supports complex security policies
- Better traffic management

---

## Limitations

- Higher memory consumption
- Requires connection tracking
- State table can become exhausted during large-scale attacks

---

# Proxy Firewall

## Overview

A Proxy Firewall operates as an intermediary between clients and external servers.

Instead of allowing direct communication, all traffic passes through the proxy first.

```text
Client
   │
Proxy Firewall
   │
Internet
```

The destination server never communicates directly with the client.

---

## Application Layer Inspection

Unlike previous firewall types, proxy firewalls inspect the contents of network traffic.

They can analyze:

- HTTP requests
- URLs
- File downloads
- Application commands
- Email traffic

This allows organizations to apply content-based filtering policies.

---

## Advantages

- Deep application inspection
- Content filtering
- Application awareness
- Internal IP address masking
- Better visibility into network traffic

---

## Limitations

- Higher latency
- Increased CPU usage
- More complex deployment

---

# Next-Generation Firewall (NGFW)

## Overview

A Next-Generation Firewall combines traditional firewall functionality with advanced security technologies.

NGFWs provide protection across multiple OSI layers while performing deep inspection of network traffic.

---

## Common Features

Modern NGFW solutions typically include:

- Stateful Inspection
- Deep Packet Inspection (DPI)
- Intrusion Prevention System (IPS)
- Application Awareness
- SSL/TLS Inspection
- Threat Intelligence Integration
- Malware Detection
- Heuristic Analysis

---

## Deep Packet Inspection

Traditional firewalls primarily inspect packet headers.

NGFWs inspect:

- Packet headers
- Application payloads
- User identities
- Application behavior
- Threat signatures

This provides much greater visibility into encrypted and application-layer traffic.

---

## Advantages

- Advanced threat detection
- Real-time attack prevention
- Application-aware filtering
- SSL/TLS inspection
- Integration with threat intelligence
- Protection against modern cyber attacks

---

## Limitations

- Higher hardware requirements
- Increased processing overhead
- More complex configuration
- Higher deployment costs

---

# Firewall Comparison

| Feature | Stateless | Stateful | Proxy | NGFW |
|---------|-----------|-----------|-------|------|
| Tracks Connections | ❌ | ✅ | ✅ | ✅ |
| Packet Filtering | ✅ | ✅ | ✅ | ✅ |
| Application Inspection | ❌ | ❌ | ✅ | ✅ |
| Deep Packet Inspection | ❌ | ❌ | Limited | ✅ |
| SSL/TLS Inspection | ❌ | ❌ | Optional | ✅ |
| Intrusion Prevention | ❌ | ❌ | ❌ | ✅ |
| Performance | Very High | High | Medium | Lower |
| Complexity | Low | Medium | High | Very High |

---

# Red Team Perspective

Understanding firewall behavior is essential during penetration testing.

Firewalls directly influence:

## Reconnaissance

- Port scanning
- Host discovery
- Service enumeration

## Enumeration

- Identifying accessible services
- Determining firewall restrictions
- Detecting filtered ports

## Exploitation

Attackers can only exploit services that are reachable through the firewall.

Poorly configured firewall rules significantly increase the attack surface.

## Lateral Movement

Internal firewalls may restrict movement between network segments after an attacker gains initial access.

---

# Blue Team Perspective

Defenders rely on firewalls to enforce security policies across their environments.

Typical defensive use cases include:

- Blocking unauthorized traffic
- Limiting administrative access
- Network segmentation
- Preventing malware communication
- Monitoring suspicious traffic
- Logging security events
- Supporting compliance requirements

Firewalls are considered one of the foundational components of a defense-in-depth strategy.

---

# Key Takeaways

- Firewalls control network traffic using predefined security rules.
- Different firewall technologies provide different levels of visibility and protection.
- Stateless firewalls filter individual packets.
- Stateful firewalls track active network sessions.
- Proxy firewalls inspect application-layer traffic.
- Next-Generation Firewalls combine traditional filtering with advanced threat prevention technologies.
- Understanding firewall architecture is fundamental for both network defense and penetration testing.

# Firewall Rules

## Overview

Firewalls make decisions based on **rules**. A firewall rule is a policy that tells the firewall how to handle specific network traffic.

Without rules, a firewall would not know whether a packet should be allowed, blocked, or redirected.

For every packet that enters or leaves a network, the firewall compares the packet's characteristics against its configured rule set before taking action.

---

## Why Firewall Rules Are Important

Every organization has different security requirements.

For example:

- A company may want to block all SSH access from the Internet.
- A web server should allow HTTP and HTTPS traffic.
- A database server should only accept connections from the application server.
- An administrator may allow remote access only from a trusted IP address.

Firewall rules make these security policies possible.

---

# How Firewall Rules Work

Whenever a packet reaches the firewall, the firewall evaluates it against the configured rules.

A simplified workflow is shown below.

```text
Incoming Packet
        │
        ▼
Check Rule #1
        │
   Match?
   │
 ┌─┴──────────────┐
 │                │
Yes              No
 │                │
 ▼                ▼
Apply Action   Check Rule #2
                    │
                    ▼
              Continue Until Match
```

Most firewalls process rules **from top to bottom** and stop once a matching rule is found.

For this reason, rule order is extremely important.

---

# Components of a Firewall Rule

A firewall rule is typically made up of several components.

## Source Address

The **Source Address** identifies where the traffic originates.

Examples:

- Single IP Address
- Network Range
- Subnet
- Any Address

Example:

```text
192.168.1.50
```

or

```text
192.168.1.0/24
```

---

## Destination Address

The **Destination Address** specifies where the traffic is going.

Example:

```text
192.168.1.8
```

This could represent:

- A web server
- A file server
- A workstation
- A router

---

## Port

Ports identify the service being accessed.

Some common ports include:

| Port | Service |
|------|----------|
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3389 | Remote Desktop (RDP) |

Firewalls commonly use port numbers to determine whether specific services should be accessible.

---

## Protocol

The protocol defines the communication method.

Common protocols include:

- TCP
- UDP
- ICMP

For example:

```text
TCP 443
```

indicates HTTPS traffic.

---

## Direction

The direction specifies whether the rule applies to:

- Incoming traffic
- Outgoing traffic
- Traffic being forwarded

Direction helps the firewall determine which communications should be inspected.

---

## Action

The action determines what the firewall should do after a rule matches.

Common actions include:

- Allow
- Deny
- Forward

This is the final decision applied to the packet.

---

# Types of Firewall Actions

## Allow

The **Allow** action permits matching traffic to pass through the firewall.

Example:

| Action | Source | Destination | Protocol | Port | Direction |
|---------|---------|------------|----------|------|-----------|
| Allow | 192.168.1.0/24 | Any | TCP | 80 | Outbound |

This rule allows every device inside the **192.168.1.0/24** network to access websites using HTTP.

```text
Internal Network
        │
HTTP (80)
        │
Firewall
        │
Internet
```

---

## Deny

The **Deny** action blocks matching traffic.

Example:

| Action | Source | Destination | Protocol | Port | Direction |
|---------|---------|------------|----------|------|-----------|
| Deny | Any | 192.168.1.0/24 | TCP | 22 | Inbound |

This rule blocks all incoming SSH connections.

```text
Internet
     │
SSH (22)
     │
Firewall
     │
Blocked
```

Blocking unnecessary services significantly reduces the network attack surface.

---

## Forward

The **Forward** action redirects traffic to another destination.

Example:

| Action | Source | Destination | Protocol | Port | Direction |
|---------|---------|------------|----------|------|-----------|
| Forward | Any | 192.168.1.8 | TCP | 80 | Inbound |

In this example, all incoming HTTP traffic is forwarded to the internal web server.

```text
Internet
     │
HTTP (80)
     │
Firewall
     │
Web Server
192.168.1.8
```

Forwarding is commonly used on firewalls that also function as routers or gateways.

---

# Firewall Rule Direction

Firewall rules are grouped according to the direction of the traffic.

---

## Inbound Rules

Inbound rules apply to traffic entering the protected device or network.

Typical examples include:

- Allow HTTPS to a web server
- Block Remote Desktop from the Internet
- Allow SSH from an administrator's workstation

```text
Internet
     │
Inbound Traffic
     │
Firewall
     │
Internal Server
```

---

## Outbound Rules

Outbound rules control traffic leaving the local device or network.

Examples include:

- Allow Internet browsing
- Block unauthorized email traffic
- Prevent malware from communicating with external servers

```text
Internal Computer
        │
Outbound Traffic
        │
Firewall
        │
Internet
```

Outbound filtering is often overlooked but plays an important role in stopping malware from communicating with command-and-control (C2) servers.

---

## Forward Rules

Forward rules redirect traffic between different network segments.

They are commonly used on:

- Enterprise firewalls
- Routers
- Network gateways

Example:

```text
Internet
     │
Firewall
     │
Forward
     │
Internal Web Server
```

---

# Example Firewall Policy

Suppose an organization has the following servers:

| Server | Service |
|---------|----------|
| Web Server | HTTP, HTTPS |
| Application Server | Internal APIs |
| Database Server | MySQL |

The administrator wants to:

- Allow public web access.
- Restrict SSH to administrators only.
- Prevent public database access.

Firewall rules may look like this:

| Source | Destination | Port | Action |
|---------|-------------|------|--------|
| Any | Web Server | 80 | Allow |
| Any | Web Server | 443 | Allow |
| Admin Network | Web Server | 22 | Allow |
| Any | Database Server | 3306 | Deny |

This policy follows the **principle of least privilege**, allowing only the traffic that is necessary.

---

# Firewall Rule Best Practices

Well-designed firewall rules should follow several security principles.

## Default Deny

A common best practice is:

> **Deny everything unless it is explicitly allowed.**

Instead of allowing all traffic, administrators create only the rules required for normal operations.

---

## Least Privilege

Grant only the minimum network access required.

Instead of:

```text
Allow SSH from Any
```

use:

```text
Allow SSH from 192.168.13.7
```

This greatly reduces the attack surface.

---

## Keep Rules Simple

Avoid unnecessary or duplicate rules.

A clean rule set is:

- Easier to maintain
- Easier to audit
- Less likely to introduce security issues

---

## Review Rules Regularly

Old firewall rules often remain after systems are retired.

Unused rules should be removed to minimize unnecessary exposure.

---

# Common Firewall Misconfigurations

Several configuration mistakes are frequently observed in real environments.

### Allow Any → Any

This effectively disables meaningful filtering and exposes the network.

---

### Exposed Administrative Services

Leaving services such as:

- SSH
- RDP
- Telnet

accessible from the Internet significantly increases risk.

---

### Incorrect Rule Order

Since most firewalls process rules from top to bottom, placing a broad **Allow** rule above a specific **Deny** rule can make the deny rule ineffective.

---

### Unused Legacy Rules

Rules created for temporary troubleshooting are often forgotten and remain active long after they are needed.

---

# Red Team Perspective

Firewall rules determine what attackers can and cannot reach.

During a penetration test, attackers often attempt to identify:

- Open ports
- Filtered ports
- Firewall restrictions
- Administrative services
- Weakly protected systems

Tools such as **Nmap** help identify firewall behavior during reconnaissance.

Poor firewall policies frequently expose services that should never be reachable from untrusted networks.

---

# Blue Team Perspective

Security teams use firewall rules to:

- Restrict network access
- Enforce organizational security policies
- Protect administrative interfaces
- Limit lateral movement
- Segment internal networks
- Reduce attack surface
- Log suspicious traffic

A properly configured firewall is one of the most effective preventative security controls in an enterprise environment.

---

# Key Takeaways

- Firewall rules determine how network traffic is handled.
- Every rule contains components such as source, destination, protocol, port, direction, and action.
- The three primary firewall actions are **Allow**, **Deny**, and **Forward**.
- Rules can apply to inbound, outbound, or forwarded traffic.
- Rule order is critical because most firewalls evaluate rules sequentially.
- Following security principles such as **Default Deny** and **Least Privilege** significantly improves an organization's security posture.
- Proper firewall rule management is essential for both system administrators and cybersecurity professionals.

# Windows Defender Firewall

## Overview

Windows Defender Firewall is Microsoft's built-in host-based firewall included in modern versions of Windows. It provides administrators and end users with the ability to control both inbound and outbound network traffic using customizable security rules.

Unlike network firewalls that protect an entire organization, Windows Defender Firewall protects individual hosts by filtering traffic before it reaches applications or services running on the system.

The firewall is enabled by default and integrates tightly with Windows Security, making it one of the first layers of defense against unauthorized network access.

---

# Windows Defender Firewall Architecture

A simplified view of how Windows Defender Firewall operates is shown below.

```text
                 Internet
                     │
                     ▼
        Windows Defender Firewall
                     │
      ┌──────────────┴──────────────┐
      │                             │
Inbound Traffic              Outbound Traffic
      │                             │
      ▼                             ▼
          Windows Operating System
                     │
             Installed Applications
```

Every inbound and outbound connection is evaluated against firewall rules before Windows decides whether the traffic should be allowed or blocked.

---

# Network Profiles

Windows automatically applies different firewall configurations depending on the type of network the computer is connected to.

This feature is known as **Network Location Awareness (NLA)**.

The room introduced two network profiles.

## Private Network

A Private Network is intended for trusted environments such as:

- Home networks
- Personal LANs
- Small office networks

Typical characteristics:

- Device discovery enabled
- File and printer sharing may be allowed
- Less restrictive firewall policies

---

## Public (Guest) Network

A Public Network is intended for untrusted environments such as:

- Coffee shops
- Airports
- Hotels
- Restaurants
- Public Wi-Fi hotspots

Typical characteristics:

- Device discovery disabled
- File sharing disabled
- More restrictive firewall rules
- Better protection against unknown devices

Using stricter firewall settings on public networks significantly reduces the risk of unauthorized access.

---

# Windows Defender Firewall Dashboard

The main dashboard provides quick access to common firewall management features.

Important options include:

- View firewall status
- Enable or disable the firewall
- Allow an application through the firewall
- Restore default settings
- Open Advanced Security
- Configure network profiles

The dashboard also displays whether the firewall is currently enabled for each network profile.

---

# Allowing Applications

Windows Defender Firewall allows administrators to permit or restrict applications rather than configuring only network ports.

Examples include:

- Web browsers
- Remote Desktop
- File sharing
- Third-party applications

Applications can be allowed separately for:

- Private networks
- Public networks

This provides greater flexibility depending on where the computer is connected.

---

# Advanced Security Console

For advanced firewall management, Windows provides **Windows Defender Firewall with Advanced Security**.

This console allows administrators to create highly customized firewall rules.

The major rule categories include:

- Inbound Rules
- Outbound Rules
- Connection Security Rules
- Monitoring

This interface is commonly used in enterprise environments.

---

# Inbound Rules

Inbound rules determine which incoming connections are allowed to reach the computer.

Examples include:

- Allow Remote Desktop
- Block SSH
- Allow HTTPS
- Allow ICMP (Ping)

Example:

```text
Internet
      │
SSH Request
      │
Windows Firewall
      │
 Blocked
```

---

# Outbound Rules

Outbound rules control traffic leaving the computer.

Examples include:

- Block HTTP traffic
- Allow HTTPS
- Restrict specific applications
- Prevent malware from contacting external servers

Although outbound filtering is less commonly configured than inbound filtering, it provides an additional layer of protection.

---

# Practical Exercise — Blocking HTTP and HTTPS

The room demonstrated how to create a custom outbound rule that blocks web browsing.

The objective was to prevent the computer from accessing websites over HTTP and HTTPS.

The process involved:

1. Opening **Windows Defender Firewall with Advanced Security**.
2. Selecting **Outbound Rules**.
3. Creating a **New Rule**.
4. Choosing **Custom Rule**.
5. Applying the rule to **All Programs**.
6. Selecting **TCP** as the protocol.
7. Specifying **Remote Ports 80 and 443**.
8. Selecting **Block the Connection**.
9. Applying the rule to all network profiles.
10. Giving the rule a descriptive name.

Once enabled, any attempt to browse websites using HTTP or HTTPS was blocked.

---

# Rule Configuration

The custom rule created during the lab can be summarized as follows.

| Setting | Value |
|----------|-------|
| Rule Type | Custom |
| Program | All Programs |
| Protocol | TCP |
| Remote Ports | 80, 443 |
| Action | Block the Connection |
| Profiles | Domain, Private, Public |
| Direction | Outbound |

This demonstrates how Windows Defender Firewall can filter traffic based on protocol and port numbers.

---

# Rule Verification

Before creating the firewall rule, the lab machine could successfully access:

```text
http://10.10.10.10/
```

After enabling the outbound blocking rule:

- HTTP traffic was blocked.
- HTTPS traffic was blocked.
- The browser displayed a connection error.

This confirmed that the firewall rule was functioning correctly.

---

# Lab Exercise

The practical exercise required examining existing firewall rules and identifying specific configurations created by the security team.

## Question 1

**What is the name of the rule that blocks all incoming SSH traffic?**

**Answer**

```text
Core Op
```

This rule blocks inbound TCP connections on **Port 22 (SSH)** from all sources.

---

## Question 2

**What is the name of the rule that allows SSH from a single IP address?**

**Answer**

```text
Infra team
```

This rule acts as an exception by allowing trusted administrative access.

---

## Question 3

**Which IP address is allowed by this rule?**

**Answer**

```text
192.168.13.7
```

Only this IP address is permitted to establish SSH connections.

All other incoming SSH traffic remains blocked.

---

# Lessons Learned

This exercise demonstrated several important firewall concepts.

## Rule Priority

Firewall behavior depends entirely on configured rules.

A single rule can completely change whether a service is reachable.

---

## Granular Access Control

Rather than allowing SSH access from every device, administrators can restrict access to specific trusted hosts.

Example:

```text
Allow SSH
From:
192.168.13.7

Block SSH
From:
Everyone Else
```

This follows the **Principle of Least Privilege**, allowing only the minimum required access.

---

## Windows Firewall Is More Powerful Than Many Users Realize

Although commonly viewed as a simple desktop firewall, Windows Defender Firewall supports:

- Port filtering
- Protocol filtering
- Application filtering
- IP-based filtering
- User-based filtering
- Network profile filtering
- Custom inbound rules
- Custom outbound rules

These capabilities make it suitable for both personal computers and enterprise environments.

---

# Red Team Notes

During penetration testing, Windows Defender Firewall is often responsible for services appearing as:

- Filtered
- Closed
- Inaccessible

For example:

```text
Nmap Scan

22/tcp    filtered
80/tcp    open
3389/tcp  filtered
```

A filtered port usually indicates that a firewall is blocking or silently dropping packets.

Understanding firewall behavior helps penetration testers distinguish between:

- A service that is not running
- A service that is running but protected by firewall rules

---

# Blue Team Notes

Windows Defender Firewall is one of the most important endpoint security controls.

Administrators commonly use it to:

- Restrict Remote Desktop access
- Limit SSH administration
- Block unnecessary services
- Control application network access
- Protect laptops on public Wi-Fi
- Enforce enterprise security policies through Group Policy

Proper firewall configuration greatly reduces the attack surface while allowing legitimate business traffic to continue uninterrupted.

---

# Key Takeaways

- Windows Defender Firewall is a built-in host-based firewall included with Microsoft Windows.
- Different firewall settings can be applied to Private and Public network profiles.
- The Advanced Security console allows administrators to create highly customized inbound and outbound rules.
- Firewall rules can filter traffic based on applications, protocols, ports, IP addresses, and network profiles.
- Restricting administrative services such as SSH to trusted IP addresses is a common security best practice.
- Windows Defender Firewall plays a critical role in endpoint protection and enterprise network security.

# Linux Firewall

## Overview

Linux provides powerful built-in firewall capabilities through the **Netfilter** framework, which is integrated directly into the Linux kernel. Unlike Windows Defender Firewall, Linux separates the packet filtering engine from the management tools, allowing administrators to choose different utilities to configure firewall rules.

Several firewall management utilities exist, each designed for different use cases and experience levels. While they all rely on Netfilter, they provide different interfaces for managing firewall policies.

Understanding how these utilities relate to one another is essential for Linux system administration, cybersecurity, and network defense.

---

# Netfilter Framework

## What is Netfilter?

**Netfilter** is the packet filtering framework built directly into the Linux kernel.

Every network packet entering or leaving a Linux system passes through Netfilter before reaching applications or network services.

Netfilter is responsible for:

- Packet filtering
- Network Address Translation (NAT)
- Connection tracking
- Packet modification
- Packet forwarding

Because Netfilter operates inside the kernel, it provides high-performance packet processing with minimal overhead.

A simplified architecture is shown below.

```text
                 Internet
                      │
                      ▼
               Linux Kernel
                      │
                Netfilter Engine
                      │
        ┌─────────────┴─────────────┐
        │                           │
 Packet Filtering              NAT / Routing
        │                           │
        ▼                           ▼
          Applications & Services
```

---

# Linux Firewall Utilities

Several utilities interact with the Netfilter framework.

Each provides a different approach to firewall management.

---

# iptables

## Overview

**iptables** is the traditional command-line firewall utility used by many Linux distributions.

It allows administrators to create highly detailed packet filtering rules by directly interacting with Netfilter.

For many years, iptables was considered the standard firewall management tool for Linux systems.

### Advantages

- Extremely flexible
- Highly customizable
- Widely supported
- Suitable for enterprise environments

### Limitations

- Complex syntax
- Difficult for beginners
- Managing large rule sets can become challenging

---

# nftables

## Overview

**nftables** is the modern successor to iptables.

It was designed to simplify firewall configuration while improving performance and scalability.

Compared to iptables, nftables offers:

- Cleaner syntax
- Better performance
- Unified IPv4 and IPv6 handling
- Improved rule management

Many modern Linux distributions now use nftables as their default firewall backend.

---

# firewalld

## Overview

**firewalld** is another firewall management utility built on top of Netfilter.

Instead of manually creating individual firewall rules, firewalld introduces the concept of **network zones**.

Common zones include:

- Public
- Home
- Internal
- Work
- Trusted
- DMZ

Each zone contains its own predefined security policies.

This allows administrators to quickly apply different firewall configurations depending on the network environment.

Firewalld is commonly found on:

- Red Hat Enterprise Linux (RHEL)
- CentOS
- Fedora

---

# UFW (Uncomplicated Firewall)

## Overview

**UFW (Uncomplicated Firewall)** is a user-friendly frontend for configuring Linux firewalls.

Instead of requiring administrators to write complex iptables or nftables rules, UFW provides simple commands that automatically generate the appropriate firewall configuration.

Ubuntu includes UFW by default, making it one of the easiest firewall utilities for beginners.

---

# Basic UFW Commands

The room introduced several commonly used UFW commands.

---

## Check Firewall Status

### Purpose

Displays whether the firewall is currently active.

### Command

```bash
sudo ufw status
```

### Example Output

```text
Status: inactive
```

If the firewall is inactive, it is not currently filtering network traffic.

---

## Enable the Firewall

### Purpose

Enables the firewall and configures it to start automatically during system boot.

### Command

```bash
sudo ufw enable
```

### Example Output

```text
Firewall is active and enabled on system startup
```

---

## Disable the Firewall

### Purpose

Turns off the firewall.

### Command

```bash
sudo ufw disable
```

Disabling the firewall should generally be avoided unless required for troubleshooting.

---

## Configure the Default Policy

### Purpose

Defines how traffic should be handled when no firewall rule matches.

### Allow All Outgoing Traffic

```bash
sudo ufw default allow outgoing
```

This sets the default outbound policy to **Allow**.

Unless another rule overrides it, all outbound connections are permitted.

---

## Block Incoming SSH Connections

### Purpose

Blocks incoming SSH connections on TCP port 22.

### Command

```bash
sudo ufw deny 22/tcp
```

### Example Output

```text
Rule added
Rule added (v6)
```

This command creates firewall rules for both IPv4 and IPv6 traffic.

---

## View Active Firewall Rules

### Purpose

Displays all configured firewall rules along with their rule numbers.

### Command

```bash
sudo ufw status numbered
```

### Example Output

```text
Status: active

     To                         Action      From
     --                         ------      ----
[1] 22/tcp                     DENY IN     Anywhere
[2] 22/tcp (v6)                DENY IN     Anywhere (v6)
```

Numbered rules simplify firewall administration by allowing administrators to reference rules directly.

---

## Delete a Firewall Rule

### Purpose

Removes an existing firewall rule.

### Command

```bash
sudo ufw delete 2
```

### Example Output

```text
Deleting:
 deny 22/tcp

Proceed with operation (y|n)? y

Rule deleted (v6)
```

Only the specified rule is removed.

---

# How UFW Works

Although UFW appears simple, it does not replace Netfilter.

Instead, it acts as a management layer that automatically generates the appropriate firewall configuration.

The relationship can be visualized as follows.

```text
Administrator
       │
       ▼
     UFW Commands
       │
       ▼
iptables / nftables
       │
       ▼
   Netfilter
       │
       ▼
 Linux Kernel
       │
       ▼
 Network Traffic
```

UFW simplifies firewall administration while still leveraging the full capabilities of the Linux kernel.

---

# Practical Example

Suppose an administrator wants to secure a Linux web server.

The following policy could be implemented:

```text
Default Incoming : DENY
Default Outgoing : ALLOW
```

Then allow only the required services.

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

This configuration allows:

- SSH
- HTTP
- HTTPS

while blocking all other unsolicited incoming traffic.

This approach follows the **Default Deny** security model.

---

# Red Team Perspective

During penetration testing, Linux firewalls frequently influence reconnaissance results.

Attackers often use tools such as **Nmap** to determine whether firewall rules are filtering network traffic.

Example:

```text
22/tcp    filtered
80/tcp    open
443/tcp   open
```

A **filtered** port typically indicates that a firewall is silently dropping packets.

Firewall restrictions may require attackers to:

- Identify alternative attack vectors
- Pivot through compromised systems
- Exploit exposed web services instead of SSH

---

# Blue Team Perspective

System administrators use Linux firewalls to:

- Restrict administrative access
- Reduce exposed services
- Prevent unauthorized remote access
- Control inbound and outbound traffic
- Enforce security policies
- Limit lateral movement
- Protect Internet-facing servers

A common enterprise configuration is:

- Deny all inbound traffic by default.
- Allow only required services.
- Permit outbound traffic unless specific restrictions are needed.

This approach significantly reduces the system's attack surface.

---

# Common Misconfigurations

Several mistakes are frequently observed when configuring Linux firewalls.

### Firewall Disabled

```text
Status: inactive
```

Running a production server without an active firewall unnecessarily exposes network services.

---

### Allowing SSH from Anywhere

Instead of:

```text
Allow SSH from Any
```

A better approach is:

```text
Allow SSH only from the administrator's IP address.
```

---

### Exposing Databases

Ports such as:

- 3306 (MySQL)
- 5432 (PostgreSQL)

should rarely be accessible directly from the Internet.

---

### Leaving Temporary Rules

Firewall rules created for troubleshooting are sometimes forgotten and remain active, increasing the attack surface.

---

# Enterprise Use Cases

Linux firewalls are widely deployed in:

- Web servers
- Cloud virtual machines
- Kubernetes worker nodes
- Docker hosts
- Reverse proxies
- VPN gateways
- Bastion hosts
- Database servers

Nearly every Linux server in production uses one or more firewall technologies to protect network services.

---

# Key Takeaways

- Netfilter is the Linux kernel framework responsible for packet filtering and network traffic control.
- Multiple firewall utilities—including **iptables**, **nftables**, **firewalld**, and **UFW**—are built on top of Netfilter.
- UFW provides a beginner-friendly interface that simplifies firewall management.
- Administrators can configure default policies, create filtering rules, list active rules, and remove existing rules using simple UFW commands.
- Following the **Default Deny** principle and exposing only necessary services significantly improves Linux server security.
- Understanding Linux firewall technologies is an essential skill for system administrators, security engineers, SOC analysts, and penetration testers.

# Skills Gained

By completing this room, I developed both theoretical knowledge and practical experience with firewall technologies across Windows and Linux environments.

## Networking Skills

- Understanding how firewalls inspect network traffic
- Understanding inbound and outbound communication
- Understanding packet filtering concepts
- Understanding TCP and UDP traffic control
- Understanding network access policies
- Understanding network segmentation concepts

---

## Windows Administration

- Navigating Windows Defender Firewall
- Understanding Network Profiles
- Managing Inbound Rules
- Managing Outbound Rules
- Creating custom firewall rules
- Configuring protocol and port filtering
- Applying firewall rules to different network profiles

---

## Linux Administration

- Understanding the Netfilter framework
- Managing Linux firewalls using UFW
- Configuring default firewall policies
- Allowing and denying services
- Listing active firewall rules
- Removing firewall rules safely

---

## Defensive Security

- Implementing access control policies
- Reducing attack surface
- Restricting unnecessary services
- Understanding firewall best practices
- Applying the Principle of Least Privilege
- Implementing the Default Deny security model

---

## Cybersecurity Skills

- Understanding host-based firewalls
- Identifying exposed network services
- Understanding firewall deployment strategies
- Recognizing enterprise firewall architectures
- Understanding the defensive role of firewalls in layered security

---

# Common Misconfigurations

Improper firewall configurations can introduce significant security risks.

Some of the most common mistakes include:

## Allowing Any-to-Any Traffic

Creating overly permissive rules such as:

```text
Source: Any
Destination: Any
Action: Allow
```

effectively defeats the purpose of having a firewall.

---

## Exposing Administrative Services

Leaving administrative services accessible from the Internet unnecessarily increases the attack surface.

Examples include:

- SSH (22)
- RDP (3389)
- WinRM
- Telnet

Administrative access should only be permitted from trusted management hosts whenever possible.

---

## Disabled Firewalls

Operating systems sometimes have their firewall disabled during troubleshooting.

Leaving it disabled afterward creates unnecessary exposure.

Always verify that the firewall remains enabled after maintenance.

---

## Unused Firewall Rules

Temporary rules created during testing are often forgotten.

Old rules should be reviewed and removed regularly to maintain a clean and secure rule set.

---

## Incorrect Rule Order

Most firewalls process rules sequentially.

Placing a broad **Allow** rule before a more specific **Deny** rule may unintentionally bypass intended restrictions.

Rule order should always be reviewed carefully.

---

# Real-World Enterprise Usage

Firewalls are deployed in virtually every modern IT environment.

Common enterprise use cases include:

## Endpoint Protection

Host-based firewalls protect employee workstations and laptops from unauthorized inbound connections.

Examples:

- Windows Defender Firewall
- UFW
- firewalld

---

## Data Center Security

Servers hosting business-critical applications are protected using firewall policies that restrict access to only required services.

Examples:

- Web Servers
- Application Servers
- Database Servers
- Domain Controllers

---

## Cloud Security

Cloud providers offer firewall functionality to secure virtual machines and cloud services.

Examples include:

- AWS Security Groups
- Azure Network Security Groups (NSGs)
- Google Cloud Firewall Rules

Although implemented differently, they follow the same fundamental firewall concepts covered in this room.

---

## Enterprise Perimeter Security

Organizations deploy dedicated hardware or virtual firewalls between internal networks and the Internet.

Typical enterprise firewall vendors include:

- Palo Alto Networks
- Fortinet
- Cisco
- Check Point
- Sophos

These devices often provide advanced capabilities such as:

- Deep Packet Inspection (DPI)
- Intrusion Prevention Systems (IPS)
- VPN services
- SSL/TLS inspection
- Threat intelligence integration

---

## Internal Network Segmentation

Large organizations commonly deploy internal firewalls between network segments to reduce lateral movement.

Example:

```text
Internet
     │
Perimeter Firewall
     │
───────────────
│             │
DMZ        Internal LAN
               │
         Internal Firewall
               │
        Database Network
```

Even if one system becomes compromised, internal firewalls help prevent attackers from moving freely throughout the environment.

---

# Interview Questions

## What is a firewall?

A firewall is a security system that monitors and filters incoming and outgoing network traffic according to predefined security rules.

---

## What is the difference between a Stateless and Stateful Firewall?

A Stateless Firewall evaluates each packet independently without remembering previous connections.

A Stateful Firewall maintains a state table and tracks active network sessions, allowing it to make more informed filtering decisions.

---

## What is the purpose of a Proxy Firewall?

A Proxy Firewall acts as an intermediary between clients and servers, inspecting application-layer traffic before forwarding requests to their destination.

---

## What is a Next-Generation Firewall (NGFW)?

An NGFW combines traditional firewall functionality with advanced security features such as:

- Deep Packet Inspection
- Intrusion Prevention
- Application Awareness
- SSL/TLS Inspection
- Threat Intelligence

---

## What are the three common firewall actions?

- Allow
- Deny
- Forward

---

## What is the difference between inbound and outbound rules?

Inbound rules control traffic entering a system.

Outbound rules control traffic leaving a system.

---

## What is Netfilter?

Netfilter is the packet filtering framework built directly into the Linux kernel.

It provides the foundation for Linux firewall utilities such as iptables, nftables, firewalld, and UFW.

---

## Why is the Default Deny principle recommended?

The Default Deny model blocks all traffic unless explicitly permitted, reducing the attack surface and preventing unintended access.

---

# Future Learning Path

This room establishes the foundation for more advanced network security topics.

Recommended next topics include:

## Firewalls

- pfSense
- Palo Alto NGFW
- FortiGate
- Cisco ASA
- Cisco Firepower

---

## Network Security

- Network Segmentation
- VLAN Security
- Access Control Lists (ACLs)
- Zero Trust Architecture
- Software-Defined Networking (SDN)

---

## Linux Security

- Advanced iptables
- nftables
- firewalld Zones
- SELinux
- AppArmor

---

## Windows Security

- Windows Defender Advanced Firewall
- Group Policy Firewall Management
- Windows Defender Application Control (WDAC)
- Microsoft Defender for Endpoint

---

## SOC & Detection

- SIEM Log Analysis
- Firewall Log Analysis
- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Network Traffic Analysis
- Threat Hunting

---

## Penetration Testing

- Nmap Firewall Evasion
- Firewall Enumeration
- Network Pivoting
- Internal Network Enumeration
- Lateral Movement Techniques

These topics naturally build upon the concepts introduced in this room and are essential for advancing toward roles such as SOC Analyst, Security Engineer, or Penetration Tester.

---

# References

## Official Documentation

- Microsoft Learn — Windows Defender Firewall Documentation
- Ubuntu Documentation — UFW (Uncomplicated Firewall)
- Netfilter Project Documentation
- nftables Wiki
- Linux man pages (`man ufw`, `man iptables`, `man nft`)

---

## TryHackMe

- Firewall Fundamentals
- Networking Concepts
- Networking Essentials
- SOC Fundamentals
- Logs Fundamentals
- SIEM
- Incident Response Fundamentals

---

# Conclusion

Firewalls are one of the most fundamental security technologies used to protect modern computer systems and networks. Throughout this room, I learned how firewalls monitor and filter network traffic, how different firewall technologies operate across the OSI model, and how firewall rules determine whether traffic is allowed, denied, or forwarded.

The hands-on exercises provided practical experience with both **Windows Defender Firewall** and **Linux UFW**, demonstrating how firewall policies can be configured to protect systems by restricting unnecessary network access. I also gained an understanding of the Linux **Netfilter** framework and how utilities such as **iptables**, **nftables**, **firewalld**, and **UFW** interact with it.

From a cybersecurity perspective, firewalls play a critical role in reducing the attack surface, enforcing security policies, and supporting a defense-in-depth strategy. Whether performing system administration, defending enterprise networks, or conducting penetration testing, understanding firewall behavior is an essential skill for any cybersecurity professional.

---

# Tags

`TryHackMe`

`Firewall`

`Windows Defender Firewall`

`Linux`

`Netfilter`

`iptables`

`nftables`

`firewalld`

`UFW`

`Network Security`

`Packet Filtering`

`Host-Based Firewall`

`Defensive Security`

`Blue Team`

`SOC`

`Cybersecurity`

`Networking`

`CyberJourney`