# JavaScript Essentials

## Executive Summary

JavaScript (JS) is one of the core technologies of the modern web alongside HTML and CSS. While HTML defines the structure of a webpage and CSS controls its appearance, JavaScript provides interactivity and dynamic behavior.

Today, nearly every modern website relies on JavaScript. Social media platforms, online banking systems, e-commerce stores, learning platforms, and web applications all use JavaScript extensively.

From a cybersecurity perspective, understanding JavaScript is critical because it executes directly inside the user's browser. Many web vulnerabilities involve JavaScript, including:

- Cross-Site Scripting (XSS)
- Client-Side Validation Bypass
- DOM-Based Attacks
- Source Code Disclosure
- API Endpoint Discovery
- Token and Secret Exposure

This room introduces the fundamental concepts of JavaScript and explains how attackers and defenders interact with JavaScript in real-world environments.

---

# Learning Objectives

By completing this room, you will learn:

- What JavaScript is and why it exists
- How JavaScript works inside a browser
- Variables and data types
- Functions and loops
- Request-response communication
- Internal and external JavaScript
- Browser dialog functions
- Control flow statements
- Authentication bypass concepts
- JavaScript minification and obfuscation
- Secure JavaScript development practices

---

# Prerequisites

Before starting this room, it is recommended to understand:

- Basic HTML
- Basic Web Application Concepts
- Linux Fundamentals
- How Websites Work
- HTTP Request and Response Cycle

---

# Why JavaScript Exists

## The Problem

Early websites were static.

A static webpage could only display information.

Example:

```html
<h1>Welcome</h1>
<p>This is a website.</p>
```

The page displayed content but could not:

- Validate forms
- React to button clicks
- Update content dynamically
- Display notifications
- Load data without refreshing

Everything required a full page reload.

---

## The Solution

JavaScript was created to add interactivity to webpages.

With JavaScript, a webpage can:

- Respond to user actions
- Validate forms
- Animate content
- Communicate with servers
- Update page elements dynamically

Example:

```javascript
alert("Welcome!");
```

Instead of simply displaying information, the page can now interact with the user.

---

# The Three Core Web Technologies

Modern websites are built using three technologies:

## HTML

Provides structure.

Example:

```html
<button>Login</button>
```

HTML creates the button.

---

## CSS

Provides styling.

Example:

```css
button {
    background-color: blue;
}
```

CSS changes the button appearance.

---

## JavaScript

Provides behavior.

Example:

```javascript
button.onclick = function() {
    alert("Login clicked");
}
```

JavaScript makes the button perform an action.

---

# Analogy: Building a House

Imagine a house.

HTML is the structure:

```text
Walls
Doors
Windows
Roof
```

CSS is the appearance:

```text
Paint
Furniture
Decoration
```

JavaScript is the functionality:

```text
Lights
Electrical System
Security System
Elevator
```

Without JavaScript, websites would be little more than digital documents.

---

# What is JavaScript?

JavaScript is a high-level interpreted programming language primarily used for creating dynamic and interactive web applications.

JavaScript executes directly inside web browsers.

Popular browsers include:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox
- Safari

Each browser contains a JavaScript engine responsible for executing JavaScript code.

Examples:

| Browser | JavaScript Engine |
|----------|----------|
| Chrome | V8 |
| Edge | V8 |
| Firefox | SpiderMonkey |
| Safari | JavaScriptCore |

---

# How JavaScript Works

When a user visits a webpage, the browser receives:

```text
HTML
CSS
JavaScript
```

Example flow:

```text
User
  │
  ▼
Browser
  │
  ▼
Server
  │
  ▼
HTML
CSS
JS
  │
  ▼
Browser Executes Code
```

The browser parses:

1. HTML
2. CSS
3. JavaScript

and renders the final page.

---

# JavaScript Execution Flow

Consider:

```javascript
console.log("Hello");
console.log("World");
```

Execution occurs line by line.

Step 1:

```javascript
console.log("Hello");
```

Output:

```text
Hello
```

Step 2:

```javascript
console.log("World");
```

Output:

```text
World
```

Final output:

```text
Hello
World
```

JavaScript generally executes from top to bottom unless control flow statements alter execution.

---

# Interpreted vs Compiled Languages

## Compiled Languages

Examples:

- C
- C++
- Go

Flow:

```text
Source Code
      │
      ▼
Compiler
      │
      ▼
Binary File
      │
      ▼
Execution
```

---

## Interpreted Languages

Examples:

- JavaScript
- Python
- PHP

Flow:

```text
Source Code
      │
      ▼
Interpreter
      │
      ▼
Execution
```

JavaScript code executes directly within the browser.

---

# Why JavaScript Matters in Cybersecurity

A critical fact:

> JavaScript executes on the client side.

This means users receive the JavaScript code.

If users receive the code:

- Attackers receive the code
- Pentesters receive the code
- Researchers receive the code

Any logic implemented in JavaScript can be analyzed.

---

# Client-Side vs Server-Side

## Client Side

Runs on the user's machine.

Examples:

- HTML
- CSS
- JavaScript

---

## Server Side

Runs on the server.

Examples:

- PHP
- Python
- Java
- Node.js
- C#

---

Client-side code should never be trusted for security decisions.

---

# Variables

## What is a Variable?

A variable is a container used to store data.

Example:

```javascript
let username = "Devdan";
```

Here:

```text
Variable Name:
username

Stored Value:
Devdan
```

---

## Why Variables Exist

Without variables:

```javascript
alert("Devdan");
alert("Devdan");
alert("Devdan");
```

With variables:

```javascript
let username = "Devdan";

alert(username);
alert(username);
alert(username);
```

The value only needs to be changed once.

---

# Variable Declaration Methods

JavaScript supports:

```javascript
var
let
const
```

---

## var

Traditional method.

Example:

```javascript
var age = 25;
```

Historically common but less recommended today.

---

## let

Allows reassignment.

Example:

```javascript
let age = 25;

age = 26;
```

Valid.

---

## const

Constant value.

Example:

```javascript
const country = "Indonesia";
```

Attempting:

```javascript
country = "Japan";
```

causes an error.

---

# Variable Scope

Scope determines where a variable can be accessed.

Two common scopes:

## Global Scope

Accessible everywhere.

Example:

```javascript
let username = "Devdan";
```

---

## Block Scope

Accessible only inside a code block.

Example:

```javascript
if (true) {
    let age = 25;
}
```

The variable age cannot be accessed outside the block.

---

# Data Types

Variables store different types of data.

---

## String

Stores text.

Example:

```javascript
let name = "Devdan";
```

---

## Number

Stores numeric values.

Example:

```javascript
let age = 23;
```

---

## Boolean

Stores true or false.

Example:

```javascript
let isAdmin = true;
```

---

## Null

Represents an intentional absence of value.

Example:

```javascript
let token = null;
```

---

## Undefined

Represents a variable without a value.

Example:

```javascript
let username;
```

Output:

```text
undefined
```

---

## Object

Stores structured data.

Example:

```javascript
let user = {
    name: "Devdan",
    age: 23
};
```

---

# Functions

## What is a Function?

A function is a reusable block of code designed to perform a specific task.

Example:

```javascript
function greet() {
    alert("Hello");
}
```

---

# Why Functions Exist

Without functions:

```javascript
alert("Hello");
alert("Hello");
alert("Hello");
```

With functions:

```javascript
function greet() {
    alert("Hello");
}

greet();
greet();
greet();
```

Code becomes reusable.

---

# Function Parameters

Functions can accept data.

Example:

```javascript
function greet(name) {
    alert("Hello " + name);
}
```

Calling:

```javascript
greet("Devdan");
```

Output:

```text
Hello Devdan
```

---

# Function Execution Process

When:

```javascript
greet("Devdan");
```

is executed:

Step 1:

Browser enters the function.

Step 2:

Parameter receives value.

```javascript
name = "Devdan"
```

Step 3:

Function code executes.

Step 4:

Control returns to the caller.

---

# Loops

## What is a Loop?

Loops repeat code multiple times.

Without loops:

```javascript
alert("Student");
alert("Student");
alert("Student");
```

With loops:

```javascript
for(let i = 0; i < 3; i++) {
    alert("Student");
}
```

Same result.

Less code.

---

# The for Loop

Example:

```javascript
for(let i = 0; i < 5; i++) {
    console.log(i);
}
```

Output:

```text
0
1
2
3
4
```

---

# Loop Components

```javascript
for(let i = 0; i < 5; i++)
```

Contains:

Initialization:

```javascript
let i = 0;
```

Condition:

```javascript
i < 5
```

Increment:

```javascript
i++
```

---

# Why Loops Matter

Loops are heavily used for:

- Processing records
- Iterating through users
- Enumerating items
- Reading arrays
- Automating repetitive tasks

---

# Arrays

Arrays store multiple values.

Example:

```javascript
const users = [
    "Alice",
    "Bob",
    "Charlie"
];
```

---

# Accessing Array Elements

Example:

```javascript
users[0]
```

Output:

```text
Alice
```

---

# Array Indexing

```text
Index 0 → Alice
Index 1 → Bob
Index 2 → Charlie
```

---

# Request-Response Cycle

Every web application relies on requests and responses.

---

# Request

A browser requests data.

Example:

```http
GET /login HTTP/1.1
```

---

# Response

Server returns data.

Example:

```http
HTTP/1.1 200 OK
```

---

# Full Communication Flow

```text
Browser
    │
    │ Request
    ▼
Server
    │
    │ Response
    ▼
Browser
```

---

# Example

User visits:

```text
https://example.com
```

Browser sends:

```http
GET /
```

Server responds:

```html
<html>
...
</html>
```

The browser renders the page.

---

# Why the Request-Response Cycle Matters

Modern JavaScript frequently sends additional requests.

Example:

```javascript
fetch("/api/profile");
```

Flow:

```text
Browser
    │
    ▼
JavaScript
    │
    ▼
Server API
    │
    ▼
Response Data
```

Understanding this cycle is essential for:

- Web Development
- API Testing
- Burp Suite
- Web Pentesting
- Bug Bounty Hunting

---

# Cybersecurity Relevance

Everything learned in this section directly connects to offensive security.

## Variables

Useful for identifying:

- API Keys
- Tokens
- Secrets
- Sensitive Data

---

## Functions

Useful for understanding:

- Authentication Logic
- Business Logic
- Validation Logic

---

## Loops

Useful for understanding:

- Enumeration
- Automation
- Brute Force Logic

---

## Request-Response Cycle

Forms the foundation of:

- Web Application Testing
- XSS
- CSRF
- API Attacks
- Authentication Testing

---

# Key Takeaways

- JavaScript provides interactivity to websites.
- JavaScript executes inside the browser.
- Client-side code should never be trusted.
- Variables store data.
- Data types define the kind of data stored.
- Functions group reusable logic.
- Loops automate repetitive actions.
- Arrays store collections of values.
- Websites operate using requests and responses.
- Understanding JavaScript is essential for modern web security.

---

# Skills Gained

After completing this section, you should understand:

- What JavaScript is
- How browsers execute JavaScript
- Variable declaration
- Data types
- Functions
- Parameters
- Loops
- Arrays
- Request-response communication
- Basic JavaScript analysis from a cybersecurity perspective


# Part 2 - JavaScript Overview and Chrome Developer Tools

---

# Introduction

In the previous section, we learned the fundamental building blocks of JavaScript:

- Variables
- Data Types
- Functions
- Loops
- Request-Response Cycle

Understanding these concepts is important, but learning syntax alone is not enough.

As cybersecurity practitioners, we must also understand:

- How JavaScript is executed
- How browsers process JavaScript
- How users interact with JavaScript
- How attackers analyze JavaScript
- How developers debug JavaScript

This section introduces Chrome Developer Tools and demonstrates how JavaScript executes inside a browser.

These skills form the foundation for later topics such as:

- Cross-Site Scripting (XSS)
- DOM Manipulation
- Source Code Analysis
- Client-Side Validation Bypass
- Web Application Testing

---

# What Happens When a Browser Loads JavaScript?

Most beginners imagine websites as static pages.

In reality, modern websites behave more like applications.

Consider a webpage containing:

```html
<h1>Welcome</h1>

<script>
console.log("Hello World");
</script>
```

When the browser loads the page:

```text
Receive HTML
      ↓
Parse HTML
      ↓
Discover JavaScript
      ↓
Execute JavaScript
      ↓
Render Page
```

JavaScript executes directly inside the browser.

This execution environment is called:

```text
JavaScript Runtime Environment
```

---

# Why This Matters for Cybersecurity

A critical security principle is:

> If the browser receives the code, the attacker receives the code.

This means attackers can:

- View JavaScript
- Analyze JavaScript
- Modify JavaScript
- Execute JavaScript manually

This principle explains why:

- Secrets should never be stored in JavaScript
- Authentication should never rely on JavaScript alone
- Validation should always occur on the server

---

# JavaScript Overview

The room introduces the following JavaScript example:

```javascript
// Hello, World! program
console.log("Hello, World!");

// Variable and Data Type
let age = 25;

// Control Flow Statement
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}

// Function
function greet(name) {
    console.log("Hello, " + name + "!");
}

// Calling the function
greet("Bob");
```

Although simple, this example contains several fundamental concepts.

---

# Understanding Program Execution

Computers execute code line by line.

Consider:

```javascript
console.log("Hello, World!");
```

The browser executes:

```text
Display:
Hello, World!
```

Output:

```text
Hello, World!
```

---

# Variables in Action

Next:

```javascript
let age = 25;
```

The browser creates:

```text
Variable:
age

Value:
25
```

Internally:

```text
Memory
 └── age = 25
```

---

# Conditional Statements

The next section contains:

```javascript
if (age >= 18)
```

The browser evaluates:

```javascript
25 >= 18
```

Result:

```javascript
true
```

Since the condition is true:

```javascript
console.log("You are an adult.");
```

executes.

Output:

```text
You are an adult.
```

---

# The Else Statement

An else statement executes when the condition evaluates to false.

Example:

```javascript
let age = 15;

if(age >= 18)
{
    console.log("Adult");
}
else
{
    console.log("Minor");
}
```

Evaluation:

```javascript
15 >= 18
```

Result:

```javascript
false
```

Output:

```text
Minor
```

---

# Why Conditional Logic Exists

Programs constantly make decisions.

Examples:

```text
Is user logged in?
Is password correct?
Is user an admin?
Is age greater than 18?
```

Without conditional statements, applications could not respond differently to different situations.

---

# Function Definition

The example then defines:

```javascript
function greet(name)
{
    console.log("Hello, " + name + "!");
}
```

This creates a reusable block of code.

At this stage:

```text
Function Exists
```

but:

```text
Function Has Not Executed Yet
```

This distinction is important.

---

# Function Invocation

The function executes only when called.

Example:

```javascript
greet("Bob");
```

Execution flow:

```text
Call Function
      ↓
Parameter Receives Value
      ↓
name = "Bob"
      ↓
Execute Code
      ↓
Return Control
```

Output:

```text
Hello, Bob!
```

---

# Final Program Output

Combining everything:

```javascript
console.log("Hello, World!");

let age = 25;

if(age >= 18)
{
    console.log("You are an adult.");
}

function greet(name)
{
    console.log("Hello, " + name + "!");
}

greet("Bob");
```

Output:

```text
Hello, World!
You are an adult.
Hello, Bob!
```

---

# Chrome Developer Tools

One of the most important tools for web application testing is:

```text
Chrome Developer Tools
```

Often called:

```text
DevTools
```

---

# Opening DevTools

Several methods exist.

Method 1:

```text
F12
```

Method 2:

```text
Ctrl + Shift + I
```

Method 3:

```text
Right Click
     ↓
Inspect
```

---

# Why Pentesters Love DevTools

Chrome DevTools provides direct access to:

- HTML
- CSS
- JavaScript
- Requests
- Responses
- Cookies
- Local Storage
- Session Storage

It is one of the primary tools used during web assessments.

---

# The Console Tab

The room focuses on:

```text
Console
```

The Console allows direct interaction with JavaScript.

Think of it as:

```text
Terminal
for
the Browser
```

---

# Running JavaScript Manually

Example:

```javascript
alert("Hello THM");
```

After pressing Enter:

```text
Popup Appears
```

The browser immediately executes the code.

---

# Testing Variables

Example:

```javascript
let age = 23;
```

Then:

```javascript
age
```

Output:

```text
23
```

The console allows you to inspect variables directly.

---

# Performing Calculations

Example:

```javascript
5 + 10
```

Output:

```text
15
```

The browser acts as a JavaScript interpreter.

---

# First Practical Program

The room introduces:

```javascript
let x = 5;
let y = 10;
let result = x + y;

console.log("The result is: " + result);
```

---

# Step-by-Step Analysis

Step 1:

```javascript
let x = 5;
```

Memory:

```text
x = 5
```

---

Step 2:

```javascript
let y = 10;
```

Memory:

```text
y = 10
```

---

Step 3:

```javascript
let result = x + y;
```

Calculation:

```javascript
5 + 10
```

Result:

```javascript
15
```

Memory:

```text
result = 15
```

---

Step 4:

```javascript
console.log("The result is: " + result);
```

Output:

```text
The result is: 15
```

---

# Why This Matters

Although simple, this example demonstrates:

- Variable creation
- Variable assignment
- Expressions
- String concatenation
- Output generation

These concepts appear constantly in modern web applications.

---

# Understanding Expressions

An expression produces a value.

Example:

```javascript
5 + 10
```

Produces:

```javascript
15
```

Another example:

```javascript
age >= 18
```

Produces:

```javascript
true
```

Understanding expressions is essential when analyzing authentication logic and validation routines.

---

# Common Pentester Use Cases for Console

The console is often used to:

## Inspect Variables

```javascript
user
```

---

## Modify Variables

```javascript
isAdmin = true
```

---

## Call Functions

```javascript
login()
```

---

## Execute JavaScript

```javascript
alert(1)
```

---

## Test XSS Payloads

```javascript
alert(document.domain)
```

---

## Inspect Browser Storage

```javascript
localStorage
```

---

## Read Cookies

```javascript
document.cookie
```

---

# Real-World Example

Suppose a webpage contains:

```javascript
let isAdmin = false;
```

A user can open DevTools and type:

```javascript
isAdmin
```

Output:

```text
false
```

Then:

```javascript
isAdmin = true;
```

The value changes.

This demonstrates why client-side variables cannot be trusted.

---

# Client-Side Trust Problem

Many beginner developers assume:

```text
Users cannot see JavaScript.
```

This assumption is incorrect.

Users can:

- Read JavaScript
- Modify JavaScript
- Execute JavaScript

Therefore:

```text
Client-Side Security
=
Weak Security
```

Server-side validation remains essential.

---

# Cybersecurity Relevance

Chrome DevTools is heavily used during:

## Web Application Testing

Inspect:

- Requests
- Responses
- JavaScript

---

## Source Code Review

Analyze:

- Functions
- Variables
- Endpoints

---

## XSS Testing

Execute payloads directly.

Example:

```javascript
alert(1)
```

---

## Authentication Analysis

Inspect login logic.

---

## Session Analysis

View:

- Cookies
- Local Storage
- Session Storage

---

# Red Team Perspective

When assessing a web application:

1. Open DevTools
2. Open Console
3. Inspect JavaScript
4. Enumerate Variables
5. Search for Secrets
6. Review Authentication Logic

The Console is often the first place attackers interact with a web application.

---

# Blue Team Perspective

Developers should assume:

```text
Attackers Have Access
to All Client-Side Code
```

Never trust:

- Browser variables
- Client-side validation
- Hidden HTML fields
- Client-side authentication checks

Always validate sensitive actions on the server.

---

# Key Takeaways

- JavaScript executes inside the browser.
- Programs execute line by line.
- Conditional statements control program flow.
- Functions execute only when called.
- Chrome DevTools is a critical security tool.
- The Console allows direct JavaScript execution.
- Users can inspect and modify client-side code.
- Client-side code should never be trusted for security decisions.

---

# Skills Gained

After completing this section, you should understand:

- JavaScript execution flow
- Conditional statements
- Function invocation
- Console usage
- Chrome Developer Tools
- Variable inspection
- Client-side code analysis
- Basic JavaScript debugging
- Security implications of browser-executed code


# Part 3 - Integrating JavaScript in HTML

---

# Introduction

In the previous section, we learned:

- JavaScript fundamentals
- Program execution flow
- Chrome Developer Tools
- Console usage
- Client-side code analysis

However, JavaScript does not usually exist by itself.

In real-world web applications, JavaScript works together with:

- HTML
- CSS
- Browser APIs

This section focuses on how JavaScript integrates with HTML and how attackers analyze that integration during web application assessments.

Understanding this relationship is critical because almost every modern web vulnerability involves interactions between:

```text
HTML
↓
JavaScript
↓
Browser
↓
User
```

---

# The Relationship Between HTML and JavaScript

HTML creates elements.

Example:

```html
<button>Login</button>
```

The browser renders:

```text
[ Login ]
```

However, clicking the button does nothing.

---

# Adding Behavior

JavaScript adds functionality.

Example:

```javascript
alert("Button Clicked");
```

Now the webpage can react to user actions.

---

# The Modern Web Stack

Most modern websites rely on three technologies:

| Technology | Purpose |
|------------|----------|
| HTML | Structure |
| CSS | Appearance |
| JavaScript | Behavior |

---

# Example

HTML:

```html
<button id="loginBtn">
Login
</button>
```

CSS:

```css
button {
    color: white;
}
```

JavaScript:

```javascript
document.getElementById("loginBtn")
.onclick = function()
{
    alert("Login Clicked");
}
```

Result:

```text
HTML creates button
CSS styles button
JavaScript adds behavior
```

---

# How Browsers Process Webpages

When a browser loads a webpage:

```text
Receive HTML
       ↓
Parse HTML
       ↓
Load CSS
       ↓
Load JavaScript
       ↓
Execute JavaScript
       ↓
Render Page
```

JavaScript can modify the page even after it has loaded.

This capability is one of the reasons modern web applications feel interactive.

---

# Internal JavaScript

Internal JavaScript refers to JavaScript code written directly inside an HTML file.

Example:

```html
<script>
alert("Hello");
</script>
```

The JavaScript exists inside the HTML document.

---

# Example Structure

```html
<!DOCTYPE html>
<html>

<head>
    <title>Example</title>
</head>

<body>

<h1>Hello World</h1>

<script>

alert("Welcome");

</script>

</body>

</html>
```

---

# How Internal JavaScript Executes

Consider:

```html
<body>

<h1>Hello</h1>

<script>

alert("Welcome");

</script>

</body>
```

Execution flow:

```text
Browser Reads HTML
        ↓
Create H1 Element
        ↓
Encounter Script Tag
        ↓
Execute JavaScript
        ↓
Continue Rendering
```

---

# Why Internal JavaScript Exists

Advantages:

- Easy for beginners
- Quick testing
- Small projects

Example:

```html
<script>

let age = 23;

alert(age);

</script>
```

Everything remains inside one file.

---

# Disadvantages of Internal JavaScript

As projects grow:

```text
HTML
+
CSS
+
JavaScript
```

inside one file becomes difficult to manage.

Example:

```text
5 lines HTML
500 lines JavaScript
```

Maintenance becomes difficult.

---

# Practical Example

The room provides:

```html
<h1>Addition of Two Numbers</h1>

<p id="result"></p>

<script>

let x = 5;
let y = 10;

let result = x + y;

document.getElementById("result")
.innerHTML =
"The result is: " + result;

</script>
```

---

# Understanding the HTML

The page contains:

```html
<p id="result"></p>
```

Initially:

```text
Paragraph Exists
```

but:

```text
Paragraph is Empty
```

---

# JavaScript Execution

Variables:

```javascript
let x = 5;
let y = 10;
```

Calculation:

```javascript
result = 15;
```

---

# DOM Interaction

Then:

```javascript
document.getElementById("result")
```

searches for:

```html
id="result"
```

inside the page.

The browser finds:

```html
<p id="result"></p>
```

---

# innerHTML

Then:

```javascript
.innerHTML
```

changes the content.

Before:

```html
<p id="result"></p>
```

After:

```html
<p id="result">

The result is: 15

</p>
```

---

# Final Output

The webpage displays:

```text
Addition of Two Numbers

The result is: 15
```

---

# External JavaScript

In professional development, JavaScript is usually stored separately.

Example:

```text
index.html
script.js
```

---

# Why Separate Files?

Advantages:

- Cleaner code
- Easier maintenance
- Reusability
- Better scalability

---

# Example

File:

```text
script.js
```

Contains:

```javascript
let x = 5;
let y = 10;

let result = x + y;

document.getElementById("result")
.innerHTML =
"The result is: " + result;
```

---

# HTML File

```html
<script src="script.js"></script>
```

---

# What Does src Mean?

The browser sees:

```html
<script src="script.js"></script>
```

and performs:

```text
Download script.js
       ↓
Execute script.js
```

---

# Browser Request Flow

When loading:

```html
external.html
```

Browser requests:

```http
GET /external.html
```

Server responds:

```http
200 OK
```

with HTML.

---

The browser discovers:

```html
<script src="script.js">
```

and sends another request:

```http
GET /script.js
```

---

Server responds:

```javascript
let x = 5;
let y = 10;
...
```

Browser executes the script.

---

# Internal vs External JavaScript

| Internal JS | External JS |
|-------------|-------------|
| Inside HTML | Separate File |
| Simple | Organized |
| Good for Small Projects | Good for Large Projects |
| Harder to Maintain | Easier to Maintain |

---

# Real-World Websites

Most modern websites use external JavaScript.

Examples:

```text
main.js
bundle.js
app.js
vendor.js
runtime.js
```

These files often contain thousands of lines of code.

---

# The Document Object Model (DOM)

One of the most important concepts in JavaScript.

DOM stands for:

```text
Document Object Model
```

---

# What is the DOM?

The browser converts HTML into a tree structure.

Example:

```html
<html>

<body>

<h1>Hello</h1>

<p>Welcome</p>

</body>

</html>
```

Becomes:

```text
document
│
└── html
    │
    └── body
         │
         ├── h1
         │
         └── p
```

---

# Why the DOM Exists

The DOM allows JavaScript to interact with webpages.

Without the DOM:

```javascript
Cannot Modify HTML
Cannot Read Elements
Cannot Update Content
```

---

# Accessing Elements

Example:

```javascript
document.getElementById("result");
```

Meaning:

```text
Find HTML Element
with ID = result
```

---

# Common DOM Functions

Find element by ID:

```javascript
document.getElementById()
```

---

Find element by class:

```javascript
document.getElementsByClassName()
```

---

Find element using CSS selector:

```javascript
document.querySelector()
```

---

# Modifying the DOM

Example:

```javascript
document.getElementById("message")
.innerHTML =
"Welcome";
```

The page changes immediately.

---

# Why Pentesters Care About DOM

Many client-side vulnerabilities involve DOM manipulation.

Examples:

- DOM-Based XSS
- Client-Side Redirects
- Sensitive Data Exposure

Understanding DOM interactions is essential for modern web testing.

---

# Viewing Source Code

One of the easiest ways to inspect a webpage.

Methods:

```text
Ctrl + U
```

or

```text
Right Click
↓
View Page Source
```

---

# Why View Source Matters

Source code reveals:

- Internal JavaScript
- External JavaScript
- Hidden Elements
- Comments
- API Endpoints

---

# Identifying Internal JavaScript

Example:

```html
<script>

alert("Hello");

</script>
```

No:

```html
src=
```

attribute exists.

Therefore:

```text
Internal JavaScript
```

---

# Identifying External JavaScript

Example:

```html
<script src="main.js"></script>
```

The browser loads:

```text
main.js
```

from another file.

---

# Pentester Methodology

When assessing a website:

Step 1:

```text
View Source
```

---

Step 2:

Search:

```html
<script
```

---

Step 3:

Identify:

```text
main.js
app.js
bundle.js
vendor.js
```

---

Step 4:

Open JavaScript Files

---

Step 5:

Analyze Content

---

# What Pentesters Search For

Common keywords:

```text
admin
```

```text
api
```

```text
debug
```

```text
internal
```

```text
secret
```

```text
token
```

```text
password
```

---

# Real Example

Suppose:

```javascript
const API =
"/internal/admin";
```

exists inside:

```text
main.js
```

The endpoint might not be visible in the user interface.

However:

```text
Source Code Review
```

reveals its existence.

---

# Hidden Functionality Discovery

Many bug bounty findings originate from:

```text
JavaScript Reconnaissance
```

Attackers inspect JavaScript files and discover:

- Hidden endpoints
- Debug functionality
- Internal APIs
- Test environments

---

# Red Team Perspective

JavaScript files are valuable sources of information.

Common targets:

- API Endpoints
- Admin Functions
- Hidden Routes
- Internal URLs
- Feature Flags

---

# Blue Team Perspective

Developers should assume:

```text
Every JavaScript File
is Public
```

Never place:

- Passwords
- Tokens
- Secrets
- Credentials

inside client-side JavaScript.

---

# Key Takeaways

- JavaScript can be integrated internally or externally.
- External JavaScript is the standard approach in modern applications.
- Browsers create a DOM from HTML.
- JavaScript interacts with webpages through the DOM.
- document.getElementById() locates HTML elements.
- innerHTML modifies page content.
- View Source reveals JavaScript integration.
- External JavaScript files are valuable targets during web assessments.

---

# Skills Gained

After completing this section, you should understand:

- Internal JavaScript
- External JavaScript
- Browser rendering process
- DOM fundamentals
- DOM manipulation
- HTML and JavaScript integration
- Source code analysis
- JavaScript enumeration techniques
- Web application reconnaissance methodology


# Part 4 - Dialogue Functions and JavaScript Abuse

---

# Introduction

One of JavaScript's primary purposes is enabling interaction between websites and users.

Without JavaScript, websites would mostly display static content.

With JavaScript, websites can:

- Display notifications
- Request user input
- Confirm user actions
- Update content dynamically
- Communicate with servers

To support these interactions, JavaScript provides several built-in dialogue functions.

The three most common are:

```javascript
alert()
prompt()
confirm()
```

Although these functions appear simple, they introduce important concepts that later become critical when studying:

- Cross-Site Scripting (XSS)
- Social Engineering
- Browser Security
- Client-Side Attacks
- Malicious JavaScript

---

# Understanding Browser Dialogues

A dialogue box is a small window generated by the browser.

Its purpose is to:

- Display information
- Request information
- Ask for confirmation

Unlike normal webpage elements, dialogue boxes interrupt normal browsing behavior.

When a dialogue box appears:

```text
User Interaction Stops
```

until the dialogue is handled.

---

# Why Dialogue Functions Exist

Imagine an online banking application.

Before transferring money:

```text
Transfer $5000
```

the application may ask:

```text
Are you sure?
```

before proceeding.

Without dialogue functions:

```text
User mistakes become more likely.
```

---

# JavaScript Built-In Dialogue Functions

JavaScript provides three primary dialogue functions:

| Function | Purpose |
|-----------|-----------|
| alert() | Display information |
| prompt() | Request user input |
| confirm() | Request confirmation |

---

# The alert() Function

## Purpose

Displays a message to the user.

Example:

```javascript
alert("Hello THM");
```

---

# Browser Behavior

The browser creates a popup window.

Example:

```text
+--------------------+
| Hello THM          |
|                    |
|       OK           |
+--------------------+
```

The user must click:

```text
OK
```

before continuing.

---

# Execution Flow

Example:

```javascript
console.log("Start");

alert("Hello");

console.log("End");
```

Execution:

```text
Start
      ↓
Alert Appears
      ↓
Program Pauses
      ↓
User Clicks OK
      ↓
End
```

---

# Why alert() Matters

Although simple, alert() demonstrates:

```text
JavaScript
can directly interact
with users.
```

This capability becomes important in later security topics.

---

# Practical Example

Example:

```javascript
alert("Welcome to THM");
```

Output:

```text
Welcome to THM
```

inside a popup dialogue.

---

# Common Uses of alert()

Historically:

- Notifications
- Warnings
- Debugging

Examples:

```javascript
alert("Incorrect Password");
```

```javascript
alert("File Uploaded");
```

```javascript
alert("Account Created");
```

---

# Modern Usage

Modern websites rarely use alert().

Instead they use:

```text
Custom Modal Windows
```

or:

```text
Notification Components
```

because alert() interrupts user interaction.

---

# The prompt() Function

## Purpose

Collect information from users.

Example:

```javascript
prompt("What is your name?");
```

---

# Browser Behavior

The browser creates:

```text
+----------------------+
| What is your name?   |
|                      |
| [______________]     |
|                      |
| OK     Cancel        |
+----------------------+
```

---

# Return Values

prompt() returns:

| User Action | Result |
|------------|----------|
| Click OK | User Input |
| Click Cancel | null |

---

# Example

Code:

```javascript
let name =
prompt("What is your name?");
```

User enters:

```text
Devdan
```

Result:

```javascript
name = "Devdan";
```

---

# Using User Input

Example:

```javascript
let name =
prompt("What is your name?");

alert("Hello " + name);
```

User enters:

```text
Devdan
```

Output:

```text
Hello Devdan
```

---

# Execution Flow

```text
Prompt Appears
      ↓
User Types Value
      ↓
Value Returned
      ↓
Stored in Variable
      ↓
Program Continues
```

---

# Why prompt() Matters

prompt() introduces:

```text
User Input
```

which is one of the most important attack surfaces in cybersecurity.

---

# The Security Problem

Whenever applications accept user input:

they must assume:

```text
Input Can Be Malicious
```

---

# Example

Normal input:

```text
Devdan
```

Malicious input:

```html
<script>alert(1)</script>
```

---

# Fundamental Security Principle

Every web vulnerability begins somewhere with:

```text
Input
       ↓
Processing
       ↓
Output
```

Understanding prompt() helps establish this mental model.

---

# The confirm() Function

## Purpose

Requests confirmation from users.

Example:

```javascript
confirm("Are you sure?");
```

---

# Browser Behavior

```text
+----------------------+
| Are you sure?        |
|                      |
| OK      Cancel       |
+----------------------+
```

---

# Return Values

| Action | Return Value |
|----------|----------|
| OK | true |
| Cancel | false |

---

# Example

```javascript
let result =
confirm("Delete file?");
```

If:

```text
OK
```

Result:

```javascript
true
```

If:

```text
Cancel
```

Result:

```javascript
false
```

---

# Practical Example

```javascript
if(confirm("Delete Account?"))
{
    alert("Account Deleted");
}
```

Execution:

```text
User Clicks OK
      ↓
Condition True
      ↓
Delete Action Executes
```

---

# Why confirm() Matters

Applications frequently use confirm() before:

- Deleting files
- Removing users
- Cancelling subscriptions
- Performing destructive actions

---

# User Interaction Model

All three dialogue functions follow the same pattern:

```text
JavaScript
      ↓
User Interaction
      ↓
User Response
      ↓
Program Continues
```

This interaction model is important when studying social engineering attacks.

---

# How Attackers Abuse Dialogue Functions

The room introduces a simple example:

```html
<script>

for(let i = 0; i < 3; i++)
{
    alert("Hacked");
}

</script>
```

---

# Understanding the Code

Loop:

```javascript
for(let i = 0; i < 3; i++)
```

runs:

```text
3 Times
```

---

Each iteration executes:

```javascript
alert("Hacked");
```

Result:

```text
Popup #1
Popup #2
Popup #3
```

---

# User Experience Impact

Each popup must be closed manually.

Execution:

```text
Alert
 ↓
Click OK
 ↓
Alert
 ↓
Click OK
 ↓
Alert
 ↓
Click OK
```

This creates an annoying experience.

---

# Increasing the Impact

Example:

```javascript
for(let i = 0; i < 500; i++)
{
    alert("Hacked");
}
```

Now:

```text
500 Popup Windows
```

appear sequentially.

---

# Why This Works

Dialogue functions are:

```text
Modal Dialogues
```

Meaning:

```text
Browser Pauses
Until User Responds
```

---

# Browser Locking

Example:

```javascript
while(true)
{
    alert("Hello");
}
```

creates:

```text
Infinite Popups
```

which may render the page unusable.

---

# Social Engineering Scenario

The room presents an example:

```text
invoice.html
```

received through email.

---

# Victim Perspective

Victim thinks:

```text
Invoice Document
```

---

Reality:

```html
<script>

for(let i = 0; i < 500; i++)
{
    alert("Hacked");
}

</script>
```

---

Opening the file executes JavaScript immediately.

This demonstrates why:

```text
HTML Files
Can Execute JavaScript
```

inside a browser.

---

# Why HTML Files Can Be Dangerous

Many users assume:

```text
HTML = Document
```

However:

```text
HTML
+
JavaScript
=
Executable Browser Content
```

---

# Malicious JavaScript

JavaScript itself is not malicious.

However:

```text
Any Technology
Can Be Abused
```

---

Legitimate Use:

```javascript
alert("Welcome");
```

---

Malicious Use:

```javascript
alert("Hacked");
```

---

The technology remains identical.

Only the intent changes.

---

# Introduction to Cross-Site Scripting (XSS)

Although this room does not fully cover XSS yet, it introduces an important concept.

Consider:

```javascript
alert("Hello");
```

---

If a developer writes it:

```text
Normal Behavior
```

---

If an attacker executes it:

```text
Cross-Site Scripting
Proof of Concept
```

---

# Why alert(1) Is Famous

The most famous XSS payload:

```javascript
alert(1);
```

---

Beginners often ask:

```text
Why alert(1)?
```

---

Because:

```text
Safe
Simple
Visible
```

---

The goal is not damage.

The goal is proving:

```text
Attacker-Controlled
JavaScript Executed
```

inside the victim's browser.

---

# Real XSS Progression

Step 1:

```javascript
alert(1)
```

---

Step 2:

```javascript
alert(document.domain)
```

---

Step 3:

```javascript
document.cookie
```

---

Step 4:

More advanced attacks.

The room only introduces the foundation.

---

# Red Team Perspective

When attackers discover JavaScript execution:

they may attempt to:

- Read page contents
- Modify page contents
- Steal data
- Manipulate forms
- Execute additional JavaScript

The initial proof is often:

```javascript
alert(1)
```

---

# Blue Team Perspective

Defenders should:

- Validate user input
- Sanitize user input
- Prevent script injection
- Use Content Security Policy (CSP)
- Avoid unsafe DOM operations

---

# Security Lessons

Dialogue functions themselves are not dangerous.

However, they demonstrate:

```text
JavaScript Executes
Inside User Browsers
```

and:

```text
User Interfaces
Can Be Manipulated
```

by JavaScript.

---

# Key Takeaways

- alert() displays messages.
- prompt() collects input.
- confirm() requests confirmation.
- Dialogue functions pause program execution.
- User input introduces security risks.
- HTML files can execute JavaScript.
- Malicious JavaScript can disrupt users.
- alert(1) is commonly used as an XSS proof-of-concept.
- Understanding browser interaction is important for web security.

---

# Skills Gained

After completing this section, you should understand:

- Browser dialogue functions
- User interaction flow
- User input processing
- Modal dialogue behavior
- Basic social engineering concepts
- Malicious JavaScript examples
- Foundations of XSS
- Red Team and Blue Team perspectives regarding browser interaction


# Part 5 - Control Flow Statements and Authentication Bypass Concepts

---

# Introduction

One of JavaScript's most important responsibilities is making decisions.

Web applications constantly evaluate conditions such as:

```text
Is the user logged in?
Is the password correct?
Is the user an administrator?
Is the account active?
Does the user have permission?
```

To handle these scenarios, JavaScript uses control flow statements.

Control flow determines:

```text
Which code executes
Which code is skipped
When code executes
How many times code executes
```

Understanding control flow is important for both developers and penetration testers because many security weaknesses originate from poorly designed client-side logic.

---

# What is Control Flow?

Control flow refers to the order in which program instructions execute.

Without control flow:

```text
Line 1
Line 2
Line 3
Line 4
```

always execute sequentially.

---

# Why Control Flow Exists

Programs must react differently under different conditions.

Example:

```text
Correct Password
      ↓
Allow Access

Incorrect Password
      ↓
Deny Access
```

Without control flow, applications could not make decisions.

---

# Common JavaScript Control Flow Structures

JavaScript provides:

## Conditional Statements

```javascript
if
else
switch
```

---

## Looping Statements

```javascript
for
while
do...while
```

---

# The if Statement

The most common control flow statement.

Example:

```javascript
let age = 25;

if(age >= 18)
{
    console.log("Adult");
}
```

---

# Execution Process

Step 1:

```javascript
age = 25
```

---

Step 2:

Evaluate:

```javascript
25 >= 18
```

---

Step 3:

Result:

```javascript
true
```

---

Step 4:

Execute:

```javascript
console.log("Adult");
```

Output:

```text
Adult
```

---

# The else Statement

Used when the condition is false.

Example:

```javascript
if(age >= 18)
{
    console.log("Adult");
}
else
{
    console.log("Minor");
}
```

---

# Execution Example

Input:

```javascript
age = 15
```

Evaluation:

```javascript
15 >= 18
```

Result:

```javascript
false
```

Output:

```text
Minor
```

---

# Visualizing Control Flow

```text
Condition
    │
    ▼
TRUE ? ───────► Execute IF Block
    │
    │
    ▼
Execute ELSE Block
```

---

# Practical Example: Age Verification

The room introduces:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Age Verification</title>
</head>

<body>

<h1>Age Verification</h1>

<p id="message"></p>

<script>

age = prompt("What is your age")

if(age >= 18)
{
    document.getElementById("message")
    .innerHTML =
    "You are an adult.";
}
else
{
    document.getElementById("message")
    .innerHTML =
    "You are a minor.";
}

</script>

</body>
</html>
```

---

# Understanding the Workflow

When the page loads:

```javascript
prompt("What is your age")
```

appears.

---

User enters:

```text
20
```

---

Variable:

```javascript
age = 20
```

---

Condition:

```javascript
20 >= 18
```

---

Result:

```javascript
true
```

---

Output:

```text
You are an adult.
```

---

# Alternative Scenario

Input:

```text
15
```

---

Evaluation:

```javascript
15 >= 18
```

---

Result:

```javascript
false
```

---

Output:

```text
You are a minor.
```

---

# Why This Example Matters

The example appears harmless.

However, it introduces a critical security concept:

```text
Client-Side Validation
```

---

# What is Client-Side Validation?

Validation performed inside the browser.

Example:

```javascript
if(age >= 18)
```

The browser decides whether access is granted.

---

# Advantages

Client-side validation:

- Fast
- Responsive
- User friendly

Example:

```text
Password too short
```

can be displayed instantly.

---

# The Security Problem

Client-side validation occurs on:

```text
User Device
```

which means:

```text
User Controls Environment
```

---

# The Golden Rule

Never trust the client.

---

# Why?

Because users can:

- Inspect JavaScript
- Modify JavaScript
- Disable JavaScript
- Execute their own JavaScript

---

# Example Attack

Developer writes:

```javascript
if(age >= 18)
{
    accessGranted();
}
```

---

Attacker opens DevTools.

Changes:

```javascript
age = 99;
```

---

Now:

```javascript
age >= 18
```

returns:

```javascript
true
```

---

Access granted.

---

# The Client Trust Problem

Everything executed in the browser should be considered:

```text
Untrusted
```

---

# Real-World Analogy

Imagine a nightclub.

Bad approach:

```text
Security Guard:
Please verify your own age.
```

---

Good approach:

```text
Security Guard:
Show me your ID.
```

The verification occurs independently.

---

# Authentication Logic

The room then introduces login functionality.

A common beginner implementation:

```javascript
let username =
prompt("Username");

let password =
prompt("Password");

if(
username == "admin"
&&
password == "Password123"
)
{
    success();
}
```

---

# Developer Perspective

The developer thinks:

```text
Credentials Hidden
Inside JavaScript
```

---

Reality:

```text
Every User Receives
The JavaScript
```

---

# Source Code Exposure

If the browser receives:

```javascript
password == "Password123"
```

then:

```text
Attackers Receive It Too
```

---

# Why Client-Side Authentication Fails

Authentication should never rely solely on JavaScript.

---

# Incorrect Design

```text
Browser
    │
    ▼
Check Password
Inside JavaScript
```

---

# Correct Design

```text
Browser
    │
    ▼
Server
    │
Check Credentials
    │
    ▼
Response
```

---

# Example of Secure Authentication

Browser:

```http
POST /login
```

---

Data:

```text
Username
Password
```

---

Server:

```text
Check Database
```

---

Response:

```http
200 OK
```

or:

```http
401 Unauthorized
```

---

# Why Servers Must Validate

Servers control:

- Databases
- Sessions
- Permissions
- Authentication

Users do not.

---

# Source Code Review

One of the most common penetration testing activities.

Objective:

```text
Understand Application Logic
```

---

# Pentester Questions

When reviewing JavaScript:

Ask:

```text
Where is validation occurring?
```

---

Ask:

```text
Can I modify this variable?
```

---

Ask:

```text
Can I bypass this condition?
```

---

Ask:

```text
Does the server verify this?
```

---

# Example Review

Suppose:

```javascript
let isAdmin = false;

if(isAdmin)
{
    accessAdminPanel();
}
```

---

Pentester immediately asks:

```text
Who controls isAdmin?
```

---

If the browser controls it:

```text
Potential Security Issue
```

---

# Authentication Bypass Concepts

Control flow statements often determine:

```text
Authenticated
or
Unauthenticated
```

---

Example:

```javascript
if(isAuthenticated)
{
    showDashboard();
}
else
{
    showLogin();
}
```

---

Attacker Goal

Instead of:

```text
showLogin()
```

the attacker wants:

```text
showDashboard()
```

---

This process is called:

```text
Authentication Bypass
```

---

# Understanding Bypass Mentality

A pentester does not immediately ask:

```text
What is the password?
```

---

A pentester often asks:

```text
Can I avoid the password check?
```

---

Examples:

```text
Can I change variables?
Can I modify requests?
Can I alter responses?
Can I bypass validation?
```

---

# Real-World Client-Side Validation Issues

Common vulnerable patterns:

```javascript
if(role == "admin")
```

---

```javascript
if(isPremium)
```

---

```javascript
if(isAuthenticated)
```

---

```javascript
if(age >= 18)
```

---

The question becomes:

```text
Who controls the value?
```

---

# Red Team Perspective

When analyzing JavaScript:

Look for:

- Authentication logic
- Authorization logic
- User roles
- Access control checks
- Hidden functionality

---

Common search terms:

```text
admin
role
login
auth
token
premium
access
permission
```

---

# Blue Team Perspective

Never trust:

```text
Client-Side Decisions
```

---

Always enforce:

```text
Server-Side Validation
```

---

Always validate:

- Permissions
- Roles
- Authentication
- Business Logic

on the server.

---

# Key Takeaways

- Control flow determines program execution paths.
- if and else are the most common decision-making structures.
- Client-side validation improves usability but not security.
- Users control their browsers.
- Client-side authentication is insecure.
- Sensitive decisions must be validated on the server.
- Source code review helps identify weak security logic.
- Authentication bypass often targets client-side trust assumptions.

---

# Skills Gained

After completing this section, you should understand:

- JavaScript control flow
- Conditional logic
- Client-side validation
- Authentication logic
- Authentication bypass concepts
- Source code review methodology
- Security implications of browser-executed code
- Red Team and Blue Team approaches to access control


# Part 6 - Minification, Obfuscation, and JavaScript Reconnaissance

---

# Introduction

Up until this point, all JavaScript examples in this room have been easy to read.

Example:

```javascript
function hi()
{
    alert("Welcome to THM");
}

hi();
```

Every line is understandable.

Every variable has a meaningful name.

Every function is clearly visible.

However, real-world applications rarely expose JavaScript in such a clean format.

Instead, penetration testers commonly encounter files such as:

```text
main.js
bundle.js
vendor.js
runtime.js
app.js
```

containing thousands or even millions of characters.

The code often appears unreadable.

Example:

```javascript
(function(_0x4fa2a3,_0x2e1b19){
...
})
```

At first glance it may appear impossible to analyze.

Fortunately, understanding JavaScript minification and obfuscation allows security professionals to reverse engineer these files and discover valuable information hidden within them.

---

# Why Developers Minify JavaScript

Modern web applications often contain:

- Thousands of functions
- Multiple frameworks
- Large libraries
- Complex business logic

Without optimization:

```text
Large File Size
        ↓
Longer Download Times
        ↓
Slower Websites
```

To improve performance, developers minimize the amount of data transferred.

---

# What is Minification?

Minification is the process of reducing JavaScript file size by removing unnecessary characters.

These include:

- Spaces
- Tabs
- Line breaks
- Comments

---

# Example

Original:

```javascript
function hi() {
    alert("Welcome to THM");
}

hi();
```

---

Minified:

```javascript
function hi(){alert("Welcome to THM")}hi();
```

---

# What Changed?

Removed:

```text
Spaces
Indentation
Line Breaks
Comments
```

---

# What Did Not Change?

The functionality remains identical.

Both versions execute:

```javascript
alert("Welcome to THM");
```

---

# Why Minification Exists

Advantages:

## Smaller Files

Example:

```text
100 KB
     ↓
70 KB
```

---

## Faster Downloads

Less data transferred.

---

## Improved Performance

Particularly important for:

- Mobile devices
- Slow internet connections
- Large web applications

---

# Minification Does Not Equal Security

A common misconception:

```text
Minified Code
=
Protected Code
```

False.

Minification only affects readability.

Attackers can still analyze the code.

---

# What is Obfuscation?

Obfuscation goes further.

Instead of merely reducing file size, obfuscation intentionally makes code difficult for humans to understand.

---

# Example

Original:

```javascript
function hi()
{
    alert("Welcome to THM");
}

hi();
```

---

Obfuscated:

```javascript
function _0x8f7a()
{
    _0x12ab("Welcome to THM");
}

_0x8f7a();
```

---

# More Aggressive Obfuscation

Example:

```javascript
(function(_0x114713,_0x2246f2){
...
})
```

This resembles the code shown in the room.

---

# Why Obfuscation Exists

Developers use obfuscation to:

- Hide application logic
- Slow down reverse engineering
- Protect intellectual property
- Discourage casual inspection

---

# Important Security Reality

Obfuscation is not encryption.

The browser still needs to execute the code.

Therefore:

```text
If The Browser Can Execute It
       ↓
An Attacker Can Analyze It
```

---

# Security Through Obscurity

A dangerous misconception:

```text
Attackers Cannot Understand My Code
Because It Is Obfuscated
```

This is called:

```text
Security Through Obscurity
```

and is generally considered weak security.

---

# Why Obfuscation Still Helps

Obfuscation provides:

```text
Delay
```

not:

```text
Protection
```

---

# Analogy

Imagine a treasure map.

Normal code:

```text
Easy To Read
```

---

Obfuscated code:

```text
Treasure Map
Written In Strange Symbols
```

The treasure still exists.

It simply takes longer to locate.

---

# Practical Example from the Room

Original:

```javascript
function hi() {
  alert("Welcome to THM");
}

hi();
```

---

Execution Flow

Step 1:

```javascript
function hi()
```

creates the function.

---

Step 2:

```javascript
hi();
```

calls the function.

---

Step 3:

```javascript
alert("Welcome to THM");
```

executes.

---

Output:

```text
Welcome to THM
```

---

# After Obfuscation

The room converts the script into a much larger block:

```javascript
(function(_0x114713,_0x2246f2){
...
})
```

---

# Why Does It Look So Strange?

Obfuscators often:

## Rename Variables

Before:

```javascript
message
```

After:

```javascript
_0x33bf
```

---

## Rename Functions

Before:

```javascript
showMessage()
```

After:

```javascript
_0xab1127()
```

---

## Split Strings

Before:

```javascript
"Welcome to THM"
```

After:

```javascript
"Welcome\x20to"
```

and

```javascript
"\x20THM"
```

---

# Understanding \x20

`\x20` is hexadecimal notation.

Value:

```text
0x20
```

represents:

```text
Space Character
```

---

Therefore:

```javascript
"Welcome\x20to"
```

becomes:

```text
Welcome to
```

---

# Why Pentesters Must Understand This

Attackers frequently encounter:

```text
Minified Code
```

or:

```text
Obfuscated Code
```

during web application assessments.

The ability to analyze these files is an essential skill.

---

# JavaScript Reconnaissance

One of the most valuable web reconnaissance techniques.

Goal:

```text
Extract Information
From JavaScript Files
```

---

# Why JavaScript Files Are Valuable

Developers often leave clues inside JavaScript.

Examples:

```javascript
const API =
"/api/v1/admin";
```

---

```javascript
const DEBUG =
"/debug";
```

---

```javascript
const TEST =
"https://staging.company.com";
```

---

Even when functionality is hidden from users:

JavaScript frequently reveals it.

---

# Hidden Endpoint Discovery

One of the most common findings.

Example:

```javascript
fetch("/internal/users");
```

---

Visible UI:

```text
No User Management Feature
```

---

JavaScript reveals:

```text
/internal/users
```

exists.

---

# Hidden API Discovery

Example:

```javascript
const API_URL =
"/api/v2/private";
```

---

The API endpoint may not appear anywhere else.

Source code review reveals its existence.

---

# Hardcoded Secret Discovery

One of the most severe mistakes developers make.

Example:

```javascript
const API_KEY =
"ABC123SECRET";
```

---

Any user can:

```text
View Source
```

or:

```text
Inspect
```

and retrieve the key.

---

# Common Sensitive Data Found in JavaScript

Examples:

```text
API Keys
```

---

```text
Access Tokens
```

---

```text
JWT Tokens
```

---

```text
Database URLs
```

---

```text
Internal Hostnames
```

---

```text
Debug Endpoints
```

---

# Pentester Workflow

When encountering:

```text
main.js
```

the workflow typically looks like:

---

## Step 1

Identify JavaScript Files

Search:

```html
<script
```

---

## Step 2

Download JavaScript Files

Examples:

```text
main.js
bundle.js
app.js
vendor.js
```

---

## Step 3

Beautify Code

Convert:

```javascript
function a(){if(b){c()}}
```

into:

```javascript
function a()
{
    if(b)
    {
        c();
    }
}
```

---

## Step 4

Search Keywords

Examples:

```text
api
```

---

```text
admin
```

---

```text
secret
```

---

```text
token
```

---

```text
debug
```

---

```text
internal
```

---

```text
password
```

---

## Step 5

Map Functionality

Document:

- Endpoints
- Roles
- APIs
- Hidden Features

---

# Reverse Engineering JavaScript

Reverse engineering means:

```text
Understanding
How Code Works
```

without having the original source code.

---

# Typical Reverse Engineering Process

```text
Minified JS
      ↓
Beautify
      ↓
Analyze Functions
      ↓
Identify Logic
      ↓
Document Findings
```

---

# Malware Analysis Similarities

The same skills are used in:

- Malware Analysis
- Browser Malware Investigation
- Malicious Script Analysis

---

Example:

```javascript
eval(...)
```

---

```javascript
atob(...)
```

---

```javascript
document.write(...)
```

---

These functions often appear in obfuscated scripts.

Understanding them helps identify suspicious behavior.

---

# Red Team Perspective

JavaScript files are reconnaissance gold mines.

Common goals:

- Find hidden APIs
- Discover admin panels
- Identify secrets
- Understand business logic
- Locate testing environments

---

# Blue Team Perspective

Assume:

```text
Every JavaScript File
Is Public
```

Never store:

- Passwords
- Secrets
- API Keys
- Access Tokens

inside client-side code.

---

# Key Takeaways

- Minification reduces file size.
- Obfuscation reduces readability.
- Neither provides real security.
- Attackers can reverse engineer JavaScript.
- JavaScript files often reveal hidden functionality.
- Source code review is a powerful reconnaissance technique.
- Hardcoded secrets are dangerous.
- JavaScript analysis is a core web pentesting skill.

---

# Skills Gained

After completing this section, you should understand:

- JavaScript minification
- JavaScript obfuscation
- Deobfuscation concepts
- Source code review techniques
- Hidden endpoint discovery
- Hardcoded secret discovery
- JavaScript reconnaissance methodology
- Reverse engineering fundamentals
- Red Team and Blue Team perspectives on client-side code


# Part 7 - Best Practices, Security Lessons, and Final Takeaways

---

# Introduction

Throughout this room, we explored:

- JavaScript fundamentals
- Variables
- Data types
- Functions
- Loops
- Browser interaction
- Internal and external JavaScript
- Control flow statements
- Authentication logic
- JavaScript minification
- Obfuscation
- Source code review

However, understanding how JavaScript works is only half of the journey.

The other half is understanding:

```text
How JavaScript Can Be Used Securely
```

and

```text
How JavaScript Can Be Abused
```

This final section focuses on security best practices and the lessons every web developer, pentester, and security professional should remember.

---

# Security Mindset

One of the most important lessons from this room is:

> The browser belongs to the user.

Not:

```text
The Developer
```

Not:

```text
The Company
```

Not:

```text
The Security Team
```

The user controls the browser.

This means users can:

- Inspect code
- Modify code
- Disable code
- Execute their own code

Every security decision should be made with this assumption.

---

# Best Practice 1: Avoid Relying on Client-Side Validation

One of the most common mistakes in web development.

---

# What is Client-Side Validation?

Validation performed by JavaScript in the browser.

Example:

```javascript
if(age >= 18)
{
    allowAccess();
}
```

---

# Why Developers Use It

Advantages:

- Fast feedback
- Better user experience
- Reduced server requests

Example:

```text
Password too short
```

can be displayed instantly.

---

# The Problem

The validation executes inside:

```text
The User's Browser
```

which means:

```text
The User Can Modify It
```

---

# Example Attack

Original:

```javascript
let age = 15;

if(age >= 18)
{
    accessGranted();
}
```

---

Attacker opens DevTools:

```javascript
age = 99;
```

---

Now:

```javascript
age >= 18
```

returns:

```javascript
true
```

---

# The Correct Approach

Client-side validation:

```text
User Experience
```

Server-side validation:

```text
Security
```

---

# Secure Workflow

```text
Browser Validation
        ↓
Submit Data
        ↓
Server Validation
        ↓
Decision
```

---

# Golden Rule

Client-side validation improves usability.

Server-side validation provides security.

You need both.

---

# Best Practice 2: Use Trusted Libraries Only

Modern web applications depend heavily on external libraries.

Examples:

- React
- Vue
- Angular
- jQuery
- Lodash

---

# Why Libraries Exist

Instead of writing:

```text
Thousands Of Lines
```

developers reuse existing code.

This saves:

- Time
- Money
- Development effort

---

# The Supply Chain Problem

Attackers know developers trust libraries.

As a result:

```text
Libraries Become Targets
```

---

# Typosquatting

A common attack.

Legitimate package:

```text
express
```

Malicious package:

```text
expres
```

Only one character differs.

---

# Developer Mistake

Developer installs:

```bash
npm install expres
```

instead of:

```bash
npm install express
```

---

# Result

Malicious code executes.

Potential consequences:

- Credential theft
- Backdoors
- Data exfiltration
- Remote access

---

# Real-World Impact

Several major supply-chain attacks have occurred through:

- NPM packages
- Browser extensions
- Open-source libraries

---

# Defensive Recommendations

Always verify:

- Package name
- Package author
- Repository
- Download count
- Maintenance status

---

# Best Practice 3: Avoid Hardcoded Secrets

One of the most common findings during source code reviews.

---

# What is a Hardcoded Secret?

Sensitive information directly embedded in source code.

Example:

```javascript
const API_KEY =
"pk_TryHackMe-1337";
```

---

# Why It Is Dangerous

JavaScript executes on:

```text
Client Side
```

Meaning:

```text
Users Receive The Code
```

---

# If Users Receive It

Then:

```text
Attackers Receive It Too
```

---

# Commonly Exposed Secrets

Examples:

```text
API Keys
```

---

```text
JWT Tokens
```

---

```text
Database Credentials
```

---

```text
Cloud Access Keys
```

---

```text
Service Credentials
```

---

# Typical Discovery Process

Attacker:

```text
View Source
```

or:

```text
Inspect
```

or:

```text
Review main.js
```

---

Finds:

```javascript
const AWS_SECRET =
"AKIA...";
```

---

# Secure Alternative

Store secrets on:

```text
Server
```

not:

```text
Browser
```

---

# Security Principle

If the browser can see it:

```text
It Is Not Secret
```

---

# Best Practice 4: Minify and Obfuscate Production Code

The room recommends:

```text
Minification
```

and:

```text
Obfuscation
```

for production environments.

---

# Why Minify?

Benefits:

- Smaller files
- Faster loading
- Better performance

---

# Why Obfuscate?

Benefits:

- Slows reverse engineering
- Hides implementation details
- Increases attacker effort

---

# Important Clarification

Obfuscation is not security.

---

# Common Misconception

Developers sometimes think:

```text
Obfuscated Code
=
Protected Code
```

This is false.

---

# Reality

The browser still executes:

```javascript
alert("Hello");
```

whether the code is readable or not.

---

# Security Through Obscurity

Obfuscation provides:

```text
Delay
```

not:

```text
Protection
```

---

# What Real Security Looks Like

Real security comes from:

- Authentication
- Authorization
- Encryption
- Server-side validation
- Secure coding practices

Not:

```text
Unreadable JavaScript
```

---

# Red Team Lessons

Throughout this room, several recurring themes appear.

---

# Lesson 1

Always inspect JavaScript.

Useful discoveries include:

- Hidden APIs
- Internal endpoints
- Admin functionality
- Debug features

---

# Lesson 2

Never trust client-side logic.

Examples:

```javascript
if(isAdmin)
```

---

```javascript
if(isPremium)
```

---

```javascript
if(isAuthenticated)
```

These conditions deserve investigation.

---

# Lesson 3

Source code review is reconnaissance.

JavaScript files often reveal:

```text
Application Architecture
```

before any exploitation begins.

---

# Lesson 4

Client-side code is public code.

Always assume:

```text
Users Can Read It
```

---

# Blue Team Lessons

Defenders should remember:

---

# Rule 1

Validate everything server-side.

---

# Rule 2

Never expose secrets.

---

# Rule 3

Review third-party dependencies.

---

# Rule 4

Keep JavaScript updated.

---

# Rule 5

Assume attackers can inspect all client-side code.

---

# Major Cybersecurity Concepts Introduced

Although this room is beginner-friendly, it introduces concepts that appear throughout web security.

---

# Cross-Site Scripting (XSS)

Introduced through:

```javascript
alert(1);
```

---

# Client-Side Validation Bypass

Introduced through:

```javascript
if(age >= 18)
```

---

# Authentication Bypass

Introduced through:

```javascript
if(username == ...)
```

---

# Source Code Review

Introduced through:

```text
View Source
```

and:

```text
Developer Tools
```

---

# JavaScript Reconnaissance

Introduced through:

```text
main.js
bundle.js
app.js
```

analysis.

---

# Skills Gained

After completing this room, you should understand:

## JavaScript Fundamentals

- Variables
- Data Types
- Functions
- Loops
- Conditions

---

## Browser Fundamentals

- Console
- DevTools
- DOM
- Internal JavaScript
- External JavaScript

---

## Security Fundamentals

- Client-side validation
- Authentication logic
- Source code review
- Minification
- Obfuscation
- Secret exposure

---

## Pentesting Fundamentals

- JavaScript reconnaissance
- Endpoint discovery
- Logic analysis
- Client-side testing

---

# How This Fits Into Your Cybersecurity Journey

This room serves as a foundation for future topics.

---

# Next Web Security Topics

You will encounter:

- HTTP Requests
- HTTP Responses
- Sessions
- Cookies
- Authentication
- Authorization
- APIs

---

# Future JavaScript Security Topics

You will eventually study:

- Cross-Site Scripting (XSS)
- DOM-Based XSS
- CSP Bypass
- Client-Side Attacks
- JavaScript Malware
- Browser Exploitation

---

# Why This Room Matters

Many beginners underestimate JavaScript.

However, modern web applications are largely powered by JavaScript.

If you cannot read JavaScript:

```text
You Cannot Fully Understand
Modern Web Applications
```

And if you cannot understand modern web applications:

```text
You Cannot Effectively Test Them
```

---

# Final Conclusion

JavaScript is far more than a programming language.

It is the technology that powers most modern web applications and serves as the bridge between users, browsers, and servers.

For developers, JavaScript enables dynamic and interactive experiences.

For attackers, JavaScript provides insight into application logic, hidden functionality, and potential weaknesses.

For defenders, JavaScript introduces security challenges that require careful design and validation.

The most important lesson from this room is:

> Never trust the client.

Everything running inside the browser should be considered visible, modifiable, and controllable by the user.

Understanding this principle provides the foundation for studying web application security, source code analysis, client-side attacks, and many of the vulnerabilities encountered in real-world penetration testing engagements.

---

# Key Takeaways

- JavaScript powers modern web applications.
- Client-side code should never be trusted.
- Variables, functions, loops, and conditions form the foundation of JavaScript.
- DevTools allows direct interaction with browser-executed code.
- Internal and external JavaScript are common integration methods.
- Source code review is a powerful reconnaissance technique.
- Client-side validation does not provide security.
- Hardcoded secrets are dangerous.
- Minification improves performance.
- Obfuscation increases difficulty but does not provide true security.
- Understanding JavaScript is essential for modern web application security.

# End of Room

**Room Completed: JavaScript Essentials**
