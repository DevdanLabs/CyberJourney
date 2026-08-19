# Bash Scripting

> Learn the fundamentals of Bash scripting, including variables, parameters, arrays, conditionals, debugging, and automation techniques commonly used in Linux system administration and cybersecurity.

---

# Executive Summary

Bash is the default shell for most Linux distributions and is one of the most important scripting languages for anyone working with Linux systems. Instead of executing commands one by one, Bash allows us to combine multiple commands into reusable scripts that automate repetitive tasks.

In this TryHackMe room, I learned how to write basic Bash scripts, work with variables and command-line parameters, store data using arrays, perform conditional logic, and debug scripts using Bash's built-in debugging features. These concepts form the foundation for creating automation scripts used by system administrators, DevOps engineers, security analysts, and penetration testers.

Throughout the room, I also learned how Bash interacts with the Linux operating system, how scripts receive input from users, and how to make decisions based on conditions such as file existence, permissions, and user input.

---

# Learning Objectives

After completing this room, I was able to:

- Understand what Bash is and why it is used.
- Create and execute Bash scripts.
- Understand the purpose of the Shebang (`#!/bin/bash`).
- Use variables to store and manipulate data.
- Pass command-line arguments to scripts.
- Receive interactive user input using `read`.
- Store multiple values using arrays.
- Access, modify, and remove array elements.
- Build decision-making logic using `if`, `else`, and comparison operators.
- Check file permissions and existence.
- Debug Bash scripts using `bash -x`, `set -x`, and `set +x`.
- Write small automation scripts following Linux scripting best practices.

---

# Room Information

| Category | Value |
|----------|-------|
| Platform | TryHackMe |
| Room | Bash Scripting |
| Difficulty | Easy |
| Operating System | Linux |
| Shell | Bash |
| Scripting Language | Bash |
| Skills Learned | Linux, Bash, Automation, Variables, Parameters, Arrays, Conditionals |

---

# Prerequisites

Before starting this room, it is recommended to understand:

- Linux command line basics
- File system navigation
- Linux permissions
- Basic shell commands
- Executable files
- Variables (basic programming knowledge is helpful)

Recommended prerequisite room:

- Linux Fundamentals Part 1

---

# What is Bash?

**Bash (Bourne Again SHell)** is a command-line interpreter (shell) that allows users to interact with the Linux operating system.

Instead of communicating directly with the Linux kernel, users send commands to Bash, which then interprets those commands and executes the appropriate programs.

```text
User
   │
   ▼
Bash Shell
   │
   ▼
Linux Kernel
   │
   ▼
Hardware
```

Bash can execute commands interactively through the terminal or automatically through **Bash scripts**.

---

# What is a Bash Script?

A Bash script is simply a text file containing a sequence of Bash commands.

Instead of manually typing commands one by one:

```bash
mkdir project
cd project
touch notes.txt
echo "Hello" > notes.txt
```

We can save them inside a file:

```bash
#!/bin/bash

mkdir project
cd project
touch notes.txt
echo "Hello" > notes.txt
```

Then execute everything automatically:

```bash
./setup.sh
```

This allows repetitive tasks to be automated efficiently.

---

# Why Bash Scripting Exists

System administrators and security professionals often perform repetitive tasks, such as:

- Creating users
- Managing files
- Backing up servers
- Parsing logs
- Checking services
- Running scheduled jobs
- Monitoring systems

Performing these tasks manually is inefficient and error-prone.

Bash scripting automates these workflows, reducing both time and human error.

---

# Why Bash Matters in Cybersecurity

Bash is heavily used throughout the cybersecurity industry.

### Penetration Testing

Automate:

- Reconnaissance
- Port scanning
- Enumeration
- Report generation
- Payload execution

Example:

```bash
./recon.sh 10.10.10.5
```

Instead of manually running:

- Nmap
- Gobuster
- FFUF
- WhatWeb
- Nikto

---

### Blue Team

Bash is commonly used for:

- Log analysis
- Threat hunting
- Backup automation
- System monitoring
- Incident response
- IOC collection

---

### DevOps & Cloud

Engineers use Bash to automate:

- Server provisioning
- Docker deployment
- Kubernetes setup
- Package installation
- CI/CD pipelines

---

# Concepts Covered

This room covers the fundamental building blocks of Bash scripting:

- Bash syntax
- Shell scripts
- Shebang
- Variables
- Parameters
- User input
- Arrays
- Conditionals
- File testing
- Operators
- Bash debugging
- Automation

These concepts are foundational for writing reusable Linux scripts.

---

# Terminology

| Term | Description |
|------|-------------|
| Bash | Bourne Again SHell, the default shell for many Linux systems |
| Shell | Command-line interface between the user and the operating system |
| Script | A file containing commands executed sequentially |
| Interpreter | Software that reads and executes source code line by line |
| Variable | A named location used to store data |
| Parameter | Input passed to a script when it is executed |
| Array | A variable capable of storing multiple values |
| Index | Numeric position of an item within an array |
| Conditional | Logic used to execute code only when a condition is met |
| Debugging | The process of finding and fixing errors in code |

---

# Technologies Used

- Bash
- Linux Terminal
- GNU Core Utilities
- Bash Built-in Commands

---

# Environment

| Component | Purpose |
|----------|---------|
| Kali Linux / AttackBox | Script development and execution |
| Bash | Script interpreter |
| Terminal | Running Bash scripts |
| Linux File System | File creation and permission testing |

---

# Skills Gained

- Linux scripting fundamentals
- Bash syntax
- Command automation
- User input handling
- File manipulation
- Script debugging
- Conditional logic
- Working with arrays
- Building reusable command-line tools

---

# Bash Fundamentals

This section introduces the fundamental structure of a Bash script and how Bash executes commands inside Linux.

Understanding these concepts is essential before learning variables, loops, or automation.

---

# What is a Shell Script?

A shell script is simply a text file containing one or more shell commands that are executed sequentially.

Instead of manually typing commands one by one:

```bash
pwd
whoami
date
```

We can save them inside a file:

```bash
#!/bin/bash

pwd
whoami
date
```

and execute everything automatically.

---

# The Shebang (`#!/bin/bash`)

Every Bash script typically begins with:

```bash
#!/bin/bash
```

This line is called the **Shebang**.

Its purpose is to tell the operating system which interpreter should execute the script.

Without a shebang, Linux may attempt to execute the file using another shell or fail to execute it correctly when launched directly.

Execution flow:

```text
example.sh
      │
      ▼
#!/bin/bash
      │
      ▼
/bin/bash
      │
      ▼
Execute each command
```

---

# Checking Bash Location

The Bash interpreter is usually located at:

```text
/bin/bash
```

We can verify its location using:

```bash
which bash
```

Example output:

```text
/bin/bash
```

---

# Creating Our First Script

Create a new file:

```bash
nano example.sh
```

Example script:

```bash
#!/bin/bash

echo "Hello World!"
```

Save the file and exit Nano.

---

# The `echo` Command

The `echo` command prints text to the terminal.

Syntax:

```bash
echo "text"
```

Example:

```bash
echo "Hello World!"
```

Output:

```text
Hello World!
```

It behaves similarly to Python's `print()` function.

---

# Running Linux Commands Inside Bash

One of Bash's greatest strengths is that it can execute any Linux command.

Example:

```bash
#!/bin/bash

pwd
whoami
date
```

When executed, Bash runs each command from top to bottom.

Example output:

```text
/home/kali
kali
Tue Aug 18 12:15:42 UTC 2026
```

---

# Example Script

```bash
#!/bin/bash

echo "Hello World"

whoami

id
```

Output:

```text
Hello World
root
uid=0(root) gid=0(root) groups=0(root),141(kaboxer)
```

---

# Understanding `whoami`

The `whoami` command displays the username of the currently logged-in user.

Syntax:

```bash
whoami
```

Example output:

```text
root
```

This is useful when verifying which account is executing a script.

---

# Understanding `id`

The `id` command displays detailed information about the current user.

Syntax:

```bash
id
```

Example output:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

Output breakdown:

| Field | Meaning |
|--------|---------|
| UID | User ID |
| GID | Primary Group ID |
| Groups | Additional groups the user belongs to |

---

# Making Scripts Executable

Newly created files usually do not have execute permission.

Check permissions:

```bash
ls -l
```

Example:

```text
-rw-r--r-- example.sh
```

Notice there is no **x** (execute permission).

To make the script executable:

```bash
chmod +x example.sh
```

Check again:

```bash
ls -l
```

Example:

```text
-rwxr-xr-x example.sh
```

Now the script can be executed directly.

---

# Running Bash Scripts

Once executable, run the script using:

```bash
./example.sh
```

Example output:

```text
Hello World
root
uid=0(root) gid=0(root)
```

---

# Why Use `./`?

Linux does **not** automatically search the current directory when executing commands.

The shell only searches directories listed in the `$PATH` environment variable.

Using:

```bash
./example.sh
```

tells Bash to execute the file located in the current directory.

---

# Alternative Execution Method

A script can also be executed directly with Bash:

```bash
bash example.sh
```

Differences:

| `./example.sh` | `bash example.sh` |
|----------------|-------------------|
| Requires execute permission | Does not require execute permission |
| Uses the shebang | Executes using Bash directly |
| Behaves like an executable program | Behaves like an interpreted script |

---

# Script Execution Flow

When executing:

```bash
./example.sh
```

Linux performs the following steps:

```text
User
 │
 ▼
Run ./example.sh
 │
 ▼
Kernel opens the file
 │
 ▼
Read Shebang
 │
 ▼
Launch /bin/bash
 │
 ▼
Bash reads the script
 │
 ▼
Execute commands sequentially
 │
 ▼
Display output
```

---

# Commands Used

## `echo`

### Purpose

Print text to the terminal.

### Syntax

```bash
echo "text"
```

### Example

```bash
echo "Hello"
```

Output:

```text
Hello
```

---

## `whoami`

### Purpose

Display the current username.

### Syntax

```bash
whoami
```

Example output:

```text
kali
```

---

## `id`

### Purpose

Display detailed user identity information.

### Syntax

```bash
id
```

Example output:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

---

## `chmod`

### Purpose

Modify file permissions.

### Syntax

```bash
chmod +x filename
```

### Flag Breakdown

| Flag | Description |
|------|-------------|
| `+` | Add permission |
| `x` | Execute permission |

Example:

```bash
chmod +x example.sh
```

---

## `which`

### Purpose

Locate an executable program.

### Syntax

```bash
which command
```

Example:

```bash
which bash
```

Output:

```text
/bin/bash
```

---

# Pentester Notes

Bash scripting is heavily used during penetration testing to automate repetitive tasks.

Common examples include:

- Running Nmap against multiple hosts.
- Performing directory enumeration automatically.
- Launching reconnaissance tools sequentially.
- Organizing scan results into folders.
- Creating helper scripts for CTFs and lab environments.

Rather than executing dozens of commands manually, a single Bash script can perform the entire workflow automatically.

---

# Key Takeaways

- Every Bash script begins with a **Shebang (`#!/bin/bash`)**.
- Bash executes commands sequentially from top to bottom.
- Linux commands can be embedded directly into Bash scripts.
- Scripts require execute permission when run using `./script.sh`.
- `chmod +x` makes a script executable.
- `whoami` and `id` are commonly used to identify the current user.
- Bash forms the foundation of Linux automation and cybersecurity scripting.

---

# Variables & Parameters

Variables and parameters allow Bash scripts to become dynamic.

Instead of hardcoding values inside a script, we can store data inside variables or receive values from users through command-line arguments or interactive input.

These concepts are essential for building reusable and flexible automation scripts.

---

# Variables

A variable is a named container that stores data.

Instead of repeatedly writing the same value throughout a script, we can store it once and reuse it whenever needed.

Example:

```bash
name="Jammy"
```

Here:

- `name` is the variable.
- `"Jammy"` is the value stored inside it.

---

# Variable Syntax

General syntax:

```bash
variable=value
```

Example:

```bash
username="alex"
age=21
```

> **Important:** Do **not** place spaces around the equals sign (`=`).

Correct:

```bash
name="Jammy"
```

Incorrect:

```bash
name = "Jammy"
```

The second example causes Bash to interpret `name` as a command instead of a variable assignment.

---

# Accessing Variables

To retrieve the value stored inside a variable, prefix its name with `$`.

Example:

```bash
name="Jammy"

echo $name
```

Output:

```text
Jammy
```

Without the `$`, Bash treats the text as a literal string.

Example:

```bash
echo name
```

Output:

```text
name
```

---

# Multiple Variables

Variables can be combined inside strings.

Example:

```bash
name="Jammy"
age=21

echo "$name is $age years old"
```

Output:

```text
Jammy is 21 years old
```

This feature is commonly known as **variable expansion**.

---

# Why Variables Matter

Variables make scripts easier to maintain.

Without variables:

```bash
echo "Alex"
mkdir Alex
touch Alex.txt
```

Changing the username requires editing every occurrence.

Using variables:

```bash
name="Alex"

echo "$name"
mkdir "$name"
touch "$name.txt"
```

Now only one line needs to be modified.

---

# Variable Expansion

When Bash encounters:

```bash
echo $name
```

it replaces `$name` with its stored value before executing the command.

Process:

```text
$name
   │
   ▼
Jammy
   │
   ▼
echo Jammy
   │
   ▼
Jammy
```

---

# Parameters

Parameters allow information to be passed to a script when it is executed.

Example:

```bash
./example.sh Alex
```

Here:

- `Alex` is the first command-line argument.

---

# Positional Parameters

Bash automatically stores command-line arguments in special variables.

| Parameter | Description |
|-----------|-------------|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `$#` | Number of supplied arguments |
| `$@` | All supplied arguments |

---

# Using `$1`

Example:

```bash
#!/bin/bash

name=$1

echo $name
```

Run:

```bash
./example.sh Alex
```

Output:

```text
Alex
```

Execution flow:

```text
./example.sh Alex
        │
        ▼
      $1 = Alex
        │
        ▼
name = Alex
        │
        ▼
echo Alex
```

---

# Using Multiple Parameters

Example:

```bash
#!/bin/bash

echo "First: $1"
echo "Second: $2"
```

Run:

```bash
./example.sh Alex Tony
```

Output:

```text
First: Alex
Second: Tony
```

---

# Interactive User Input

Sometimes we want users to enter information after the script starts.

Bash provides the `read` command for this purpose.

Example:

```bash
#!/bin/bash

echo "Enter your name"

read name

echo "Your name is $name"
```

Example execution:

```text
Enter your name

Alex

Your name is Alex
```

---

# How `read` Works

Execution flow:

```text
Script starts
      │
      ▼
Display prompt
      │
      ▼
Wait for user input
      │
      ▼
Store input in variable
      │
      ▼
Continue execution
```

Unlike command-line parameters, `read` pauses the script until the user presses **Enter**.

---

# Parameter vs `read`

| Command-Line Parameters | `read` |
|--------------------------|--------|
| Input provided before execution | Input provided during execution |
| Suitable for automation | Suitable for interactive scripts |
| Faster for repeated execution | More user-friendly |
| Frequently used in command-line tools | Frequently used in menus and setup scripts |

---

# Debugging Bash Scripts

Debugging is the process of identifying and fixing errors in code.

Bash includes built-in debugging capabilities.

Execute a script in debug mode:

```bash
bash -x example.sh
```

Example output:

```text
+ echo Hello World
Hello World

+ whoami
root

+ id
uid=0(root) gid=0(root)
```

The `+` symbol indicates each command immediately before it is executed.

---

# Partial Debugging

Debugging can also be enabled only for a specific section of a script.

Example:

```bash
#!/bin/bash

echo "Starting"

set -x

whoami
id

set +x

echo "Finished"
```

Everything between `set -x` and `set +x` is displayed in debug mode.

---

# Commands Used

## `read`

### Purpose

Receive user input from the terminal.

### Syntax

```bash
read variable
```

Example:

```bash
read username
```

---

## `bash -x`

### Purpose

Execute a Bash script while displaying every command before execution.

### Syntax

```bash
bash -x script.sh
```

---

## `set -x`

### Purpose

Enable debugging mode within a running script.

---

## `set +x`

### Purpose

Disable debugging mode.

---

# Pentester Notes

Variables and parameters are heavily used during penetration testing.

Common examples include:

```bash
TARGET=$1

WORDLIST="/usr/share/seclists/Discovery/Web-Content/common.txt"

nmap -A $TARGET

ffuf -u http://$TARGET/FUZZ -w $WORDLIST
```

Instead of editing the script every time the target changes, only the command-line argument needs to change.

Interactive input (`read`) is commonly used in custom tools that request usernames, passwords, domains, or IP addresses before launching scans.

---

# Key Takeaways

- Variables store reusable data.
- Variable assignments cannot contain spaces around `=`.
- `$` retrieves the value stored inside a variable.
- Positional parameters (`$1`, `$2`, etc.) receive command-line arguments.
- `$0` contains the script name, while `$#` stores the number of supplied arguments.
- `read` allows interactive user input.
- `bash -x`, `set -x`, and `set +x` help debug Bash scripts by displaying executed commands.

---

# Arrays

Arrays allow a single variable to store multiple values.

Instead of creating many separate variables, an array groups related data together and allows each item to be accessed using an **index**.

Arrays are extremely useful when working with lists of hosts, users, filenames, directories, ports, or any collection of related information.

---

# What is an Array?

A normal variable stores only one value.

Example:

```bash
name="Jammy"
```

An array stores multiple values.

Example:

```bash
transport=('car' 'train' 'bike' 'bus')
```

Visual representation:

```text
transport
│
├── [0] car
├── [1] train
├── [2] bike
└── [3] bus
```

---

# Why Use Arrays?

Without arrays:

```bash
host1="10.10.10.5"
host2="10.10.10.6"
host3="10.10.10.7"
host4="10.10.10.8"
```

Managing many variables quickly becomes difficult.

Using an array:

```bash
hosts=("10.10.10.5" "10.10.10.6" "10.10.10.7" "10.10.10.8")
```

All related values are stored together, making scripts cleaner and easier to maintain.

---

# Zero-Based Indexing

Bash arrays use **zero-based indexing**.

This means the first element is always at index **0**.

Example:

```bash
transport=('car' 'train' 'bike' 'bus')
```

| Index | Value |
|------:|-------|
| 0 | car |
| 1 | train |
| 2 | bike |
| 3 | bus |

Visualization:

```text
Index

0      1       2      3
│      │       │      │
▼      ▼       ▼      ▼

car  train   bike   bus
```

---

# Creating Arrays

General syntax:

```bash
array_name=('value1' 'value2' 'value3')
```

Example:

```bash
transport=('car' 'train' 'bike' 'bus')
```

Each element is separated by a space and enclosed within parentheses.

---

# Displaying All Elements

To print every element inside an array:

```bash
echo "${transport[@]}"
```

Output:

```text
car train bike bus
```

Explanation:

| Symbol | Meaning |
|--------|---------|
| `${}` | Parameter expansion |
| `@` | All elements in the array |

---

# Accessing Individual Elements

An individual element can be accessed using its index.

Example:

```bash
echo "${transport[1]}"
```

Output:

```text
train
```

Execution:

```text
transport

0 car

1 train ◄

2 bike

3 bus
```

---

# Modifying Array Elements

An existing element can be replaced by assigning a new value to its index.

Example:

```bash
transport[1]="trainride"
```

Array contents:

```text
car trainride bike bus
```

Output:

```bash
echo "${transport[@]}"
```

```text
car trainride bike bus
```

---

# Removing Elements

Elements can be removed using the `unset` command.

Example:

```bash
unset transport[1]
```

The element at index **1** is removed.

Remaining elements:

```text
car bike bus
```

> **Note:** Removing an element does not automatically renumber every index. Bash arrays can contain "gaps" after elements are removed.

---

# Example Script

```bash
#!/bin/bash

transport=('car' 'train' 'bike' 'bus')

echo "Original array:"
echo "${transport[@]}"

echo

echo "Second item:"
echo "${transport[1]}"

echo

transport[1]="trainride"

echo "Modified array:"
echo "${transport[@]}"
```

Output:

```text
Original array:
car train bike bus

Second item:
train

Modified array:
car trainride bike bus
```

---

# Arrays vs Variables

| Variable | Array |
|----------|-------|
| Stores one value | Stores multiple values |
| Single identifier | Indexed elements |
| Simple data | Collections of related data |

Example:

Variable:

```bash
name="Jammy"
```

Array:

```bash
names=("Jammy" "Alex" "Tony")
```

---

# Common Use Cases

Arrays are useful for storing:

- IP addresses
- Hostnames
- Usernames
- File paths
- Wordlists
- Services
- Ports
- Domains

Example:

```bash
targets=("10.10.10.5" "10.10.10.6" "10.10.10.7")
```

Later, these values can be processed automatically using loops.

---

# Commands Used

## `unset`

### Purpose

Remove an element from an array or delete an entire variable.

### Syntax

Remove one element:

```bash
unset array[index]
```

Example:

```bash
unset transport[1]
```

Remove the entire array:

```bash
unset transport
```

---

## `echo`

### Purpose

Display array contents.

### Syntax

Display all elements:

```bash
echo "${array[@]}"
```

Display one element:

```bash
echo "${array[index]}"
```

---

# Red Team Perspective

Arrays are commonly used to automate repetitive penetration testing tasks.

Example:

```bash
targets=("10.10.10.5" "10.10.10.6" "10.10.10.7")
```

Combined with loops, a single script can scan multiple hosts automatically.

Typical uses include:

- Multiple target IPs
- Domain lists
- Directory wordlists
- Payload collections
- Port lists

Arrays make automation scripts reusable and scalable.

---

# Blue Team Perspective

System administrators frequently use arrays for:

- Backup directories
- Log locations
- Critical services
- User accounts
- Monitoring targets

Example:

```bash
services=("nginx" "ssh" "mysql")
```

Each service can later be checked automatically within a monitoring script.

---

# Common Mistakes

### Forgetting Zero-Based Indexing

Incorrect expectation:

```text
transport[1] = car
```

Correct:

```text
transport[0] = car
transport[1] = train
```

---

### Forgetting Quotes Around Expansion

Preferred:

```bash
echo "${transport[@]}"
```

Instead of:

```bash
echo ${transport[@]}
```

Using quotes helps preserve elements that contain spaces.

---

### Assuming `unset` Renumbers the Array

Removing an element does **not** automatically shift every index.

Always verify array contents after deleting elements.

---

# Interview Questions

### What is an array?

A variable capable of storing multiple values.

---

### What index does a Bash array start with?

Index **0**.

---

### How do you print every element?

```bash
echo "${array[@]}"
```

---

### How do you access the third element?

```bash
echo "${array[2]}"
```

---

### How do you remove an element?

```bash
unset array[index]
```

---

# Pentester Notes

Arrays become significantly more powerful when combined with loops.

For example:

```bash
targets=("10.10.10.5" "10.10.10.6" "10.10.10.7")
```

Later, a loop can automatically perform:

- Nmap scans
- Directory enumeration
- HTTP requests
- Vulnerability checks

against every target in the array.

This pattern is commonly seen in custom reconnaissance and automation scripts.

---

# Key Takeaways

- Arrays store multiple values inside a single variable.
- Bash arrays use **zero-based indexing**.
- `${array[@]}` displays every element.
- `${array[index]}` retrieves a specific element.
- Elements can be modified by assigning a new value to an index.
- `unset` removes elements or entire arrays.
- Arrays simplify automation by organizing related data into a single structure.

---

# Conditionals

Conditionals allow Bash scripts to make decisions based on whether a condition evaluates to **true** or **false**.

Instead of executing every command sequentially, the script can choose different execution paths depending on variables, user input, file permissions, or system state.

Conditionals are one of the most important concepts in Bash scripting because they enable automation that reacts intelligently to different situations.

---

# What is a Conditional?

A conditional checks whether a condition is true.

If the condition evaluates to **true**, one block of code is executed.

Otherwise, another block is executed.

Execution flow:

```text
            Condition
                │
        ┌───────┴────────┐
        │                │
      True            False
        │                │
 Execute Block A   Execute Block B
```

---

# Basic `if` Statement

The basic syntax of an `if` statement is:

```bash
if [ condition ]
then
    commands
else
    commands
fi
```

Every `if` statement ends with:

```bash
fi
```

which simply marks the end of the conditional block.

---

# Example: Comparing Numbers

```bash
#!/bin/bash

count=10

if [ $count -eq 10 ]
then
    echo "true"
else
    echo "false"
fi
```

Output:

```text
true
```

Since `count` equals `10`, the condition evaluates to **true**.

---

# Understanding the Syntax

Consider:

```bash
if [ $count -eq 10 ]
```

Breaking it down:

| Component | Purpose |
|-----------|---------|
| `if` | Begin conditional |
| `[` | Start test expression |
| `$count` | Left operand |
| `-eq` | Equality operator |
| `10` | Right operand |
| `]` | End test expression |

> **Important:** Spaces around `[` and `]` are mandatory in Bash.

Correct:

```bash
if [ $count -eq 10 ]
```

Incorrect:

```bash
if [$count -eq 10]
```

---

# Comparison Operators

Bash provides several relational operators for comparing numeric values.

| Operator | Description |
|----------|-------------|
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal to |
| `-le` | Less than or equal to |

Examples:

Equal:

```bash
if [ $a -eq $b ]
```

Greater than:

```bash
if [ $a -gt $b ]
```

Less than:

```bash
if [ $a -lt $b ]
```

Not equal:

```bash
if [ $a -ne $b ]
```

---

# Comparing Strings

Strings are compared using the `=` operator.

Example:

```bash
#!/bin/bash

guess=$1

if [ "$guess" = "guessme" ]
then
    echo "They are equal"
else
    echo "They are not equal"
fi
```

Execution:

```bash
./example.sh guessme
```

Output:

```text
They are equal
```

Another execution:

```bash
./example.sh hello
```

Output:

```text
They are not equal
```

---

# Using Parameters in Conditions

Conditionals become much more useful when combined with parameters.

Example:

```bash
#!/bin/bash

password=$1

if [ "$password" = "secret" ]
then
    echo "Access Granted"
else
    echo "Access Denied"
fi
```

This demonstrates how Bash scripts can respond differently based on user input.

---

# Logical Operators

Multiple conditions can be combined using logical operators.

The room demonstrates the **AND** operator:

```bash
&&
```

Meaning:

> Both conditions must evaluate to **true**.

Example:

```bash
if [ -f "$file" ] && [ -w "$file" ]
```

Execution flow:

```text
File exists?
      │
      ▼
Writable?
      │
      ▼
Both True?
      │
      ▼
Execute commands
```

If either condition is false, the entire expression evaluates to false.

---

# File Tests

Bash provides built-in operators for testing files.

The room focuses on two common file tests.

---

## `-f`

Checks whether a regular file exists.

Example:

```bash
if [ -f notes.txt ]
```

Returns:

- True → File exists
- False → File does not exist

---

## `-w`

Checks whether the file is writable.

Example:

```bash
if [ -w notes.txt ]
```

Returns:

- True → Current user has write permission
- False → File cannot be written

---

# Practical Example

The room builds a script that:

1. Receives a filename as a parameter.
2. Checks whether the file exists.
3. Checks whether the file is writable.
4. Writes `"hello"` into the file.
5. Creates the file if it does not already exist.

Example:

```bash
#!/bin/bash

filename=$1

if [ -f "$filename" ] && [ -w "$filename" ]
then
    echo "hello" > "$filename"
else
    touch "$filename"
    echo "hello" > "$filename"
fi
```

Execution:

```bash
./example.sh hello.txt
```

Contents:

```text
hello
```

---

# Understanding the Redirection Operator

The script uses:

```bash
echo "hello" > hello.txt
```

The `>` operator redirects standard output into a file.

If the file already exists:

- Existing contents are overwritten.

If the file does not exist:

- The file is automatically created.

---

# The `touch` Command

Purpose:

Create an empty file.

Syntax:

```bash
touch filename
```

Example:

```bash
touch hello.txt
```

If the file already exists, `touch` simply updates its timestamp.

---

# Commands Used

## `touch`

### Purpose

Create an empty file.

### Syntax

```bash
touch filename
```

---

## File Test Operators

### Check file exists

```bash
-f filename
```

### Check file is writable

```bash
-w filename
```

---

## Output Redirection

Overwrite file contents:

```bash
echo "text" > file.txt
```

Append instead of overwrite:

```bash
echo "text" >> file.txt
```

---

# Real-World Usage

Conditionals are commonly used to:

- Verify files before modifying them.
- Check whether services are running.
- Validate user input.
- Confirm required tools are installed.
- Verify network connectivity.
- Prevent automation failures.

Without conditionals, scripts would blindly execute commands regardless of the system state.

---

# Red Team Perspective

Conditionals are widely used during offensive security automation.

Examples include:

- Verify a target is reachable before scanning.
- Check if Nmap is installed.
- Confirm payload generation succeeded.
- Ensure credentials were obtained before continuing.
- Detect successful reverse shell connections.

Example:

```bash
if ping -c 1 "$TARGET" >/dev/null
then
    nmap -A "$TARGET"
else
    echo "Host unreachable."
fi
```

This prevents wasting time scanning unavailable systems.

---

# Blue Team Perspective

System administrators frequently rely on conditionals to automate maintenance tasks.

Common examples include:

- Verify backup directories exist.
- Ensure configuration files are present.
- Check disk space before backups.
- Restart services only if they have stopped.
- Monitor log files for anomalies.

Example:

```bash
if [ -f /etc/nginx/nginx.conf ]
then
    echo "Configuration found."
fi
```

---

# Common Mistakes

### Forgetting Spaces

Incorrect:

```bash
if [$count -eq 10]
```

Correct:

```bash
if [ $count -eq 10 ]
```

---

### Forgetting `fi`

Every `if` statement must end with:

```bash
fi
```

Otherwise, Bash reports a syntax error.

---

### Using Numeric Operators for Strings

Incorrect:

```bash
if [ "$name" -eq "Alex" ]
```

Correct:

```bash
if [ "$name" = "Alex" ]
```

Use:

- `=` for strings.
- `-eq`, `-gt`, `-lt`, etc. for numbers.

---

# Interview Questions

### What is the purpose of an `if` statement?

Execute code only when a condition evaluates to true.

---

### What does `-eq` mean?

Checks whether two numeric values are equal.

---

### What does `-f` check?

Whether a regular file exists.

---

### What does `-w` check?

Whether the current user has permission to write to the file.

---

### What is the purpose of `&&`?

Logical **AND**.

Both conditions must evaluate to true.

---

# Pentester Notes

Conditionals are fundamental to writing intelligent automation scripts.

Instead of blindly executing every command, scripts can verify prerequisites before continuing.

Examples include:

- Skip offline hosts.
- Check if discovered files exist.
- Validate enumeration results.
- Confirm successful exploitation before privilege escalation.
- Exit gracefully when required resources are unavailable.

These defensive checks make automation significantly more reliable during engagements.

---

# Key Takeaways

- Conditionals allow scripts to make decisions.
- Every `if` statement ends with `fi`.
- Numeric comparisons use operators such as `-eq`, `-gt`, and `-lt`.
- String comparisons use `=`.
- `-f` checks file existence.
- `-w` checks write permissions.
- `&&` combines multiple conditions using logical AND.
- Conditionals are essential for reliable system administration, automation, and penetration testing scripts.

---

# Commands Reference

This section summarizes the Bash commands, operators, and built-in utilities used throughout the room.

---

# `echo`

## Purpose

Display text or variable values on the terminal.

## Syntax

```bash
echo "text"
```

## Examples

Print text:

```bash
echo "Hello World"
```

Print a variable:

```bash
name="Jammy"

echo "$name"
```

## Example Output

```text
Hello World

Jammy
```

## Common Use Cases

- Display information
- Print variable values
- Display debugging messages
- Redirect output into files

---

# `chmod`

## Purpose

Modify file permissions.

## Syntax

```bash
chmod +x filename
```

## Flag Breakdown

| Flag | Description |
|------|-------------|
| `+` | Add permission |
| `x` | Execute permission |

## Example

```bash
chmod +x example.sh
```

## Result

The script becomes executable and can be run using:

```bash
./example.sh
```

---

# `whoami`

## Purpose

Display the currently logged-in user.

## Syntax

```bash
whoami
```

## Example Output

```text
kali
```

## Common Use Cases

- Verify execution context
- Confirm user privileges
- Troubleshooting

---

# `id`

## Purpose

Display detailed user identity information.

## Syntax

```bash
id
```

## Example Output

```text
uid=1000(kali)
gid=1000(kali)
groups=1000(kali),27(sudo)
```

## Output Breakdown

| Field | Description |
|--------|-------------|
| UID | User Identifier |
| GID | Primary Group |
| Groups | Secondary group memberships |

---

# `which`

## Purpose

Locate the executable path of a program.

## Syntax

```bash
which command
```

## Example

```bash
which bash
```

## Output

```text
/bin/bash
```

---

# `read`

## Purpose

Receive interactive user input.

## Syntax

```bash
read variable
```

## Example

```bash
echo "Enter your name"

read name

echo "$name"
```

---

# `touch`

## Purpose

Create an empty file.

## Syntax

```bash
touch filename
```

## Example

```bash
touch notes.txt
```

If the file already exists, only its timestamp is updated.

---

# `unset`

## Purpose

Remove variables or array elements.

## Syntax

Remove an array element:

```bash
unset array[index]
```

Remove an entire variable:

```bash
unset variable
```

## Example

```bash
unset transport[1]
```

---

# `bash -x`

## Purpose

Execute a Bash script in debug mode.

## Syntax

```bash
bash -x script.sh
```

## Example

```bash
bash -x example.sh
```

## Example Output

```text
+ echo Hello
Hello

+ whoami
root
```

Every executed command is displayed before execution.

---

# `set -x`

## Purpose

Enable debugging for part of a script.

## Example

```bash
set -x
```

---

# `set +x`

## Purpose

Disable debugging.

## Example

```bash
set +x
```

---

# Bash Special Parameters

| Parameter | Description |
|-----------|-------------|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `$#` | Number of supplied arguments |
| `$@` | All supplied arguments |
| `$*` | All supplied arguments as a single string |
| `$$` | Current process ID |
| `$?` | Exit status of the previous command |

---

# Conditional Operators

## Numeric Operators

| Operator | Meaning |
|----------|---------|
| `-eq` | Equal to |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal |
| `-le` | Less than or equal |

---

## String Operator

| Operator | Meaning |
|----------|---------|
| `=` | Equal |

---

## File Test Operators

| Operator | Description |
|----------|-------------|
| `-f` | File exists |
| `-w` | File is writable |

---

# Output Redirection

Overwrite a file:

```bash
echo "hello" > file.txt
```

Append to a file:

```bash
echo "hello" >> file.txt
```

---

# Troubleshooting

## Problem

```text
Permission denied
```

### Cause

The script does not have execute permission.

### Solution

```bash
chmod +x script.sh
```

---

## Problem

```text
command not found
```

### Cause

- Command is not installed.
- Incorrect spelling.
- Executable is not included in `$PATH`.

### Solution

Verify the command exists:

```bash
which command
```

---

## Problem

```text
syntax error near unexpected token
```

### Cause

Common causes include:

- Missing `fi`
- Missing spaces around `[ ]`
- Incorrect quotation marks
- Unclosed strings

### Solution

Review the script syntax carefully or execute it using:

```bash
bash -x script.sh
```

---

## Problem

Variables display nothing.

Example:

```bash
echo $name
```

Output:

```text

```

### Cause

The variable was never assigned or was misspelled.

### Solution

Check assignments and use debug mode.

---

## Problem

Wrong comparison operator.

Incorrect:

```bash
if [ "$age" = 18 ]
```

Correct:

```bash
if [ "$age" -eq 18 ]
```

Use numeric operators for numbers and `=` for strings.

---

# Pentester Notes

Bash is one of the most valuable scripting languages for offensive security.

Typical automation tasks include:

## Reconnaissance

- Nmap automation
- DNS enumeration
- HTTP fingerprinting
- Directory brute forcing

---

## Enumeration

- SMB enumeration
- FTP checks
- Service identification
- User enumeration

---

## Post Exploitation

- Collecting system information
- Downloading artifacts
- Automating privilege enumeration
- Generating reports

---

## Blue Team Usage

Bash is also widely used for:

- Log parsing
- Backup automation
- Scheduled maintenance
- Service monitoring
- Incident response
- IOC collection

---

# Skills Gained

After completing this room, I learned how to:

- Write executable Bash scripts.
- Use the Shebang correctly.
- Execute Linux commands inside scripts.
- Store information using variables.
- Accept command-line arguments.
- Read interactive user input.
- Work with arrays.
- Access, modify, and remove array elements.
- Build conditional logic with `if` and `else`.
- Check file existence and permissions.
- Debug Bash scripts using built-in tools.
- Build reusable Linux automation scripts.

---

# Key Takeaways

- Bash is the standard scripting language for Linux automation.
- Scripts execute commands sequentially from top to bottom.
- Variables and parameters make scripts reusable.
- Arrays organize related data efficiently.
- Conditionals enable intelligent decision-making.
- Debugging tools simplify troubleshooting.
- Bash is heavily used in System Administration, DevOps, Cloud Engineering, Incident Response, and Penetration Testing.

---

# Future Learning Path

This room provides the foundation for more advanced Bash scripting concepts.

Recommended next topics include:

- `for` loops
- `while` loops
- Functions
- Case statements
- Exit codes
- Process management
- File descriptors
- Pipes and filters
- Cron jobs
- Bash scripting best practices
- Secure shell scripting
- Linux automation projects

These topics build upon the concepts learned here and are essential for writing production-quality automation scripts.

---

# References

- TryHackMe — Bash Scripting Room
- GNU Bash Manual
- Bash Reference Manual
- Linux man pages (`man bash`)
- Bash Cheat Sheet (Devhints)

---

# Tags

`bash` `linux` `shell-scripting` `automation` `tryhackme` `cybersecurity` `linux-administration` `devops` `penetration-testing` `scripting`

