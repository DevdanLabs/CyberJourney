# Security Principles

> Learn the fundamental principles that form the foundation of information security, including the CIA Triad, DAD Triad, security models, defense strategies, trust models, and risk concepts used throughout modern cybersecurity.

---

# Executive Summary

Security is not achieved by deploying a single tool such as a firewall or antivirus. Instead, it is built upon a collection of principles, models, and architectural decisions that work together to protect systems, applications, and data.

In this room, we explored the **CIA Triad**, the three primary security objectives that guide information security: **Confidentiality, Integrity, and Availability**. We then examined the **DAD Triad**, which represents the opposite perspective by describing how attackers compromise these security objectives through **Disclosure, Alteration, and Destruction/Denial**.

The room also introduced several classical **security models**, including the **Bell-LaPadula Model**, **Biba Model**, and **Clark-Wilson Model**, each designed to protect different aspects of information security. Beyond these models, we learned important security principles such as **Defense-in-Depth**, **Zero Trust**, and **Trust but Verify**, which are widely implemented in enterprise environments today.

Finally, we explored the **ISO/IEC 19249** security design principles, distinguished the concepts of **vulnerability**, **threat**, and **risk**, and learned how organizations share security responsibilities when using cloud services through the **Shared Responsibility Model**.

Rather than teaching offensive techniques, this room establishes the theoretical foundation required to understand why security controls exist and how they protect modern information systems.

---

# Learning Objectives

By completing this room, I learned how to:

- Explain the CIA (Confidentiality, Integrity, Availability) security triad.
- Understand the DAD (Disclosure, Alteration, Destruction/Denial) model from an attacker's perspective.
- Differentiate between authenticity, nonrepudiation, utility, and possession.
- Understand the purpose of common security models such as Bell-LaPadula, Biba, and Clark-Wilson.
- Explain the principles of Defense-in-Depth, Zero Trust, and Trust but Verify.
- Understand the architectural and design principles defined in ISO/IEC 19249.
- Distinguish between vulnerability, threat, and risk.
- Understand the Shared Responsibility Model in cloud environments.
- Recognize how these concepts apply to both offensive and defensive cybersecurity.

---

# Room Information

| Category | Details |
|----------|---------|
| Platform | TryHackMe |
| Room | Security Principles |
| Difficulty | Easy |
| Learning Path | Pre Security |
| Focus Area | Information Security Fundamentals |

---

# Prerequisites

Before completing this room, it is helpful to understand:

- Basic computer systems
- Networking fundamentals
- Basic operating system concepts
- Introductory cybersecurity terminology

No hands-on hacking experience is required.

---

# Why This Room Matters

Most cybersecurity tools and techniques exist to achieve one or more security objectives.

For example:

- Encryption protects **Confidentiality**.
- Hashing helps preserve **Integrity**.
- Backups improve **Availability**.
- Multi-Factor Authentication supports **Confidentiality** and **Authenticity**.
- Firewalls and network segmentation implement **Defense-in-Depth**.
- Identity verification systems are fundamental to **Zero Trust**.

Understanding these principles makes it much easier to understand why security controls are implemented in real-world environments.

Instead of memorizing tools, this room teaches **the reasoning behind security**.

---

# Security Concepts Covered

This room introduces several core concepts that appear throughout cybersecurity.

```text
Security Principles
│
├── CIA Triad
│   ├── Confidentiality
│   ├── Integrity
│   └── Availability
│
├── DAD Triad
│   ├── Disclosure
│   ├── Alteration
│   └── Destruction / Denial
│
├── Security Models
│   ├── Bell-LaPadula
│   ├── Biba
│   └── Clark-Wilson
│
├── Security Principles
│   ├── Defense-in-Depth
│   ├── Zero Trust
│   └── Trust but Verify
│
├── ISO/IEC 19249
│
└── Risk Concepts
    ├── Vulnerability
    ├── Threat
    └── Risk
```

---

# Terminology

| Term | Definition |
|------|------------|
| CIA Triad | Three primary objectives of information security: Confidentiality, Integrity, and Availability. |
| DAD Triad | The attacker's perspective consisting of Disclosure, Alteration, and Destruction/Denial. |
| Confidentiality | Preventing unauthorized access to information. |
| Integrity | Ensuring data remains accurate and unmodified without authorization. |
| Availability | Ensuring systems and information remain accessible when required. |
| Authenticity | Verifying that information originates from the claimed source. |
| Nonrepudiation | Preventing an entity from denying actions they previously performed. |
| Security Model | A formal framework that defines how security policies should be enforced. |
| Defense-in-Depth | Applying multiple layers of security controls instead of relying on a single protection mechanism. |
| Zero Trust | A security model based on the principle "Never Trust, Always Verify." |
| Least Privilege | Granting only the minimum permissions necessary to perform a task. |
| Vulnerability | A weakness that can potentially be exploited. |
| Threat | A potential source capable of exploiting a vulnerability. |
| Risk | The likelihood and business impact of a threat exploiting a vulnerability. |

---

# CIA Triad Overview

The CIA Triad is the foundation of information security and defines the three primary objectives that security controls are designed to protect.

```text
                 Confidentiality
                        ▲
                       / \
                      /   \
                     / CIA \
                    /       \
                   /         \
          Integrity──────────Availability
```

Every security mechanism implemented in modern systems—including encryption, authentication, backups, firewalls, monitoring, and access control—supports one or more elements of this triad.

Throughout this writeup, every concept will ultimately relate back to protecting one or more of these three security objectives.

---

> **Pentester Note**
>
> During penetration testing, every finding should be evaluated in terms of its impact on the CIA Triad.
>
> For example:
>
> - SQL Injection → **Confidentiality**
> - Website Defacement → **Integrity**
> - DDoS Attack → **Availability**
>
> Thinking in terms of business impact rather than simply identifying vulnerabilities leads to stronger and more professional security assessments.

# CIA Triad

The **CIA Triad** is the most fundamental concept in information security. Nearly every security control, framework, and technology exists to protect one or more of these three security objectives.

CIA stands for:

- **Confidentiality**
- **Integrity**
- **Availability**

Rather than describing technologies, the CIA Triad defines **what security is trying to achieve**.

```text
                 Confidentiality
                        ▲
                       / \
                      /   \
                     / CIA \
                    /       \
                   /         \
          Integrity──────────Availability
```

A system cannot be considered fully secure if one of these three objectives is not adequately protected.

---

# Confidentiality

## Objective

Confidentiality ensures that information is accessible **only to authorized individuals**.

The goal is to prevent sensitive information from being disclosed to unauthorized users.

---

## Why It Matters

Organizations store large amounts of sensitive information, including:

- Customer records
- Passwords
- Financial information
- Medical records
- Intellectual property
- Source code

If unauthorized individuals can access this information, confidentiality has been compromised.

---

## Real-World Example

Consider an online shopping website.

When purchasing a product, customers enter sensitive information such as:

- Credit card numbers
- Billing addresses
- Personal details

Customers expect this information to be transmitted only to the payment processor.

If attackers intercept or steal this information, the confidentiality of the data is lost.

---

## Common Security Controls

Confidentiality is commonly protected through:

- Encryption
- Passwords
- Multi-Factor Authentication (MFA)
- Access Control Lists (ACLs)
- VPNs
- HTTPS
- Identity and Access Management (IAM)

---

## Common Attacks

Examples of attacks against confidentiality include:

- SQL Injection
- Credential theft
- Packet sniffing
- Data breaches
- Insider threats
- Cloud storage misconfiguration

---

## Pentester Perspective

Many penetration testing activities aim to demonstrate a loss of confidentiality.

Examples include:

- Dumping database contents
- Reading configuration files
- Extracting password hashes
- Accessing sensitive documents

The objective is to prove that unauthorized information disclosure is possible.

---

# Integrity

## Objective

Integrity ensures that information remains **accurate, complete, and unmodified** unless changed by an authorized entity.

The goal is to prevent unauthorized or accidental modification of data.

---

## Why It Matters

Incorrect or manipulated data can have severe consequences.

For example:

- Incorrect financial records
- Modified medical records
- Altered software source code
- Fake audit logs

Even if attackers cannot steal data, modifying it can be equally damaging.

---

## Real-World Example

Suppose an attacker changes the shipping address after a customer places an online order.

The customer successfully pays for the product, but the package is delivered to the attacker's address instead.

Although the shopping website remains operational, the integrity of the order information has been compromised.

---

## Healthcare Example

Medical records must remain accurate.

If an attacker changes:

- Blood type
- Allergy information
- Medication history

Doctors may administer incorrect treatment, potentially endangering patients.

---

## Common Security Controls

Integrity is protected through:

- Hash functions
- Checksums
- Digital signatures
- File Integrity Monitoring (FIM)
- Version control
- Audit logging
- Database constraints

---

## Common Attacks

Examples include:

- Website defacement
- Database manipulation
- Log tampering
- Malware modifying files
- Man-in-the-Middle attacks

---

## Pentester Perspective

Integrity issues commonly appear when attackers can:

- Modify user accounts
- Change configurations
- Alter databases
- Replace application files
- Modify system logs

---

# Availability

## Objective

Availability ensures that systems, applications, and information remain **accessible whenever authorized users need them**.

A secure system is not useful if legitimate users cannot access it.

---

## Why It Matters

Downtime affects:

- Business operations
- Revenue
- Productivity
- Customer trust
- Critical services

Availability is especially important for:

- Hospitals
- Banks
- Emergency services
- Government systems
- Cloud platforms

---

## Real-World Example

Imagine an online shopping platform experiencing a Distributed Denial-of-Service (DDoS) attack.

Customers cannot:

- Browse products
- Log in
- Complete purchases

Although customer data remains confidential and unmodified, the business still suffers because the service is unavailable.

---

## Healthcare Example

A doctor needs to access a patient's medical history during an emergency.

If the hospital database is offline, treatment decisions become more difficult and potentially dangerous.

---

## Common Security Controls

Availability is commonly maintained through:

- Backups
- Disaster Recovery Plans
- Load balancing
- Redundant servers
- RAID storage
- UPS systems
- Monitoring solutions

---

## Common Attacks

Examples include:

- DDoS attacks
- Ransomware
- Hardware failure
- Power outages
- Resource exhaustion
- Database deletion

---

## Pentester Perspective

Although penetration tests generally avoid disrupting production services, findings often demonstrate how an attacker could impact availability if vulnerabilities remain unpatched.

---

# CIA Triad in Practice

Different systems prioritize different elements of the CIA Triad.

| System | Confidentiality | Integrity | Availability |
|----------|:--------------:|:---------:|:------------:|
| Medical Records | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Banking System | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| University Announcement | ⭐ | ⭐⭐⭐ | ⭐⭐ |
| News Website | ⭐ | ⭐⭐ | ⭐⭐⭐ |
| Public Blog | ⭐ | ⭐ | ⭐⭐⭐ |

Security priorities should always be based on the organization's business requirements and the value of the protected assets.

---

# Beyond the CIA Triad

While the CIA Triad forms the foundation of information security, many real-world systems require additional security properties.

Two of the most important are:

- **Authenticity**
- **Nonrepudiation**

---

# Authenticity

Authenticity ensures that data, messages, or files genuinely originate from the claimed source.

In other words, it answers the question:

> **"Can I trust who sent this information?"**

---

## Real-World Example

A company receives an email claiming to be from its bank.

Without authenticity, attackers could easily impersonate the bank using spoofed email addresses.

Authenticity mechanisms verify whether the message truly originated from the legitimate sender.

---

## Common Technologies

Authenticity is commonly achieved through:

- Digital certificates
- Public Key Infrastructure (PKI)
- Digital signatures
- Certificate Authorities (CA)
- Code signing

---

# Nonrepudiation

Nonrepudiation ensures that an individual **cannot deny performing an action** after it has occurred.

This property provides accountability.

---

## Real-World Example

A customer submits an online purchase order for expensive equipment.

Later, they claim:

> "I never placed that order."

Using digital signatures, timestamps, and secure audit logs, the organization can prove that the order genuinely originated from the customer's account.

---

## Common Technologies

Nonrepudiation commonly relies on:

- Digital signatures
- Cryptographic certificates
- Secure audit logs
- Trusted timestamps

---

# Authenticity vs Nonrepudiation

Although closely related, these concepts answer different questions.

| Authenticity | Nonrepudiation |
|--------------|----------------|
| Is the sender genuine? | Can the sender deny sending it later? |
| Verifies identity | Provides accountability |
| Focuses on trust | Focuses on evidence |

---

# Parkerian Hexad

In 1998, Donn Parker proposed expanding the CIA Triad into the **Parkerian Hexad**, introducing two additional security properties.

The six security elements are:

```text
Availability
Integrity
Confidentiality
Authenticity
Utility
Possession
```

---

## Utility

Utility refers to whether information remains **useful and usable**.

Information may still exist but become unusable.

### Example

A laptop uses full-disk encryption.

The hard drive is intact.

However, the encryption key has been permanently lost.

Although the data still exists, it cannot be accessed.

The information has lost its utility.

---

## Possession

Possession refers to **who controls the information**, regardless of whether it has been read.

### Example

An attacker steals an organization's backup drive.

Even if the attacker never decrypts or reads the data, the organization has lost possession of it.

Similarly, ransomware may encrypt files without stealing them, effectively giving attackers control over the organization's access to its own data.

---

# Key Takeaways

- The **CIA Triad** defines the three primary objectives of information security.
- **Confidentiality** protects information from unauthorized disclosure.
- **Integrity** ensures information remains accurate and trustworthy.
- **Availability** ensures systems remain accessible when needed.
- **Authenticity** verifies that information originates from a legitimate source.
- **Nonrepudiation** prevents individuals from denying their actions.
- The **Parkerian Hexad** expands traditional security thinking by introducing **Utility** and **Possession**, providing a broader view of information protection.

# DAD Triad

The **DAD Triad** represents the attacker's perspective of information security.

While the **CIA Triad** defines what defenders aim to protect, the **DAD Triad** describes how attackers compromise those security objectives.

DAD stands for:

- **Disclosure**
- **Alteration**
- **Destruction / Denial**

```text
                Disclosure
                     ▲
                    / \
                   /   \
                  / DAD \
                 /       \
                /         \
      Alteration────────Destruction
             (Denial)
```

The relationship between CIA and DAD is straightforward.

| CIA Objective | Opposite Attack |
|--------------|-----------------|
| Confidentiality | Disclosure |
| Integrity | Alteration |
| Availability | Destruction / Denial |

---

# Disclosure

## Objective

Disclosure occurs when confidential information becomes accessible to unauthorized individuals.

This attack directly compromises **Confidentiality**.

---

## Real-World Example

Suppose attackers successfully breach an e-commerce database containing:

- Customer names
- Password hashes
- Credit card information
- Billing addresses

Even if the website continues operating normally, exposing this information represents a serious confidentiality breach.

---

## Healthcare Example

Medical records contain highly sensitive patient information.

If attackers steal and publish patient records online, hospitals may face:

- Legal consequences
- Financial penalties
- Loss of reputation
- Regulatory investigations

---

## Common Attacks

Examples include:

- SQL Injection
- Database dumping
- Credential theft
- Packet sniffing
- Cloud storage exposure
- Insider data theft

---

# Alteration

## Objective

Alteration refers to the unauthorized modification of information.

This attack directly compromises **Integrity**.

---

## Real-World Example

An attacker changes a customer's shipping address after checkout.

Although payment succeeds, the purchased item is delivered to the wrong address.

The order still exists—but its integrity has been compromised.

---

## Healthcare Example

An attacker modifies a patient's medical record by changing:

- Blood type
- Allergy information
- Medication dosage

Doctors relying on this inaccurate information may unintentionally provide dangerous treatment.

---

## Common Attacks

Examples include:

- Website defacement
- Database manipulation
- Log tampering
- Malware modifying files
- DNS poisoning
- Configuration changes

---

# Destruction / Denial

## Objective

Destruction or Denial prevents legitimate users from accessing systems or information.

This attack targets **Availability**.

---

## Destruction vs Denial

Although often grouped together, they describe slightly different situations.

### Destruction

The data or service is permanently damaged or removed.

Examples:

- File deletion
- Database deletion
- Disk destruction

---

### Denial

The data still exists but cannot be accessed.

Examples:

- DDoS attacks
- Ransomware encryption
- Resource exhaustion

---

## Healthcare Example

Imagine a hospital that stores all patient records electronically.

If attackers successfully disable the database servers, doctors lose access to:

- Medical history
- Lab results
- Allergies
- Current medications

Even though the data may still exist, healthcare services become severely disrupted.

---

# CIA vs DAD

The CIA and DAD Triads describe the same security concepts from opposite perspectives.

```text
Defender's Goal (CIA)

Confidentiality
Integrity
Availability

          ▲
          │ Protect
          ▼

Attacker's Goal (DAD)

Disclosure
Alteration
Destruction / Denial
```

Security professionals continuously work to maintain CIA while preventing DAD attacks.

---

# Security Models

Security objectives such as Confidentiality and Integrity require formal methods to enforce them.

These methods are known as **Security Models**.

A security model defines rules that determine:

- Who may access information
- What actions are permitted
- How security policies are enforced

This room introduces three classical security models:

- Bell-LaPadula Model
- Biba Model
- Clark-Wilson Model

---

# Bell-LaPadula Model

## Purpose

The **Bell-LaPadula (BLP) Model** focuses on protecting **Confidentiality**.

Originally developed for military environments, it prevents sensitive information from flowing to users with lower security clearances.

---

## Core Rules

### Simple Security Property

Also known as:

> **No Read Up**

Subjects cannot read data at a higher security classification.

```text
Top Secret
     ▲
     │ ❌
Secret User
```

---

### Star (*) Property

Also known as:

> **No Write Down**

Subjects cannot write information to lower security levels.

```text
Top Secret User
        │
        ▼
 Public File

❌ Not Allowed
```

This prevents confidential information from leaking into less secure domains.

---

## Memory Tip

Bell-LaPadula can be summarized as:

```text
Read Down

Write Up
```

or

```text
No Read Up

No Write Down
```

---

## Real-World Example

Government agencies classify information using security levels such as:

- Public
- Confidential
- Secret
- Top Secret

Personnel may only access information appropriate to their security clearance.

---

## Limitations

Bell-LaPadula primarily protects confidentiality and does not adequately address:

- Data integrity
- Modern collaboration
- File sharing environments

---

# Biba Model

## Purpose

The **Biba Model** focuses on protecting **Integrity**.

Rather than preventing information disclosure, it prevents low-integrity information from contaminating trusted data.

---

## Core Rules

### Simple Integrity Property

Also known as:

> **No Read Down**

High-integrity subjects should not read low-integrity data.

This prevents trusted systems from relying on untrusted information.

---

### Star Integrity Property

Also known as:

> **No Write Up**

Low-integrity subjects cannot modify high-integrity data.

This protects important information from unauthorized modification.

---

## Memory Tip

Biba can be summarized as:

```text
Read Up

Write Down
```

or

```text
No Read Down

No Write Up
```

---

## Real-World Example

A normal user should never be able to modify:

- Financial records
- Operating system files
- Medical databases

Only trusted processes should update high-integrity information.

---

## Limitations

The Biba Model does not adequately address insider threats or many practical business workflows.

---

# Bell-LaPadula vs Biba

| Bell-LaPadula | Biba |
|--------------|-------|
| Protects Confidentiality | Protects Integrity |
| No Read Up | No Read Down |
| No Write Down | No Write Up |
| Prevents information leakage | Prevents information corruption |

Notice that the two models are almost mirror images of one another.

---

# Clark-Wilson Model

## Purpose

Like Biba, the **Clark-Wilson Model** focuses on **Integrity**.

However, instead of relying solely on security labels, it protects integrity through controlled business processes.

It is much more practical for commercial environments.

---

# Core Concepts

## Constrained Data Item (CDI)

Critical information whose integrity must always be preserved.

Examples include:

- Bank balances
- Payroll records
- Inventory databases
- Medical records

---

## Unconstrained Data Item (UDI)

Input originating from outside trusted systems.

Examples include:

- User input
- Uploaded files
- API requests
- Keyboard input

Because UDIs are untrusted, they must be validated before affecting protected data.

---

## Transformation Procedure (TP)

A trusted operation that modifies protected data.

Examples:

- Bank transfer
- Payroll calculation
- Purchase approval

Transformation Procedures ensure that all modifications follow authorized business rules.

---

## Integrity Verification Procedure (IVP)

Procedures that verify whether protected data remains valid.

Examples include:

- Database consistency checks
- File integrity verification
- Financial reconciliation
- Audit validation

---

# Banking Example

Consider an online money transfer.

```text
Customer Input
      │
      ▼
     UDI
      │
Input Validation
      │
      ▼
 Transformation Procedure
      │
      ▼
     CDI Updated
      │
      ▼
Integrity Verification
```

The customer's input cannot directly modify account balances.

Instead, it must pass through trusted business logic before updating protected financial records.

---

# Comparison of Security Models

| Model | Primary Objective | Main Rules | Common Usage |
|-------|-------------------|------------|--------------|
| Bell-LaPadula | Confidentiality | No Read Up, No Write Down | Military, Government |
| Biba | Integrity | No Read Down, No Write Up | Trusted systems |
| Clark-Wilson | Integrity | CDI, UDI, TP, IVP | Banking, Finance, Enterprise Applications |

---

# Pentester Notes

Understanding security models helps explain **why security controls exist**, not just **how they work**.

For example:

- Access control preventing ordinary users from reading classified documents reflects **Bell-LaPadula**.
- Restricting ordinary users from modifying system files follows **Biba**.
- Banking applications validating transactions before updating account balances implement the **Clark-Wilson Model**.

Although these models are rarely implemented exactly as described today, they continue to influence modern operating systems, enterprise applications, identity management solutions, and access control mechanisms.

# Defence-in-Depth

Security should never rely on a single protection mechanism.

A firewall, antivirus, or strong password alone cannot completely protect a system. Every security control has weaknesses, can be bypassed, or may eventually fail.

**Defense-in-Depth (DiD)** is the security principle of implementing **multiple layers of security controls** so that if one layer is compromised, additional layers continue protecting the organization's assets.

This strategy is often referred to as **Layered Security** or **Multi-Level Security**.

---

## Why Defense-in-Depth Matters

Imagine storing valuable documents inside your home.

Would a single drawer lock be enough?

Probably not.

A more secure approach would involve multiple protective layers.

```text
Street
   │
   ▼
Building Gate
   │
   ▼
Front Door
   │
   ▼
Bedroom Door
   │
   ▼
Locked Drawer
   │
   ▼
Important Documents
```

Additional controls could include:

- CCTV cameras
- Alarm systems
- Motion sensors
- Security guards

Even if an attacker bypasses one layer, several others remain.

---

## Security Layers

Defense-in-Depth is commonly implemented across several layers.

```text
+-------------------------+
| Physical Security       |
+-------------------------+
| Perimeter Security      |
+-------------------------+
| Network Security        |
+-------------------------+
| Endpoint Security       |
+-------------------------+
| Application Security    |
+-------------------------+
| Identity & Access       |
+-------------------------+
| Data Security           |
+-------------------------+
```

Each layer contributes to protecting one or more elements of the CIA Triad.

---

## Examples of Layered Security

| Security Layer | Example Controls |
|----------------|------------------|
| Physical | Locks, CCTV, Security Guards, Access Cards |
| Network | Firewalls, IDS, IPS, VLANs |
| Endpoint | Antivirus, EDR, Disk Encryption |
| Identity | MFA, RBAC, Least Privilege |
| Application | Input Validation, WAF, Secure Coding |
| Data | Encryption, Backups, Digital Signatures |

---

## Real-World Example

Suppose a company hosts a web application.

Instead of relying solely on a firewall, the infrastructure may look like this:

```text
Internet
     │
Firewall
     │
Web Application Firewall
     │
Authentication
     │
Multi-Factor Authentication
     │
Role-Based Access Control
     │
Encrypted Database
     │
Monitoring & Logging
```

Even if one control fails, attackers still encounter additional security barriers.

---

## Benefits

Defense-in-Depth provides several advantages:

- Reduces single points of failure.
- Slows attackers during an intrusion.
- Improves detection opportunities.
- Limits the impact of compromised systems.
- Supports Confidentiality, Integrity, and Availability simultaneously.

---

> **Pentester Note**
>
> During a penetration test, identifying a vulnerability is only part of the assessment.
>
> A mature organization may still prevent successful exploitation through additional security layers such as:
>
> - Multi-Factor Authentication
> - Network segmentation
> - Endpoint Detection & Response (EDR)
> - Web Application Firewalls (WAF)
> - Least Privilege
> - SIEM monitoring
>
> Evaluating these compensating controls is an important part of assessing an organization's overall security posture.

---

# ISO/IEC 19249

**ISO/IEC 19249:2017** is an international standard jointly developed by the **International Organization for Standardization (ISO)** and the **International Electrotechnical Commission (IEC)**.

The standard provides architectural and design principles for building secure:

- Products
- Systems
- Applications

Instead of describing individual security technologies, it focuses on **how secure systems should be designed from the beginning**.

---

# Five Architectural Principles

These principles describe how secure systems should be structured.

---

## 1. Domain Separation

Components with similar security requirements should be grouped into separate security domains.

Each domain has its own security policies and access controls.

### Example

Modern operating systems separate:

```text
Ring 0
Kernel Mode
Highest Privilege

↓

Ring 3
User Applications
Lowest Privilege
```

User applications cannot directly access kernel memory.

This isolation reduces the impact of compromised applications.

---

## 2. Layering

Systems should be divided into multiple layers, each providing services to the layer above.

This improves:

- Security
- Maintainability
- Validation
- Troubleshooting

Examples include:

- OSI Networking Model
- Software architecture
- Defense-in-Depth

---

## 3. Encapsulation

Internal implementation details should remain hidden.

Applications should interact with systems only through controlled interfaces.

Instead of directly modifying data structures, applications use:

- APIs
- Methods
- Services

This reduces accidental misuse and unauthorized manipulation.

---

## 4. Redundancy

Critical components should have backups or duplicate systems.

Examples include:

- Multiple power supplies
- RAID storage
- Backup servers
- High Availability clusters

Redundancy improves:

- Availability
- Fault tolerance
- Business continuity

---

## 5. Virtualization

Virtualization isolates workloads using virtual machines or containers.

Benefits include:

- Stronger security boundaries
- Malware sandboxing
- Resource isolation
- Easier recovery

Examples:

- VMware
- Hyper-V
- VirtualBox
- KVM
- Docker Containers

---

# Five Design Principles

ISO/IEC 19249 also defines five important secure design principles.

These principles frequently appear in security architecture and software engineering.

---

## 1. Least Privilege

Every user, process, or application should receive **only the permissions required** to perform its task.

### Example

If an employee only needs to view a document:

✅ Read Permission

❌ Write Permission

❌ Delete Permission

❌ Administrator Rights

---

## 2. Attack Surface Minimisation

Reduce the number of possible attack vectors.

Unused components should be removed or disabled.

### Example

Instead of exposing:

- FTP
- Telnet
- SMTP
- SSH
- HTTP
- HTTPS

A web server that only requires HTTPS should disable every unnecessary service.

Smaller attack surface → fewer opportunities for attackers.

---

## 3. Centralized Parameter Validation

All user input should be validated consistently using centralized validation mechanisms.

This helps prevent attacks such as:

- SQL Injection
- Command Injection
- Buffer Overflow
- Denial of Service

Centralized validation also improves maintainability by avoiding inconsistent validation logic across multiple applications.

---

## 4. Centralized General Security Services

Security functions should be centralized whenever possible.

Examples include:

- Authentication
- Authorization
- Logging
- Certificate Management

Instead of every application implementing its own login system, organizations often deploy centralized services such as:

- Active Directory
- LDAP
- Kerberos
- Single Sign-On (SSO)

---

## 5. Preparing for Error and Exception Handling

Systems should be designed assuming that failures will occur.

Applications should fail securely rather than exposing sensitive information.

### Good Practice

If a firewall crashes:

```text
Block All Traffic
```

Not:

```text
Allow All Traffic
```

Similarly, applications should never expose:

- Stack traces
- Database credentials
- Memory dumps
- Internal file paths

to end users.

---

# Summary of ISO/IEC 19249 Design Principles

| No. | Design Principle | Primary Goal |
|------|------------------|--------------|
| 1 | Least Privilege | Minimize permissions |
| 2 | Attack Surface Minimisation | Reduce attack opportunities |
| 3 | Centralized Parameter Validation | Prevent malicious input |
| 4 | Centralized General Security Services | Centralize security functions |
| 5 | Preparing for Error & Exception Handling | Fail securely |

---

# Relationship to Defense-in-Depth

Several ISO/IEC 19249 principles directly support Defense-in-Depth.

```text
Defense-in-Depth
        │
        ├── Layering
        ├── Domain Separation
        ├── Least Privilege
        ├── Attack Surface Minimisation
        ├── Centralized Security Services
        └── Secure Error Handling
```

Rather than relying on a single protection mechanism, these principles encourage multiple overlapping security controls throughout the system.

---

> **Pentester Note**
>
> Many penetration testing findings result from violations of these design principles.
>
> Examples include:
>
> - Privilege Escalation → Least Privilege violation
> - Numerous unnecessary open services → Poor Attack Surface Minimisation
> - SQL Injection → Missing Centralized Parameter Validation
> - Multiple inconsistent authentication systems → Lack of Centralized Security Services
> - Information leakage via verbose error messages → Poor Exception Handling
>
> Understanding these principles helps explain not only **what is vulnerable**, but also **why the vulnerability exists**.

# Trust but Verify vs Zero Trust

Trust is an essential part of every computing environment. Organizations trust employees, operating systems, hardware vendors, cloud providers, and internal networks every day.

However, blindly trusting users, devices, or systems creates opportunities for attackers.

Modern cybersecurity primarily follows two philosophies regarding trust:

- **Trust but Verify**
- **Zero Trust**

Although both aim to improve security, they approach trust in fundamentally different ways.

---

# Trust but Verify

## Principle

The **Trust but Verify** principle assumes that an entity is generally trustworthy but continuously verifies its actions.

Instead of blindly accepting every action, organizations monitor and validate behavior to detect suspicious activities.

In other words:

> **Trust is granted, but verification never stops.**

---

## How It Works

After a user successfully authenticates, the organization continues monitoring their activities through logging, auditing, and security monitoring systems.

Typical verification mechanisms include:

- Audit Logs
- SIEM Platforms
- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Proxy Servers
- Endpoint Monitoring

---

## Example

An administrator logs into a production server.

The administrator is trusted because they have legitimate credentials.

However, every action performed is recorded.

```text
Administrator Login
        │
        ▼
 Authentication
        │
        ▼
 Access Granted
        │
        ▼
 Logging & Monitoring
        │
        ▼
 Alert if Suspicious Activity Occurs
```

If unusual behavior is detected, the security team can investigate immediately.

---

## Advantages

- Simple to implement
- Supports continuous monitoring
- Provides accountability
- Generates audit trails
- Enables incident investigations

---

## Limitations

Trust is still granted at the beginning.

If attackers successfully steal valid credentials, they initially receive the same level of trust as legitimate users.

Detection often occurs **after** suspicious activity begins.

---

# Zero Trust

## Principle

Zero Trust is a modern security architecture based on one fundamental assumption:

> **Never Trust, Always Verify**

Instead of trusting users or devices because they belong to an internal network, every access request must be verified before access is granted.

Trust is treated as a potential vulnerability rather than an advantage.

---

## Core Principles

Zero Trust continuously evaluates:

- User identity
- Device identity
- Authentication status
- Authorization level
- Device security posture
- Location
- Risk level
- Requested resource

No request is automatically trusted.

---

## Example Authentication Flow

```text
User Requests Access
         │
         ▼
Identity Verification
         │
         ▼
Multi-Factor Authentication
         │
         ▼
Device Compliance Check
         │
         ▼
Authorization
         │
         ▼
Resource Access
```

Every request is evaluated independently.

---

## Why Zero Trust Exists

Traditional network security assumed that internal networks were trusted.

```text
Outside Network

   Untrusted

──────────── Firewall ────────────

Inside Network

   Trusted
```

Once attackers breached the perimeter, they often moved freely across internal systems.

Zero Trust eliminates this assumption.

Every user, device, and application must continuously prove its identity regardless of location.

---

# Microsegmentation

One important implementation of Zero Trust is **Microsegmentation**.

Instead of treating an entire internal network as one trusted environment, it divides networks into many small isolated segments.

```text
Traditional Network

PC ───────── Server ───────── Database

Compromise One Host
        │
        ▼
Easy Lateral Movement
```

With Microsegmentation:

```text
PC
 │
Firewall
 │
Authentication
 │
Server
 │
Firewall
 │
ACL
 │
Database
```

Each segment enforces its own authentication and authorization policies.

Even if attackers compromise one device, moving to another becomes significantly more difficult.

---

# Trust but Verify vs Zero Trust

| Trust but Verify | Zero Trust |
|------------------|------------|
| Trust is initially granted | Trust is never assumed |
| Activities are monitored after access | Every access request is verified before access |
| Primarily detection-focused | Prevention-focused |
| Suitable for traditional enterprise environments | Commonly used in modern cloud and hybrid infrastructures |

---

# Why Zero Trust Matters

Zero Trust significantly reduces the impact of compromised accounts.

Suppose an attacker steals an employee's credentials.

Traditional approach:

```text
Credential Theft
        │
        ▼
Internal Network Access
        │
        ▼
Lateral Movement
```

Zero Trust approach:

```text
Credential Theft
        │
        ▼
MFA Required
        │
Device Validation
        │
Conditional Access
        │
Least Privilege
        │
Access Limited
```

Attackers encounter multiple security checks before reaching sensitive resources.

---

> **Pentester Note**
>
> During an internal penetration test, evaluating an organization's Zero Trust implementation is extremely valuable.
>
> Questions to consider include:
>
> - Is Multi-Factor Authentication enforced?
> - Is Role-Based Access Control implemented?
> - Are privileged accounts protected?
> - Can compromised users move laterally?
> - Are network segments isolated?
>
> Weak Zero Trust implementation often leads to successful privilege escalation and lateral movement.

---

# Vulnerability vs Threat vs Risk

These three terms are frequently confused, yet they describe completely different concepts.

Understanding their relationship is essential for vulnerability management, penetration testing, and risk assessment.

---

# Vulnerability

A **vulnerability** is a weakness that could potentially be exploited.

It does **not** necessarily mean an attack is currently taking place.

Examples include:

- Weak passwords
- SQL Injection
- Buffer Overflow
- Missing security patches
- Misconfigured firewalls
- Exposed services

A vulnerability simply provides an opportunity for attackers.

---

# Threat

A **threat** is any person, event, or circumstance capable of exploiting a vulnerability.

Threats may include:

- Cybercriminals
- Nation-state attackers
- Insider threats
- Malware
- Ransomware
- Natural disasters
- Hardware failures

A threat represents **potential danger**, not the weakness itself.

---

# Risk

Risk combines:

- The likelihood that a threat exploits a vulnerability
- The resulting business impact

Conceptually:

```text
Risk ≈ Likelihood × Impact
```

Organizations prioritize vulnerabilities according to risk rather than simply counting weaknesses.

---

# Relationship Between the Three

```text
Weakness
     │
     ▼
Vulnerability
     │
Threat Exploits
     ▼
Business Impact
     │
     ▼
Risk
```

Without a vulnerability, a threat has nothing to exploit.

Without a threat, the vulnerability may never be exercised.

Risk considers both together.

---

# Real-World Example

Suppose a hospital database server is running outdated software.

### Vulnerability

The database software contains a critical Remote Code Execution vulnerability.

↓

### Threat

A public exploit has been released, allowing attackers to exploit the vulnerability.

↓

### Risk

Patient records could be stolen or modified, disrupting hospital operations and causing significant financial and legal consequences.

---

# Comparison

| Vulnerability | Threat | Risk |
|--------------|--------|------|
| Weakness | Potential danger | Likelihood and business impact |
| Exists within the system | Attempts to exploit the weakness | Determines organizational priority |
| Example: SQL Injection | Example: Hacker | Example: Customer database compromise |

---

# Shared Responsibility Model

As organizations increasingly migrate to cloud platforms, security responsibilities become shared between:

- The **Cloud Service Provider (CSP)**
- The **Customer**

This concept is known as the **Shared Responsibility Model**.

The provider secures the cloud infrastructure.

The customer secures what they deploy and configure inside the cloud.

---

## Infrastructure as a Service (IaaS)

Examples include:

- Amazon EC2
- Microsoft Azure Virtual Machines
- Google Compute Engine

### Cloud Provider Responsibilities

- Physical hardware
- Networking
- Storage
- Hypervisor

### Customer Responsibilities

- Operating System
- Security patches
- Applications
- User accounts
- Firewalls
- Data

---

## Software as a Service (SaaS)

Examples include:

- Microsoft 365
- Google Workspace
- Salesforce

In SaaS environments, customers do not manage the operating system.

Instead, they remain responsible for:

- User management
- Password policies
- Multi-Factor Authentication
- Data protection
- Access permissions

---

# Why the Shared Responsibility Model Matters

A common misconception is:

> "My data is stored in the cloud, so the cloud provider is responsible for everything."

This is incorrect.

For example, if an organization accidentally makes an Amazon S3 bucket publicly accessible, AWS did not create the misconfiguration.

The customer remains responsible for securing their own data and configurations.

Understanding where responsibilities begin and end is critical when designing secure cloud environments.

---

# Key Takeaways

- **Trust but Verify** assumes trust but continuously monitors behavior.
- **Zero Trust** removes implicit trust and requires continuous verification.
- **Microsegmentation** limits attacker movement within networks.
- **Vulnerability** is a weakness.
- **Threat** is anything capable of exploiting that weakness.
- **Risk** represents the likelihood and business impact of successful exploitation.
- The **Shared Responsibility Model** defines how cloud security responsibilities are divided between providers and customers.
- These principles form the foundation of modern enterprise security architectures and influence how organizations design, monitor, and protect their environments.

# Commands Used

This room is theory-based and does not require any terminal commands or hands-on exploitation.

Instead, it focuses on understanding the security principles, models, and architectural concepts that underpin modern cybersecurity.

---

# Security Models Summary

| Model | Security Objective | Key Rules | Primary Use Case |
|--------|--------------------|-----------|------------------|
| Bell-LaPadula | Confidentiality | No Read Up, No Write Down | Military, Government |
| Biba | Integrity | No Read Down, No Write Up | Trusted Systems |
| Clark-Wilson | Integrity | CDI, UDI, TP, IVP | Banking, Enterprise Applications |

---

# CIA vs DAD Summary

| CIA (Security Goal) | DAD (Attacker Goal) |
|---------------------|---------------------|
| Confidentiality | Disclosure |
| Integrity | Alteration |
| Availability | Destruction / Denial |

The CIA Triad defines what defenders seek to protect, while the DAD Triad illustrates how attackers compromise those objectives.

---

# ISO/IEC 19249 Summary

## Architectural Principles

| Principle | Purpose |
|-----------|----------|
| Domain Separation | Isolate components into separate security domains. |
| Layering | Apply security across multiple abstraction layers. |
| Encapsulation | Hide internal implementation behind controlled interfaces. |
| Redundancy | Improve availability and fault tolerance. |
| Virtualization | Isolate workloads using virtual machines or containers. |

---

## Design Principles

| No. | Principle | Purpose |
|------|-----------|----------|
| 1 | Least Privilege | Grant only the permissions required. |
| 2 | Attack Surface Minimisation | Reduce unnecessary attack vectors. |
| 3 | Centralized Parameter Validation | Validate all inputs consistently. |
| 4 | Centralized General Security Services | Centralize authentication and security services. |
| 5 | Preparing for Error & Exception Handling | Fail securely without exposing sensitive information. |

---

# Security Principles Learned

Throughout this room, several important security principles were introduced.

- CIA Triad
- DAD Triad
- Defense-in-Depth
- Zero Trust
- Trust but Verify
- Least Privilege
- Attack Surface Minimisation
- Domain Separation
- Layering
- Encapsulation
- Redundancy
- Virtualization
- Shared Responsibility Model

Together, these principles provide the foundation for designing secure systems and reducing organizational risk.

---

# Common Interview Questions

## What is the CIA Triad?

The CIA Triad consists of three primary security objectives:

- Confidentiality
- Integrity
- Availability

Almost every security control is designed to protect one or more of these objectives.

---

## What is the difference between CIA and DAD?

CIA represents the defender's goals.

DAD represents the attacker's perspective.

| CIA | DAD |
|------|-----|
| Confidentiality | Disclosure |
| Integrity | Alteration |
| Availability | Destruction / Denial |

---

## What is Defense-in-Depth?

Defense-in-Depth is the practice of implementing multiple independent security controls so that if one control fails, additional layers continue protecting the system.

---

## What is the difference between Trust but Verify and Zero Trust?

**Trust but Verify** assumes trust first and continuously monitors behavior.

**Zero Trust** assumes no trust by default and requires continuous authentication and authorization for every access request.

---

## What is the principle of Least Privilege?

Users, applications, and services should receive only the minimum permissions required to perform their tasks.

This reduces the impact of compromised accounts and limits privilege escalation opportunities.

---

## What is the difference between Vulnerability, Threat, and Risk?

- **Vulnerability** → A weakness in a system.
- **Threat** → Anything capable of exploiting that weakness.
- **Risk** → The likelihood and business impact of successful exploitation.

---

## Which security model focuses on Confidentiality?

**Bell-LaPadula Model**

---

## Which security model focuses on Integrity?

Both:

- Biba Model
- Clark-Wilson Model

---

## Why is Zero Trust becoming increasingly popular?

Modern organizations operate in cloud, hybrid, and remote-work environments where network location is no longer a reliable indicator of trust.

Zero Trust continuously verifies identities, devices, and permissions, significantly reducing lateral movement opportunities for attackers.

---

# Skills Gained

After completing this room, I gained a stronger understanding of:

- Information security fundamentals
- Security objectives (CIA Triad)
- Security attack concepts (DAD Triad)
- Security architecture principles
- Enterprise security design
- Access control models
- Risk management concepts
- Secure system design
- Cloud security responsibilities
- Defensive security strategies

These concepts form the theoretical foundation for many cybersecurity domains, including penetration testing, SOC operations, cloud security, governance, risk management, and security architecture.

---

# Pentester Notes

Although this room contains very little hands-on content, nearly every future penetration test relies on the concepts introduced here.

Examples include:

| Pentesting Activity | Related Security Principle |
|---------------------|----------------------------|
| SQL Injection | Confidentiality |
| Website Defacement | Integrity |
| DDoS | Availability |
| Privilege Escalation | Least Privilege |
| Lateral Movement | Zero Trust, Microsegmentation |
| Password Attacks | Confidentiality |
| Active Directory Assessment | Bell-LaPadula, Zero Trust |
| Cloud Security Review | Shared Responsibility Model |
| Network Enumeration | Attack Surface Minimisation |
| Security Architecture Review | Defense-in-Depth |

Understanding **why** a security control exists is just as important as knowing **how** to bypass or test it.

---

# Key Takeaways

- Security is built on principles, not individual tools.
- The **CIA Triad** defines the primary goals of information security.
- The **DAD Triad** describes how attackers compromise those goals.
- Security models such as **Bell-LaPadula**, **Biba**, and **Clark-Wilson** formalize access control and data protection.
- **Defense-in-Depth** advocates multiple layers of security instead of relying on a single control.
- **ISO/IEC 19249** provides internationally recognized architectural and design principles for secure systems.
- **Zero Trust** assumes no implicit trust and continuously verifies users, devices, and access requests.
- Understanding the distinction between **Vulnerability**, **Threat**, and **Risk** is essential for effective risk management.
- Cloud environments require both providers and customers to fulfill their responsibilities through the **Shared Responsibility Model**.

---

# Future Learning Path

This room establishes the theoretical foundation for many advanced cybersecurity topics.

Recommended next topics include:

- Intro to Cryptography
- Identity and Access Management (IAM)
- Active Directory Fundamentals
- Windows Security
- Linux Security
- Secure Network Architecture
- Web Application Security
- Cloud Security Fundamentals
- Security Operations Center (SOC)
- Governance, Risk, and Compliance (GRC)

Many of these subjects will continuously reference the concepts introduced in this room.

---

# References

- TryHackMe — Security Principles Room
- ISO/IEC 19249:2017 — *Information technology — Security techniques — Catalogue of architectural and design principles for secure products, systems and applications*
- NIST Cybersecurity Framework (CSF)
- NIST Special Publication 800-53
- OWASP Top 10
- Microsoft Zero Trust Architecture
- CIS Critical Security Controls

---

# Personal Reflection

This room serves as one of the most important theoretical foundations in cybersecurity. Unlike rooms that focus on specific tools or attacks, it explains **why security controls exist** and **what they are designed to protect**.

Understanding concepts such as the CIA Triad, Defense-in-Depth, Zero Trust, and Risk Management provides a strong mental framework for approaching future topics. As I continue learning penetration testing, cloud security, Active Directory, and incident response, these principles will help me evaluate not only whether a system is vulnerable, but also how those vulnerabilities impact an organization's overall security posture.

---

# Tags

`tryhackme`
`security-principles`
`information-security`
`cia-triad`
`dad-triad`
`bell-lapadula`
`biba`
`clark-wilson`
`zero-trust`
`defense-in-depth`
`least-privilege`
`iso-iec-19249`
`risk-management`
`cloud-security`
`shared-responsibility-model`
`cybersecurity-fundamentals`