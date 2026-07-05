# Exercise 1: Gathering Information About a Target Website Using WhatWeb

## Objective

This lab demonstrates how to use **WhatWeb**, a website fingerprinting tool, to identify technologies used by a target website.

### Learning Objectives

- Identify technologies used by a website
- Perform aggressive scans
- Save scan results to a file
- Understand how website fingerprinting helps during reconnaissance

---

# What is WhatWeb?

**WhatWeb** is an information gathering (reconnaissance) tool used by penetration testers to identify the technologies running on a website.

It can detect:

- Web Server (Apache, Nginx, IIS)
- Programming Language (PHP, ASP.NET, Node.js)
- Content Management System (WordPress, Joomla, Drupal)
- JavaScript Libraries (jQuery, React)
- CSS Frameworks (Bootstrap)
- Analytics Tools (Google Analytics)
- Cookies
- Security Headers
- CDN
- Email Technologies
- Embedded Devices

> **Note:** WhatWeb does **not** hack a website. It only analyzes publicly available information returned by the web server.

---

# Why is Website Fingerprinting Important?

Before testing a website for vulnerabilities, a penetration tester first wants to know:

- What server is running?
- Which CMS is installed?
- Which programming language is used?
- Are any frameworks or plugins present?
- Can version numbers be identified?

Example:

If WhatWeb detects:

- Apache 2.4.49
- PHP 7.2
- WordPress 5.7

The tester can search for known vulnerabilities affecting those specific versions.

---

# Lab Steps

## Step 1 – View WhatWeb Help

Run:

```bash
whatweb
```

### Purpose

Displays the available commands and options.

Example output:

```
Usage:
whatweb [options] <URL>

-v
-a
--log-verbose
--log-xml
```

---

## Step 2 – Perform a Basic Scan

Run:

```bash
whatweb www.luxurytreats.com
```

### What Happens?

WhatWeb sends an HTTP request to the website.

It examines:

- HTTP Headers
- HTML Source Code
- Cookies
- JavaScript Files
- Meta Tags

Then it identifies technologies used by the website.

Example:

```
Apache
PHP
Bootstrap
Google Analytics
jQuery
```

---

# How Does WhatWeb Detect Technologies?

### Example 1

HTML:

```html
<script src="jquery.min.js"></script>
```

Detection:

```
JavaScript Library:
jQuery
```

---

### Example 2

HTML:

```html
<meta name="generator" content="WordPress 6.5">
```

Detection:

```
CMS:
WordPress
```

---

### Example 3

HTTP Header:

```
Server: Apache/2.4.57
```

Detection:

```
Web Server:
Apache
```

---

# Step 3 – Use Verbose Mode

Run:

```bash
whatweb -v www.luxurytreats.com
```

### Meaning

`-v` = Verbose Mode

Instead of displaying everything on one line, WhatWeb organizes the output.

Example:

```
Status Code:
200 OK

Title:
Luxury Treats

Server:
Apache

Country:
United States

Cookies:
PHPSESSID

JavaScript:
jQuery

Framework:
Bootstrap
```

This makes the report much easier to read.

---

# Step 4 – Perform an Aggressive Scan

Run:

```bash
whatweb -a 3 www.luxurytreats.com
```

### Meaning

`-a` = Aggression Level

Available levels:

```
1
2
3
4
```

Higher levels send additional requests to gather more information.

Aggressive mode may reveal:

- Software versions
- Plugin versions
- Framework versions
- Additional technologies

Example:

Basic Scan:

```
WordPress
```

Aggressive Scan:

```
WordPress 6.5.2
```

---

# Why Are Version Numbers Important?

Knowing the exact version helps identify known vulnerabilities.

Example:

```
Apache 2.4.49
```

Searching for:

```
Apache 2.4.49 vulnerabilities
```

may reveal publicly known exploits.

---

# Step 5 – Save the Scan Report

Run:

```bash
whatweb --log-verbose=luxurytreats_report www.luxurytreats.com
```

### Purpose

Saves the verbose output into a file named:

```
luxurytreats_report
```

instead of displaying everything only in the terminal.

---

# Step 6 – Open the Report

Navigate to:

```
Places
    ↓
Home Folder
    ↓
luxurytreats_report
```

The report contains detailed information about the website infrastructure.

Example:

```
Target:
www.luxurytreats.com

Status:
200 OK

Title:
Luxury Treats

Server:
Apache

Language:
PHP

Framework:
Bootstrap

Analytics:
Google Analytics

Cookies:
PHPSESSID
```

---

# Export Scan Results as XML

To save the output in XML format:

```bash
whatweb --log-xml=report.xml www.luxurytreats.com
```

XML reports are useful because they can be:

- Imported into security tools
- Shared with team members
- Parsed automatically by scripts

---

# Information Collected by WhatWeb

| Information | Example |
|-------------|---------|
| Web Server | Apache |
| Operating System | Linux |
| Programming Language | PHP |
| CMS | WordPress |
| Framework | Bootstrap |
| JavaScript Library | jQuery |
| Analytics | Google Analytics |
| Cookies | PHPSESSID |
| HTTP Headers | Server, X-Powered-By |
| Page Title | Luxury Treats |
| Country | United States |
| IP Address | 104.x.x.x |

---

# How WhatWeb Works

```
                User
                  │
                  │
      whatweb website.com
                  │
                  ▼
            Target Website
                  │
      HTTP Response Returned
                  │
        ┌─────────┴─────────┐
        │                   │
     HTTP Headers       HTML Source
        │                   │
     Cookies          JavaScript
        │                   │
        └─────────┬─────────┘
                  │
                  ▼
          WhatWeb Detection Engine
                  │
                  ▼
          Identified Technologies
                  │
                  ▼
               Final Report
```

---

# Common WhatWeb Commands

| Command | Description |
|---------|-------------|
| `whatweb target.com` | Basic fingerprinting |
| `whatweb -v target.com` | Verbose output |
| `whatweb -a 3 target.com` | Aggressive scan |
| `whatweb --log-verbose=report target.com` | Save verbose report |
| `whatweb --log-xml=report.xml target.com` | Save XML report |

---

# Key Takeaways

- WhatWeb is a website fingerprinting tool.
- It is commonly used during the reconnaissance phase of penetration testing.
- It identifies web technologies by analyzing publicly available information.
- Verbose mode (`-v`) makes the output easier to understand.
- Aggressive mode (`-a`) performs additional checks to identify software versions.
- Reports can be saved as text or XML for documentation and later analysis.
- Fingerprinting helps penetration testers decide which vulnerabilities to investigate next.
