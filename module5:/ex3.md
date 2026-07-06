# Exercise 3: Performing Web Vulnerability Scanning Using AI

## Objective

The objective of this lab is to use an AI tool such as ChatGPT to generate a Python script that automates **web vulnerability scanning**. Instead of manually testing the website, the script performs multiple security checks and saves the results in a report.

> **Difference between Exercise 2 and Exercise 3**
>
> - **Exercise 2:** Website Footprinting → Collect information about the website.
> - **Exercise 3:** Web Vulnerability Scanning → Test whether the website has security vulnerabilities.

---

# What is Web Vulnerability Scanning?

Web vulnerability scanning is the process of testing a web application for known security weaknesses that attackers could exploit.

Instead of asking:

> "What technologies does this website use?"

We now ask:

- Can the website be attacked using SQL Injection?
- Can JavaScript be injected (XSS)?
- Is the web server misconfigured?
- Are there hidden vulnerabilities?
- Does the application properly validate user input?

---

# Why Use AI?

Writing a complete vulnerability scanner requires hundreds of lines of Python code.

Instead, we provide ChatGPT with a detailed prompt, and it generates a Python script that automates the scanning process.

The generated script combines multiple security checks into one workflow.

---

# Lab Procedure

## Step 1: Generate the Python Script

Open ChatGPT and enter the prompt provided in the lab.

ChatGPT generates a Python script that performs web vulnerability scanning.

Save the generated code as:

```text
Web_Vulnerability_Scanning.py
```

> **Note:** AI-generated code may vary each time. Different implementations are normal.

---

## Step 2: Run the Script

Open Terminal.

Become the root user.

```bash
sudo su
```

Password:

```text
toor
```

Navigate to the script location and execute it.

```bash
python3 Web_Vulnerability_Scanning.py
```

When prompted, enter the target URL.

Example:

```text
http://testphp.vulnweb.com/
```

---

# What Happens After Running the Script?

The script performs several security tests one after another.

---

## 1. Identify User Input Entry Points

### What are Entry Points?

Entry points are places where users can provide input to the application.

Examples:

- Search box
- Login form
- Registration form
- Contact form
- URL parameters
- Cookies
- HTTP Headers

Example URL:

```
http://example.com/search.php?q=laptop
```

Here,

```
q=laptop
```

is a user input.

### Why is this important?

Most web vulnerabilities exist where the application accepts user input.

If there is no user input,

- No SQL Injection
- No XSS
- No Command Injection

The first step of any vulnerability scan is identifying these input locations.

---

## 2. Modify User Inputs

After finding the input fields, the script begins modifying them.

Example:

Original input:

```
q=laptop
```

Modified inputs:

```
q=test

q='

q=<script>

q=AAAAAAAAAAAA
```

### Why?

The goal is to observe how the server responds to unexpected input.

Different responses may indicate vulnerabilities.

Example:

Input:

```
'
```

Server response:

```
SQL Syntax Error
```

This suggests that user input is reaching the database, indicating a possible SQL Injection vulnerability.

---

## 3. Analyze Server Responses

The script compares the normal response with the modified response.

Example:

Normal request:

```
Product Found
```

Modified request:

```
Database Error
```

The difference indicates that the application processes user input insecurely.

---

## 4. Scan for Known Vulnerabilities (Nikto)

The script uses **Nikto**, a web server vulnerability scanner.

Nikto checks for:

- Outdated web server software
- Dangerous files
- Default pages
- Backup files
- Security misconfigurations
- Insecure HTTP methods

Unlike SQL Injection testing, Nikto focuses on the **web server itself** rather than the application's input fields.

---

## 5. SQL Injection Testing

The script sends SQL payloads to user input fields.

Example payloads:

```
'

' OR '1'='1

'--
```

### Purpose

To determine whether user input can manipulate SQL queries executed by the database.

If successful, attackers may gain unauthorized access to database information.

---

## 6. Cross-Site Scripting (XSS) Testing

The script checks whether JavaScript code can be injected into the application.

Example payload:

```html
<script>alert(1)</script>
```

If the website reflects or stores this payload without proper filtering, it may be vulnerable to XSS.

---

## 7. Fuzz Testing

### What is Fuzzing?

Fuzzing means sending unexpected or invalid input to observe how the application behaves.

Examples:

```
AAAAAAAAAAAAAAAAAAAAAAAAAA

''''''''''''''''

<script>

Very long strings

Special characters
```

### Why?

Unexpected input may reveal:

- Application crashes
- Error messages
- Memory issues
- Poor input validation

These behaviors often indicate security weaknesses.

---

## 8. Buffer Overflow Testing

The script may also send extremely large input values.

Example:

```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...
```

Purpose:

To check whether excessive input causes unexpected behavior or application crashes.

This technique is more common in native applications than modern web applications.

---

# Output Report

After completing all tests, the script generates:

```text
Web_Vulnerability_Scanning_Results.txt
```

The report typically contains:

- User Input Entry Points
- SQL Injection Results
- XSS Results
- Nikto Scan Results
- Fuzz Testing Results
- Server Responses

This allows the tester to review all findings in one place.

---

# Overall Workflow

```
                User
                  │
                  ▼
     Run Python Script
                  │
                  ▼
        Enter Target URL
                  │
                  ▼
      Identify Input Points
                  │
                  ▼
     Test SQL Injection
                  │
                  ▼
          Test XSS
                  │
                  ▼
       Run Nikto Scan
                  │
                  ▼
       Perform Fuzz Testing
                  │
                  ▼
     Analyze Server Responses
                  │
                  ▼
Generate Web_Vulnerability_Scanning_Results.txt
```

---

# Why is `testphp.vulnweb.com` Used?

`testphp.vulnweb.com` is a deliberately vulnerable website designed for security training.

It allows students to safely practice vulnerability scanning techniques without targeting real production websites.

---

# Key Takeaways

- Web vulnerability scanning identifies security weaknesses in web applications.
- The first step is identifying user input entry points.
- SQL Injection testing checks whether database queries can be manipulated.
- XSS testing checks whether malicious JavaScript can be executed.
- Nikto scans the web server for known vulnerabilities and misconfigurations.
- Fuzz testing sends unexpected input to observe application behavior.
- All results are saved in `Web_Vulnerability_Scanning_Results.txt` for later analysis.
