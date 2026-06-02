# Linux Shell - TryHackMe Writeup

## Room Overview

This room introduced the Linux Shell, different shell types available in Linux, and basic shell scripting concepts. The room covered interaction with the Command Line Interface (CLI), common shell commands, shell scripting fundamentals, and a practical exercise involving a custom script used to locate a hidden flag.

---

# Learning Objectives

* Understand the purpose of Linux shells
* Learn how to interact with a shell
* Use common Linux shell commands
* Explore different shell types
* Learn shell scripting fundamentals
* Create and execute shell scripts
* Understand variables, loops, conditional statements, and comments
* Complete a practical scripting challenge

---

# Why Linux Shell Matters for Pentesters

Most Linux servers, cloud instances, Docker containers, and compromised machines do not provide a graphical interface.

A penetration tester will often interact with systems through:

```bash
ssh user@target
```

or a reverse shell such as:

```bash
www-data@target:/var/www/html$
```

Therefore, shell proficiency is a fundamental skill for enumeration, privilege escalation, automation, and post-exploitation activities.

---

# Task 1 - Introduction to Linux Shell

## What is a Shell?

A shell is a program that acts as an interface between the user and the Linux kernel.

Workflow:

```text
User
 ↓
Shell
 ↓
Kernel
 ↓
Hardware
```

When a user enters a command:

```bash
ls
```

The shell interprets the command and passes it to the kernel for execution.

---

# GUI vs CLI

## GUI (Graphical User Interface)

Examples:

* File Explorer
* Settings
* Web Browser

Advantages:

* Easy to use
* Beginner friendly

Disadvantages:

* Slower for repetitive tasks
* Difficult to automate
* Often unavailable on servers

---

## CLI (Command Line Interface)

Examples:

```bash
ls
cd
cat
grep
```

Advantages:

* Fast
* Efficient
* Easily automated
* Commonly used in servers and penetration testing

---

# Task 2 - Interacting with the Shell

## Check Current Directory

### Command

```bash
pwd
```

### Meaning

Print Working Directory

### Example Output

```bash
/home/user
```

### How to Read It

You are currently located inside:

```text
/home/user
```

### Pentesting Use Case

After obtaining a shell, determine where you currently are:

```bash
www-data@target:/var/www/html$ pwd
```

Output:

```bash
/var/www/html
```

---

## Change Directory

### Command

```bash
cd Desktop
```

### Meaning

Change Directory

### Example Output

```bash
user@tryhackme:~/Desktop$
```

### How to Read It

The current location is now:

```text
/home/user/Desktop
```

---

## List Directory Contents

### Command

```bash
ls
```

### Meaning

List files and directories.

### Example Output

```bash
Desktop
Documents
Downloads
Pictures
Videos
```

### How to Read It

These are the folders inside the current directory.

### Pentesting Use Case

Check what files are available:

```bash
ls
```

Example:

```bash
backup.zip
passwords.txt
config.php
```

---

## Display File Contents

### Command

```bash
cat filename.txt
```

### Meaning

Display file contents.

### Example Output

```text
this is a sample file
this is the second line
```

### How to Read It

The contents of the file are displayed directly in the terminal.

### Pentesting Use Case

Read configuration files:

```bash
cat config.php
```

Read flags:

```bash
cat flag.txt
```

Read SSH keys:

```bash
cat id_rsa
```

---

## Search Text Using grep

### Command

```bash
grep THM dictionary.txt
```

### Meaning

Search for a specific keyword or pattern.

### Example Output

```text
The flag is THM
```

### How to Read It

The line containing the keyword "THM" is displayed.

### Pentesting Use Case

Search for:

```bash
grep password config.php
grep admin users.txt
grep api config.php
```

---

# Task 3 - Types of Linux Shells

## Check Current Shell

### Command

```bash
echo $SHELL
```

### Example Output

```bash
/bin/bash
```

### Meaning

Displays the active shell.

---

## List Installed Shells

### Command

```bash
cat /etc/shells
```

### Example Output

```bash
/bin/sh
/bin/bash
/bin/dash
/bin/zsh
```

### Meaning

Displays all available login shells.

---

# Bash (Bourne Again Shell)

### Features

* Most common Linux shell
* Strong scripting support
* Command history
* Tab completion

### Command History

```bash
history
```

Example:

```bash
1 ls
2 cd Desktop
3 cat flag.txt
```

---

# Fish (Friendly Interactive Shell)

### Features

* User friendly
* Auto suggestions
* Syntax highlighting
* Auto correction

### Advantages

* Beginner friendly
* Easy customization

### Disadvantages

* Less compatible with Bash scripts

---

# Zsh (Z Shell)

### Features

* Advanced tab completion
* Auto correction
* Plugin support
* Highly customizable

### Popular Tool

Oh-My-Zsh

Commonly used by:

* Pentesters
* Developers
* Linux power users

---

# Shell Comparison

| Feature           | Bash      | Fish     | Zsh       |
| ----------------- | --------- | -------- | --------- |
| Scripting         | Excellent | Limited  | Excellent |
| Tab Completion    | Basic     | Advanced | Advanced  |
| Customization     | Basic     | Good     | Excellent |
| User Friendliness | Moderate  | High     | High      |
| Pentesting Usage  | Excellent | Moderate | Excellent |

---

# Task 4 - Shell Scripting and Components

## What is Shell Scripting?

A shell script is a collection of commands stored in a file.

Instead of executing commands individually:

```bash
mkdir project
cd project
touch notes.txt
```

We can automate them:

```bash
./script.sh
```

---

# Creating a Script

### Command

```bash
nano first_script.sh
```

---

# Shebang

Every Bash script should start with:

```bash
#!/bin/bash
```

### Meaning

Tell Linux to execute the script using Bash.

---

# Variables

Example:

```bash
#!/bin/bash

echo "What's your name?"
read name

echo "Welcome, $name"
```

### Example Execution

```bash
./first_script.sh
```

Output:

```text
What's your name?
John
Welcome, John
```

### Explanation

```bash
read name
```

Stores user input inside:

```bash
name
```

Variable.

---

# Execution Permission

### Command

```bash
chmod +x first_script.sh
```

### Meaning

Make the script executable.

---

# Running a Script

### Command

```bash
./first_script.sh
```

### Why Use "./"?

```text
. = Current Directory
```

Linux searches commands in PATH by default.

Using:

```bash
./
```

Tells Linux to execute the file in the current directory.

---

# Loops

### Script

```bash
#!/bin/bash

for i in {1..10}
do
    echo $i
done
```

### Output

```text
1
2
3
4
5
6
7
8
9
10
```

### Explanation

The loop repeats ten times.

---

### Pentesting Use Case

Scan multiple hosts:

```bash
for ip in {1..254}
do
    ping -c 1 192.168.1.$ip
done
```

---

# Conditional Statements

### Script

```bash
#!/bin/bash

echo "Enter your name:"
read name

if [ "$name" = "Stewart" ]; then
    echo "Access Granted"
else
    echo "Access Denied"
fi
```

### Example Output

```text
Enter your name:
Stewart
Access Granted
```

---

### Pentesting Use Case

Check whether a file exists:

```bash
if [ -f passwords.txt ]
then
    cat passwords.txt
fi
```

---

# Comments

### Syntax

```bash
# This is a comment
```

### Purpose

Improve readability and maintenance.

Comments are ignored during execution.

---

# Task 5 - Locker Script

The room combined:

* Variables
* Loops
* Conditional Statements
* User Input

### Purpose

Authenticate a user before allowing access to a locker.

Required Credentials:

```text
Username: John
Company: Tryhackme
PIN: 7385
```

### Authentication Logic

```bash
if [ "$username" = "John" ] &&
   [ "$companyname" = "Tryhackme" ] &&
   [ "$pin" = "7385" ]
```

All conditions must be true.

### Successful Authentication

```text
Authentication Successful.
```

### Failed Authentication

```text
Authentication Denied!!
```

---

# Task 6 - Practical Exercise

## Objective

Modify a script to search for a hidden flag.

Given Information:

```text
Keyword: thm-flag01-script
Directory: /var/log
```

---

# Initial Problem

The script contained empty values:

```bash
directory=""
flag=""
```

and

```bash
for file in ""/*.log
```

These values needed to be completed.

---

# Correct Values

```bash
directory="/var/log"
flag="thm-flag01-script"
```

and:

```bash
for file in "$directory"/*.log
```

---

# Become Root

### Command

```bash
sudo su
```

### Password

```text
user@Tryhackme
```

### Verify

```bash
whoami
```

Output:

```text
root
```

---

# Execute Script

### Command

```bash
chmod +x script.sh
./script.sh
```

### Output

```text
Flag found in: authentication.log
```

---

# Investigation

Initially, it appeared that:

```text
thm-flag01-script
```

was the flag.

However, this was only the search keyword.

The script only identified the file containing the keyword.

---

# Manual Investigation

Navigate to log directory:

```bash
cd /var/log
```

List files:

```bash
ls
```

Read file:

```bash
cat authentication.log
```

Output:

```text
the cat is sleeping under the table
thm-flag01-script
```

---

# Actual Flag

```text
the cat is sleeping under the table
```

---

# Problem Encountered

## Issue

The script successfully located:

```text
authentication.log
```

but did not display the actual flag.

### Why?

The script only searched for the keyword:

```bash
grep -q "$flag" "$file"
```

and printed the filename.

It did not display the contents of the file.

---

# Solution

Investigate the file manually:

```bash
cat authentication.log
```

This revealed the actual flag hidden above the keyword.

---

# Lessons Learned

* Understand Linux shell fundamentals
* Use essential commands effectively
* Work with Bash, Fish, and Zsh
* Create executable shell scripts
* Use variables, loops, and conditions
* Read and modify existing scripts
* Troubleshoot script logic
* Combine automation with manual investigation
* Think like a penetration tester rather than relying solely on tool output

---

# Key Commands Learned

```bash
pwd
cd
ls
cat
grep
echo
history
chmod +x
./script.sh
sudo su
whoami
nano
```

---

# Pentester Takeaway

This room provides the foundation for many future activities:

* Linux Enumeration
* Privilege Escalation
* Reverse Shell Management
* Bash Automation
* CTF Challenges
* OSCP-style Labs

Understanding the Linux shell is one of the most important skills for any aspiring penetration tester because most real-world targets are accessed and controlled through command-line interfaces rather than graphical environments.
