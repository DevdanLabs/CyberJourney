# Active Directory Basics

## Overview

In this room, I learned the fundamentals of Active Directory (AD), the identity and access management solution used in most Windows enterprise environments. The room covered how domains are structured, how users and computers are managed, how Group Policies are deployed, and how authentication works within an Active Directory environment.

Key topics covered:

* Windows Domains
* Active Directory Domain Services (AD DS)
* Domain Controllers
* Users, Groups, and Computer Objects
* Organizational Units (OUs)
* Group Policy Objects (GPOs)
* Kerberos and NTLM Authentication
* Trees, Forests, and Trust Relationships

---

# Windows Domain Fundamentals

As organizations grow, managing users and computers individually becomes impractical. Windows Domains solve this problem by centralizing administration through Active Directory.

## Active Directory (AD)

Active Directory is a centralized directory service that stores information about:

* Users
* Computers
* Groups
* Printers
* Shared Resources
* Security Policies

## Domain Controller (DC)

A Domain Controller is a server that runs Active Directory services and is responsible for:

* User authentication
* Policy enforcement
* Directory management
* Domain administration

## Benefits of a Windows Domain

* Centralized identity management
* Centralized security policy management
* Consistent authentication across the network
* Simplified administration

---

# Active Directory Objects

## Users

Users are security principals that can be authenticated and granted permissions to access resources.

### User Accounts

Represent employees or individuals who require access to the network.

Examples:

```text
john
mark
sophie
```

### Service Accounts

Used by services and applications.

Examples:

```text
sql_service
web_service
backup_service
```

---

## Computer Objects

Every machine that joins the domain automatically receives a computer account in Active Directory.

Machine accounts follow the naming convention:

```text
COMPUTERNAME$
```

Example:

```text
TOM-PC$
```

Machine account passwords are automatically managed by Active Directory and rotated periodically.

---

## Security Groups

Security Groups simplify permission management by allowing administrators to assign permissions to groups instead of individual users.

Examples:

* IT Support
* VPN Users
* Printer Access
* File Share Access

### Important Default Groups

#### Domain Admins

Full administrative control over the domain.

#### Domain Users

Contains all domain users.

#### Domain Computers

Contains all joined computers.

#### Domain Controllers

Contains all Domain Controllers.

#### Backup Operators

Can access files regardless of normal file permissions.

---

# Organizational Units (OUs)

Organizational Units (OUs) are containers used to organize users and computers within Active Directory.

Example structure:

```text
THM
├── IT
├── Management
├── Marketing
├── R&D
└── Sales
```

OUs simplify administration and policy deployment across departments.

## OU vs Security Group

| Organizational Unit        | Security Group                       |
| -------------------------- | ------------------------------------ |
| Used for policy management | Used for permissions                 |
| A user belongs to one OU   | A user can belong to multiple groups |
| Organizes objects          | Grants access rights                 |

---

# Managing Users in Active Directory

## Removing Unused Organizational Units

An outdated department OU existed in the environment and needed to be removed.

### Issue

The OU could not be deleted because Active Directory protects OUs from accidental deletion.

### Solution

Enable:

```text
View
→ Advanced Features
```

Then:

```text
OU
→ Properties
→ Object
```

Uncheck:

```text
Protect object from accidental deletion
```

After disabling the protection, the OU could be removed successfully.

---

## Updating User Accounts

The user accounts within several departments did not match the organizational chart provided.

Actions performed:

* Removed obsolete user accounts
* Created missing user accounts
* Verified department membership

New users were created through:

```text
Right Click OU
→ New
→ User
```

---

# Delegation of Control

Delegation allows specific administrative tasks to be assigned without granting full Domain Administrator privileges.

## Scenario

Phillip, an IT Support employee, needed permission to reset passwords for Sales department users.

### Steps

```text
Sales OU
→ Right Click
→ Delegate Control
```

Added:

```text
phillip
```

Selected:

```text
Reset user passwords and force password change at next logon
```

---

## Password Reset via PowerShell

Logged in as:

```text
THM\phillip
```

Reset Sophie's password:

```powershell
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

Forced a password change at the next login:

```powershell
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

---

# Managing Computers in Active Directory

By default, all joined computers are placed in the:

```text
Computers
```

container.

To improve organization and future policy management, computers were separated based on their roles.

## Workstations

User devices such as:

* Desktop PCs
* Office Workstations
* Employee Laptops

## Servers

Systems providing services such as:

* Web Servers
* Database Servers
* File Servers

## Domain Controllers

Systems responsible for managing the domain and Active Directory infrastructure.

---

## Creating Organizational Units

Created the following structure:

```text
thm.local
├── Workstations
├── Servers
└── Domain Controllers
```

### Steps

```text
Right Click thm.local
→ New
→ Organizational Unit
```

Created:

```text
Workstations
Servers
```

---

## Moving Computer Objects

Computer objects were moved from the default Computers container.

### Steps

```text
Right Click Computer
→ Move
```

Moved:

* Workstations and laptops → Workstations OU
* Servers → Servers OU

---

# Group Policy Objects (GPOs)

Group Policy Objects allow administrators to enforce configurations and security settings across users and computers.

## Core Concept

```text
OU = Target
GPO = Policy
```

A GPO is created first and then linked to the appropriate OU.

---

# Restrict Control Panel Access

## Objective

Prevent non-IT users from accessing Control Panel and Windows Settings.

## Creating the GPO

Created a new GPO:

```text
Restrict Control Panel Access
```

### Configuration

```text
User Configuration
→ Policies
→ Administrative Templates
→ Control Panel
```

Enabled:

```text
Prohibit access to Control Panel and PC settings
```

### Linked To

* Marketing
* Management
* Sales

The IT department was intentionally excluded.

---

# Auto Lock Screen Policy

## Objective

Automatically lock computers after five minutes of inactivity.

## Creating the GPO

Created:

```text
Auto Lock Screen
```

### Configuration

```text
Computer Configuration
→ Policies
→ Windows Settings
→ Security Settings
→ Local Policies
→ Security Options
```

Modified:

```text
Interactive logon:
Machine inactivity limit
```

Value:

```text
300
```

(300 seconds = 5 minutes)

### Linked To

```text
thm.local
```

This ensured all computer OUs inherited the policy.

---

# Authentication Protocols

## Kerberos Authentication

Kerberos is the default authentication protocol used in modern Windows domains.

### Authentication Flow

1. User authenticates with the Key Distribution Center (KDC)
2. Receives a Ticket Granting Ticket (TGT)
3. Requests a Ticket Granting Service (TGS) ticket
4. Uses the TGS to access services

### Important Components

#### TGT (Ticket Granting Ticket)

Proof that a user has already authenticated.

#### TGS (Ticket Granting Service)

Allows access to a specific service.

---

## NTLM Authentication

NTLM is a legacy authentication protocol retained for compatibility.

It uses a challenge-response mechanism:

1. Client requests authentication
2. Server sends a challenge
3. Client generates a response
4. Domain Controller validates the response

Passwords are never transmitted directly across the network.

---

# Trees, Forests, and Trusts

## Trees

A tree consists of multiple domains that share the same namespace.

Example:

```text
thm.local
├── uk.thm.local
└── us.thm.local
```

Each domain maintains its own users, policies, and administration.

---

## Forests

A forest consists of multiple trees with different namespaces.

Example:

```text
thm.local
mht.local
```

Forests provide a way to integrate multiple organizations or business units into a single Active Directory environment.

---

## Administrative Roles

### Domain Admin

Administrative control over a single domain.

### Enterprise Admin

Administrative control across the entire forest.

---

## Trust Relationships

Trusts allow users from one domain to access resources in another domain.

### One-Way Trust

```text
AAA trusts BBB
```

Users from BBB can be authorized to access resources in AAA.

### Two-Way Trust

Both domains trust each other and can authorize users from the opposite domain.

A trust relationship does not automatically grant access; permissions must still be explicitly assigned.

---

# Challenges Encountered

## RDP Certificate Warning

### Issue

A certificate warning appeared when connecting to the Domain Controller via RDP.

### Resolution

Accepted the certificate warning and continued the connection, as the lab environment uses internal certificates.

---

## Password Change Required at First Logon

### Issue

Sophie’s account required a password change before login.

### Cause

The following setting had been applied:

```powershell
Set-ADUser -ChangePasswordAtLogon $true
```

### Resolution

Changed the password according to the domain password policy requirements.

---

## RDP Session Limit Reached

### Issue

Received the message:

```text
The number of connections to this computer is limited
```

### Cause

Multiple active RDP sessions remained connected.

### Resolution

* Reconnected to existing sessions
* Logged off unused sessions
* Restarted the lab machine when necessary

---

# Conclusion

This room provided a strong foundation in Active Directory administration and enterprise Windows environments.

Key skills and concepts learned include:

* Managing users and groups
* Creating and organizing Organizational Units
* Managing computer objects
* Deploying Group Policy Objects
* Understanding Kerberos and NTLM authentication
* Understanding Trees, Forests, and Trust Relationships

These concepts form the basis for more advanced Active Directory security topics such as enumeration, privilege escalation, Kerberoasting, Pass-the-Hash, Golden Ticket attacks, BloodHound analysis, and enterprise network administration.
