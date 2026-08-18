# Python Basics

> Learn the fundamentals of Python programming and understand how Python is used to build automation scripts, security tools, and penetration testing utilities.

---

# Executive Summary

Python is one of the most widely used programming languages in cybersecurity due to its simplicity, readability, and extensive ecosystem of libraries. From automating repetitive tasks to developing exploit scripts and security tools, Python enables security professionals to work more efficiently and solve complex problems with fewer lines of code.

In this room, I learned the fundamental concepts of Python programming, including variables, data types, operators, conditional statements, loops, functions, file handling, and importing libraries. These concepts form the foundation required before moving on to more advanced topics such as scripting for penetration testing, network automation, malware analysis, and exploit development.

Rather than focusing solely on Python syntax, this room emphasizes understanding the logic behind programming—how computers process instructions, make decisions, repeat actions, and organize reusable code.

---

# Learning Objectives

After completing this room, I was able to:

- Understand what Python is and why it is widely used in cybersecurity.
- Understand basic Python syntax.
- Print output to the terminal.
- Perform mathematical and logical operations.
- Store and manipulate data using variables.
- Understand common Python data types.
- Make decisions using conditional statements.
- Build repetitive tasks using loops.
- Create reusable code with functions.
- Read from and write to files.
- Import and use Python libraries.
- Understand how these concepts apply to penetration testing and defensive security.

---

# Prerequisites

Before starting this room, it is helpful to understand:

- Basic computer operations
- Basic command-line usage
- Simple mathematical operations
- Basic problem-solving skills

No prior programming experience is required.

---

# Why Python Matters in Cybersecurity

Programming is not a strict requirement to become a penetration tester or security analyst, but it is one of the most valuable skills a cybersecurity professional can have.

Many tasks performed during security assessments are repetitive:

- Scanning hundreds of hosts
- Enumerating web applications
- Parsing log files
- Testing multiple credentials
- Reading large datasets
- Automating reconnaissance

Without programming, these tasks would have to be performed manually.

Python allows these repetitive tasks to be automated.

Instead of spending hours repeating the same commands, a Python script can complete them in seconds.

---

## Python in Offensive Security

Python is heavily used by Red Team members and penetration testers for:

- Network scanning
- Web enumeration
- Brute-force automation
- Exploit development
- Payload generation
- Active Directory enumeration
- API interaction
- Parsing scan results
- Custom security tools
- Capture The Flag (CTF) scripting

Many well-known offensive security tools are written in Python, including:

- sqlmap
- Impacket
- Pwntools
- Scapy
- mitmproxy
- CrackMapExec (NetExec)
- Responder

---

## Python in Defensive Security

Python is equally valuable for Blue Team operations.

Security analysts use Python to automate:

- Log analysis
- IOC extraction
- Threat hunting
- Malware analysis
- Digital forensics
- SIEM integrations
- Report generation
- File integrity checking
- Security monitoring

Python has become a common language across nearly every cybersecurity discipline.

---

# What is Python?

Python is a **high-level interpreted programming language** designed to be simple, readable, and easy to learn.

Unlike lower-level languages such as C, Python hides many of the underlying implementation details, allowing developers to focus on solving problems rather than managing hardware resources.

For example:

### Python

```python
print("Hello World")
```

### C

```c
#include <stdio.h>

int main()
{
    printf("Hello World");
    return 0;
}
```

Although both programs produce the same output, Python requires significantly less code.

This simplicity is one of the primary reasons why Python has become one of the most popular programming languages in both software development and cybersecurity.

---

# Why Was Python Created?

Before Python became popular, many programming languages required large amounts of boilerplate code and complicated syntax.

Python was created with a philosophy that emphasizes:

- Simplicity
- Readability
- Productivity

One of Python's most famous design principles states:

> **"Code is read much more often than it is written."**

This philosophy encourages developers to write clean, understandable code that can easily be maintained by others.

---

# What is Programming?

Programming is the process of writing instructions that tell a computer how to perform specific tasks.

A program is simply a sequence of instructions executed one after another.

For example:

```text
Start Program
      │
      ▼
Print "Hello World"
      │
      ▼
Exit Program
```

As programs become more advanced, they begin making decisions, repeating tasks, storing data, and interacting with external systems.

Python provides simple syntax for performing all of these operations.

---

# Python Syntax

Programming languages follow strict grammatical rules known as **syntax**.

Syntax defines how code must be written so that the interpreter can understand it.

A small mistake can prevent an entire program from running.

Correct example:

```python
print("Hello")
```

Incorrect example:

```python
Print("Hello")
```

Python treats `print` and `Print` as completely different identifiers because Python is **case-sensitive**.

Another example:

Correct:

```python
if age > 18:
    print("Adult")
```

Incorrect:

```python
if age > 18
    print("Adult")
```

The missing colon (`:`) results in a `SyntaxError`.

---

# The Python Interpreter

When writing Python code, the CPU does not directly understand statements such as:

```python
print("Hello")
```

Instead, Python uses an **interpreter** to translate Python code into instructions that the computer can execute.

The simplified execution flow looks like this:

```text
Python Source Code
        │
        ▼
Python Interpreter
        │
        ▼
Bytecode
        │
        ▼
Python Virtual Machine (PVM)
        │
        ▼
Machine Instructions
        │
        ▼
CPU
```

Because Python is interpreted rather than compiled directly into machine code, it is generally slower than languages such as C or Rust.

However, the productivity gained from Python's simplicity makes it an excellent choice for scripting and automation.

---

# Hello World

The traditional first program in almost every programming language is:

```python
# This is an example comment
print("Hello World")
```

Output:

```text
Hello World
```

Although simple, this program introduces several important concepts.

---

## Comments

A comment begins with the `#` symbol.

```python
# This is a comment
```

Comments are ignored by Python and exist solely to help humans understand the code.

Good comments explain **why** something is done rather than simply repeating what the code already says.

---

## The `print()` Function

`print()` is one of Python's built-in functions.

Its purpose is to display information in the terminal.

Example:

```python
print("Cyber Journey")
```

Output:

```text
Cyber Journey
```

Anything enclosed within quotation marks is treated as a **string**, which represents textual data.

---

# Mathematical Operators

Python supports standard mathematical operations.

| Operator | Description | Example | Result |
|----------|-------------|---------|--------|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `10 - 4` | `6` |
| `*` | Multiplication | `6 * 7` | `42` |
| `/` | Division | `10 / 2` | `5.0` |
| `%` | Modulus (remainder) | `10 % 3` | `1` |
| `**` | Exponent | `5 ** 2` | `25` |

---

## Modulus Operator

The modulus operator (`%`) returns the remainder after division.

Example:

```python
10 % 3
```

Calculation:

```text
10 ÷ 3

Quotient = 3

Remainder = 1
```

Result:

```text
1
```

This operator is commonly used to determine whether a number is even or odd.

---

# Comparison Operators

Comparison operators compare two values and always produce a Boolean result (`True` or `False`).

| Operator | Description |
|----------|-------------|
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

Example:

```python
age = 20

print(age >= 18)
```

Output:

```text
True
```

---

# Boolean Operators

Boolean operators combine multiple conditions.

| Operator | Description |
|----------|-------------|
| `and` | All conditions must be true |
| `or` | At least one condition must be true |
| `not` | Reverses a Boolean value |

Example:

```python
age = 20
citizen = True

print(age >= 18 and citizen)
```

Output:

```text
True
```

These operators become extremely important when working with conditional statements and loops.

---

# Red Team Notes

Understanding Python fundamentals enables penetration testers to:

- Read public exploit code.
- Modify proof-of-concept exploits.
- Automate reconnaissance.
- Create custom enumeration scripts.
- Process scan results automatically.
- Develop lightweight offensive tools.

Even simple concepts such as variables, operators, and loops are heavily used in nearly every penetration testing script.

---

# Blue Team Notes

Blue Team members use these same programming concepts to:

- Analyze log files.
- Automate incident response.
- Parse security events.
- Generate reports.
- Process threat intelligence feeds.
- Build defensive automation.

Python serves as a common language across both offensive and defensive security operations.

---

# Key Takeaways

- Python is one of the most important programming languages in cybersecurity.
- Clean syntax makes Python beginner-friendly while remaining powerful enough for professional use.
- Syntax defines the grammatical rules of Python and must be followed exactly.
- Comments improve code readability but are ignored during execution.
- Mathematical operators perform calculations.
- Comparison operators evaluate conditions.
- Boolean operators combine multiple logical conditions.
- These concepts provide the foundation for variables, conditionals, loops, functions, and automation scripts covered in the remainder of the room.

# Variables

Variables are containers used to store data in memory so it can be accessed, modified, and reused throughout a program.

Instead of repeatedly typing the same value, we can assign it to a variable and reference that variable whenever needed.

General syntax:

```python
variable_name = value
```

Example:

```python
food = "ice cream"
money = 2000
```

Here:

- `food` stores the string `"ice cream"`
- `money` stores the integer `2000`

---

## Variables as Containers

A useful way to think about variables is as labeled boxes.

```text
┌──────────────┐
│    food      │ ─────► "ice cream"
└──────────────┘

┌──────────────┐
│    money     │ ─────► 2000
└──────────────┘
```

The label is the **variable name**, while the contents represent the **stored value**.

---

## Assignment Operator (`=`)

Many beginners confuse the assignment operator (`=`) with the mathematical equals sign.

In Python:

```python
age = 30
```

does **not** mean:

> age equals 30

Instead, it means:

> Store the value **30** inside a variable named **age**.

The `=` symbol assigns a value to a variable.

---

## Updating Variables

Variables can be updated at any point during program execution.

Example:

```python
age = 30
age = age + 1

print(age)
```

Output:

```text
31
```

Python evaluates the right-hand side first:

```text
age = age + 1

↓

30 + 1

↓

31
```

The new value replaces the old one stored inside the variable.

---

## Variable Naming Best Practices

Variable names should clearly describe the data they contain.

Good examples:

```python
user_name
password
target_ip
scan_result
failed_attempts
```

Poor examples:

```python
x
abc
temp1
test
```

Meaningful names improve readability and make future maintenance much easier.

---

# Data Types

Every variable stores a specific type of data.

Python automatically determines the data type based on the assigned value.

Different data types behave differently.

For example:

```python
20 + 5
```

performs mathematical addition.

However:

```python
"20" + "5"
```

produces:

```text
205
```

because both values are strings and are concatenated together.

---

## String (`str`)

A **String** stores textual data.

Examples:

```python
name = "Alice"
city = "Jakarta"
website = "tryhackme.com"
```

Strings must be enclosed in quotation marks.

---

## Integer (`int`)

An Integer stores whole numbers.

Examples:

```python
age = 25
port = 80
score = 100
```

Integers do not contain decimal points.

---

## Float (`float`)

A Float stores decimal numbers.

Examples:

```python
temperature = 36.5
rating = 9.8
price = 10.99
```

Floats are commonly used whenever precision beyond whole numbers is required.

---

## Boolean (`bool`)

A Boolean stores only one of two possible values:

```python
True
False
```

Examples:

```python
logged_in = True
is_admin = False
```

Boolean values are primarily used when making decisions with conditional statements.

---

## List (`list`)

A List stores multiple values inside a single variable.

Example:

```python
users = [
    "Alice",
    "Bob",
    "Charlie"
]
```

Lists may contain different data types:

```python
mixed = [
    "Alice",
    25,
    True,
    9.8
]
```

Lists become extremely useful when processing multiple targets, usernames, IP addresses, or files.

---

# Data Types in Practice

The room provides a simple movie database example.

| Field | Data Type | Example |
|--------|-----------|---------|
| Title | String | `"Star Wars"` |
| Rating | Float | `9.8` |
| Times Viewed | Integer | `24` |
| Favorite | Boolean | `True` |
| Seen By | List | `["Alice", "Bob"]` |

This demonstrates that real-world applications rarely use only one data type.

Instead, different types are combined to represent different kinds of information.

---

# Introduction to If Statements

Programs become useful when they are able to make decisions.

Conditional statements allow programs to execute different blocks of code depending on whether a condition evaluates to `True` or `False`.

General syntax:

```python
if condition:
    # code
else:
    # alternative code
```

Example:

```python
age = 20

if age >= 17:
    print("You are old enough to drive.")
else:
    print("You are NOT old enough to drive.")
```

---

## How an If Statement Works

Python evaluates the condition first.

```text
age >= 17
```

If the result is:

```text
True
```

Python executes the indented code beneath the `if`.

Otherwise:

```text
False
```

Python skips the `if` block and executes the `else` block.

---

## Execution Flow

```text
        Start
          │
          ▼
   Is age >= 17?
      │       │
    Yes       No
      │        │
      ▼        ▼
 Print Adult  Print Too Young
      │
      ▼
      End
```

Only **one** branch executes.

---

## Indentation

Unlike many programming languages, Python uses indentation to define blocks of code.

Correct:

```python
if age >= 18:
    print("Adult")
```

Incorrect:

```python
if age >= 18:
print("Adult")
```

Without indentation, Python raises an `IndentationError`.

For this reason, indentation is considered part of Python's syntax rather than simply a formatting preference.

---

## Else

The `else` block runs whenever every previous condition evaluates to `False`.

Example:

```python
password = "admin123"

if password == "admin123":
    print("Access Granted")
else:
    print("Access Denied")
```

Only one message will be printed.

---

# Shopping Basket Challenge

The room concludes with a simple exercise that combines several concepts learned so far.

Scenario:

- Customers spending **more than $100** receive free shipping.
- Otherwise, shipping costs **$1.20 per kilogram**.

Example solution:

```python
customer_basket_cost = 34
customer_basket_weight = 44

if customer_basket_cost > 100:
    print(customer_basket_cost)
else:
    shipping_cost = customer_basket_weight * 1.20
    total_cost = customer_basket_cost + shipping_cost
    print(total_cost)
```

Concepts reinforced by this challenge:

- Variables
- Mathematical operators
- Comparison operators
- If statements
- Assignment
- Program flow

---

# Common Beginner Mistakes

## Confusing `=` and `==`

Incorrect:

```python
if age = 18:
```

Correct:

```python
if age == 18:
```

`=` assigns a value.

`==` compares two values.

---

## Forgetting the Colon

Incorrect:

```python
if age >= 18
```

Correct:

```python
if age >= 18:
```

---

## Incorrect Indentation

Python relies on indentation to determine which statements belong inside a conditional block.

Incorrect indentation will cause an error.

---

## Mixing Data Types

This:

```python
"20" + "5"
```

produces:

```text
205
```

while:

```python
20 + 5
```

produces:

```text
25
```

Always be aware of the data type you are working with.

---

# Red Team Notes

Variables are heavily used during penetration testing to store:

- Target IP addresses
- Hostnames
- URLs
- Ports
- Credentials
- Payloads
- HTTP responses
- Enumeration results

Conditional statements determine how tools react to discovered information.

Examples include:

- Whether a port is open
- Whether authentication succeeded
- Whether a target is vulnerable
- Whether exploitation should continue

Nearly every penetration testing script relies on variables and conditional logic.

---

# Blue Team Notes

Blue Team automation also depends heavily on variables and conditions.

Common examples include:

- Counting failed login attempts
- Detecting suspicious processes
- Monitoring system health
- Parsing log files
- Triggering alerts
- Responding automatically to security events

These concepts serve as the foundation for larger security automation frameworks.

---

# Key Takeaways

- Variables provide reusable storage for data.
- Python automatically assigns an appropriate data type to each variable.
- The primary data types introduced in this room are String, Integer, Float, Boolean, and List.
- Conditional statements allow programs to make decisions based on Boolean expressions.
- Proper indentation is mandatory in Python and defines code blocks.
- Understanding variables and conditional logic is essential before learning loops, functions, and automation.

# Loops

One of the biggest advantages of programming is automation.

Without loops, repeating an action multiple times would require writing the same code repeatedly.

Example without a loop:

```python
print("Hello")
print("Hello")
print("Hello")
print("Hello")
print("Hello")
```

Example using a loop:

```python
for i in range(5):
    print("Hello")
```

Both examples produce the same result, but the loop is significantly shorter, easier to maintain, and far more scalable.

---

# What is a Loop?

A **loop** is a programming structure that repeatedly executes a block of code until a condition is met or until all items in a collection have been processed.

Loops are commonly used for:

- Processing multiple files
- Reading log entries
- Scanning multiple hosts
- Trying multiple passwords
- Enumerating ports
- Automating repetitive tasks

Python provides two primary loop types:

- `while`
- `for`

---

# While Loop

A **while loop** continues executing **as long as its condition remains True**.

General syntax:

```python
while condition:
    # code
```

Example:

```python
i = 1

while i <= 10:
    print(i)
    i = i + 1
```

Output:

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

---

## How a While Loop Works

Let's examine the execution process.

### Step 1

Initialize the variable.

```python
i = 1
```

---

### Step 2

Evaluate the condition.

```text
i <= 10
```

Result:

```text
True
```

Since the condition is true, Python enters the loop.

---

### Step 3

Execute the loop body.

```python
print(i)
```

Output:

```text
1
```

---

### Step 4

Update the variable.

```python
i = i + 1
```

Now:

```text
i = 2
```

---

### Step 5

Return to the beginning.

Python checks:

```text
2 <= 10
```

The process repeats until:

```text
11 <= 10
```

becomes:

```text
False
```

At that point, the loop terminates.

---

## While Loop Flow

```text
      i = 1
         │
         ▼
   Is i <= 10?
      │      │
     Yes     No
      │       │
      ▼       ▼
 Print(i)   Exit Loop
      │
      ▼
 i = i + 1
      │
      └───────────────▲
```

---

## Infinite Loops

One of the most common beginner mistakes is forgetting to update the loop variable.

Incorrect example:

```python
i = 1

while i <= 10:
    print(i)
```

Output:

```text
1
1
1
1
1
...
```

The condition never becomes false because `i` never changes.

This is called an **Infinite Loop**.

Infinite loops may cause programs to consume unnecessary CPU resources or appear frozen.

---

# For Loop

A **for loop** iterates over each item within a sequence (called an iterable).

General syntax:

```python
for variable in collection:
    # code
```

Unlike a `while` loop, a `for` loop automatically moves through each element without requiring manual updates.

---

## Example Using a List

```python
websites = [
    "facebook.com",
    "google.com",
    "amazon.com"
]

for site in websites:
    print(site)
```

Output:

```text
facebook.com
google.com
amazon.com
```

---

## What Happens Internally?

The list contains three elements.

```text
┌───────────────────┐
│ facebook.com      │
├───────────────────┤
│ google.com        │
├───────────────────┤
│ amazon.com        │
└───────────────────┘
```

Python performs the following:

Iteration 1

```text
site = facebook.com
```

Iteration 2

```text
site = google.com
```

Iteration 3

```text
site = amazon.com
```

Once every element has been processed, the loop automatically ends.

---

# The `range()` Function

Sometimes we don't have a list but simply want to repeat something a specific number of times.

Python provides the built-in `range()` function.

Example:

```python
for i in range(5):
    print(i)
```

Output:

```text
0
1
2
3
4
```

Notice that counting starts at **0**.

---

## Why Does Python Start at Zero?

Most programming languages use **zero-based indexing**.

For example:

```python
fruits = [
    "Apple",
    "Orange",
    "Banana"
]
```

Internally:

```text
Index

0 → Apple
1 → Orange
2 → Banana
```

For consistency, `range(5)` also begins at zero.

Therefore:

```python
range(5)
```

produces:

```text
0
1
2
3
4
```

instead of:

```text
1
2
3
4
5
```

---

## Different Forms of `range()`

### One Argument

```python
range(5)
```

Produces:

```text
0 1 2 3 4
```

---

### Two Arguments

```python
range(3, 8)
```

Produces:

```text
3
4
5
6
7
```

The second value is **exclusive**, meaning it is not included.

---

### Three Arguments

```python
range(2, 10, 2)
```

Produces:

```text
2
4
6
8
```

The third value specifies the **step size**.

---

# While vs For

Although both perform repetition, they are designed for different situations.

| While Loop | For Loop |
|------------|----------|
| Runs while a condition remains true | Iterates over a sequence |
| Number of iterations may be unknown | Number of iterations is usually known |
| Requires manual updates | Updates automatically |
| Easier to accidentally create infinite loops | Less error-prone |

### Use a While Loop When

- Waiting for user input
- Monitoring a service
- Running until a condition changes
- Number of iterations is unknown

Example:

```python
while logged_in:
    check_notifications()
```

---

### Use a For Loop When

- Reading files
- Processing lists
- Enumerating targets
- Scanning multiple hosts
- Repeating a fixed number of times

Example:

```python
for ip in ip_addresses:
    scan(ip)
```

---

# Loop Challenge

The room demonstrates several practical loop examples.

One example prints numbers from 1 to 10:

```python
i = 1

while i <= 10:
    print(i)
    i = i + 1
```

Another iterates through a list:

```python
websites = [
    "facebook.com",
    "google.com",
    "amazon.com"
]

for site in websites:
    print(site)
```

Although these examples are simple, they demonstrate the core idea behind automation:

**Execute the same operation repeatedly while changing the input each iteration.**

---

# Common Beginner Mistakes

## Forgetting to Update the Variable

Incorrect:

```python
i = 1

while i <= 5:
    print(i)
```

Correct:

```python
i = 1

while i <= 5:
    print(i)
    i += 1
```

---

## Incorrect Indentation

Incorrect:

```python
for i in range(5):
print(i)
```

Correct:

```python
for i in range(5):
    print(i)
```

---

## Misunderstanding `range()`

Many beginners expect:

```python
range(5)
```

to produce:

```text
1
2
3
4
5
```

Instead, Python returns:

```text
0
1
2
3
4
```

because counting begins at zero.

---

## Modifying the Wrong Variable

Incorrect:

```python
i = 1

while i <= 5:
    print(i)
    j = j + 1
```

Since `i` never changes, the loop never ends.

---

# Red Team Notes

Loops are used constantly in offensive security automation.

Common examples include:

Scanning multiple IP addresses:

```python
for ip in ip_list:
    scan(ip)
```

Testing multiple passwords:

```python
for password in wordlist:
    login(password)
```

Scanning ports:

```python
for port in range(1, 65536):
    scan(port)
```

Reading target lists:

```python
for host in hosts:
    enumerate(host)
```

Nearly every penetration testing tool relies on loops internally.

---

# Blue Team Notes

Blue Team automation also depends heavily on loops.

Examples include:

- Reading every line in a log file
- Checking every running process
- Parsing Windows Event Logs
- Monitoring multiple endpoints
- Processing threat intelligence feeds
- Generating security reports

Example:

```python
for log in logs:
    analyze(log)
```

Without loops, security automation would not be practical.

---

# Key Takeaways

- Loops automate repetitive tasks by repeatedly executing code.
- `while` loops continue running while a condition remains true.
- `for` loops iterate over collections or sequences.
- `range()` generates a sequence of numbers and begins counting from zero.
- Infinite loops usually occur when the loop condition never becomes false.
- Loops are fundamental to automation and are heavily used in both offensive and defensive cybersecurity.

# Functions

As programs grow larger, writing the same code repeatedly becomes inefficient and difficult to maintain.

Functions solve this problem by grouping related instructions into reusable blocks of code that can be executed whenever needed.

Instead of copying the same logic multiple times, we write it once inside a function and call it whenever required.

---

# What is a Function?

A **function** is a reusable block of code designed to perform a specific task.

Think of a function as a machine.

```text
          Input
            │
            ▼
     ┌───────────────┐
     │   Function    │
     │   (Process)   │
     └───────────────┘
            │
            ▼
          Output
```

You provide input, the function processes it, and optionally returns a result.

---

# Why Use Functions?

Imagine greeting three different people.

Without a function:

```python
print("Hello Ben! Nice to meet you.")
print("Hello Alice! Nice to meet you.")
print("Hello Charlie! Nice to meet you.")
```

Now imagine greeting one hundred people.

Repeating the same code becomes unnecessary.

With a function:

```python
def sayHello(name):
    print("Hello " + name + "! Nice to meet you.")
```

Calling it:

```python
sayHello("Ben")
sayHello("Alice")
sayHello("Charlie")
```

The same logic can now be reused indefinitely.

---

# Function Syntax

General syntax:

```python
def function_name(parameters):
    # code
```

Example:

```python
def sayHello(name):
    print("Hello " + name + "!")
```

---

## Function Components

### 1. `def`

The `def` keyword tells Python that a new function is being defined.

Example:

```python
def
```

Without `def`, Python would not recognize the function declaration.

---

### 2. Function Name

```python
sayHello
```

The function name should describe what the function does.

Good examples:

```python
calculate_total()

scan_port()

check_login()

encrypt_file()
```

Poor examples:

```python
abc()

test()

x()
```

Descriptive names improve readability and maintainability.

---

### 3. Parameters

Parameters define the input that the function expects.

Example:

```python
def sayHello(name):
```

Here, `name` is the parameter.

When the function is called, Python stores the provided value inside this parameter.

---

### 4. Colon (`:`)

The colon marks the end of the function header.

Example:

```python
def sayHello(name):
```

Everything indented beneath the colon belongs to the function.

---

### 5. Indentation

Python uses indentation to define the function body.

Correct:

```python
def sayHello(name):
    print("Hello")
```

Incorrect:

```python
def sayHello(name):
print("Hello")
```

Improper indentation results in an `IndentationError`.

---

# Calling a Function

Defining a function does not execute it.

Example:

```python
def sayHello(name):
    print("Hello")
```

This produces no output.

The function must be called:

```python
sayHello("Ben")
```

Output:

```text
Hello Ben!
```

---

# Function Execution Flow

When a function is called, Python performs the following steps:

```text
Program
    │
    ▼
Call Function
    │
    ▼
Pass Arguments
    │
    ▼
Execute Function Body
    │
    ▼
(Optional) Return Value
    │
    ▼
Continue Program
```

After the function finishes, execution returns to the next line in the main program.

---

# Parameters vs Arguments

These two terms are often confused.

Function definition:

```python
def sayHello(name):
```

`name` is the **parameter**.

Function call:

```python
sayHello("Ben")
```

`"Ben"` is the **argument**.

In simple terms:

| Parameter | Argument |
|-----------|----------|
| Variable inside the function definition | Actual value passed into the function |

---

# Return Values

Functions do not always print information.

Many functions calculate a result and return it to the caller.

Example:

```python
def square(number):
    return number ** 2
```

Calling the function:

```python
result = square(5)

print(result)
```

Output:

```text
25
```

The function sends the calculated value back to the program.

---

# `print()` vs `return`

This is one of the most important concepts for beginners.

Although both may appear similar, they serve completely different purposes.

---

## `print()`

Displays information on the screen.

Example:

```python
def hello():
    print("Hello")
```

Output:

```text
Hello
```

The value is displayed but **not returned**.

---

## `return`

Returns a value to the calling code.

Example:

```python
def hello():
    return "Hello"

text = hello()

print(text)
```

Output:

```text
Hello
```

Unlike `print()`, `return` allows the returned value to be stored, modified, or used in further calculations.

---

# Multiple Return Paths

A function may return different values depending on conditions.

Example:

```python
def calcCost(item):

    if item == "sweets":
        return 3.99

    elif item == "oranges":
        return 1.99

    else:
        return 0.99
```

When called:

```python
calcCost("sweets")
```

Python returns:

```text
3.99
```

If called with:

```python
calcCost("oranges")
```

Python returns:

```text
1.99
```

Each `return` immediately ends the function.

---

# Using Returned Values

Example:

```python
spent = 10

spent = spent + calcCost("sweets")

print(spent)
```

Execution flow:

```text
spent = 10
        │
        ▼
calcCost("sweets")
        │
        ▼
Returns 3.99
        │
        ▼
10 + 3.99
        │
        ▼
13.99
```

Output:

```text
13.99
```

---

# Bitcoin Challenge

The room concludes with a practical exercise.

The objective is to:

- Create a function that converts Bitcoin to USD.
- Return the calculated value.
- Alert the user if the investment falls below **$30,000**.

Example solution:

```python
investment_in_bitcoin = 1.2
bitcoin_to_usd = 40000

def bitcoinToUSD(bitcoin_amount, bitcoin_value_usd):
    usd_value = bitcoin_amount * bitcoin_value_usd
    return usd_value

usd_value = bitcoinToUSD(
    investment_in_bitcoin,
    bitcoin_to_usd
)

if usd_value < 30000:
    print("Warning! Your Bitcoin value is below $30,000.")
```

Given:

```text
1.2 × 40000 = 48000
```

The condition:

```text
48000 < 30000
```

evaluates to:

```text
False
```

Therefore, no warning is displayed.

---

# Common Beginner Mistakes

## Forgetting to Call the Function

Creating a function alone does not execute it.

Incorrect:

```python
def hello():
    print("Hello")
```

Correct:

```python
hello()
```

---

## Forgetting `return`

Incorrect:

```python
def square(x):
    x * x
```

Correct:

```python
def square(x):
    return x * x
```

Without `return`, the calculated value is lost.

---

## Typographical Errors

Example:

```python
bitcoin_amount
```

versus

```python
bicoin_amount
```

A small spelling mistake results in a `NameError`.

---

## Confusing `print()` and `return`

Many beginners expect this:

```python
result = hello()
```

to contain:

```text
Hello
```

when the function only uses:

```python
print("Hello")
```

Since nothing is returned, the variable receives:

```python
None
```

Understanding this difference is essential before writing larger programs.

---

# Function Best Practices

- Give functions meaningful names.
- Design each function to perform **one specific task**.
- Avoid repeating identical code.
- Use parameters instead of hardcoding values.
- Return values whenever the result needs to be reused.
- Keep functions short and focused.

Following these practices improves readability and simplifies debugging.

---

# Red Team Notes

Functions are widely used to organize offensive security tools.

Common examples include:

```python
def scan_port():
```

```python
def enumerate_users():
```

```python
def exploit_target():
```

```python
def send_payload():
```

Breaking a tool into multiple functions makes the code easier to test, maintain, and extend.

Most penetration testing frameworks follow this modular design.

---

# Blue Team Notes

Defensive automation also relies heavily on functions.

Examples include:

```python
def parse_logs():
```

```python
def detect_iocs():
```

```python
def calculate_hash():
```

```python
def generate_report():
```

Separating responsibilities into individual functions makes defensive scripts more organized and easier to troubleshoot.

---

# Key Takeaways

- Functions group reusable code into a single named block.
- Functions are defined using the `def` keyword.
- Parameters receive input values from function calls.
- Arguments are the actual values passed into a function.
- `return` sends a value back to the caller, while `print()` only displays output.
- Modular functions reduce duplicated code and improve maintainability.
- Functions form the foundation of larger automation scripts and cybersecurity tools.

# Files

One of Python's greatest strengths is its ability to interact with files.

In cybersecurity, scripts rarely operate on hardcoded data. Instead, they often read information from files or save their results for later analysis.

Common examples include:

- Reading password wordlists
- Loading target IP addresses
- Parsing web server logs
- Saving scan results
- Generating reports
- Reading Indicators of Compromise (IOCs)

These operations are collectively known as **File Input/Output (File I/O).**

---

# What is File I/O?

**File I/O** stands for **File Input/Output**.

It refers to the process of reading data from files (input) and writing data to files (output).

General workflow:

```text
          File
            │
            ▼
        Open File
            │
     ┌──────┴──────┐
     │             │
 Read Data     Write Data
     │             │
     ▼             ▼
   Program      Program
```

---

# Opening Files

Python provides the built-in `open()` function.

General syntax:

```python
open(filename, mode)
```

Example:

```python
f = open("file_name.txt", "r")
```

This opens the file named `file_name.txt` in **read mode**.

The returned object is stored inside the variable `f`.

---

# File Objects

When Python opens a file, it creates a **file object**.

```text
file.txt
    │
    ▼
File Object
    │
    ▼
Variable (f)
```

This object contains several useful methods such as:

- `read()`
- `readline()`
- `readlines()`
- `write()`
- `close()`

These methods allow programs to interact with the file.

---

# File Modes

The second argument passed to `open()` specifies how the file will be used.

| Mode | Purpose |
|------|---------|
| `"r"` | Read an existing file |
| `"w"` | Write to a file (creates or overwrites) |
| `"a"` | Append data to an existing file |

Understanding file modes is important because choosing the wrong mode can unintentionally erase existing data.

---

# Reading Files

Reading a file allows a program to retrieve its contents.

Example:

```python
f = open("passwords.txt", "r")

print(f.read())
```

Suppose the file contains:

```text
admin
guest
root
```

Output:

```text
admin
guest
root
```

The `read()` method reads the **entire file** at once.

---

# Reading Line by Line

Instead of reading the entire file, Python can read each line individually.

Example:

```python
lines = f.readlines()
```

If the file contains:

```text
admin
guest
root
```

Python returns:

```python
[
    "admin\n",
    "guest\n",
    "root\n"
]
```

This list can then be processed using a loop.

Example:

```python
for line in lines:
    print(line)
```

Reading files line by line is particularly useful for large files such as password lists or log files.

---

# Writing Files

Python can also create new files or modify existing ones.

---

## Append Mode (`a`)

Append mode adds new data to the **end** of an existing file.

Example:

```python
f = open("results.txt", "a")

f.write("New Scan Result")

f.close()
```

Existing content remains unchanged.

Example:

Before:

```text
Scan Started
```

After:

```text
Scan Started
New Scan Result
```

---

## Write Mode (`w`)

Write mode creates a new file if it does not exist.

If the file already exists, **its contents are erased before writing new data**.

Example:

```python
f = open("output.txt", "w")

f.write("Scan Complete")

f.close()
```

If the file previously contained:

```text
Old Data
```

It now contains:

```text
Scan Complete
```

The previous contents are permanently overwritten.

---

# The `write()` Method

The `write()` method stores text inside a file.

Example:

```python
f = open("report.txt", "w")

f.write("Report Generated Successfully")
```

The text is written exactly as provided.

---

# Closing Files

After finishing file operations, the file should always be closed.

Example:

```python
f.close()
```

Closing a file:

- Releases system resources
- Ensures all data has been written
- Prevents accidental modifications
- Allows other programs to access the file

Leaving files open unnecessarily is considered poor programming practice.

---

# File Operation Workflow

Reading a file:

```text
Open File
     │
     ▼
Read Data
     │
     ▼
Process Data
     │
     ▼
Close File
```

Writing a file:

```text
Open File
     │
     ▼
Write Data
     │
     ▼
Save Changes
     │
     ▼
Close File
```

---

# Best Practice: Using `with open()`

The room demonstrates the traditional approach:

```python
f = open("data.txt", "r")

print(f.read())

f.close()
```

Although this works correctly, modern Python code typically uses a **context manager**.

Example:

```python
with open("data.txt", "r") as f:
    print(f.read())
```

Advantages:

- Automatically closes the file
- Prevents resource leaks
- Cleaner syntax
- Safer when exceptions occur

This is the preferred approach in production code.

---

# Practical Examples

## Reading a Wordlist

```python
with open("passwords.txt", "r") as f:
    passwords = f.readlines()
```

---

## Reading Target Hosts

```python
with open("targets.txt", "r") as f:
    for target in f:
        print(target)
```

---

## Saving Scan Results

```python
with open("scan_results.txt", "w") as f:
    f.write("Scan completed successfully.")
```

---

## Appending Log Entries

```python
with open("activity.log", "a") as f:
    f.write("New login detected\n")
```

Appending is commonly used for logging because existing data is preserved.

---

# Common Beginner Mistakes

## Forgetting to Close the File

Incorrect:

```python
f = open("file.txt", "r")

print(f.read())
```

Correct:

```python
f = open("file.txt", "r")

print(f.read())

f.close()
```

Or preferably:

```python
with open("file.txt", "r") as f:
    print(f.read())
```

---

## Using the Wrong File Mode

Using:

```python
open("important.txt", "w")
```

will overwrite the entire file.

If the goal is to keep existing data, use:

```python
open("important.txt", "a")
```

instead.

---

## Forgetting That `read()` Consumes the File

After calling:

```python
f.read()
```

the file pointer reaches the end of the file.

Calling:

```python
f.read()
```

again immediately afterward returns:

```text
""
```

because there is nothing left to read.

---

## Opening a Nonexistent File in Read Mode

Example:

```python
open("missing.txt", "r")
```

Produces:

```text
FileNotFoundError
```

The file must exist before it can be opened in read mode.

---

# Red Team Notes

File operations are extremely common during penetration testing.

Typical use cases include:

Reading:

- Password wordlists
- Username lists
- Target IP lists
- Payload templates
- Configuration files

Writing:

- Scan results
- Loot files
- Extracted credentials
- Enumeration reports
- Discovered vulnerabilities

Example:

```python
with open("targets.txt") as f:
    for host in f:
        scan(host.strip())
```

Many penetration testing tools rely heavily on file handling.

---

# Blue Team Notes

Blue Team workflows also depend on file operations.

Common examples include:

Reading:

- Windows Event Logs
- Apache access logs
- Firewall logs
- Threat intelligence feeds
- IOC databases

Writing:

- Incident reports
- Alert logs
- Detection results
- Forensic artifacts

Example:

```python
with open("access.log") as log:
    for entry in log:
        analyze(entry)
```

Security automation would not be practical without efficient file handling.

---

# Key Takeaways

- File I/O enables programs to read from and write to files.
- The `open()` function returns a file object used for file operations.
- `"r"` opens files for reading.
- `"w"` creates or overwrites files.
- `"a"` appends new data while preserving existing content.
- `read()` retrieves the entire file, while `readlines()` returns each line as a list.
- Files should always be closed after use, or preferably managed using `with open()`.
- File handling is a fundamental skill for automation, penetration testing, incident response, and digital forensics.

# Imports

As programs become more complex, developers rarely write every piece of code themselves.

Instead, they reuse existing code that has already been written, tested, and maintained by others.

Python allows this through **modules** and **libraries**, which can be imported into a program using the `import` keyword.

Using libraries significantly reduces development time and allows developers to focus on solving problems instead of reinventing common functionality.

---

# What is an Import?

An **import** tells Python to load a module or library so its functions, classes, and variables become available for use.

General syntax:

```python
import library_name
```

Example:

```python
import datetime
```

After importing the library, its functions can be accessed using dot notation.

---

# What is a Library?

A **library** is a collection of pre-written code that provides functionality for solving common problems.

Rather than writing everything from scratch, developers can reuse these existing components.

Think of a library as a toolbox.

```text
Without Libraries

Need a screwdriver?

↓

Build one yourself.

Need a hammer?

↓

Build one yourself.

Need a wrench?

↓

Build one yourself.
```

With libraries:

```text
Need a screwdriver?

↓

Open the toolbox.

↓

Use it immediately.
```

Libraries save time, reduce errors, and improve productivity.

---

# Using the `datetime` Library

The room demonstrates Python's built-in `datetime` library.

Example:

```python
import datetime

current_time = datetime.datetime.now()

print(current_time)
```

Possible output:

```text
2026-08-18 18:35:41.734826
```

The current date and time are retrieved directly from the operating system.

---

# Understanding `datetime.datetime.now()`

At first glance:

```python
datetime.datetime.now()
```

looks repetitive.

Breaking it down:

```text
datetime
     │
     ▼
Library

datetime
     │
     ▼
Class

now()
     │
     ▼
Method
```

This statement means:

- Import the **datetime** library.
- Access the **datetime** class.
- Call its **now()** method.
- Return the current date and time.

---

# Dot Notation

Python uses **dot notation (`.`)** to access objects contained within other objects.

General format:

```python
library.function()
```

or

```python
library.class.method()
```

Example:

```python
datetime.datetime.now()
```

The dot operator tells Python where to find the next object.

---

# Built-in Libraries

Python ships with an extensive **Standard Library**.

These libraries are installed automatically when Python is installed.

Examples include:

| Library | Purpose |
|----------|---------|
| `datetime` | Date and time |
| `math` | Mathematical functions |
| `os` | Operating system interaction |
| `random` | Random number generation |
| `json` | JSON parsing |
| `socket` | Network communication |
| `hashlib` | Hash generation |
| `csv` | CSV file processing |

These libraries require no additional installation.

Example:

```python
import math

print(math.sqrt(25))
```

Output:

```text
5.0
```

---

# Third-Party Libraries

Python also supports thousands of community-developed libraries.

Unlike the Standard Library, these must be installed separately.

Popular examples include:

| Library | Purpose |
|----------|---------|
| Requests | HTTP requests |
| Scapy | Packet crafting and sniffing |
| Pwntools | Binary exploitation and CTF |
| BeautifulSoup | HTML parsing |
| Flask | Web application development |

These libraries greatly extend Python's capabilities.

---

# Python Package Manager (pip)

Third-party libraries are installed using **pip**, Python's package manager.

General syntax:

```bash
pip install package_name
```

Example:

```bash
pip install scapy
```

Once installed, the library can be imported.

```python
import scapy
```

or

```python
from scapy.all import *
```

`pip` automatically downloads the package and its required dependencies.

---

# Popular Cybersecurity Libraries

## Requests

Requests is one of the most popular HTTP libraries.

Example:

```python
import requests

response = requests.get("https://example.com")

print(response.status_code)
```

Common use cases:

- Web automation
- API interaction
- Login automation
- Web enumeration
- HTTP testing

---

## Scapy

Scapy is a powerful packet manipulation library.

Example:

```python
from scapy.all import *

packet = IP(dst="8.8.8.8") / ICMP()

send(packet)
```

Common use cases:

- Packet crafting
- Packet sniffing
- ARP spoofing
- Network analysis
- Protocol testing

---

## Pwntools

Pwntools is widely used in exploit development and Capture The Flag (CTF) competitions.

Example:

```python
from pwn import *

p = remote("10.10.10.5", 1337)
```

Common use cases:

- Buffer overflow exploitation
- Remote exploitation
- Binary analysis
- Return-Oriented Programming (ROP)
- CTF automation

---

# Why Libraries Matter

Imagine writing an HTTP client yourself.

Without a library, you would need to:

- Open sockets
- Build HTTP packets
- Parse responses
- Handle redirects
- Process headers
- Handle SSL/TLS
- Manage errors

With the Requests library:

```python
requests.get(url)
```

One line replaces hundreds of lines of code.

Libraries allow developers to focus on solving security problems instead of implementing low-level functionality.

---

# Common Beginner Mistakes

## Forgetting to Import a Library

Incorrect:

```python
print(datetime.datetime.now())
```

Produces:

```text
NameError
```

Correct:

```python
import datetime

print(datetime.datetime.now())
```

---

## Misspelling Library Names

Incorrect:

```python
import datatime
```

Correct:

```python
import datetime
```

Python cannot import modules that do not exist.

---

## Forgetting to Install Third-Party Libraries

Example:

```python
import scapy
```

If Scapy has not been installed:

```text
ModuleNotFoundError
```

Solution:

```bash
pip install scapy
```

---

## Confusing Modules and Libraries

Although often used interchangeably, there is a slight distinction:

- A **module** is a single Python file containing code.
- A **library** is a collection of one or more related modules.

For beginners, treating them as reusable packages is generally sufficient.

---

# Red Team Notes

Python libraries play a central role in offensive security.

Examples include:

- Requests for web enumeration
- Scapy for packet crafting
- Pwntools for exploit development
- Impacket for Active Directory attacks
- Paramiko for SSH automation

Many well-known penetration testing tools are built using these libraries.

Rather than developing every capability from scratch, security professionals combine existing libraries to rapidly build custom tooling.

---

# Blue Team Notes

Blue Team automation also relies heavily on Python libraries.

Examples include:

- `hashlib` for file integrity verification
- `json` for parsing structured logs
- `csv` for report generation
- `os` for filesystem interaction
- `datetime` for timestamps
- `requests` for consuming threat intelligence APIs

Modern Security Operations Centers (SOCs) frequently use Python libraries to automate detection, reporting, and incident response.

---

# Skills Gained

After completing this room, I gained practical experience with:

- Understanding Python syntax
- Creating and updating variables
- Working with common data types
- Performing mathematical operations
- Using comparison and Boolean operators
- Writing conditional statements
- Building `while` and `for` loops
- Creating reusable functions
- Returning values from functions
- Reading and writing files
- Importing built-in and third-party libraries
- Solving beginner programming challenges

These skills form the foundation for more advanced scripting and cybersecurity automation.

---

# Key Takeaways

- Python is one of the most valuable programming languages for cybersecurity.
- Clean syntax makes Python easy to learn while remaining powerful enough for professional automation.
- Variables store data that programs can manipulate.
- Data types determine how values behave.
- Conditional statements enable decision-making.
- Loops automate repetitive tasks.
- Functions improve code organization and reusability.
- File handling allows scripts to process external data.
- Libraries provide pre-built functionality, reducing development time.
- Mastering these fundamentals prepares learners for penetration testing scripts, security automation, exploit development, and defensive tooling.

---

# Future Learning Path

After completing this room, recommended next topics include:

1. Python Data Structures (Lists, Dictionaries, Tuples, Sets)
2. Exception Handling (`try` / `except`)
3. Object-Oriented Programming (OOP)
4. Regular Expressions (Regex)
5. Networking with Python (`socket`)
6. HTTP Requests using Requests
7. Multithreading and Concurrency
8. Scapy for Packet Manipulation
9. Pwntools for Binary Exploitation
10. TryHackMe — **Python for Pentesters**

These topics build directly upon the concepts introduced in this room and are essential for developing practical cybersecurity automation skills.

---

# References

- TryHackMe — *Python Basics*
- Python Documentation — https://docs.python.org/3/
- Python Standard Library Documentation — https://docs.python.org/3/library/
- Requests Documentation — https://requests.readthedocs.io/
- Scapy Documentation — https://scapy.readthedocs.io/
- Pwntools Documentation — https://docs.pwntools.com/

---

# Tags

`Python`
`Programming`
`Python Basics`
`Automation`
`Scripting`
`Cybersecurity`
`TryHackMe`
`Beginner`
`Pentesting`
`Blue Team`
`Red Team`
`Python Fundamentals`
`Learning Notes`
`CyberJourney`