# Linux Fundamentals Part 3

## Overview

Linux Fundamentals Part 3 focuses on practical Linux administration concepts that are commonly used in both system administration and cybersecurity. Topics covered include terminal text editors, file transfer utilities, process management, task automation, package management, and log analysis.

---

## Objectives

* Learn how to edit files using terminal-based text editors
* Transfer files between systems using Linux utilities
* Understand Linux process and service management
* Automate tasks using cron jobs
* Manage software packages and repositories
* Investigate and analyze Linux log files

---

## Terminal Text Editors

### Nano

Nano is a simple and beginner-friendly terminal text editor used to create and modify files directly from the command line.

### Basic Usage

```bash
nano filename
```

### Common Shortcuts

| Shortcut | Function |
|-----------|------------|
| Ctrl + O | Save file |
| Ctrl + X | Exit Nano |
| Ctrl + W | Search text |
| Ctrl + C | Display cursor position |
| Ctrl + _ | Go to a specific line |

### Key Takeaways

* Nano is easy to learn and widely available on Linux systems.
* Useful for editing configuration files and scripts directly from the terminal.
* Frequently used when connected to remote systems through SSH.

---

### VIM

VIM is a powerful and highly customizable terminal text editor commonly used by Linux administrators and developers.

### Basic Usage

```bash
vim filename
```

### Common Commands

| Command | Function |
|----------|----------|
| i | Enter Insert Mode |
| ESC | Exit Insert Mode |
| :wq | Save and Quit |
| :q! | Quit Without Saving |

### Key Takeaways

* VIM provides advanced editing capabilities.
* Available on most Linux distributions.
* Commonly encountered in server environments.

---

## Useful Utilities

### Wget

Wget is used to download files from web servers directly through the terminal.

### Example

```bash
wget http://IP:PORT/file
```

### Practical Example

```bash
wget http://10.49.182.174:8000/.flag.txt
```

### Flag

```text
THM{WGET_WEBSERVER}
```

### Key Takeaways

* Downloads files using HTTP, HTTPS, and FTP.
* Commonly used to retrieve scripts, tools, and payloads during assessments.
* Frequently used in penetration testing environments.

---

### SCP (Secure Copy)

SCP securely transfers files between systems using the SSH protocol.

### Upload a File

```bash
scp file.txt user@IP:/path/file.txt
```

### Download a File

```bash
scp user@IP:/path/file.txt localfile.txt
```

### Key Takeaways

* Provides encrypted file transfers.
* Uses SSH for authentication and security.
* Useful for moving files between local and remote systems.

---

### Python HTTP Server

Python includes a lightweight web server that can be used to host files quickly.

### Start a Web Server

```bash
python3 -m http.server
```

### Default Port

```text
8000
```

### Download Hosted Files

```bash
wget http://IP:8000/file
```

### Key Takeaways

* Useful for quickly sharing files between systems.
* Frequently used during penetration testing engagements.
* Hosts files from the current working directory.

---

## Process Management

Processes are programs currently running on a Linux system.

### Viewing Processes

Display processes running in the current session:

```bash
ps
```

Display all processes:

```bash
ps aux
```

View real-time process information:

```bash
top
```

### Managing Processes

Terminate a process:

```bash
kill PID
```

Force terminate a process:

```bash
kill -9 PID
```

### Common Signals

| Signal | Purpose |
|----------|----------|
| SIGTERM | Graceful termination |
| SIGKILL | Force termination |
| SIGSTOP | Pause or suspend a process |

### Managing Services

Check service status:

```bash
systemctl status service
```

Start a service:

```bash
systemctl start service
```

Stop a service:

```bash
systemctl stop service
```

Enable a service at boot:

```bash
systemctl enable service
```

Disable a service:

```bash
systemctl disable service
```

### Background and Foreground Processes

Run a process in the background:

```bash
command &
```

Suspend a running process:

```text
Ctrl + Z
```

View background jobs:

```bash
jobs
```

Return a process to the foreground:

```bash
fg
```

### Flag

```text
THM{PROCESSES}
```

### Key Takeaways

* Every process has a unique Process ID (PID).
* Processes can be monitored, suspended, resumed, or terminated.
* Services are managed through systemd using systemctl.

---

## Automation (Cron)

Cron is a scheduling utility used to automatically execute commands at specified times.

### Edit Crontab

```bash
crontab -e
```

### View Existing Cron Jobs

```bash
crontab -l
```

### Crontab Format

```text
MIN HOUR DOM MON DOW CMD
```

| Field | Description |
|---------|------------|
| MIN | Minute |
| HOUR | Hour |
| DOM | Day of Month |
| MON | Month |
| DOW | Day of Week |
| CMD | Command |

### Example

```bash
0 */12 * * * cp -R /home/user/Documents /var/backups/
```

This command backs up the Documents directory every 12 hours.

### Special Values

| Value | Description |
|---------|------------|
| @reboot | Run at system startup |
| @hourly | Run every hour |
| @daily | Run every day |
| @weekly | Run every week |
| @monthly | Run every month |
| @yearly | Run every year |

### Example

```bash
@reboot /var/opt/processes.sh
```

### Key Takeaways

* Cron automates repetitive administrative tasks.
* Scheduled jobs are stored in crontabs.
* Cron jobs are commonly reviewed during privilege escalation assessments.

---

## Package Management

Ubuntu uses APT (Advanced Package Tool) to install, update, and remove software.

### Update Package Lists

```bash
sudo apt update
```

### Upgrade Installed Packages

```bash
sudo apt upgrade
```

### Install Software

```bash
sudo apt install package-name
```

### Remove Software

```bash
sudo apt remove package-name
```

### Repository Locations

```text
/etc/apt/sources.list
```

```text
/etc/apt/sources.list.d/
```

### GPG Keys

GPG keys are used to verify the authenticity and integrity of software packages before installation.

### Key Takeaways

* APT simplifies software management.
* Repositories provide software sources.
* GPG keys help verify package authenticity.
* Package managers make software installation and updates easier.

---

## Logging

Linux stores logs that record system activity, service events, user actions, and security-related information.

### Common Log Directory

```text
/var/log
```

### Common Log Files

Authentication logs:

```text
/var/log/auth.log
```

System logs:

```text
/var/log/syslog
```

Apache access logs:

```text
/var/log/apache2/access.log
```

Apache error logs:

```text
/var/log/apache2/error.log
```

Fail2Ban logs:

```text
/var/log/fail2ban.log
```

UFW firewall logs:

```text
/var/log/ufw.log
```

### Viewing Logs

Display an entire log file:

```bash
cat logfile
```

Display the last 10 lines:

```bash
tail logfile
```

Display the last 20 lines:

```bash
tail -20 logfile
```

Monitor logs in real time:

```bash
tail -f logfile
```

### Searching Logs

Search for specific entries:

```bash
grep "keyword" logfile
```

Example:

```bash
grep "Failed" /var/log/auth.log
```

### Key Takeaways

* Logs are essential for troubleshooting and monitoring systems.
* Authentication logs help identify login activity.
* Web server logs record requests and errors.
* Logs are frequently used during incident response and forensic investigations.

---

## Pentesting Relevance

The concepts introduced in this room are directly applicable to penetration testing and system administration:

* Terminal text editors are used to modify configuration files and scripts.
* Wget, SCP, and Python HTTP Server are commonly used to transfer files and payloads.
* Process enumeration helps identify running services and potential attack vectors.
* Cron jobs may reveal privilege escalation opportunities.
* Package management is used to install and maintain security tools.
* Log analysis assists with system monitoring, troubleshooting, and security investigations.

---

## Skills Gained

* Linux Text Editors
* File Transfers
* Process Management
* Service Management
* Task Automation
* Package Management
* Log Analysis
* Linux Administration

---

## Conclusion

This room expanded my understanding of Linux by introducing practical system administration concepts such as process management, automation, package management, file transfer utilities, and log analysis. These skills provide a strong foundation for future topics including system enumeration, privilege escalation, incident response, and penetration testing.