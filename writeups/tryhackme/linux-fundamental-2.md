# Linux Fundamentals Part 2

## Overview

Linux Fundamentals Part 2 builds upon the concepts introduced in Part 1 by focusing on remote system access, file system management, permissions, user switching, and common Linux directories. These topics provide a strong foundation for Linux administration and future penetration testing activities.

---

## Objectives

* Learn how to access remote Linux systems using SSH
* Understand flags and switches used in Linux commands
* Manage files and directories from the command line
* Understand Linux file permissions and ownership
* Switch between user accounts
* Explore common Linux system directories

---

## SSH (Secure Shell)

SSH is a protocol used to securely connect to remote systems over a network. It allows users to execute commands on another machine while keeping communications encrypted.

### Basic Syntax

```bash
ssh username@ip-address
```

### Example

```bash
ssh tryhackme@10.10.10.10
```

### Useful Commands

```bash
whoami
hostname
pwd
```

### Key Takeaways

* SSH is the standard method for remote Linux administration.
* All communication between client and server is encrypted.
* Once connected, commands are executed on the remote machine.

---

## Flags and Switches

Flags modify the behavior of Linux commands and provide additional functionality.

### Common Examples

```bash
ls
```

List visible files.

```bash
ls -a
```

Display all files, including hidden files.

```bash
ls -l
```

Display files using long listing format.

```bash
ls -lah
```

Display detailed information, hidden files, and human-readable file sizes.

### Documentation Commands

```bash
command --help
```

```bash
man command
```

### Key Takeaways

* Most Linux commands support flags and arguments.
* Documentation can be accessed directly from the terminal using `--help` and `man`.
* Combining multiple flags can significantly improve command output.

---

## File System Interaction

### Creating Files

```bash
touch note.txt
```

### Creating Directories

```bash
mkdir mydirectory
```

### Copying Files

```bash
cp source.txt destination.txt
```

### Moving or Renaming Files

```bash
mv old.txt new.txt
```

```bash
mv file.txt Documents/
```

### Removing Files and Directories

```bash
rm file.txt
```

```bash
rm -r directory
```

### Determining File Types

```bash
file filename
```

### Key Takeaways

* Linux provides simple command-line tools for file and directory management.
* `mv` can be used both for moving and renaming files.
* The `file` command is useful for identifying unknown files during system enumeration.

---

## Linux Permissions

Linux permissions determine which users can read, write, or execute files and directories.

### Example Permission

```text
-rwxr-xr-x
```

Breakdown:

```text
d rwx r-x r-x
│ │   │   │
│ │   │   └── Others
│ │   └────── Group
│ └────────── Owner
└──────────── File Type
```

### Permission Types

| Permission | Meaning |
| ---------- | ------- |
| r          | Read    |
| w          | Write   |
| x          | Execute |

### Numeric Permission Values

Linux permissions can also be represented numerically.

| Permission  | Value |
| ----------- | ----- |
| Read (r)    | 4     |
| Write (w)   | 2     |
| Execute (x) | 1     |

The numeric value is calculated by adding the permissions together.

### Examples

| Symbolic | Calculation | Numeric |
| -------- | ----------- | ------- |
| rwx      | 4 + 2 + 1   | 7       |
| rw-      | 4 + 2 + 0   | 6       |
| r-x      | 4 + 0 + 1   | 5       |
| r--      | 4 + 0 + 0   | 4       |

### Common Permission Sets

| Symbolic  | Numeric | Description                                    |
| --------- | ------- | ---------------------------------------------- |
| rwxrwxrwx | 777     | Full access for everyone                       |
| rwxr-xr-x | 755     | Full access for owner, read/execute for others |
| rw-r--r-- | 644     | Owner can read/write, others can read only     |
| rwx------ | 700     | Only the owner has access                      |

### Changing Permissions

```bash
chmod 755 script.sh
```

Permission breakdown:

```text
755
││└── Others = 5 = r-x
│└─── Group  = 5 = r-x
└──── Owner  = 7 = rwx
```

Result:

```text
rwxr-xr-x
```

### Key Takeaways

* Permissions are divided into Owner, Group, and Others.
* Numeric permissions provide a faster way to configure access rights.
* Understanding permissions is essential for Linux security and privilege escalation.
* Misconfigured permissions can introduce security risks.

---

## User Switching

Linux allows users to switch accounts using the `su` command.

### Basic Usage

```bash
su username
```

### Login Shell

```bash
su -l username
```

### Key Takeaways

* User switching is commonly used in system administration.
* The `-l` option loads the target user's environment and home directory.

---

## Common Linux Directories

### /etc

Stores system configuration files.

Important files:

```text
/etc/passwd
/etc/shadow
/etc/sudoers
```

Used for:

* User account information
* Password hashes
* Sudo permissions

---

### /var

Stores variable application and service data.

Common location:

```text
/var/log
```

Contains:

* Authentication logs
* Service logs
* System logs

---

### /root

Home directory of the root user.

```text
/root
```

Not:

```text
/home/root
```

---

### /tmp

Temporary storage directory.

Characteristics:

* Writable by all users
* Frequently used during penetration testing
* Often cleared after system reboot

Common usage:

```bash
cd /tmp
```

Store temporary tools, scripts, and enumeration utilities.

---

## Pentesting Relevance

The concepts introduced in this room are directly applicable to penetration testing:

* SSH is commonly used to access remote systems.
* File management commands help organize and execute tools.
* Permissions analysis is critical during privilege escalation.
* Directories such as `/etc`, `/var/log`, and `/tmp` are frequently examined during system enumeration.
* Misconfigured permissions may expose sensitive information or privilege escalation vectors.

---

## Skills Gained

* SSH
* Linux Navigation
* File Management
* User Management
* Linux Permissions
* System Enumeration
* Basic Linux Administration

---

## Conclusion

This room strengthened my understanding of Linux fundamentals by introducing secure remote access, file system operations, permission management, user switching, and key system directories. These concepts provide a solid foundation for future topics such as system enumeration, privilege escalation, and penetration testing.
