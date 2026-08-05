# Hack The Box - Support

> **Platform:** Hack The Box  
> **Machine Name:** Support  
> **Difficulty:** Easy  
> **Operating System:** Windows (Active Directory Domain Controller)  
> **Author:** Hack The Box  
> **Writeup By:** DevdanLabs  
> **Repository:** Cyber Journey

---

# Executive Summary

**Support** is an Easy-rated Windows Active Directory machine on Hack The Box that demonstrates how a seemingly low-privileged domain user can compromise an entire Active Directory environment by abusing insecure permissions and Kerberos delegation.

Unlike traditional Windows machines that focus on local privilege escalation, this machine emphasizes **Active Directory enumeration**, **directory services**, **access control**, and **Kerberos authentication**. Throughout the attack chain, we enumerate SMB shares, reverse engineer a .NET application to recover hardcoded credentials, enumerate LDAP objects, gain an initial foothold through WinRM, analyze Active Directory permissions using BloodHound, and finally abuse **Resource-Based Constrained Delegation (RBCD)** to impersonate the domain administrator and obtain **NT AUTHORITY\SYSTEM** access on the Domain Controller.

Rather than relying on software vulnerabilities or exploits, this machine showcases how **misconfigured Active Directory permissions** can become the weakest link in an enterprise environment. Every stage builds upon legitimate Windows functionality, making the attack both realistic and highly relevant to modern penetration testing.

This writeup focuses not only on reproducing the exploitation steps but also on explaining the underlying technologies, protocols, and security concepts involved. Every command, tool, and attack technique is accompanied by detailed explanations so that readers understand **why** each step works rather than simply following commands.

---

# Learning Objectives

By completing this machine and studying this writeup, readers will learn how to:

- Perform network reconnaissance against a Windows Active Directory environment.
- Enumerate SMB shares and identify valuable files exposed through anonymous access.
- Reverse engineer a .NET application using ILSpy to recover embedded credentials.
- Understand Base64 encoding and XOR encryption used to obfuscate sensitive information.
- Authenticate and enumerate Active Directory using LDAP.
- Discover user information stored within LDAP attributes.
- Gain remote access to a Windows server through WinRM.
- Collect Active Directory data using SharpHound.
- Analyze Active Directory relationships and permissions using BloodHound.
- Understand Access Control Lists (ACLs) and why **GenericAll** permissions are dangerous.
- Learn how **Machine Account Quota** allows authenticated users to create computer accounts.
- Create a new machine account using Impacket's `addcomputer.py`.
- Understand the theory behind Kerberos delegation and Resource-Based Constrained Delegation (RBCD).
- Modify the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute using `rbcd.py`.
- Understand the Kerberos **S4U2Self** and **S4U2Proxy** extensions.
- Request Kerberos service tickets using `getST.py`.
- Perform Pass-the-Ticket authentication using Kerberos credential caches (`ccache`).
- Obtain SYSTEM privileges on a Domain Controller using `psexec.py`.

---

# Skills Gained

After completing this machine, you should have practical experience with:

## Windows Enumeration

- SMB Enumeration
- LDAP Enumeration
- WinRM Access
- Windows Host Enumeration

## Reverse Engineering

- .NET Assembly Analysis
- ILSpy
- Base64 Analysis
- XOR Decryption

## Active Directory

- LDAP
- Active Directory Objects
- Access Control Lists (ACL)
- GenericAll Abuse
- Computer Accounts
- Machine Account Quota
- Service Principal Names (SPN)

## Kerberos

- Ticket Granting Ticket (TGT)
- Service Tickets (TGS)
- Kerberos Delegation
- Resource-Based Constrained Delegation (RBCD)
- S4U2Self
- S4U2Proxy
- Pass-the-Ticket

## Offensive Security Tools

- Nmap
- smbclient
- ILSpy
- ldapsearch
- Evil-WinRM
- SharpHound
- BloodHound
- addcomputer.py
- rbcd.py
- getST.py
- psexec.py

---

# Attack Chain Overview

The compromise follows a logical Active Directory attack path.

```text
Network Enumeration
        │
        ▼
Anonymous SMB Enumeration
        │
        ▼
Download UserInfo.exe.zip
        │
        ▼
Reverse Engineering (.NET)
        │
        ▼
Recover LDAP Credentials
        │
        ▼
LDAP Enumeration
        │
        ▼
Discover Support User Password
        │
        ▼
WinRM Initial Access
        │
        ▼
SharpHound Data Collection
        │
        ▼
BloodHound Analysis
        │
        ▼
GenericAll ACL Abuse
        │
        ▼
Create Machine Account
        │
        ▼
Resource-Based Constrained Delegation
        │
        ▼
Kerberos S4U2Self + S4U2Proxy
        │
        ▼
Administrator Service Ticket
        │
        ▼
Pass-the-Ticket
        │
        ▼
NT AUTHORITY\SYSTEM
```

---

# MITRE ATT&CK Mapping

| Tactic | Technique | Description |
|---------|-----------|-------------|
| Reconnaissance | T1595 | Active Scanning |
| Discovery | T1135 | Network Share Discovery |
| Credential Access | T1552 | Unsecured Credentials |
| Credential Access | T1003 (Conceptual) | Credential Discovery through application analysis |
| Discovery | T1087 | Account Discovery |
| Discovery | T1069 | Permission Group Discovery |
| Discovery | T1482 | Domain Trust Discovery |
| Lateral Movement | T1021.006 | Windows Remote Management (WinRM) |
| Discovery | T1087.002 | Domain Account Discovery |
| Privilege Escalation | T1098 | Account Manipulation |
| Credential Access | T1558 | Kerberos Tickets |
| Lateral Movement | T1550.003 | Pass the Ticket |
| Privilege Escalation | T1558.003 | Kerberos Delegation Abuse |

> **Note:** This writeup maps the techniques to the closest applicable MITRE ATT&CK techniques. Some actions (such as reverse engineering the custom application to recover credentials) are educational scenarios that do not map perfectly to a single ATT&CK technique.

---

# What Makes This Machine Valuable?

Support is much more than an "Easy" Windows machine. It introduces many core Active Directory concepts that repeatedly appear in enterprise penetration tests and advanced certification paths such as **CRTP (Certified Red Team Professional)**.

Instead of exploiting vulnerable software, the attack abuses **misconfigured trust relationships**, **directory permissions**, and **Kerberos delegation**, demonstrating that a fully patched Windows server can still be compromised through poor Active Directory configuration.

For aspiring penetration testers, red teamers, and security engineers, Support serves as an excellent introduction to modern Active Directory attacks while providing a strong foundation for more advanced techniques encountered in real-world enterprise environments.

---

# Prerequisites

Before diving into the walkthrough, it is important to understand the technologies that make up a Windows Active Directory environment. This machine does not rely on exploiting vulnerable software; instead, it abuses legitimate Windows features and misconfigured permissions.

Having a solid understanding of these concepts will make each stage of the attack much easier to follow.

---

# Active Directory

## What is Active Directory?

Active Directory (AD) is Microsoft's centralized directory service used to manage identities, computers, users, groups, permissions, and security policies across an organization's network.

Instead of storing user accounts separately on every computer, Active Directory centralizes authentication and authorization into a single domain.

Think of Active Directory as the "brain" of a Windows enterprise network.

```
                Active Directory Domain

          +------------------------------+
          |      Domain Controller       |
          |                              |
          |  Users                       |
          |  Computers                   |
          |  Groups                      |
          |  Policies                    |
          |  Authentication              |
          +--------------+---------------+
                         |
        ------------------------------------------
        |                |                 |
        ▼                ▼                 ▼
   Employee PC      File Server      Application Server
```

Without Active Directory:

- Every computer manages its own users.
- Passwords exist independently.
- Permissions become difficult to maintain.
- Administrators must configure each machine individually.

With Active Directory:

- Users have one account.
- Authentication is centralized.
- Permissions are centrally managed.
- Security policies are applied automatically.

---

## Why Does Active Directory Exist?

Large organizations may have:

- Hundreds of employees
- Thousands of computers
- Multiple offices
- Numerous servers

Managing each system independently would be nearly impossible.

Active Directory solves this by providing:

- Centralized authentication
- Centralized authorization
- Centralized identity management
- Centralized policy management

---

## Active Directory Components

Some of the most important objects inside Active Directory include:

| Object | Description |
|----------|-------------|
| User | Represents a person or service account |
| Computer | Represents a Windows machine joined to the domain |
| Group | Collection of users or computers |
| Organizational Unit (OU) | Container used to organize objects |
| Domain Controller | Server responsible for authentication and directory services |
| Group Policy | Configuration applied automatically to users and computers |

Throughout this machine, we will interact with several of these objects directly.

---

# Domain Controller

The **Domain Controller (DC)** is the most important server in an Active Directory environment.

It hosts:

- Active Directory Database
- LDAP Service
- Kerberos Authentication Service
- DNS
- Group Policy

Whenever a user logs in to a domain-joined computer, the authentication request is typically handled by the Domain Controller.

```
User Login
     │
     ▼
Domain Controller
     │
     ├── Verify Username
     ├── Verify Password
     ├── Determine Group Membership
     ├── Apply Policies
     └── Issue Kerberos Tickets
```

Because the Domain Controller controls authentication for the entire domain, compromising it usually means compromising the entire Active Directory environment.

---

# SMB (Server Message Block)

## What is SMB?

SMB is Microsoft's network file-sharing protocol.

It allows computers to:

- Share files
- Share folders
- Share printers
- Access remote resources

SMB is commonly used inside corporate environments for file servers and administrative shares.

Common SMB ports include:

| Port | Protocol |
|-------|----------|
| 445 | SMB over TCP |
| 139 | NetBIOS Session Service (legacy) |

---

## Why SMB Matters to Pentesters

SMB is often one of the first services enumerated during an internal penetration test because it may expose:

- Shared documents
- Scripts
- Configuration files
- Backups
- Passwords
- Internal tools

In this machine, anonymous SMB access allows us to download an internal application that eventually leads to credential disclosure.

---

# LDAP (Lightweight Directory Access Protocol)

## What is LDAP?

LDAP is the protocol used to communicate with Active Directory.

If Active Directory is considered a database, LDAP is the language used to query that database.

LDAP allows clients to:

- Search users
- Search computers
- Search groups
- Read object attributes
- Authenticate users

Example:

```
Active Directory Database

Users
Computers
Groups
Policies

        ▲
        │
     LDAP Queries
```

---

## Why LDAP Matters

LDAP is one of the richest sources of information during Active Directory enumeration.

Attackers frequently use LDAP to discover:

- Usernames
- Email addresses
- Group memberships
- Descriptions
- Service accounts
- Interesting object attributes

During this machine, LDAP enumeration reveals credentials stored within a user object's **info** attribute.

---

# WinRM (Windows Remote Management)

## What is WinRM?

Windows Remote Management is Microsoft's implementation of the WS-Management protocol.

It allows administrators to remotely execute commands and manage Windows systems using PowerShell.

Default ports:

| Port | Protocol |
|-------|----------|
| 5985 | HTTP |
| 5986 | HTTPS |

---

## Why WinRM Matters

For penetration testers, WinRM is a common method of obtaining an interactive shell after valid credentials have been discovered.

Unlike SMB, WinRM provides a fully interactive PowerShell session.

Throughout this machine, we use **Evil-WinRM** to obtain our initial foothold.

---

# Kerberos

## What is Kerberos?

Kerberos is the default authentication protocol used by modern Active Directory environments.

Unlike NTLM, Kerberos avoids sending passwords across the network.

Instead, it relies on cryptographically signed tickets.

The simplified authentication process looks like this:

```
User
 │
 │ Request Authentication
 ▼
Domain Controller
 │
 │ Issue Ticket Granting Ticket (TGT)
 ▼
User
 │
 │ Request Service Ticket
 ▼
Domain Controller
 │
 │ Issue Service Ticket
 ▼
Requested Service
```

During this machine, we abuse Kerberos extensions known as **S4U2Self** and **S4U2Proxy** to impersonate the domain administrator.

---

# BloodHound

## What is BloodHound?

BloodHound is an Active Directory attack path analysis tool developed by SpecterOps.

Instead of showing a simple list of users and groups, BloodHound builds a graph representing relationships between Active Directory objects.

```
User
   │
GenericAll
   ▼
Computer
   │
AdminTo
   ▼
Server
```

This graph makes privilege escalation paths much easier to identify.

---

## Why BloodHound Matters

BloodHound helps attackers identify:

- Privilege escalation paths
- Delegation abuse
- ACL misconfigurations
- Administrative relationships
- Lateral movement opportunities

In this machine, BloodHound reveals that our compromised user has **GenericAll** permissions over the Domain Controller's computer object, enabling the Resource-Based Constrained Delegation attack.

---

# Resource-Based Constrained Delegation (RBCD)

Resource-Based Constrained Delegation is one of the most powerful delegation mechanisms available in Active Directory.

Unlike traditional delegation, where a trusted computer is configured to delegate access, RBCD allows the **target resource** to decide which computers are trusted.

```
Traditional Delegation

Computer A
      │
Trusted To Delegate
      ▼
Server


Resource-Based Delegation

Server
      │
AllowedToActOnBehalfOfOtherIdentity
      ▼
Computer A
```

If an attacker gains permission to modify this trust relationship, Kerberos can be abused to impersonate privileged users without ever knowing their passwords.

This is the core privilege escalation technique used in this machine.

---

# Prerequisite Skills

Readers are expected to have basic familiarity with:

- Linux command-line navigation
- Basic networking concepts
- TCP/IP fundamentals
- Windows file system
- Basic PowerShell commands
- Common penetration testing workflow

No prior knowledge of Active Directory exploitation is required, as every major concept introduced throughout this writeup will be explained from first principles before being applied in practice.

---

# What You Will Learn

By the end of this writeup, you will understand not only how to compromise the **Support** machine, but also the underlying technologies that make each attack possible.

Rather than memorizing commands, the goal is to build a mental model of how Active Directory, LDAP, Kerberos, and delegation interact, enabling you to recognize and exploit similar misconfigurations in real-world enterprise environments.

# Technologies & Terminology

Before starting the exploitation process, it is essential to understand the terminology that will repeatedly appear throughout this writeup.

Many Active Directory attacks are difficult to understand because they involve multiple components working together. Rather than memorizing commands, understanding these concepts will help explain **why** each step succeeds.

---

# Domain

A **Domain** is a logical boundary used to centrally manage users, computers, and security policies.

Instead of every computer maintaining its own user database, all identities are managed by the Domain Controller.

Example:

```
support.htb

├── Users
├── Computers
├── Groups
├── Servers
└── Policies
```

Every object inside the domain belongs to the same security boundary.

Throughout this machine, the target domain is:

```
support.htb
```

---

# Domain Controller (DC)

The **Domain Controller (DC)** is the server responsible for managing the Active Directory domain.

It provides several critical services:

- User Authentication
- Kerberos
- LDAP
- DNS
- Group Policy
- Active Directory Database

Think of the Domain Controller as the identity server for the entire company.

```
             Domain Controller

        Authentication
             │
             ▼
      Verify Credentials

             │
             ▼
      Issue Kerberos Tickets

             │
             ▼
      Control Permissions
```

Compromising the Domain Controller usually means compromising the entire domain.

---

# Active Directory Object

Everything inside Active Directory is stored as an **Object**.

Examples include:

| Object | Purpose |
|----------|---------|
| User | Employee or service account |
| Computer | Domain-joined machine |
| Group | Collection of users or computers |
| OU | Organizational container |
| Printer | Network printer |
| Shared Folder | Shared resource |

Every object contains multiple **attributes**.

Example:

```
User

Name
Password
Description
SID
MemberOf
mail
telephoneNumber
info
```

During this machine we will enumerate these attributes using LDAP.

---

# Distinguished Name (DN)

Every Active Directory object has a unique path called its **Distinguished Name (DN)**.

Example:

```
CN=Administrator,CN=Users,DC=support,DC=htb
```

Breaking it down:

| Component | Meaning |
|------------|----------|
| CN | Common Name |
| OU | Organizational Unit |
| DC | Domain Component |

Think of it like a complete filesystem path.

Windows example:

```
C:\Users\Administrator
```

LDAP example:

```
CN=Administrator,CN=Users,DC=support,DC=htb
```

Both uniquely identify an object.

---

# LDAP

LDAP stands for:

**Lightweight Directory Access Protocol**

LDAP is the protocol used to communicate with Active Directory.

Instead of querying SQL databases like:

```sql
SELECT * FROM users;
```

LDAP queries directory objects:

```
Users

Computers

Groups

Policies

Printers
```

Example query:

```
Find all users.

Find all computers.

Find all groups.

Find attributes.
```

Throughout this machine we use:

```
ldapsearch
```

to enumerate Active Directory.

---

# LDAP Attribute

Each LDAP object contains multiple attributes.

Example:

```
User

Name
Description
mail
telephoneNumber
info
memberOf
objectSID
```

Some attributes are highly valuable during penetration testing.

Examples:

| Attribute | Why it Matters |
|------------|----------------|
| description | Often contains notes or passwords |
| info | Frequently abused by administrators to store credentials |
| memberOf | Shows group membership |
| objectSID | Unique security identifier |
| servicePrincipalName | Kerberos-enabled services |
| pwdLastSet | Password age |

During this machine we discover a password stored inside the **info** attribute.

---

# SMB Share

A **Share** is a folder exposed over SMB.

Example:

```
\\SERVER\Finance

\\SERVER\HR

\\SERVER\Backups
```

Administrative shares:

```
ADMIN$

C$

IPC$

NETLOGON

SYSVOL
```

Custom shares may expose sensitive files.

In this machine we discover:

```
support-tools
```

which contains an internal application.

---

# Computer Account

Most people know about **User Accounts**, but Windows domains also contain **Computer Accounts**.

Example:

```
Administrator

Alice

Bob
```

These are users.

Computer accounts:

```
PC01$

WEB01$

DC$

FILE01$
```

Notice the trailing:

```
$
```

That symbol identifies the object as a computer account.

Computer accounts:

- Have passwords
- Can authenticate
- Can receive Kerberos tickets
- Possess Security Identifiers (SID)
- Can be assigned permissions

This concept is essential for understanding Resource-Based Constrained Delegation later in the attack.

---

# Machine Account Quota

Active Directory contains an attribute called:

```
ms-DS-MachineAccountQuota
```

By default:

```
10
```

This means any authenticated user can create up to **10 new computer accounts**.

Example:

```
support

↓

Create

↓

FAKEPC$
```

This default configuration is frequently abused during Active Directory attacks.

---

# Security Identifier (SID)

Every security principal in Windows has a unique identifier called a **SID**.

Example:

```
S-1-5-21-1677581083-3380853377-188903654-6101
```

Users have SIDs.

Groups have SIDs.

Computers have SIDs.

Windows permissions are assigned to SIDs—not usernames.

Think of usernames as labels and SIDs as permanent identities.

```
support

↓

Rename Account

↓

support123
```

The username changes.

The SID stays the same.

This is why Windows permissions continue working even after accounts are renamed.

---

# Access Control List (ACL)

Every object in Active Directory has an **Access Control List (ACL)**.

The ACL determines:

```
Who

Can do

What
```

Example:

```
DC$

ACL

Administrator → Full Control

SYSTEM → Full Control

support → GenericAll
```

BloodHound analyzes these relationships to identify privilege escalation opportunities.

---

# GenericAll

One of the most dangerous permissions inside Active Directory is:

```
GenericAll
```

Think of it as:

> **Full Control over an object.**

If you have **GenericAll** over a user, computer, or group, you can usually modify its properties.

For example:

```
support

↓

GenericAll

↓

DC$
```

This allows us to modify the Domain Controller's attributes.

One of those attributes is:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

which becomes the foundation of the RBCD attack.

---

# Service Principal Name (SPN)

A **Service Principal Name (SPN)** uniquely identifies a service in Kerberos.

Example:

```
HTTP/web.support.htb

HOST/dc.support.htb

CIFS/dc.support.htb

LDAP/dc.support.htb
```

When requesting a Kerberos Service Ticket, we request it for a specific SPN.

In this machine we request:

```
cifs/dc.support.htb
```

because we ultimately want to authenticate to SMB using `psexec.py`.

---

# Kerberos Ticket

Kerberos uses tickets instead of transmitting passwords.

The two most common tickets are:

## Ticket Granting Ticket (TGT)

Issued after successful authentication.

```
User

↓

TGT

↓

Request Services
```

---

## Service Ticket (TGS)

Used to access a specific service.

Example:

```
CIFS

HTTP

LDAP

HOST
```

One service ticket cannot normally be reused for another service.

---

# Resource-Based Constrained Delegation (RBCD)

Resource-Based Constrained Delegation allows a resource to define which computers may impersonate users when accessing that resource.

Unlike traditional delegation, the trust relationship is configured **on the target server**.

```
DC$

↓

AllowedToActOnBehalfOfOtherIdentity

↓

FAKEPC$
```

After this relationship is established:

```
FAKEPC$

↓

Administrator Ticket

↓

CIFS

↓

SYSTEM
```

This is the primary privilege escalation technique used in this machine.

---

# SharpHound

SharpHound is the data collector for BloodHound.

Its job is to gather:

- Users
- Groups
- Computers
- Sessions
- ACLs
- Trusts
- Delegation settings

The collected data is exported into JSON files and imported into BloodHound for analysis.

---

# BloodHound

BloodHound is a graph analysis tool for Active Directory.

Rather than showing isolated objects, it visualizes relationships between them.

Example:

```
support

↓

GenericAll

↓

DC$

↓

Administrator
```

This graph reveals attack paths that would be difficult to identify manually.

---

# Impacket

Impacket is a collection of Python tools for interacting with Windows network protocols.

Throughout this machine we use several Impacket utilities:

| Tool | Purpose |
|------|----------|
| addcomputer.py | Create a new machine account |
| rbcd.py | Configure Resource-Based Constrained Delegation |
| getST.py | Request Kerberos service tickets |
| psexec.py | Execute commands remotely using SMB |
| smbclient.py | Interact with SMB shares |

Impacket is one of the most widely used offensive toolkits for Active Directory penetration testing and red team engagements.

---

# Why Understanding These Concepts Matters

At first glance, the exploitation chain in **Support** may appear to be a series of unrelated commands. In reality, every step builds upon the previous one.

- SMB exposes an internal application.
- Reverse engineering reveals LDAP credentials.
- LDAP enumeration discovers a second password.
- WinRM provides an initial foothold.
- BloodHound identifies an ACL misconfiguration.
- GenericAll allows modification of the Domain Controller object.
- RBCD abuses Kerberos delegation.
- Kerberos issues a valid Administrator service ticket.
- Pass-the-Ticket grants SYSTEM access.

Understanding how these technologies interact is far more valuable than memorizing commands. This knowledge forms the foundation for many modern Active Directory attacks encountered in enterprise penetration tests and advanced red team operations.

# Initial Enumeration

Enumeration is the foundation of every penetration test. Before attempting exploitation, we must first understand what services are exposed, identify potential attack surfaces, and collect as much information as possible about the target.

A common mistake among beginners is rushing into exploitation without fully understanding the environment. In reality, experienced penetration testers often spend a significant portion of an engagement performing reconnaissance and enumeration.

For the **Support** machine, our initial objective is to answer several questions:

- What operating system is running?
- Which network services are exposed?
- Which ports are open?
- Is this machine part of an Active Directory environment?
- Which service provides the easiest initial foothold?

---

# Learning Objectives

In this section, we will learn how to:

- Perform TCP port scanning with Nmap.
- Interpret Nmap results.
- Identify services running on the target.
- Understand why each service matters during Active Directory penetration testing.
- Enumerate SMB shares anonymously.
- Discover exposed files that may lead to credential disclosure.

---

# Step 1 — Network Discovery with Nmap

## What is Nmap?

**Nmap (Network Mapper)** is one of the most widely used network reconnaissance tools in cybersecurity.

It is used to discover:

- Live hosts
- Open ports
- Running services
- Service versions
- Operating systems
- Firewall behavior

For penetration testers, Nmap is almost always the first tool executed against a new target.

---

## Why Do We Start with Nmap?

Imagine arriving at a building you've never seen before.

Before attempting to enter, you would naturally ask:

- How many doors are there?
- Which doors are open?
- Which doors are locked?
- Who is inside?

Nmap answers these same questions for computer networks.

Without enumeration, exploitation becomes little more than guessing.

---

## Command Used

```bash
nmap -Pn -sC -sV <TARGET_IP>
```

Example:

```bash
nmap -Pn -sC -sV 10.129.x.x
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| `-Pn` | Treat the host as online and skip host discovery (ICMP ping). |
| `-sC` | Run Nmap's default NSE (Nmap Scripting Engine) scripts. |
| `-sV` | Detect service versions running on open ports. |

---

## Why These Flags?

### `-Pn`

Some environments block ICMP echo requests (ping).

Without `-Pn`, Nmap may incorrectly assume the host is offline.

```
Attacker
     │
Ping
     ▼

Firewall

❌ Blocked

↓

Nmap thinks host is offline
```

Using `-Pn` forces Nmap to continue scanning regardless of ping responses.

---

### `-sC`

The default script set performs lightweight enumeration automatically.

Examples include:

- SMB information
- SSL certificate details
- HTTP titles
- LDAP information

Instead of manually running dozens of scripts, Nmap executes the most useful ones.

---

### `-sV`

Knowing that port **445** is open is helpful.

Knowing that it is running **Microsoft SMB on Windows Server 2022** is far more valuable.

Version detection helps identify:

- Software versions
- Potential vulnerabilities
- Operating system fingerprints
- Attack paths

---

# Scan Results

The scan identified several open ports, including:

| Port | Service | Importance |
|------|----------|------------|
| 53 | DNS | Domain name resolution |
| 88 | Kerberos | Active Directory authentication |
| 135 | MSRPC | Windows Remote Procedure Calls |
| 139 | NetBIOS | Legacy SMB communication |
| 389 | LDAP | Directory services |
| 445 | SMB | File sharing and remote administration |
| 464 | Kerberos Password Change | Kerberos management |
| 593 | RPC over HTTP | Remote procedure communication |
| 636 | LDAPS | Secure LDAP |
| 3268 | Global Catalog | Active Directory searches |
| 3269 | Global Catalog over SSL | Secure directory searches |
| 5985 | WinRM | Remote PowerShell access |

---

# What Do These Ports Tell Us?

At first glance, this may look like just a list of numbers.

However, together they reveal a much bigger picture.

```
53
│
DNS

88
│
Kerberos

389
│
LDAP

445
│
SMB

5985
│
WinRM
```

This combination strongly suggests that the target is a **Windows Active Directory Domain Controller**.

Why?

Because these services commonly work together:

- DNS locates domain resources.
- Kerberos authenticates users.
- LDAP stores directory information.
- SMB provides file sharing.
- WinRM enables remote administration.

Recognizing these patterns is an essential skill for Active Directory penetration testing.

---

# Step 2 — SMB Enumeration

Among all discovered services, SMB immediately stands out as a high-value target.

Why?

Corporate environments frequently use SMB shares to distribute:

- Internal tools
- Installation packages
- Backup files
- Configuration files
- Administrative scripts

Any exposed share may contain sensitive information.

---

# Listing SMB Shares

We first check whether anonymous access is allowed.

Command:

```bash
smbclient -L //<TARGET_IP> -N
```

Example:

```bash
smbclient -L //10.129.x.x -N
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| `-L` | List available SMB shares. |
| `-N` | Authenticate without providing a password (anonymous login). |

---

## Why Try Anonymous Access?

Many organizations intentionally disable anonymous SMB access.

Unfortunately, administrators sometimes expose shares that are readable without authentication.

This can unintentionally leak:

- Internal documentation
- Passwords
- Scripts
- Software
- Source code

Testing anonymous access is quick, safe, and often highly rewarding.

---

# SMB Enumeration Results

The target exposes several standard administrative shares:

```
ADMIN$
C$
IPC$
NETLOGON
SYSVOL
```

Additionally, we discover an interesting custom share:

```
support-tools
```

Unlike the default administrative shares, **support-tools** appears to store files created by the organization's support team.

Custom shares deserve special attention because they often contain internally developed software or sensitive operational data.

---

# Accessing the Share

After identifying the share, we connect to it using:

```bash
smbclient //<TARGET_IP>/support-tools -N
```

Once connected, we can list the available files:

```bash
ls
```

Among the contents, one file immediately attracts our attention:

```
UserInfo.exe.zip
```

---

# Why Is This File Interesting?

Applications developed for internal use often contain:

- Hardcoded credentials
- API keys
- Connection strings
- LDAP credentials
- Database credentials
- Proprietary business logic

Since the file is available through anonymous SMB access, it becomes our primary target for further analysis.

---

# Downloading the File

To retrieve the archive:

```bash
get UserInfo.exe.zip
```

After downloading it, we extract the contents:

```bash
unzip UserInfo.exe.zip
```

This reveals the executable:

```
UserInfo.exe
```

At this stage, we have successfully obtained an internal .NET application without authenticating to the domain.

---

# Pentester Notes

## Red Team Perspective

SMB is one of the most valuable services during internal penetration tests.

Anonymous or weakly protected shares frequently expose:

- Deployment scripts
- Backup files
- Passwords
- Credentials
- Internal applications

Even if no credentials are immediately available, downloaded applications can often be reverse engineered to reveal sensitive information.

---

## Blue Team Perspective

Administrators should:

- Disable anonymous SMB access whenever possible.
- Restrict share permissions using the principle of least privilege.
- Monitor access to sensitive shares.
- Remove outdated internal tools from publicly accessible locations.

---

## Detection Opportunities

Security teams should monitor:

- Anonymous SMB authentication attempts.
- Enumeration of network shares.
- Unusual downloads from internal SMB shares.
- Access to sensitive software repositories.

Relevant Windows Event IDs include:

| Event ID | Description |
|-----------|-------------|
| 5140 | Network share accessed |
| 5145 | Detailed SMB share access |
| 4624 | Successful logon (including network logons) |

---

# Key Takeaways

At the end of this phase, we have learned that:

- Nmap revealed an Active Directory environment.
- SMB enumeration exposed a custom share.
- Anonymous access allowed us to download an internal application.
- Internal applications frequently become excellent reverse engineering targets.
- Proper enumeration often eliminates the need for noisy exploitation.

In the next section, we will reverse engineer **UserInfo.exe** to understand how it works internally and determine whether it exposes sensitive information that can be leveraged for further access.

# Reverse Engineering the Internal Application

At the end of the previous section, we successfully downloaded an internal application named **UserInfo.exe** from the anonymous SMB share.

At first glance, it appears to be just another Windows executable. However, from a penetration tester's perspective, internally developed applications are often among the most valuable assets discovered during enumeration.

Unlike commercial software, internal tools frequently contain:

- Hardcoded credentials
- Database connection strings
- API keys
- LDAP credentials
- Proprietary algorithms
- Sensitive business logic

Rather than attempting to execute the application, we will inspect its source code to understand how it works internally.

---

# Learning Objectives

In this section, we will learn:

- Why reverse engineering is valuable during penetration testing.
- How .NET applications differ from native executables.
- Why ILSpy is an excellent tool for analyzing .NET binaries.
- How Base64 encoding works.
- How XOR encryption works.
- How the application stores LDAP credentials.
- How to recover the plaintext password hidden inside the application.

---

# What is Reverse Engineering?

Reverse engineering is the process of analyzing software to understand how it works without having access to its original source code.

Instead of reading the developer's code directly, we examine the compiled program and reconstruct its logic.

```
Developer

Source Code (.cs)

        │
        ▼

Compiler

        │
        ▼

UserInfo.exe

        │
        ▼

Reverse Engineering

        │
        ▼

Recovered Program Logic
```

For penetration testers, reverse engineering is often used to discover:

- Hardcoded credentials
- Hidden functionality
- Authentication mechanisms
- Cryptographic keys
- Vulnerabilities
- Sensitive configuration values

---

# Why Analyze Internal Applications?

Organizations frequently develop small internal utilities to simplify administrative tasks.

Examples include:

- Password reset tools
- Employee lookup systems
- Inventory applications
- IT support utilities
- Monitoring dashboards

Because these applications are intended only for internal use, developers sometimes prioritize convenience over security.

Common mistakes include:

- Embedding passwords directly into the application.
- Using weak encryption.
- Storing API keys in plaintext.
- Hardcoding LDAP credentials.

This machine intentionally demonstrates one such mistake.

---

# Native Executables vs .NET Applications

Not all Windows executables are created the same.

There are two common categories:

## Native Applications

Written in languages such as:

- C
- C++

Compiled directly into machine code.

```
Source Code

↓

Machine Code

↓

CPU
```

These applications are generally more difficult to reverse engineer because variable names, functions, and program structure are mostly lost during compilation.

Common tools include:

- Ghidra
- IDA Pro
- Binary Ninja

---

## .NET Applications

Written in languages such as:

- C#
- VB.NET
- F#

These applications are **not** compiled directly into machine code.

Instead, they are compiled into an intermediate language called **Microsoft Intermediate Language (MSIL)**.

```
C#

↓

MSIL

↓

CLR

↓

Machine Code
```

At runtime, the **Common Language Runtime (CLR)** translates the intermediate language into native machine code using Just-In-Time (JIT) compilation.

Because much of the application's structure is preserved, .NET programs are significantly easier to analyze than native executables.

---

# Why Use ILSpy?

Since **UserInfo.exe** is a .NET application, we can inspect its code using **ILSpy**.

ILSpy is an open-source .NET decompiler that reconstructs readable C# source code from compiled assemblies.

Instead of viewing raw assembly instructions, we see something very close to the original developer's code.

This dramatically simplifies reverse engineering.

---

# Opening the Application

After opening **UserInfo.exe** in ILSpy, we begin exploring the application's classes and methods.

One class immediately stands out because it contains two interesting variables:

```csharp
private static string enc_password = "...";
private static byte[] key = Encoding.ASCII.GetBytes("armando");
```

At first glance, this reveals two important observations:

1. The application contains an encrypted password.
2. The application also contains the encryption key.

This is a serious security flaw.

---

# Why Is This Dangerous?

Encryption is only effective when the encryption key is protected.

Imagine locking your house and then taping the key directly to the front door.

```
Encrypted Password

        +

Encryption Key

        =

No Security
```

Although the password is technically encrypted, anyone capable of reading the application's code can also recover the key used to decrypt it.

In other words, the application provides everything an attacker needs.

---

# Understanding Base64

The encrypted password appears as a long string of readable characters.

This often indicates **Base64 encoding**.

It is important to understand that:

> **Base64 is NOT encryption.**

Base64 is simply an encoding format that converts binary data into printable ASCII characters.

Example:

```
hello

↓

Base64

↓

aGVsbG8=
```

Anyone can immediately decode Base64 without requiring a password or key.

Developers sometimes mistakenly believe Base64 provides security.

It does not.

---

# Understanding XOR Encryption

After decoding the Base64 string, the resulting bytes still appear unreadable.

This tells us another transformation has been applied.

The application reveals the encryption key:

```
armando
```

This strongly suggests the program uses **XOR encryption**.

---

# What is XOR?

XOR (Exclusive OR) is a simple binary operation.

Its truth table looks like this:

| A | B | XOR |
|---|---|-----|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

One remarkable property makes XOR useful for encryption:

```
A XOR B XOR B = A
```

The same key used to encrypt the data can also decrypt it.

```
Plaintext

↓

XOR Key

↓

Ciphertext

↓

Same XOR Key

↓

Plaintext
```

This is exactly what the application does.

---

# Recovering the Password

By recreating the application's decryption logic, we can:

1. Decode the Base64 string.
2. XOR every byte using the repeating key:

```
armando
```

3. Recover the original plaintext password.

The recovered LDAP password is:

```text
nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
```

At this point, we have successfully extracted valid credentials directly from the application's code without exploiting any software vulnerability.

Instead, we simply analyzed the program exactly as the operating system would.

---

# Why This Worked

The application's security relies on **obfuscation rather than proper secret management**.

Several critical mistakes were made:

- The encrypted password was embedded inside the executable.
- The encryption key was also embedded inside the executable.
- A weak reversible encryption scheme (XOR) was used.
- No external secret management solution was implemented.

Any attacker with access to the binary could recover the original password.

---

# Red Team Perspective

Reverse engineering is an extremely valuable skill during internal penetration tests.

Attackers routinely analyze:

- Desktop applications
- Internal utilities
- Agent software
- Backup tools
- Configuration managers

These applications often reveal credentials that are difficult or impossible to discover through network enumeration alone.

---

# Blue Team Perspective

Developers should never assume compiled applications hide sensitive information.

To reduce risk:

- Never hardcode credentials inside applications.
- Store secrets in secure secret-management systems.
- Use operating system credential stores where appropriate.
- Rotate credentials regularly.
- Review binaries for embedded secrets before deployment.

Even if encryption is used, the encryption key must never be distributed alongside the encrypted data.

---

# Detection Opportunities

Reverse engineering typically occurs offline, making direct detection difficult.

However, defenders can reduce exposure by:

- Monitoring downloads of internal applications.
- Restricting anonymous access to software repositories.
- Performing static secret scanning during CI/CD pipelines.
- Reviewing binaries for embedded credentials before release.

The most effective defense is preventing sensitive information from being embedded into the application in the first place.

---

# Key Takeaways

In this section, we learned that:

- Reverse engineering focuses on understanding how software works internally.
- .NET applications preserve much of their original structure, making them easier to analyze than native binaries.
- ILSpy can reconstruct readable C# code from compiled .NET assemblies.
- Base64 is an encoding format, not encryption.
- XOR is a reversible operation commonly used in weak custom encryption schemes.
- Hardcoded credentials remain one of the most common security mistakes in internally developed software.
- By combining Base64 decoding and XOR decryption, we successfully recovered valid LDAP credentials that will be used in the next phase of the attack.

In the next section, we will use these credentials to authenticate to **LDAP**, enumerate Active Directory objects, and search for additional information that can be leveraged to gain our initial foothold.

# LDAP Enumeration

In the previous section, we successfully recovered a set of credentials by reverse engineering an internal .NET application.

At this point, we possess something far more valuable than an executable—we now have valid credentials that may allow us to communicate directly with Active Directory.

The next logical step is to determine:

- Are the recovered credentials valid?
- Which Active Directory domain do they belong to?
- What information can be enumerated?
- Can we discover additional users or credentials?

To answer these questions, we begin interacting directly with **LDAP**, the primary protocol used to query Active Directory.

---

# Learning Objectives

In this section, we will learn:

- What LDAP is and why it is important.
- How LDAP authentication works.
- How to identify the Active Directory Base DN.
- How to enumerate users using LDAP.
- Why LDAP attributes are valuable during penetration testing.
- How poor operational security can expose credentials inside directory objects.

---

# Why LDAP?

When attacking an Active Directory environment, LDAP is one of the richest sources of information available.

Unlike SMB, which primarily exposes files, LDAP provides structured information about the entire directory.

Examples include:

- Users
- Computers
- Groups
- Organizational Units
- Password policies
- Service accounts
- Group memberships
- Object descriptions
- Custom attributes

Think of LDAP as querying the company's employee database.

```
Active Directory Database

├── Users
├── Computers
├── Groups
├── Policies
└── Attributes

        ▲
        │
      LDAP
```

Instead of guessing usernames or manually browsing the environment, LDAP allows us to retrieve this information directly.

---

# Step 1 — Testing the Credentials

The password recovered from the application appears to be an LDAP credential.

Before performing large-scale enumeration, we first verify that the credentials work.

Command:

```bash
ldapsearch \
-x \
-H ldap://10.129.x.x \
-D "support\\ldap" \
-w 'RecoveredPassword' \
-s base
```

---

## Command Breakdown

| Option | Purpose |
|----------|----------|
| `-x` | Use simple authentication instead of SASL. |
| `-H` | Specify the LDAP server URI. |
| `-D` | Distinguished Name (or account) used for authentication. |
| `-w` | Password used during authentication. |
| `-s base` | Query only the Base DN object. |

---

# Why Query the Base Object?

Before searching the directory, we first need to know **where the directory begins**.

Active Directory stores all objects beneath a root called the **Base Distinguished Name (Base DN)**.

Think of it like the root directory of a filesystem.

Windows:

```
C:\
```

LDAP:

```
DC=support,DC=htb
```

Without knowing the Base DN, we don't know where to start searching.

---

# Initial Problem

Our first authentication attempt did **not** succeed.

Although the credentials appeared valid, LDAP authentication failed because the Domain Controller expected communication using its proper hostname rather than its IP address.

This is a common issue in Active Directory environments.

Many services—including Kerberos and LDAP—depend on correct DNS resolution.

---

# Discovering the Domain Information

After correcting the connection parameters, we successfully queried the Base object.

The response included several important attributes:

```
defaultNamingContext: DC=support,DC=htb

dnsHostName: dc.support.htb
```

---

## Why Is This Important?

### defaultNamingContext

```
DC=support,DC=htb
```

This tells us the root of the Active Directory database.

Every user, computer, and group exists somewhere beneath this location.

```
DC=support,DC=htb

├── Users
├── Computers
├── Groups
└── OUs
```

Nearly every LDAP query from this point forward will use this Base DN.

---

### dnsHostName

```
dc.support.htb
```

This reveals the hostname of the Domain Controller.

Many Active Directory tools—including Kerberos utilities—prefer or even require the correct hostname rather than a raw IP address.

Knowing the fully qualified domain name (FQDN) will prove useful throughout the remainder of the attack.

---

# Step 2 — Enumerating Users

Now that we know the Base DN, we can search for user objects.

Command:

```bash
ldapsearch \
-x \
-H ldap://10.129.x.x \
-D "support\\ldap" \
-w 'RecoveredPassword' \
-b "DC=support,DC=htb" \
"(objectClass=user)"
```

---

## Additional Command Breakdown

| Option | Purpose |
|----------|----------|
| `-b` | Base DN where the search begins. |
| `(objectClass=user)` | LDAP filter that returns user objects only. |

---

# Understanding LDAP Filters

LDAP filters determine which objects are returned.

Examples:

Return every user:

```
(objectClass=user)
```

Return every computer:

```
(objectClass=computer)
```

Return every group:

```
(objectClass=group)
```

Filters make LDAP enumeration highly flexible and extremely powerful.

---

# Enumeration Results

The directory returns information about multiple user accounts.

Each object contains numerous attributes.

Example:

```
cn

mail

description

memberOf

info

telephoneNumber

objectSID
```

Most attributes contain ordinary administrative information.

However, one attribute immediately stands out.

---

# The `info` Attribute

One user object contains the following value:

```
info:
Ironside47pleasure40Watchful
```

At first glance, this may appear to be an arbitrary note.

However, experienced penetration testers quickly recognize that seemingly random strings often turn out to be passwords.

In this case, that assumption proves correct.

The value stored inside the **info** attribute is the password for the **support** user.

---

# Why Is This Such a Serious Mistake?

LDAP attributes are designed to store directory information.

Examples include:

- Office location
- Telephone number
- Department
- Job title
- Email address

Unfortunately, administrators sometimes misuse free-text fields such as:

- description
- comment
- info

to store operational notes.

Examples seen during real-world assessments include:

```
VPN Password:
Summer2024!

Temporary Password:
Welcome123

Admin Password:
P@ssw0rd!

Reset after migration
```

Although convenient for administrators, these notes become visible to anyone with permission to read the directory.

This transforms Active Directory itself into a credential repository.

---

# Why This Worked

No vulnerability was exploited.

Instead:

1. We obtained valid LDAP credentials.
2. LDAP allowed us to read user objects.
3. One administrator had stored a password inside an attribute intended for documentation.

This is a classic example of **credential exposure through poor operational practices** rather than a software flaw.

---

# Red Team Perspective

LDAP enumeration is one of the highest-value activities during internal penetration testing.

Attackers routinely search for:

- Descriptions
- Comments
- Service accounts
- Group memberships
- SPNs
- Password hints
- Misconfigured attributes

Many successful Active Directory compromises begin with information gathered through LDAP rather than through exploitation.

---

# Blue Team Perspective

Administrators should never store sensitive information inside LDAP attributes.

Recommended practices include:

- Treat directory attributes as publicly readable unless absolutely necessary.
- Restrict read permissions where appropriate.
- Regularly audit user objects for sensitive content.
- Use enterprise password managers or secret management platforms instead of directory notes.

Even temporary passwords should never be documented inside Active Directory objects.

---

# Detection Opportunities

LDAP is a legitimate administrative protocol, making detection challenging.

However, defenders can monitor for:

- Large-scale LDAP enumeration.
- Unusual LDAP queries from user workstations.
- Enumeration of every user object.
- Repeated searches targeting descriptive attributes.

Combining LDAP telemetry with authentication logs can help identify suspicious directory reconnaissance.

---

# Key Takeaways

In this section, we learned that:

- LDAP is the primary protocol used to query Active Directory.
- The Base Distinguished Name (Base DN) defines the root of the directory.
- LDAP filters allow precise enumeration of specific object types.
- Directory objects contain numerous attributes that may expose valuable information.
- Administrators sometimes misuse attributes such as **info** to store passwords.
- Using the recovered LDAP credentials, we successfully discovered the password for the **support** domain user.

With valid credentials for a real domain account, we are now ready to attempt interactive access to the target through **Windows Remote Management (WinRM)**, which will become our initial foothold inside the Active Directory environment.

# Initial Access via WinRM

At this stage of the attack, we have successfully obtained valid credentials for the **support** domain user through LDAP enumeration.

The next objective is straightforward:

- Can these credentials authenticate to a Windows service?
- If so, which service provides an interactive shell?

Looking back at our Nmap scan, one service immediately stands out.

```
5985/tcp open  wsman
```

This is **Windows Remote Management (WinRM)**, Microsoft's remote administration service.

If the recovered account has permission to use WinRM, we can obtain our initial foothold without exploiting a single software vulnerability.

---

# Learning Objectives

In this section, we will learn:

- What Windows Remote Management (WinRM) is.
- How WinRM authentication works.
- Why WinRM is frequently targeted during Active Directory penetration tests.
- How Evil-WinRM establishes a remote PowerShell session.
- How to perform basic Windows host enumeration.
- Why privilege enumeration is an important habit after gaining access.

---

# What is WinRM?

Windows Remote Management (WinRM) is Microsoft's implementation of the **WS-Management (WS-Man)** protocol.

It allows administrators to remotely manage Windows computers using PowerShell.

Instead of physically logging into a server, administrators can execute commands remotely.

```
Administrator

        │
        │ WinRM
        ▼

Windows Server

        │
        ▼

PowerShell Session
```

Think of WinRM as the Windows equivalent of SSH on Linux.

---

# Default WinRM Ports

| Port | Protocol |
|-------|----------|
| 5985 | HTTP |
| 5986 | HTTPS |

During enumeration we discovered:

```
5985/tcp
```

which means WinRM is available over HTTP.

---

# Why Do Attackers Love WinRM?

Once valid credentials are obtained, WinRM often provides:

- Interactive PowerShell
- File upload/download
- Command execution
- Script execution
- Process enumeration
- System administration capabilities

Unlike SMB, which mainly provides access to files, WinRM gives attackers a full remote shell.

---

# Introducing Evil-WinRM

Although Windows includes native WinRM clients, penetration testers typically use **Evil-WinRM**.

Evil-WinRM is an open-source tool specifically designed for offensive security engagements.

Features include:

- Interactive PowerShell shell
- File upload
- File download
- Tab completion
- PowerShell script execution
- Credential support
- Kerberos support

It has become one of the standard tools used during Windows and Active Directory penetration testing.

---

# Step 1 — Connecting via WinRM

Using the credentials recovered from LDAP enumeration, we attempt authentication.

Command:

```bash
evil-winrm \
-i 10.129.x.x \
-u support \
-p 'Ironside47pleasure40Watchful'
```

---

# Command Breakdown

| Option | Purpose |
|----------|----------|
| `-i` | Target IP address or hostname |
| `-u` | Username |
| `-p` | Password |

---

# Successful Authentication

Authentication succeeds and we receive an interactive PowerShell session.

```
Evil-WinRM

↓

Authenticated

↓

PowerShell

↓

PS C:\Users\Support\Documents>
```

This is our **initial foothold** inside the Active Directory environment.

Notice that no exploit was required.

The attack relied entirely on:

- Recovering credentials
- Identifying an exposed management service
- Authenticating successfully

---

# Why This Worked

WinRM itself is **not vulnerable**.

The service behaved exactly as intended.

The real issue is that:

- valid credentials were exposed,
- the account was permitted to use WinRM,
- and remote management was accessible from our network location.

This demonstrates an important principle:

> Legitimate administrative services become attack vectors when attackers obtain valid credentials.

---

# Step 2 — Identifying Our Current User

One of the first commands executed after gaining access is:

```powershell
whoami
```

Output:

```
support\support
```

This confirms that we are authenticated as the **support** domain user.

---

# Why Check `whoami`?

Never assume which account is being used.

Windows environments often contain:

- Local users
- Domain users
- Service accounts
- Managed service accounts

Knowing exactly which identity we control is essential before attempting privilege escalation.

---

# Step 3 — Enumerating Group Membership

Next, we inspect the security groups assigned to our account.

Command:

```powershell
whoami /all
```

---

# What Does This Command Show?

Unlike `whoami`, which only displays the username, `whoami /all` provides a detailed overview of the current security context.

It includes:

- User SID
- Group memberships
- User privileges
- Integrity level
- Authentication identifiers

Think of it as an identity card for the current session.

---

# Interesting Groups

Among the listed groups, two entries stand out.

```
Remote Management Users

Shared Support Accounts
```

### Remote Management Users

This group grants permission to authenticate through WinRM.

Without membership in this group (or Administrators), the connection would typically be rejected.

### Shared Support Accounts

Although not immediately useful, custom groups often deserve further investigation because they may possess delegated permissions within Active Directory.

Later, BloodHound will reveal that our user has dangerous rights over another object.

---

# Step 4 — Enumerating User Privileges

Next, we inspect the privileges assigned to the current token.

Command:

```powershell
whoami /priv
```

---

# Interesting Privileges

Among the privileges displayed are:

```
SeMachineAccountPrivilege

SeChangeNotifyPrivilege

SeIncreaseWorkingSetPrivilege
```

At first glance, these privileges may not seem particularly exciting.

However, one deserves special attention.

---

## SeMachineAccountPrivilege

This privilege allows authenticated users to create computer accounts within the domain, provided the **Machine Account Quota** has not been exhausted.

Although it appears harmless, it becomes a key component of the privilege escalation chain later in this machine.

Combined with an ACL misconfiguration, it enables the Resource-Based Constrained Delegation (RBCD) attack.

We will revisit this privilege in detail during the privilege escalation phase.

---

# Step 5 — Host Identification

We also verify the hostname.

Command:

```powershell
hostname
```

Output:

```
dc
```

This confirms that the system we are connected to is the **Domain Controller**.

This is an important observation.

Although we have shell access on the Domain Controller, we are **not** an administrator.

We are simply a low-privileged domain user executing commands on the server.

---

# Why Enumeration Matters

Many beginners stop enumerating as soon as they obtain a shell.

Experienced penetration testers do the opposite.

Every new shell is an opportunity to collect information.

Common post-exploitation questions include:

- Who am I?
- What groups do I belong to?
- What privileges do I have?
- What processes are running?
- Which services are installed?
- Which network connections exist?
- What Active Directory permissions do I possess?

Answering these questions often reveals the next attack path.

---

# Red Team Perspective

WinRM is one of the most valuable services during an internal penetration test.

Because administrators rely on it for daily operations, it often remains enabled across enterprise environments.

Once valid credentials are obtained, attackers frequently prefer WinRM because it provides:

- Stable interactive shells
- Native PowerShell execution
- Easy post-exploitation
- Low operational overhead

---

# Blue Team Perspective

Organizations should carefully control WinRM access.

Recommended practices include:

- Restrict WinRM to administrative jump servers.
- Limit membership of the **Remote Management Users** group.
- Require multi-factor authentication where possible.
- Monitor remote PowerShell activity.
- Disable WinRM on systems where remote administration is unnecessary.

---

# Detection Opportunities

Security teams should monitor for:

- Successful WinRM logons from unusual hosts.
- Remote PowerShell sessions initiated by non-administrative accounts.
- PowerShell execution on Domain Controllers.
- Authentication using accounts that rarely perform remote administration.

Relevant Windows Event IDs include:

| Event ID | Description |
|-----------|-------------|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4688 | Process creation |
| 4103 | PowerShell module logging |
| 4104 | PowerShell script block logging |

Combining WinRM logs with PowerShell logging provides excellent visibility into post-exploitation activity.

---

# Key Takeaways

In this section, we successfully obtained our initial foothold using WinRM.

More importantly, we learned that:

- WinRM is Microsoft's remote management protocol.
- Evil-WinRM provides an interactive PowerShell shell for penetration testers.
- Valid credentials are often more valuable than software exploits.
- Initial access should always be followed by careful host and privilege enumeration.
- Our account possesses **SeMachineAccountPrivilege**, a capability that will later become part of the privilege escalation chain.
- Although we now have access to the Domain Controller, we remain a low-privileged user.

The next step is to determine whether this user has any hidden privileges within Active Directory. To answer that question, we will collect domain data using **SharpHound** and analyze the environment with **BloodHound**.

# Active Directory Attack Path Analysis with BloodHound

At this point, we have successfully obtained an interactive PowerShell session on the Domain Controller as the low-privileged **support** user.

Although we have command execution on the server, we still lack administrative privileges.

The next question becomes:

- Does our user have any hidden privileges inside Active Directory?
- Can our account modify other objects?
- Is there an unintended privilege escalation path?

Answering these questions manually would require inspecting thousands of Active Directory objects and their permissions—a task that is both tedious and error-prone.

Instead, we use **SharpHound** and **BloodHound**, two of the most powerful tools for Active Directory attack path analysis.

---

# Learning Objectives

In this section, we will learn:

- What BloodHound is.
- How SharpHound collects Active Directory data.
- Why graph-based analysis is superior to manual enumeration.
- How Active Directory relationships are represented.
- How to identify privilege escalation paths.
- Why Access Control Lists (ACLs) are critical during internal penetration testing.

---

# Why Traditional Enumeration Isn't Enough

Imagine an organization with:

- 5,000 users
- 2,000 computers
- 800 groups
- Hundreds of Organizational Units
- Thousands of Access Control Entries (ACEs)

Manually answering questions like:

> "Can user A modify computer B?"

would require inspecting thousands of permissions individually.

This quickly becomes impractical.

---

# Introducing BloodHound

BloodHound is an Active Directory attack path analysis tool developed by **SpecterOps**.

Instead of displaying a simple list of users and groups, BloodHound represents Active Directory as a graph.

```
User

    │
    │ GenericAll
    ▼

Computer

    │
    │ AdminTo
    ▼

Server
```

This visualization makes privilege escalation paths much easier to discover.

---

# Why Graphs?

Imagine trying to navigate a city.

You could read every street name individually.

Or you could open Google Maps.

BloodHound acts like Google Maps for Active Directory.

Instead of asking:

> "What permissions does this user have?"

It asks:

> "How can this user eventually become Domain Admin?"

That difference is what makes BloodHound so powerful.

---

# BloodHound Architecture

BloodHound consists of two components.

## SharpHound

Data Collector

Responsible for gathering:

- Users
- Groups
- Computers
- Sessions
- ACLs
- Trusts
- Group Membership
- Delegation Settings

```
Domain Controller

        │

        ▼

SharpHound

        │

Collect Data

        ▼

JSON Files
```

---

## BloodHound

Visualization Engine

Responsible for:

- Importing collected data
- Building relationship graphs
- Identifying attack paths

```
JSON

    │

    ▼

BloodHound

    │

Relationship Graph

    ▼

Privilege Escalation Paths
```

---

# Step 1 — Collecting Active Directory Data

To begin, we upload **SharpHound.exe** to the compromised host.

Once uploaded, we execute:

```powershell
.\SharpHound.exe
```

By default, SharpHound performs a comprehensive collection of Active Directory information.

Depending on the version and collection method, it gathers information about:

- Domain users
- Groups
- Computers
- Sessions
- ACLs
- Group memberships
- Delegation settings
- Trust relationships

---

# What Happens Behind the Scenes?

SharpHound communicates with:

- LDAP
- Active Directory
- Windows APIs

to retrieve information that authenticated users are normally allowed to read.

It does **not** exploit vulnerabilities.

Instead, it simply automates the collection of publicly readable directory information.

```
SharpHound

        │

LDAP Queries

        ▼

Active Directory

        │

Return Objects

        ▼

JSON Output
```

---

# Output

After the collection completes, SharpHound generates several JSON files (or a ZIP archive containing them).

These files describe:

- Users
- Computers
- Groups
- ACLs
- Sessions
- Trusts

No passwords are collected.

Instead, BloodHound focuses on **relationships**.

---

# Step 2 — Importing into BloodHound

After transferring the collected data back to the attack machine, we import it into BloodHound.

BloodHound reads the JSON data and constructs a graph representing the Active Directory environment.

```
JSON

↓

BloodHound

↓

Nodes

↓

Relationships

↓

Attack Paths
```

Instead of manually reading LDAP output, we can now visually explore the environment.

---

# Understanding Nodes

Every object becomes a **node**.

Examples:

```
User

Computer

Group

OU

GPO
```

Example graph:

```
support

Administrator

DC$

Domain Admins

Backup Operators
```

---

# Understanding Edges

Relationships between nodes become **edges**.

Examples include:

```
MemberOf

AdminTo

GenericAll

CanPSRemote

Owns

ForceChangePassword
```

Edges answer questions such as:

- Can this user administer this computer?
- Can this group modify another group?
- Can this account change another account's password?

---

# Step 3 — Investigating the Support User

Searching for the **support** user reveals a particularly interesting relationship.

```
support

↓

GenericAll

↓

DC$
```

This single edge changes the entire attack.

---

# What Does GenericAll Mean?

At first glance:

```
GenericAll
```

looks like just another label.

In reality, it represents one of the most dangerous permissions available in Active Directory.

GenericAll means:

> **Full Control over the target object.**

That includes the ability to modify many of the object's attributes.

---

# Why Is This Dangerous?

The target object is:

```
DC$
```

Notice the trailing:

```
$
```

This indicates that the object is **not a user**.

It is the **computer account** representing the Domain Controller.

```
DC$

↓

Computer Object

↓

Active Directory
```

Every domain-joined Windows computer has a corresponding computer object inside Active Directory.

Those objects also possess permissions.

---

# Why Doesn't This Give Us Administrator Immediately?

This is an important concept.

Having:

```
GenericAll

↓

DC$
```

does **not** mean:

```
SYSTEM
```

Instead, it means we are allowed to modify the Domain Controller's **directory object**.

Those are two completely different things.

Think of it this way.

```
Computer

↓

Physical Machine
```

versus

```
Computer Object

↓

Directory Entry
```

We are controlling the **directory entry**, not the Windows operating system itself.

The challenge is finding an attribute that can be abused to gain code execution.

---

# The Critical Discovery

Among the writable attributes of a computer object is:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

Although the name is intimidating, this attribute controls **Resource-Based Constrained Delegation (RBCD).**

If we can modify it, we can instruct Kerberos to trust another computer account when requesting service tickets.

This becomes the foundation of our privilege escalation.

We will explore this attribute in depth later in the writeup.

---

# Why BloodHound Was Essential

Without BloodHound, discovering this relationship manually would have required:

- Enumerating ACLs
- Parsing Security Descriptors
- Mapping SIDs
- Understanding inheritance
- Correlating permissions

BloodHound performs this analysis automatically and presents the result as an easy-to-understand graph.

---

# Red Team Perspective

BloodHound is one of the most important tools during internal Active Directory assessments.

It helps identify:

- ACL abuse
- Delegation abuse
- Privilege escalation paths
- Lateral movement opportunities
- Hidden administrative relationships

Rather than searching for software vulnerabilities, attackers search for **permission vulnerabilities**.

---

# Blue Team Perspective

Organizations should regularly audit Active Directory permissions.

Special attention should be paid to:

- GenericAll
- GenericWrite
- WriteDACL
- WriteOwner
- ForceChangePassword

Misconfigured ACLs often remain unnoticed for years because they do not generate obvious errors.

Regular BloodHound-style audits can identify dangerous privilege escalation paths before attackers do.

---

# Detection Opportunities

BloodHound itself is passive, but SharpHound generates observable activity.

Defenders can monitor:

- Large volumes of LDAP queries.
- Enumeration of ACLs.
- SharpHound execution.
- Collection of Active Directory relationship data.
- Unexpected LDAP activity originating from workstations or low-privileged users.

Although each LDAP query appears legitimate, the overall pattern of enumeration can indicate reconnaissance.

---

# Key Takeaways

In this section, we learned that:

- BloodHound visualizes Active Directory as a graph of nodes and relationships.
- SharpHound collects directory information using legitimate LDAP queries.
- Graph analysis is significantly more effective than manual ACL inspection.
- The **support** user possesses **GenericAll** over the **DC$** computer object.
- GenericAll grants full control over the Active Directory object—not the operating system itself.
- This permission enables us to modify the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute, laying the groundwork for a **Resource-Based Constrained Delegation (RBCD)** attack.

In the next section, we will take a deeper look at **Access Control Lists (ACLs)** and **GenericAll**, explaining why this seemingly simple permission is powerful enough to compromise an entire Active Directory domain.

# Access Control Lists (ACL) & GenericAll Abuse

After importing the Active Directory data into BloodHound, we discovered the following relationship:

```
support

        │
        │ GenericAll
        ▼

DC$
```

At first glance, this may seem like just another edge in BloodHound's graph.

In reality, this single relationship is the reason we are able to compromise the entire Active Directory domain.

To understand why, we first need to understand how Windows permissions work inside Active Directory.

---

# Learning Objectives

In this section, we will learn:

- What an Access Control List (ACL) is.
- What an Access Control Entry (ACE) is.
- How Windows stores permissions.
- Why permissions are assigned to Security Identifiers (SIDs) instead of usernames.
- What the **GenericAll** permission allows.
- Why GenericAll is considered one of the most dangerous Active Directory permissions.
- How BloodHound identifies ACL abuse paths.

---

# What is an Access Control List (ACL)?

Every object in Active Directory has a security descriptor attached to it.

One component of that security descriptor is the **Access Control List (ACL)**.

The ACL answers a simple question:

> **Who is allowed to do what with this object?**

Think of an office building.

```
Office

Employees

Managers

Security

Visitors
```

Not everyone has access to every room.

Some people can:

- Enter the building
- Unlock offices
- Access the server room
- Modify security systems

Windows works the same way.

Every object has rules defining:

- Who can read it.
- Who can modify it.
- Who owns it.
- Who can change permissions.

Those rules are stored inside the ACL.

---

# Security Descriptor Overview

Every Active Directory object contains a **Security Descriptor**.

```
Security Descriptor

├── Owner
├── Group
├── DACL
└── SACL
```

The two most important components are:

## DACL (Discretionary Access Control List)

Defines:

```
Who

Can

Do

What
```

This is where permissions such as:

- Read
- Write
- GenericAll
- GenericWrite
- WriteOwner
- WriteDACL

are stored.

---

## SACL (System Access Control List)

The SACL is different.

Instead of granting permissions, it defines:

```
What should be audited?
```

Example:

```
Audit

↓

Failed Login

↓

Generate Event Log
```

Blue Teams use SACLs for auditing and detection.

---

# What is an Access Control Entry (ACE)?

An ACL is actually composed of many smaller rules called **Access Control Entries (ACEs).**

Example:

```
DC$

ACL

├── Administrator → Full Control
├── SYSTEM → Full Control
├── Domain Admins → Full Control
├── support → GenericAll
└── Backup Operators → Read
```

Each line is an **ACE**.

Each ACE defines:

```
Identity

+

Permission
```

Together, all ACEs form the complete ACL.

---

# Why Permissions Use SID Instead of Usernames

One of the most common misconceptions among beginners is that Windows permissions are tied to usernames.

They are not.

Permissions are assigned to **Security Identifiers (SIDs).**

Example:

```
support

↓

S-1-5-21-1677581083-3380853377-188903654-1105
```

Windows stores the SID, not the name.

Why?

Imagine an employee changes their username.

```
support

↓

helpdesk
```

If permissions were tied to usernames, every ACL in the domain would need updating.

Instead:

```
Username

↓

Changes

↓

SID

↓

Remains the Same
```

The SID is permanent.

---

# Common Active Directory Permissions

Some of the most important permissions include:

| Permission | Description |
|------------|-------------|
| Read | View object information |
| Write | Modify selected attributes |
| GenericWrite | Modify many attributes |
| WriteOwner | Change object ownership |
| WriteDACL | Modify permissions |
| ResetPassword | Reset another user's password |
| GenericAll | Full control over the object |

Not every permission leads directly to privilege escalation.

Some are harmless.

Others are extremely dangerous.

---

# Understanding GenericAll

Among all Active Directory permissions, **GenericAll** is one of the most powerful.

BloodHound represents it like this:

```
support

↓

GenericAll

↓

DC$
```

This means:

> **The support user has full control over the Active Directory computer object representing the Domain Controller.**

Notice the wording carefully.

It does **not** say:

```
support

↓

Administrator
```

Nor does it say:

```
support

↓

SYSTEM
```

Instead, it refers specifically to the **directory object**.

---

# Computer vs Computer Object

This distinction is extremely important.

There are two different things involved.

## Physical Computer

```
Windows Server

Operating System

Files

Processes

Services
```

---

## Active Directory Object

```
DC$

Attributes

ACL

Delegation

SPN

SID
```

GenericAll gives us control over the **directory object**, not the Windows operating system.

That is why we still cannot:

- Execute code as Administrator
- Read protected files
- Dump LSASS
- Disable Windows Defender

At least, not yet.

---

# So Why Is GenericAll Dangerous?

Because Active Directory objects contain attributes that directly influence authentication.

One of those attributes is:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

This attribute controls **Resource-Based Constrained Delegation (RBCD).**

If we can modify it, we can change which computer accounts Kerberos trusts.

That eventually allows us to impersonate privileged users.

---

# Visualizing the Attack

Initially:

```
support

↓

GenericAll

↓

DC$
```

We use GenericAll to modify:

```
DC$

↓

msDS-AllowedToActOnBehalfOfOtherIdentity

↓

FAKEPC$
```

Kerberos then trusts our fake computer account.

```
FAKEPC$

↓

Request Administrator Ticket

↓

Kerberos

↓

Approved
```

This entire attack becomes possible because of a single ACL entry.

---

# Why Didn't BloodHound Show "Become Administrator"?

BloodHound only reports relationships.

It does not perform exploitation.

It simply informs us:

```
You control this object.
```

As penetration testers, we must understand:

- What the object represents.
- Which attributes are writable.
- Which attributes can be abused.

BloodHound provides the map.

Human knowledge provides the route.

---

# Real-World Examples of GenericAll Abuse

GenericAll is dangerous because it enables many different attacks.

Depending on the target object, attackers may:

## User Object

- Reset passwords
- Modify SPNs
- Perform Kerberoasting
- Configure Shadow Credentials
- Add SSH keys (where applicable)

---

## Group Object

- Add themselves to privileged groups
- Remove administrators
- Modify memberships

---

## Computer Object

- Configure RBCD
- Modify SPNs
- Change delegation settings
- Abuse authentication mechanisms

---

## Organizational Unit (OU)

- Modify child object permissions
- Create new users
- Link Group Policy Objects (depending on delegated rights)

---

# Red Team Perspective

ACL abuse is one of the most effective privilege escalation techniques in modern Active Directory environments.

Unlike software vulnerabilities:

- No exploit is required.
- No malware is required.
- No memory corruption occurs.

Instead, attackers simply use the permissions that already exist.

This makes ACL abuse both reliable and difficult to distinguish from legitimate administrative activity.

---

# Blue Team Perspective

Organizations often focus heavily on patch management while overlooking delegated permissions.

Regular ACL reviews should identify:

- GenericAll
- GenericWrite
- WriteDACL
- WriteOwner
- ResetPassword
- ForceChangePassword

Any unnecessary delegated permission should be removed.

Following the **Principle of Least Privilege** significantly reduces the attack surface.

---

# Detection Opportunities

ACL abuse is difficult to detect because modifying Active Directory objects is a legitimate administrative task.

However, defenders should monitor:

- Changes to computer object attributes.
- Modification of delegation settings.
- Updates to security descriptors.
- Unexpected ACL changes initiated by low-privileged users.

Particular attention should be given to changes involving:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

because legitimate modifications are relatively uncommon.

---

# Key Takeaways

In this section, we learned that:

- Every Active Directory object is protected by an **Access Control List (ACL)**.
- ACLs are composed of individual **Access Control Entries (ACEs)**.
- Windows assigns permissions to **Security Identifiers (SIDs)** rather than usernames.
- **GenericAll** grants full control over an Active Directory object.
- Controlling an object is different from controlling the underlying operating system.
- The **support** user has GenericAll over the **DC$** computer object.
- This permission allows us to modify authentication-related attributes, specifically `msDS-AllowedToActOnBehalfOfOtherIdentity`.
- That seemingly small permission becomes the foundation of the **Resource-Based Constrained Delegation (RBCD)** attack that ultimately leads to full Domain Controller compromise.

In the next section, we will examine **Machine Account Quota**, explaining why ordinary domain users are allowed to create computer accounts and how attackers leverage this feature as the first step in an RBCD attack.

# Machine Account Quota (MAQ)

In the previous section, we discovered that the **support** user has **GenericAll** over the **DC$** computer object.

At first glance, this appears to be everything we need.

However, there is one problem.

Although we have permission to modify the Domain Controller's Active Directory object, **we still need a computer account that Kerberos can trust.**

Unfortunately, we do not control any existing computer accounts.

So the obvious question becomes:

> **Can we create our own?**

Surprisingly, the answer is **yes**.

This is possible because of a default Active Directory feature called **Machine Account Quota (MAQ).**

---

# Learning Objectives

In this section, we will learn:

- What Machine Account Quota (MAQ) is.
- Why authenticated users can create computer accounts.
- Why computer accounts have passwords.
- How computer accounts authenticate to Active Directory.
- Why attackers abuse Machine Account Quota.
- How MAQ becomes the first step of a Resource-Based Constrained Delegation (RBCD) attack.

---

# What is Machine Account Quota?

Machine Account Quota (MAQ) is an Active Directory setting that determines how many **computer accounts** an authenticated user is allowed to create.

The setting is stored in the domain attribute:

```
ms-DS-MachineAccountQuota
```

By default:

```
ms-DS-MachineAccountQuota = 10
```

This means:

> Every authenticated domain user may create up to **10 computer accounts** without requiring Domain Administrator privileges.

---

# Why Does This Feature Exist?

At first glance, allowing ordinary users to create computers sounds like a terrible idea.

To understand why Microsoft introduced this feature, we need to look at how organizations traditionally deployed computers.

Imagine a company purchasing 300 new laptops.

Without Machine Account Quota:

```
New Laptop

↓

IT Ticket

↓

Domain Administrator

↓

Join Computer

↓

Repeat 300 Times
```

This would quickly become a bottleneck.

Instead, Active Directory allows trusted users to join new computers to the domain without requiring Domain Administrator intervention.

```
Employee

↓

Join New Computer

↓

Active Directory

↓

Computer Account Created
```

This significantly simplifies large-scale deployments.

---

# Computer Account vs User Account

Many beginners assume that only users have accounts.

In reality, every domain-joined Windows machine also has its own identity.

Example:

```
Users

Administrator

Alice

Bob
```

```
Computers

DC$

WEB01$

CLIENT01$

FILE01$
```

Notice the trailing:

```
$
```

This suffix indicates that the object represents a **computer account**.

---

# Why Do Computers Need Accounts?

Computers are not just devices.

Within Active Directory, they are treated as security principals.

That means they can:

- Authenticate
- Receive Kerberos tickets
- Possess Security Identifiers (SIDs)
- Own objects
- Receive permissions
- Access network resources

Think of a computer account as the machine's digital identity.

```
Computer

↓

Identity

↓

Kerberos Authentication

↓

Access Resources
```

Without a computer account, a Windows machine cannot fully participate in the Active Directory domain.

---

# Do Computer Accounts Have Passwords?

Yes.

This often surprises newcomers.

Every computer account has its own password.

Unlike user passwords, however, computer account passwords are typically:

- Randomly generated
- Very long
- Managed automatically
- Rotated periodically by Windows

For example:

```
User

↓

Password

↓

Chosen by Human
```

versus

```
Computer

↓

Password

↓

Generated Automatically
```

These passwords are rarely seen by administrators because Windows manages them behind the scenes.

---

# Why Does This Matter?

Kerberos does not distinguish between:

```
User

or

Computer
```

Both are security principals capable of authenticating to the Key Distribution Center (KDC).

This is exactly what makes the upcoming attack possible.

Later, our fake computer account will authenticate exactly like a legitimate domain-joined workstation.

---

# How Machine Account Quota Works

Imagine a new employee receives a laptop.

```
Employee

↓

Join Laptop

↓

Active Directory

↓

Create Computer Object

↓

Assign SID

↓

Generate Password

↓

Ready
```

Internally, Active Directory creates a brand-new computer object with:

- Computer name
- Security Identifier (SID)
- Password
- Service Principal Names (SPNs)
- Default attributes

From Active Directory's perspective, it is a legitimate machine.

---

# Why Attackers Love Machine Account Quota

Attackers quickly realized that Machine Account Quota could be abused.

Instead of joining a real workstation, they simply create a fake one.

```
Attacker

↓

Create

↓

FAKEPC$

↓

Valid Computer Account
```

Although no physical computer exists, Active Directory has no way of distinguishing it from a real machine.

As long as the account authenticates correctly, Kerberos treats it as legitimate.

---

# Why Can't We Skip This Step?

A common beginner question is:

> "Why don't we simply modify the Domain Controller directly?"

Because Resource-Based Constrained Delegation requires **another trusted computer account**.

The trust relationship looks like this:

```
DC$

↓

Trust

↓

Another Computer
```

Not:

```
DC$

↓

Trust

↓

User
```

Kerberos delegation operates between **computer accounts**, not ordinary user accounts.

Therefore, we first need to create a computer account that we control.

---

# The Attack Chain So Far

At this point, the attack looks like this:

```
support

↓

GenericAll

↓

DC$

❌

Need a Computer Account
```

Machine Account Quota solves that problem.

```
support

↓

Create

↓

FAKEPC$

↓

GenericAll

↓

DC$
```

Now we have everything required for the next stage.

---

# Why Microsoft Hasn't Removed MAQ

A natural question is:

> "If attackers abuse Machine Account Quota so often, why does Microsoft keep it?"

The answer is compatibility.

Many organizations still rely on workflows where:

- Helpdesk staff
- Deployment teams
- Automated provisioning systems

join new computers to the domain without Domain Administrator privileges.

Disabling Machine Account Quota by default would break those deployment processes.

Instead, Microsoft leaves the decision to administrators.

---

# Red Team Perspective

Machine Account Quota is frequently abused during Active Directory assessments because it allows attackers to create a legitimate computer identity inside the domain.

Combined with:

- GenericAll
- GenericWrite
- WriteDACL
- Shadow Credentials
- RBCD

it becomes an extremely powerful privilege escalation primitive.

Modern offensive tooling, including **Impacket**, automates this process with a single command.

---

# Blue Team Perspective

Many organizations never need ordinary users to join computers to the domain.

In those environments, administrators should consider reducing:

```
ms-DS-MachineAccountQuota
```

from:

```
10
```

to:

```
0
```

This simple change eliminates one of the prerequisites for several well-known Active Directory attacks.

If delegated computer creation is required, it should be granted explicitly to trusted administrative groups rather than to every authenticated user.

---

# Detection Opportunities

Defenders should monitor for:

- Creation of new computer accounts by non-administrative users.
- Unexpected computer objects appearing in Active Directory.
- Computer names that do not match organizational naming conventions.
- Authentication from newly created machine accounts.

Relevant Windows Event IDs include:

| Event ID | Description |
|-----------|-------------|
| 4741 | A computer account was created |
| 4742 | A computer account was changed |
| 4624 | Successful logon by a machine account |

Unexpected computer creation is relatively uncommon in mature enterprise environments and should always be investigated.

---

# Key Takeaways

In this section, we learned that:

- **Machine Account Quota (MAQ)** controls how many computer accounts an authenticated user may create.
- The setting is stored in the **`ms-DS-MachineAccountQuota`** domain attribute.
- By default, every authenticated user may create **10 computer accounts**.
- Computer accounts are first-class security principals with passwords, SIDs, and Kerberos identities.
- Attackers abuse MAQ to create fake machine accounts that appear legitimate to Active Directory.
- Creating a machine account is a prerequisite for many Resource-Based Constrained Delegation attacks because Kerberos delegation operates between **computer accounts**, not user accounts.

In the next section, we will use **Impacket's `addcomputer.py`** to create our own computer account (`FAKEPC$`) and examine exactly what happens inside Active Directory when a new machine joins the domain.

# Creating a Machine Account with `addcomputer.py`

In the previous section, we learned that **Machine Account Quota (MAQ)** allows authenticated domain users to create new computer accounts.

At this point, our attack chain looks like this:

```
support
        │
        ▼
GenericAll
        │
        ▼
DC$

Need a Computer Account
```

Although we already have permission to modify the Domain Controller's Active Directory object, we still lack a computer account that Kerberos can recognize and trust.

The solution is to create one ourselves.

---

# Learning Objectives

In this section, we will learn:

- What `addcomputer.py` does.
- How computer accounts are created inside Active Directory.
- Why attackers create fake computer accounts.
- The difference between SAMR and LDAP account creation.
- Why computer accounts end with the `$` character.
- What happens internally when a new machine account is created.

---

# Introducing `addcomputer.py`

`addcomputer.py` is part of the **Impacket** toolkit.

Its purpose is simple:

> Create a new computer account inside an Active Directory domain.

Although system administrators normally join computers using the Windows GUI or PowerShell, `addcomputer.py` allows penetration testers to perform the same operation from Linux.

---

# Why Use `addcomputer.py`?

Normally, joining a computer to a domain looks like this:

```
Windows Laptop

        │

Join Domain

        ▼

Domain Controller

        │

Create Computer Object

        ▼

Success
```

With Impacket, we skip the Windows GUI entirely.

```
Linux

↓

addcomputer.py

↓

Domain Controller

↓

Computer Object Created
```

The result is exactly the same.

Active Directory has no way of distinguishing whether the request originated from a real Windows workstation or from Impacket.

---

# Understanding the Command

We use the following command:

```bash
addcomputer.py \
-method SAMR \
-dc-host dc.support.htb \
-computer-name FAKEPC$ \
-computer-pass 'Password123!' \
support.htb/support:'Ironside47pleasure40Watchful'
```

---

# Command Breakdown

| Parameter | Description |
|-----------|-------------|
| `-method SAMR` | Create the computer account using the Security Account Manager Remote protocol. |
| `-dc-host` | Hostname of the Domain Controller. |
| `-computer-name` | Name of the new computer account. |
| `-computer-pass` | Password assigned to the machine account. |
| `support.htb/support` | Domain and username used for authentication. |
| `Password` | Password of the authenticated user. |

---

# Why Use SAMR Instead of LDAP?

`addcomputer.py` supports multiple methods for creating computer accounts.

The two most common are:

- LDAP
- SAMR

---

## LDAP

```
Linux

↓

LDAP

↓

Active Directory
```

LDAP directly modifies directory objects.

Advantages:

- Fast
- Common
- Native Active Directory protocol

Disadvantages:

- May require LDAPS.
- Certificate validation issues can occur.
- More dependent on LDAP configuration.

---

## SAMR

```
Linux

↓

RPC

↓

SAMR

↓

Domain Controller
```

SAMR stands for:

> **Security Account Manager Remote Protocol**

It communicates using Microsoft's Remote Procedure Call (RPC) interface.

Advantages:

- Often works when LDAP creation fails.
- Less dependent on LDAP security configuration.
- Supported by most Windows environments.

During this machine, the LDAP method resulted in an SSL wrapping error, while the SAMR method succeeded immediately.

---

# Why Did LDAP Fail?

Our initial attempt used:

```bash
-method LDAPS
```

The Domain Controller responded with an SSL wrapping error.

Although LDAPS (port 636) was available, the TLS negotiation was unsuccessful.

This was **not** an authentication failure.

Instead, the problem occurred during the secure connection setup.

Switching to the SAMR method bypassed this issue entirely.

---

# Successful Computer Creation

The command returns:

```text
[*] Successfully added machine account FAKEPC$ with password Password123!
```

This confirms that Active Directory accepted our request.

At this moment, **FAKEPC$ becomes a legitimate domain computer account.**

---

# What Happened Behind the Scenes?

Although the command completed in just a few seconds, Active Directory performed several operations.

```
support

        │

Authenticated

        ▼

Active Directory

        │

Create Object

        ▼

CN=FAKEPC,CN=Computers,DC=support,DC=htb

        │

Assign SID

        │

Store Password

        │

Generate SPNs

        ▼

Ready
```

The newly created computer now has:

- A unique Security Identifier (SID)
- A machine password
- Service Principal Names (SPNs)
- Default computer attributes
- A Kerberos identity

From the Domain Controller's perspective, **FAKEPC$ is indistinguishable from a legitimate workstation.**

---

# Why Does the Name End with `$`?

Computer accounts in Active Directory conventionally end with:

```
$
```

Examples:

```
DC$

WEB01$

SQL01$

CLIENT15$

FAKEPC$
```

The `$` suffix tells Active Directory that the object represents a **computer account** rather than a user account.

Although the `$` is part of the account name, many Linux shells interpret it as a variable indicator.

Because of this, it often needs to be:

Escaped:

```bash
FAKEPC\$
```

or quoted:

```bash
'FAKEPC$'
```

to avoid shell expansion.

---

# Do We Control This Computer?

Yes.

Although no physical machine exists, we fully control its identity.

We know:

- The computer name.
- The machine password.
- The machine account credentials.

Later, these credentials will allow us to authenticate to Kerberos exactly like a legitimate Windows computer.

---

# Why Is This Important?

Before this step:

```
support

↓

GenericAll

↓

DC$
```

After this step:

```
support

        │

Create

        ▼

FAKEPC$

        │

GenericAll

        ▼

DC$
```

We now possess **both** ingredients required for the upcoming Resource-Based Constrained Delegation attack:

- A computer account we control.
- Permission to modify the Domain Controller's computer object.

---

# Red Team Perspective

Creating a machine account is a well-known Active Directory attack technique.

Attackers frequently abuse Machine Account Quota to:

- Perform Resource-Based Constrained Delegation (RBCD).
- Abuse Shadow Credentials.
- Configure Kerberos delegation.
- Conduct relay attacks.
- Establish persistence through machine accounts.

Because computer creation is a legitimate Active Directory operation, the activity often blends into normal administrative behavior.

---

# Blue Team Perspective

Organizations should regularly review:

- Newly created computer accounts.
- Accounts created by non-administrative users.
- Unused machine accounts.
- Unexpected naming conventions.

If ordinary users are not expected to join computers to the domain, administrators should set:

```
ms-DS-MachineAccountQuota = 0
```

and delegate computer creation only to trusted IT personnel.

---

# Detection Opportunities

Security teams should monitor for:

- Creation of new computer accounts by low-privileged users.
- Machine accounts created outside standard provisioning workflows.
- Unusual computer names.
- Authentication using recently created machine accounts.

Relevant Windows Event IDs include:

| Event ID | Description |
|-----------|-------------|
| **4741** | A computer account was created |
| **4742** | A computer account was modified |
| **4624** | Successful logon by a machine account |

Correlating these events with subsequent Kerberos activity can help identify attempts to abuse Machine Account Quota.

---

# Key Takeaways

In this section, we successfully created our own computer account using **Impacket's `addcomputer.py`**.

More importantly, we learned that:

- `addcomputer.py` creates legitimate computer accounts inside Active Directory.
- Computer accounts are full-fledged security principals with passwords, SIDs, and Kerberos identities.
- The **SAMR** method communicates over Microsoft's RPC interface and can succeed even when LDAP-based creation fails.
- The `$` suffix identifies an Active Directory object as a computer account.
- By creating **FAKEPC$**, we now control a trusted machine identity within the domain.

Our attack chain is now complete enough to begin abusing **Resource-Based Constrained Delegation (RBCD)**. In the next section, we will explore the theory behind delegation in Kerberos and understand why modifying the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute ultimately allows us to impersonate the Domain Administrator.

# Resource-Based Constrained Delegation (RBCD)

At this stage, we have assembled every prerequisite needed for the final privilege escalation.

So far, we have:

- A low-privileged domain user (**support**)
- **GenericAll** over the **DC$** computer object
- A machine account that we control (**FAKEPC$**)

The only remaining question is:

> **How can controlling one computer account allow us to impersonate the Domain Administrator?**

The answer lies in one of Kerberos' most powerful—and frequently misunderstood—features:

**Delegation.**

To understand the attack, we first need to understand **why delegation exists**, how Kerberos uses it, and how **Resource-Based Constrained Delegation (RBCD)** changes the trust model.

---

# Learning Objectives

In this section, we will learn:

- What delegation is.
- Why delegation exists in Active Directory.
- The different types of delegation.
- What Resource-Based Constrained Delegation (RBCD) is.
- How the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute works.
- Why RBCD is considered one of the most powerful Active Directory privilege escalation techniques.

---

# Why Does Delegation Exist?

Imagine a user accesses a web application.

```
User

↓

Web Server

↓

SQL Server
```

The user authenticates to the web server.

Later, the web server needs to retrieve data from the SQL server **on behalf of the user**.

Without delegation, this becomes a problem.

The SQL server only trusts the user's identity—not the web server's.

```
User

↓

Web Server

❌

SQL Server
```

The web server would have to:

- Ask the user for their password again.
- Store the user's credentials.
- Use service accounts with excessive privileges.

None of these options are desirable.

---

# Delegation Solves This Problem

Delegation allows one service to temporarily act **on behalf of** another user.

```
User

↓

Web Server

↓

Kerberos

↓

SQL Server
```

Instead of knowing the user's password, the web server receives a special Kerberos ticket that represents the user's identity.

This enables **Single Sign-On (SSO)** across multiple services.

---

# An Everyday Analogy

Imagine you authorize a coworker to collect a package for you.

Without delegation:

```
Package

↓

Only You
```

With delegation:

```
Package

↓

Coworker

↓

Acts on Your Behalf
```

The coworker does **not** become you.

They simply receive permission to perform one specific task in your name.

Kerberos delegation follows the same principle.

---

# Types of Delegation

Active Directory supports three primary delegation models.

---

## 1. Unconstrained Delegation

```
User

↓

Server A

↓

Any Service
```

The delegated server can request service tickets for **any** service.

This is extremely powerful and considered highly insecure.

If Server A is compromised, attackers may impersonate users across the domain.

---

## 2. Constrained Delegation

```
User

↓

Server A

↓

Only SQL

Only CIFS

Only HTTP
```

The administrator explicitly specifies which services can be delegated.

This is much safer than unconstrained delegation.

---

## 3. Resource-Based Constrained Delegation (RBCD)

Instead of configuring trust on the **delegating server**, RBCD moves the decision to the **target resource**.

```
Traditional Delegation

Server A

↓

Trust SQL
```

versus

```
RBCD

SQL

↓

Trust Server A
```

The resource decides who may act on behalf of users.

This subtle difference dramatically changes the security model.

---

# The Key Attribute

RBCD is controlled by one Active Directory attribute:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

Although the name is long, its purpose is straightforward.

It stores a list of computer accounts that are allowed to impersonate users **when accessing this computer**.

Think of it as an access control list specifically for delegation.

---

# Visualizing the Attribute

Initially, the Domain Controller's object contains no trusted computers.

```
DC$

↓

Allowed To Act

↓

(None)
```

After modification:

```
DC$

↓

Allowed To Act

↓

FAKEPC$
```

Now Kerberos believes:

> **FAKEPC$ is trusted to request service tickets on behalf of other users when accessing the Domain Controller.**

That single change completely alters the authentication flow.

---

# Why Our GenericAll Permission Matters

Earlier, BloodHound revealed:

```
support

↓

GenericAll

↓

DC$
```

GenericAll allows us to modify writable attributes on the Domain Controller's computer object.

One of those writable attributes is:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

Because we control the object, we can simply add our fake computer account to the list of trusted delegates.

No exploit is necessary.

We are using the permissions that Active Directory already grants us.

---

# What Happens After We Modify the Attribute?

Once the attribute is updated, the trust relationship changes.

Before:

```
FAKEPC$

↓

Request Ticket

↓

Kerberos

↓

Denied
```

After:

```
FAKEPC$

↓

Request Ticket

↓

Kerberos

↓

Approved
```

Kerberos now accepts delegation requests originating from **FAKEPC$** when the target service is hosted on the Domain Controller.

---

# Does This Give Us Administrator Immediately?

Not yet.

Another common misconception is:

> "If I configure RBCD, do I instantly become Domain Admin?"

No.

At this point, we have only established **trust**.

We still need to:

1. Authenticate as **FAKEPC$**.
2. Request a delegated Kerberos service ticket.
3. Specify which user we want to impersonate.
4. Use that ticket to access the Domain Controller.

Only after completing those steps do we obtain administrative access.

---

# Why Is RBCD So Powerful?

RBCD is powerful because it combines several legitimate Active Directory features:

- Machine accounts
- Kerberos delegation
- Access Control Lists
- Service ticket impersonation

None of these components are vulnerabilities on their own.

The privilege escalation occurs when they are combined in an unintended way.

This is a recurring theme in Active Directory security:

> **Misconfigured permissions often create more dangerous attack paths than software bugs.**

---

# Red Team Perspective

Resource-Based Constrained Delegation is one of the most reliable privilege escalation techniques in Active Directory.

Attackers commonly use it when they possess:

- GenericAll
- GenericWrite
- WriteDACL
- WriteOwner

over a computer object.

Modern frameworks such as **Impacket**, **Rubeus**, and **PowerView** automate much of the attack, but understanding the underlying Kerberos workflow remains essential for adapting to different environments.

---

# Blue Team Perspective

Organizations should carefully audit permissions on computer objects.

Particular attention should be paid to:

- GenericAll
- GenericWrite
- WriteDACL

These permissions should rarely be granted to ordinary users.

Administrators should also monitor changes to:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

because legitimate modifications are uncommon and often indicate administrative changes or malicious activity.

Regular BloodHound assessments can help identify dangerous delegation paths before attackers exploit them.

---

# Detection Opportunities

Although configuring RBCD is a legitimate Active Directory operation, it leaves observable traces.

Defenders should monitor for:

- Changes to computer object attributes.
- Modifications to delegation settings.
- Newly established trust relationships between computer accounts.
- Kerberos service ticket requests involving recently modified machine accounts.

Correlating directory changes with unusual Kerberos activity provides strong indicators of an RBCD attack.

---

# Key Takeaways

In this section, we learned that:

- Delegation allows one service to act on behalf of another user.
- Active Directory supports **Unconstrained**, **Constrained**, and **Resource-Based Constrained Delegation (RBCD)**.
- RBCD is controlled by the **`msDS-AllowedToActOnBehalfOfOtherIdentity`** attribute.
- Because the **support** user has **GenericAll** over the **DC$** computer object, we are allowed to modify that attribute.
- Adding **FAKEPC$** to the delegation list establishes a new trust relationship between our machine account and the Domain Controller.
- Configuring RBCD alone does not grant administrative access—it simply prepares the Kerberos infrastructure for impersonation.

In the next section, we will use **Impacket's `rbcd.py`** to modify the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute, enabling **FAKEPC$** to impersonate privileged users when requesting Kerberos service tickets for the Domain Controller.

# Configuring Resource-Based Constrained Delegation with `rbcd.py`

In the previous section, we learned how Resource-Based Constrained Delegation (RBCD) works internally.

At this point, we have everything required to perform the attack.

Current attack chain:

```
support
        │
        ▼
GenericAll
        │
        ▼
DC$
        ▲
        │
    FAKEPC$
```

We already control:

- A low-privileged domain user (`support`)
- A computer account (`FAKEPC$`)
- Full control (`GenericAll`) over the Domain Controller's computer object

The only thing left is to **tell Active Directory that the Domain Controller trusts our fake computer account.**

This is accomplished by modifying the **`msDS-AllowedToActOnBehalfOfOtherIdentity`** attribute using **Impacket's `rbcd.py`**.

---

# Learning Objectives

In this section, we will learn:

- What `rbcd.py` does.
- How `rbcd.py` modifies Active Directory.
- Why `GenericAll` allows us to perform this operation.
- What the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute actually contains.
- What changes inside Active Directory after the command succeeds.
- Why this step prepares—but does not complete—the privilege escalation.

---

# Introducing `rbcd.py`

`rbcd.py` is another tool included in the **Impacket** framework.

Its purpose is to configure **Resource-Based Constrained Delegation** by modifying the delegation attribute of a computer object.

Instead of manually editing Active Directory attributes, `rbcd.py` automates the entire process.

Conceptually, it performs this operation:

```
Computer Object

↓

Modify

↓

msDS-AllowedToActOnBehalfOfOtherIdentity

↓

Done
```

---

# The Command

We execute:

```bash
rbcd.py \
-action write \
-delegate-from FAKEPC$ \
-delegate-to DC$ \
-dc-ip 10.129.x.x \
support.htb/support:'Ironside47pleasure40Watchful'
```

---

# Command Breakdown

| Option | Description |
|---------|-------------|
| `-action write` | Write a new RBCD configuration. |
| `-delegate-from` | Computer account that will receive delegation rights. |
| `-delegate-to` | Target computer object whose delegation settings will be modified. |
| `-dc-ip` | IP address of the Domain Controller. |
| `support.htb/support` | Authenticated domain user performing the modification. |

Notice something important.

We are **not** authenticating as `FAKEPC$`.

We are authenticating as the **support** user because it is the account that possesses the **GenericAll** permission over `DC$`.

---

# What Happens Internally?

Before the command executes:

```
DC$

↓

msDS-AllowedToActOnBehalfOfOtherIdentity

↓

(empty)
```

After execution:

```
DC$

↓

msDS-AllowedToActOnBehalfOfOtherIdentity

↓

FAKEPC$
```

The Domain Controller now records that **FAKEPC$ is trusted to act on behalf of other users when accessing services hosted on DC$.**

---

# Why Were We Allowed to Modify It?

The answer goes back to our BloodHound findings.

```
support

↓

GenericAll

↓

DC$
```

`GenericAll` grants full control over the Active Directory object.

That includes permission to modify writable attributes such as:

- Service Principal Names (SPNs)
- Description fields
- Delegation settings
- Various security-related attributes

One of those writable attributes is:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

Because we own the necessary permission, Active Directory accepts the modification.

No exploit occurs.

No vulnerability is triggered.

The directory simply enforces the permissions that were configured.

---

# What Does the Attribute Store?

Although we often describe it as "a list of trusted computers," the attribute actually stores a **Windows Security Descriptor**.

Inside that security descriptor are **Access Control Entries (ACEs)** referencing the Security Identifiers (SIDs) of trusted computer accounts.

Conceptually, it looks like this:

```
msDS-AllowedToActOnBehalfOfOtherIdentity

↓

Security Descriptor

↓

Allowed SID

↓

SID of FAKEPC$
```

Notice that Active Directory stores the **SID**, not the computer name.

This follows the same security model used throughout Windows.

---

# Successful Configuration

When the command succeeds, `rbcd.py` reports that delegation rights have been successfully written.

Conceptually:

```
support

↓

Modify Attribute

↓

Success
```

At this stage, our fake computer account is officially trusted by the Domain Controller for Resource-Based Constrained Delegation.

---

# Why Doesn't This Give Us Administrator Yet?

This is one of the most common misconceptions about RBCD.

Many beginners assume:

```
Configure RBCD

↓

Administrator
```

That is **not** what happens.

Instead:

```
Configure RBCD

↓

Trust Relationship Established

↓

Request Kerberos Ticket

↓

Impersonate User

↓

Administrator
```

The configuration step only establishes trust.

We still need Kerberos to issue us a delegated service ticket.

---

# Updated Attack Chain

Before this step:

```
support

↓

GenericAll

↓

DC$

FAKEPC$

(No Trust)
```

After this step:

```
support

↓

GenericAll

↓

DC$

▲

Trust

▲

FAKEPC$
```

The infrastructure is now ready.

The only remaining task is to ask Kerberos for a service ticket while impersonating the **Administrator** account.

---

# Why Kerberos Trusts This

When Kerberos later receives a request from **FAKEPC$**, it checks the target computer's delegation configuration.

```
Request

↓

Kerberos

↓

Check

↓

msDS-AllowedToActOnBehalfOfOtherIdentity
```

Since **FAKEPC$** appears in the allowed list, the request is approved.

Kerberos assumes that the administrator intentionally configured this trust relationship.

From Kerberos' perspective, everything is functioning exactly as designed.

---

# Why This Isn't a Kerberos Vulnerability

An important lesson from this machine is that **Kerberos is not broken.**

Kerberos correctly enforces the rules defined in Active Directory.

The real weakness is the **misconfigured permission** that allowed a low-privileged user to modify those rules.

This distinction is critical.

The attack succeeds because of:

- Excessive Active Directory permissions.
- Legitimate Kerberos functionality.

Not because of a flaw in the Kerberos protocol itself.

---

# Red Team Perspective

RBCD has become one of the most common privilege escalation techniques during internal Active Directory engagements.

Once attackers discover write permissions over a computer object, configuring RBCD is often:

- Reliable
- Fast
- Low-noise
- Highly effective

It avoids memory corruption, kernel exploits, and password cracking by leveraging built-in authentication mechanisms.

---

# Blue Team Perspective

Defenders should ensure that low-privileged users cannot modify computer objects unless absolutely necessary.

Particular attention should be paid to permissions that allow modification of:

- `msDS-AllowedToActOnBehalfOfOtherIdentity`
- ACLs
- Delegation settings

Regular permission reviews and BloodHound assessments can identify risky delegation paths before they are exploited.

---

# Detection Opportunities

Changes to the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute are relatively uncommon.

Security teams should monitor for:

- LDAP modifications affecting computer objects.
- Updates to delegation-related attributes.
- Low-privileged accounts modifying Domain Controller objects.
- Subsequent Kerberos service ticket requests originating from newly trusted computer accounts.

Correlating directory modifications with Kerberos activity provides strong indicators of an RBCD attack in progress.

---

# Key Takeaways

In this section, we successfully configured **Resource-Based Constrained Delegation** using **Impacket's `rbcd.py`**.

More importantly, we learned that:

- `rbcd.py` modifies the **`msDS-AllowedToActOnBehalfOfOtherIdentity`** attribute.
- The modification is authorized because the **support** user has **GenericAll** over the **DC$** computer object.
- The attribute stores trusted computer **SIDs** inside a Windows Security Descriptor.
- Configuring RBCD establishes a **trust relationship**, but does not immediately grant administrative privileges.
- Kerberos will later consult this trust relationship when deciding whether to issue delegated service tickets.

The next step is the final stage of the attack: using **Impacket's `getST.py`** to request a Kerberos service ticket that impersonates the **Administrator** account. That ticket will allow us to authenticate to the Domain Controller as if we were the Domain Administrator, completing the privilege escalation.

# Requesting a Kerberos Service Ticket with `getST.py`

At this point, the attack infrastructure is fully prepared.

We have:

- A low-privileged domain user (**support**)
- A machine account that we control (**FAKEPC$**)
- Resource-Based Constrained Delegation (RBCD) configured on the Domain Controller

Our attack chain now looks like this:

```
support

↓

Create

↓

FAKEPC$

↓

Trusted By

↓

DC$
```

However, we still cannot log in as **Administrator**.

The final step is convincing Kerberos to issue us a service ticket that represents the Administrator account.

This is where **Impacket's `getST.py`** comes into play.

---

# Learning Objectives

In this section, we will learn:

- What a Service Ticket (ST) is.
- The difference between a Ticket Granting Ticket (TGT) and a Service Ticket (ST).
- What **S4U2Self** and **S4U2Proxy** are.
- How `getST.py` abuses Resource-Based Constrained Delegation.
- Why Kerberos issues an Administrator ticket even though we do not know the Administrator's password.

---

# Kerberos Refresher

Kerberos uses multiple ticket types.

The two most important are:

```
Ticket Granting Ticket (TGT)

↓

Used to request

↓

Service Tickets (ST)
```

Think of the TGT as your employee ID card.

```
Employee Badge

↓

Reception

↓

Temporary Access Pass

↓

Meeting Room
```

You first prove your identity.

Then you receive temporary access to a specific service.

---

# Ticket Granting Ticket (TGT)

A TGT proves:

> **"I am who I claim to be."**

Example:

```
FAKEPC$

↓

Authenticate

↓

KDC

↓

Receive TGT
```

The TGT does **not** grant access to services.

Instead, it allows additional tickets to be requested later.

---

# Service Ticket (ST)

A Service Ticket is different.

It proves:

> **"I am allowed to access this specific service."**

Example:

```
User

↓

Request CIFS Ticket

↓

KDC

↓

CIFS Service Ticket
```

That ticket can only be used with the specified service.

---

# Introducing S4U

The attack relies on two Kerberos protocol extensions collectively known as **Service for User (S4U)**.

Although the names sound complicated, the concepts are surprisingly simple.

---

## S4U2Self

S4U2Self allows a trusted service to request a ticket **on behalf of another user**.

Conceptually:

```
FAKEPC$

↓

Ask Kerberos

↓

"I need a ticket representing Administrator."

↓

KDC

↓

Returns Ticket
```

Notice that we never provide the Administrator's password.

Kerberos creates the ticket because **FAKEPC$** is a trusted service.

---

## S4U2Proxy

S4U2Self alone is not enough.

The ticket it returns cannot yet be used to access another service.

S4U2Proxy solves this problem.

```
S4U2Self Ticket

↓

S4U2Proxy

↓

CIFS Ticket

↓

Access Domain Controller
```

This second step converts the delegated identity into a service ticket that is valid for the requested service.

---

# Why RBCD Matters

Normally, Kerberos would reject these requests.

```
FAKEPC$

↓

Impersonate Administrator

↓

Denied
```

But earlier, we configured:

```
DC$

↓

Trust

↓

FAKEPC$
```

Now the same request becomes:

```
FAKEPC$

↓

Impersonate Administrator

↓

Approved
```

Kerberos simply follows the trust relationship stored in Active Directory.

---

# Requesting the Ticket

We execute:

```bash
getST.py \
-spn cifs/dc.support.htb \
-impersonate Administrator \
support.htb/FAKEPC\$:'Password123!'
```

---

# Command Breakdown

| Option | Description |
|---------|-------------|
| `-spn` | Service Principal Name of the target service. |
| `-impersonate` | User we want Kerberos to represent. |
| `FAKEPC$` | Machine account we control. |
| `Password123!` | Password of the machine account. |

Notice something important.

We are **not** authenticating as:

```
support
```

We are authenticating as:

```
FAKEPC$
```

This is possible because we created the machine account ourselves and know its password.

---

# Understanding the SPN

The command specifies:

```text
cifs/dc.support.htb
```

This is the **Service Principal Name (SPN)**.

SPNs uniquely identify services within Kerberos.

Examples include:

| SPN | Service |
|------|---------|
| `cifs/server` | SMB file sharing |
| `http/server` | Web server |
| `ldap/server` | LDAP |
| `host/server` | Generic host services |
| `rpc/server` | Remote Procedure Call |

Because we ultimately want SMB-based administrative access, we request a **CIFS** ticket.

---

# What Happens Internally?

Behind the scenes, Kerberos performs several operations.

```
FAKEPC$

↓

Authenticate

↓

Receive TGT

↓

S4U2Self

↓

Administrator Identity

↓

S4U2Proxy

↓

CIFS Service Ticket

↓

Save Ticket
```

Although `getST.py` displays only a few lines of output, it is coordinating multiple Kerberos exchanges with the Key Distribution Center (KDC).

---

# Successful Ticket Retrieval

When the command succeeds, Impacket reports that it has saved a Kerberos credential cache (`ccache`) file.

For example:

```text
Saving ticket in Administrator@cifs_dc.support.htb@SUPPORT.HTB.ccache
```

This file contains the newly issued Kerberos service ticket.

Importantly:

- It is **not** the Administrator's password.
- It is **not** a password hash.
- It is a valid Kerberos credential that proves our identity to the specified service.

---

# Why Didn't We Need the Administrator Password?

This is one of the most fascinating aspects of the attack.

Kerberos is not authenticating **Administrator**.

Instead, Kerberos is authenticating **FAKEPC$**.

```
Authenticate

↓

FAKEPC$
```

Because **FAKEPC$** is trusted for delegation, Kerberos agrees to issue a ticket **representing Administrator**.

The Administrator's credentials are never exposed.

No password is guessed.

No hash is cracked.

Everything occurs through legitimate Kerberos delegation.

---

# The Attack Flow

Putting everything together:

```
support

↓

Create FAKEPC$

↓

Configure RBCD

↓

Authenticate as FAKEPC$

↓

Request Administrator Ticket

↓

Kerberos Approves

↓

Administrator Service Ticket
```

At this point, we effectively possess an Administrator identity for the requested service.

---

# Why This Isn't Pass-the-Hash

Beginners sometimes confuse this attack with Pass-the-Hash.

They are fundamentally different.

Pass-the-Hash:

```
Stolen NTLM Hash

↓

Authenticate
```

RBCD:

```
Legitimate Kerberos Delegation

↓

Issue New Ticket
```

No password hash is stolen.

No password is recovered.

The ticket is generated entirely through authorized Kerberos operations.

---

# Red Team Perspective

`getST.py` is the final offensive step in many RBCD attacks.

Rather than attacking passwords, attackers exploit trust relationships within Kerberos.

This technique is:

- Reliable
- Fast
- Difficult to distinguish from legitimate Kerberos traffic
- Extremely common during Active Directory assessments

Understanding the Kerberos workflow is far more valuable than simply memorizing the command.

---

# Blue Team Perspective

Defenders should monitor for:

- Unusual S4U2Self requests.
- S4U2Proxy activity involving recently created machine accounts.
- Delegation requests targeting privileged users.
- Service tickets issued on behalf of accounts that rarely use delegation.

These patterns are uncommon in most enterprise environments and may indicate an ongoing delegation attack.

---

# Detection Opportunities

Potential indicators include:

- Kerberos service tickets issued through S4U extensions.
- Authentication from newly created machine accounts.
- Administrative access using delegated Kerberos tickets.
- Recent modifications to RBCD-related Active Directory attributes followed by Kerberos ticket requests.

Combining Active Directory change logs with Kerberos authentication events provides valuable visibility into this attack chain.

---

# Key Takeaways

In this section, we successfully requested a delegated Kerberos service ticket using **Impacket's `getST.py`**.

More importantly, we learned that:

- Kerberos distinguishes between **Ticket Granting Tickets (TGTs)** and **Service Tickets (STs)**.
- The **S4U2Self** extension allows a trusted service to request a ticket representing another user.
- The **S4U2Proxy** extension converts that delegated identity into a service ticket for a specific service.
- Because **FAKEPC$** is trusted through Resource-Based Constrained Delegation, Kerberos willingly issues a **CIFS service ticket** representing the **Administrator** account.
- The Administrator's password is never recovered or cracked—the privilege escalation relies entirely on legitimate Kerberos functionality and a misconfigured Active Directory permission.

The next and final exploitation step is to load the generated Kerberos ticket into our session and authenticate to the Domain Controller using **Impacket's `psexec.py`**, ultimately obtaining a SYSTEM shell and completing the compromise.

# Authenticating with the Kerberos Ticket and Obtaining SYSTEM

At this stage, the attack is almost complete.

We have successfully:

- Created our own machine account (`FAKEPC$`)
- Configured Resource-Based Constrained Delegation (RBCD)
- Requested a Kerberos service ticket that impersonates the **Administrator** account

The only remaining task is to **use that ticket**.

Unlike traditional attacks where attackers authenticate with a password or an NTLM hash, we now authenticate using a **Kerberos ticket**.

This technique is commonly known as **Pass-the-Ticket (PtT).**

---

# Learning Objectives

In this section, we will learn:

- What a Kerberos Credential Cache (ccache) file is.
- What Pass-the-Ticket (PtT) means.
- How Linux applications use Kerberos tickets.
- Why the `KRB5CCNAME` environment variable is required.
- How `psexec.py` authenticates using Kerberos.
- Why we obtain a SYSTEM shell instead of just an Administrator shell.

---

# What Did `getST.py` Produce?

After successfully requesting the delegated ticket, Impacket created a file similar to:

```
Administrator@cifs_dc.support.htb@SUPPORT.HTB.ccache
```

This file is called a **Kerberos Credential Cache**.

Think of it as a digital wallet containing Kerberos tickets.

```
Wallet

↓

Cash

↓

Buy Something
```

Similarly:

```
ccache

↓

Kerberos Ticket

↓

Authenticate
```

The ticket has already been issued.

We simply need to present it when accessing the target service.

---

# What is a Credential Cache?

On Linux, Kerberos stores tickets inside a **Credential Cache**.

Instead of asking for your password every time you access a service, applications simply retrieve the ticket from the cache.

```
Application

↓

Read Ticket

↓

Authenticate

↓

Access Granted
```

This mechanism enables seamless Single Sign-On (SSO).

---

# Why Do We Need `KRB5CCNAME`?

Linux applications do not automatically know where your ticket is stored.

Instead, they look for the environment variable:

```bash
KRB5CCNAME
```

This variable tells Kerberos:

> **"Use this credential cache when authenticating."**

Without it, tools such as `psexec.py` would not know which ticket to present.

---

# Exporting the Ticket

We point the environment variable to our newly created cache file.

```bash
export KRB5CCNAME=Administrator@cifs_dc.support.htb@SUPPORT.HTB.ccache
```

---

# Command Breakdown

| Component | Description |
|-----------|-------------|
| `export` | Creates or updates an environment variable in the current shell. |
| `KRB5CCNAME` | Specifies the Kerberos credential cache to use. |
| `.ccache` | Kerberos ticket cache generated by `getST.py`. |

After executing this command, every Kerberos-aware application launched from the current terminal will automatically use the cached ticket.

---

# What Happens Internally?

```
Shell

↓

KRB5CCNAME

↓

Kerberos Library

↓

Read Ticket

↓

Ready
```

Notice that no authentication occurs at this stage.

We are simply telling the operating system where our Kerberos ticket is located.

---

# Introducing `psexec.py`

The final tool used in this machine is **Impacket's `psexec.py`**.

`psexec.py` is an implementation of Microsoft's PsExec concept.

It allows remote command execution over SMB by communicating with Windows administrative services.

Conceptually:

```
Attacker

↓

SMB

↓

Windows Service

↓

Create Process

↓

Interactive Shell
```

---

# Why PsExec Works

PsExec requires administrative privileges because it performs several sensitive operations.

It:

- Connects to the `ADMIN$` share.
- Uploads a temporary service executable.
- Creates a Windows service.
- Starts the service.
- Redirects the input and output back to the attacker.

If the authenticated user is not an administrator, these operations fail.

---

# The Final Command

We execute:

```bash
psexec.py \
-k \
-no-pass \
support.htb/Administrator@dc.support.htb
```

---

# Command Breakdown

| Option | Description |
|---------|-------------|
| `-k` | Use Kerberos authentication instead of NTLM. |
| `-no-pass` | Do not prompt for a password. Use the Kerberos ticket from the credential cache. |
| `Administrator@dc.support.htb` | Target account and host. |

Notice something important.

We do **not** provide:

- A password
- An NTLM hash
- An AES key

Authentication relies entirely on the Kerberos ticket stored in our `.ccache` file.

---

# What Happens Behind the Scenes?

The authentication flow looks like this:

```
psexec.py

↓

Read Ticket

↓

KRB5CCNAME

↓

Kerberos Library

↓

Present CIFS Ticket

↓

Domain Controller

↓

Authentication Successful

↓

SMB Session Established

↓

Create Service

↓

SYSTEM Shell
```

From the Domain Controller's perspective, the presented ticket is a valid service ticket representing the **Administrator** account.

---

# Why Do We Become SYSTEM?

This often surprises beginners.

We impersonated **Administrator**, so why do we receive:

```
NT AUTHORITY\SYSTEM
```

instead of:

```
SUPPORT\Administrator
```

The answer lies in how PsExec operates.

`psexec.py` creates a **Windows Service**.

By default, Windows services execute under the **LocalSystem** account.

```
Administrator

↓

Install Service

↓

Windows

↓

Run Service

↓

NT AUTHORITY\SYSTEM
```

The Administrator account has permission to create services.

The service itself runs as **SYSTEM**, which is the highest local privilege on Windows.

---

# Verifying the Compromise

We can verify our privileges with:

```cmd
whoami
```

Output:

```text
nt authority\system
```

This confirms that we have obtained full control of the Domain Controller.

---

# Retrieving the Root Flag

With SYSTEM privileges, we can access the Administrator's desktop.

```
C:\Users\Administrator\Desktop\
```

Reading the flag:

```powershell
type root.txt
```

Output:

```
e41f03609b27313a1e3845cfa5134cee
```

The machine has now been fully compromised.

---

# Complete Attack Chain

The entire attack can now be visualized as follows:

```
Anonymous SMB

↓

Download UserInfo.exe

↓

Reverse Engineer Binary

↓

Recover LDAP Password

↓

LDAP Enumeration

↓

Recover Support Password

↓

WinRM Shell

↓

BloodHound

↓

GenericAll over DC$

↓

Create FAKEPC$

↓

Configure RBCD

↓

Request Administrator Service Ticket

↓

Load Kerberos Ticket

↓

Pass-the-Ticket

↓

PsExec

↓

SYSTEM

↓

Root Flag
```

Notice that no software exploit was used after the initial foothold.

The compromise relied almost entirely on:

- Weak secret management
- Active Directory misconfigurations
- Kerberos delegation
- Excessive permissions

This makes the machine an excellent example of **identity-based attacks** rather than vulnerability-based attacks.

---

# Red Team Perspective

This machine demonstrates a modern Active Directory privilege escalation chain.

Rather than exploiting memory corruption or unpatched software, the attacker abuses:

- Legitimate authentication mechanisms.
- Active Directory permissions.
- Kerberos delegation.
- Machine account creation.
- Trust relationships.

These attacks are highly reliable because they use built-in Windows functionality instead of unstable exploits.

---

# Blue Team Perspective

Preventing this attack requires defense in depth.

Organizations should:

- Remove unnecessary `GenericAll` permissions.
- Reduce `MachineAccountQuota` to `0` where appropriate.
- Monitor changes to `msDS-AllowedToActOnBehalfOfOtherIdentity`.
- Audit newly created computer accounts.
- Restrict delegation wherever possible.
- Monitor unusual Kerberos delegation requests.
- Review BloodHound attack paths regularly.

No single control would have prevented every step, but multiple layered defenses would have broken the attack chain.

---

# Detection Opportunities

Security teams should investigate combinations of the following activities:

- Creation of new machine accounts.
- Modification of delegation attributes.
- Kerberos S4U2Self and S4U2Proxy requests.
- Administrative SMB authentication using delegated tickets.
- Remote service creation on Domain Controllers.

Although each action may appear legitimate in isolation, their sequence strongly indicates an RBCD-based privilege escalation attack.

---

# Key Takeaways

In this final exploitation stage, we successfully authenticated using a delegated Kerberos service ticket and obtained a **SYSTEM** shell on the Domain Controller.

More importantly, we learned that:

- Kerberos tickets can be reused through **Pass-the-Ticket** without knowing a user's password.
- The `KRB5CCNAME` environment variable tells Kerberos-aware applications which credential cache to use.
- `psexec.py` authenticates using the cached Kerberos ticket and executes commands by creating a temporary Windows service.
- Windows services run as **NT AUTHORITY\SYSTEM**, explaining why we obtained SYSTEM instead of an Administrator shell.
- The complete compromise was achieved without exploiting any software vulnerability. Instead, it relied on credential exposure, Active Directory permission abuse, and legitimate Kerberos delegation features.

This concludes the technical exploitation of the **Support** machine. The attack highlights an essential lesson in Active Directory security: **misconfigured permissions and identity trust relationships can be just as dangerous as unpatched software vulnerabilities, often enabling complete domain compromise without a single traditional exploit.**

# Conclusion

The **Support** machine is an outstanding example of how modern Active Directory attacks rarely rely on software vulnerabilities alone. Instead, it demonstrates how small weaknesses spread across multiple systems can combine into a complete domain compromise.

Throughout this machine, no memory corruption bug was exploited, no kernel vulnerability was abused, and no password hash was cracked. Every stage of the attack relied on legitimate Windows and Active Directory functionality being used in unintended ways.

The attack began with an anonymously accessible SMB share that exposed an internal application. Reverse engineering that application revealed hardcoded LDAP credentials hidden behind weak obfuscation. Those credentials enabled LDAP enumeration, which exposed another password stored insecurely inside an LDAP attribute. That password provided a WinRM foothold on the Domain Controller.

From there, the attack shifted away from credential theft and focused on **identity and permission abuse**. BloodHound revealed that the low-privileged **support** account possessed **GenericAll** over the Domain Controller's computer object. Although this permission did not immediately grant administrative access, it allowed modification of delegation-related attributes within Active Directory.

Using the default **Machine Account Quota**, we created a new computer account that appeared completely legitimate to Active Directory. By configuring **Resource-Based Constrained Delegation (RBCD)**, we established trust between our fake computer account and the Domain Controller. Kerberos then issued a delegated service ticket representing the **Administrator** account without ever exposing or recovering the Administrator's password.

Finally, by authenticating with that Kerberos ticket using **Pass-the-Ticket**, we gained administrative access through **PsExec**, which ultimately resulted in a **SYSTEM** shell on the Domain Controller.

This attack demonstrates a critical lesson about enterprise security:

> **Attackers do not need software vulnerabilities when identity, permissions, and trust relationships are misconfigured.**

Modern Active Directory environments are often compromised not because Windows is insecure, but because permissions accumulate over time, secrets are stored improperly, and administrative trust relationships are not regularly audited.

---

# Skills Gained

Completing this machine strengthened knowledge in multiple cybersecurity domains.

## Enumeration

- Network service enumeration with Nmap
- SMB share enumeration
- LDAP enumeration
- Active Directory reconnaissance
- WinRM service identification

---

## Reverse Engineering

- Reverse engineering .NET applications
- Using ILSpy
- Understanding Intermediate Language (IL)
- Recovering hardcoded secrets
- Base64 decoding
- XOR deobfuscation

---

## Active Directory

- LDAP authentication
- LDAP object enumeration
- Active Directory object structure
- Security Identifiers (SIDs)
- Access Control Lists (ACLs)
- Access Control Entries (ACEs)
- GenericAll permissions
- Computer objects
- Machine Account Quota (MAQ)

---

## Kerberos

- Ticket Granting Tickets (TGT)
- Service Tickets (ST)
- Service Principal Names (SPNs)
- Credential Cache (`ccache`)
- Pass-the-Ticket
- S4U2Self
- S4U2Proxy
- Kerberos delegation
- Resource-Based Constrained Delegation (RBCD)

---

## Privilege Escalation

- BloodHound attack path analysis
- SharpHound data collection
- ACL abuse
- Machine account creation
- RBCD configuration
- Kerberos impersonation
- Remote service creation
- SYSTEM-level compromise

---

## Offensive Tooling

- Nmap
- smbclient
- ldapsearch
- ILSpy
- Evil-WinRM
- SharpHound
- BloodHound
- Impacket
  - `addcomputer.py`
  - `rbcd.py`
  - `getST.py`
  - `psexec.py`

---

# MITRE ATT&CK Mapping

| Tactic | Technique |
|---------|-----------|
| Reconnaissance | Gather Victim Network Information (T1590) |
| Discovery | Network Service Discovery (T1046) |
| Discovery | Remote System Discovery (T1018) |
| Credential Access | Unsecured Credentials (T1552) |
| Credential Access | Credentials in Files (T1552.001) |
| Initial Access | Valid Accounts (T1078) |
| Lateral Movement | Remote Services (T1021) |
| Privilege Escalation | Abuse Elevation Control Mechanism (ACL Abuse) |
| Credential Access | Steal or Forge Kerberos Tickets (T1558) |
| Defense Evasion | Pass-the-Ticket (T1550.003) |
| Persistence | Create or Modify System Process / Machine Accounts |
| Execution | Windows Service (T1569.002) |

---

# Defensive Lessons

The Support machine highlights several defensive best practices that would have disrupted the attack chain.

## Protect Sensitive Credentials

Applications should never contain:

- Hardcoded passwords
- API keys
- Connection strings
- Encryption keys

Secrets should be stored using dedicated secret management solutions such as:

- Azure Key Vault
- AWS Secrets Manager
- HashiCorp Vault

---

## Secure LDAP

Sensitive information should never be stored in attributes such as:

- `info`
- `description`
- `comment`

Even though these fields appear harmless, they are often readable by any authenticated domain user.

---

## Audit Active Directory Permissions

Regular reviews should identify dangerous delegated permissions, including:

- GenericAll
- GenericWrite
- WriteDACL
- WriteOwner
- ForceChangePassword

Tools such as BloodHound can help organizations proactively discover privilege escalation paths before attackers do.

---

## Restrict Machine Account Quota

If ordinary users are not expected to join computers to the domain, administrators should reduce:

```
ms-DS-MachineAccountQuota
```

from:

```
10
```

to:

```
0
```

This simple configuration change prevents attackers from creating rogue machine accounts.

---

## Monitor Delegation Changes

Special attention should be paid to modifications of:

```
msDS-AllowedToActOnBehalfOfOtherIdentity
```

Unexpected changes to this attribute may indicate attempts to configure Resource-Based Constrained Delegation.

---

## Monitor Kerberos Activity

Security teams should investigate:

- S4U2Self requests
- S4U2Proxy requests
- Pass-the-Ticket activity
- Authentication from newly created machine accounts
- Administrative service ticket requests

Correlating these events with recent Active Directory changes provides strong indicators of compromise.

---

# Real-World Relevance

Although this machine is intentionally designed for learning, the techniques demonstrated are directly applicable to real enterprise environments.

Many organizations have experienced Active Directory compromises caused by:

- Weak secret management
- Excessive delegated permissions
- Misconfigured Access Control Lists
- Kerberos delegation abuse
- Poor identity governance

These attacks are frequently observed during professional penetration tests and red team engagements because they leverage legitimate administrative features rather than software exploits.

For defenders, understanding these attack paths is equally important. Effective Active Directory security requires more than patch management—it demands continuous auditing of identities, permissions, delegation settings, and trust relationships.

---

# Final Thoughts

The **Support** machine teaches one of the most important lessons in modern Windows security:

> **Identity is the new security perimeter.**

An attacker does not always need a vulnerability to compromise a domain. Legitimate administrative features—such as LDAP, WinRM, Kerberos, and Active Directory delegation—can become powerful attack vectors when combined with exposed credentials and overly permissive access controls.

By completing this machine, you have gained practical experience in:

- Active Directory enumeration
- Reverse engineering
- LDAP analysis
- WinRM access
- BloodHound attack path analysis
- Access Control List (ACL) abuse
- Machine Account Quota abuse
- Resource-Based Constrained Delegation
- Kerberos internals
- Pass-the-Ticket authentication
- Domain Controller compromise

More importantly, you have learned to think beyond individual vulnerabilities and view an Active Directory environment as a network of interconnected identities, permissions, and trust relationships. Developing this mindset is essential for both penetration testers and defenders, as it enables you to identify and mitigate complex attack paths before they can be exploited.