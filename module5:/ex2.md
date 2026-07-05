# Exercise 2: Performing Website Footprinting Using AI

## Objective

The purpose of this lab is to use an AI tool such as ChatGPT to generate a Python script that automates website footprinting and enumeration. Instead of manually running multiple tools, the generated script performs several reconnaissance tasks and saves the collected information into a report.

---

# Why Use AI?

Traditionally, penetration testers use many different tools to gather information about a target website. Writing a script to automate all these tasks manually takes time.

AI tools like ChatGPT can generate the required Python code from a simple prompt, allowing you to automate repetitive reconnaissance tasks.

---

# Lab Workflow

```
          User
            │
            ▼
      Open ChatGPT
            │
            ▼
Paste the Prompt Given in the Lab
            │
            ▼
 ChatGPT Generates Python Script
            │
            ▼
 Save as Website_Footprinting.py
            │
            ▼
 Run the Script
            │
            ▼
 Enter Target Website URL
            │
            ▼
 Script Performs Information Gathering
            │
            ▼
 Creates Information_Gathered.txt
```

---

# Step 1: Open ChatGPT

Open Mozilla Firefox on the Parrot Security machine.

Visit:

```
https://chatgpt.com
```

This will be used to generate the Python automation script.

---

# Step 2: Enter the Prompt

Copy and paste the prompt provided in the lab into ChatGPT.

The prompt instructs ChatGPT to generate a Python script that performs the following tasks:

1. Website Footprinting
2. Website Enumeration
3. HTML Source Code Analysis
4. HTTP/HTML Processing Analysis
5. Server-side Technology Detection
6. Website Crawling
7. Sitemap Discovery
8. Word List Extraction
9. Metadata Extraction
10. WAF Detection
11. Load Balancer Detection
12. HTTP Service Discovery
13. Banner Grabbing
14. Directory Enumeration
15. Proxy Detection

It also asks ChatGPT to save all collected information into:

```
Information_Gathered.txt
```

---

# Step 3: ChatGPT Generates the Script

After submitting the prompt, ChatGPT generates a Python program.

The generated code usually contains:

- Required Python libraries
- Functions for each reconnaissance task
- User input for the target URL
- Report generation
- Error handling

> **Note:** The generated code may differ each time. This is completely normal because AI can produce different implementations for the same prompt.

---

# Step 4: Save the Script

Copy the generated Python code.

Open a text editor (Mousepad, Gedit, Nano, etc.).

Save the file as:

```
Website_Footprinting.py
```

---

# Step 5: Run the Script

Open Terminal.

Become the root user.

```bash
sudo su
```

Password:

```
toor
```

Navigate to the folder containing the script.

Example:

```bash
cd /home/penetester/Scripts/Module\ 5
```

Run the script.

```bash
python3 Website_Footprinting.py
```

---

# Step 6: Enter the Target URL

The script prompts:

```
Enter Target URL:
```

Enter:

```
http://www.luxurytreats.com
```

The script then begins gathering information about the website.

---

# Step 7: Information Gathering Process

The script performs each task one by one.

## 1. Website Footprinting

Collects basic information such as:

- Website title
- IP address
- Technologies
- Server information

---

## 2. Website Enumeration

Searches for publicly accessible resources.

Examples:

- Files
- Directories
- Web pages

---

## 3. HTML Source Code Analysis

Downloads the HTML page and extracts:

- Forms
- JavaScript
- CSS
- Comments
- Meta tags

---

## 4. HTTP/HTML Processing

Analyzes:

- HTTP status codes
- Redirects
- Response headers
- Content types

---

## 5. Server-side Technology Detection

Attempts to identify:

- Apache
- Nginx
- IIS
- PHP
- ASP.NET
- Node.js
- WordPress
- Drupal

---

## 6. Website Crawling

Recursively explores the website to discover:

- Pages
- Files
- Images
- JavaScript files
- Directories

---

## 7. Sitemap Discovery

Checks whether:

```
/sitemap.xml
```

exists and lists URLs if found.

---

## 8. Word List Extraction

Extracts commonly occurring words from the website.

Useful for:

- Directory enumeration
- Password wordlists
- Content analysis

---

## 9. Metadata Extraction

Extracts metadata such as:

- Author
- Description
- Keywords
- Generator

---

## 10. WAF Detection

Checks whether the website is protected by a Web Application Firewall.

Examples:

- Cloudflare
- AWS WAF
- Imperva
- Akamai

---

## 11. Load Balancer Detection

Attempts to determine whether the website is behind:

- AWS ELB
- HAProxy
- F5
- Nginx Load Balancer

---

## 12. HTTP Service Discovery

Discovers available HTTP services.

---

## 13. Banner Grabbing

Collects server banners.

Example:

```
Server: Apache/2.4.57

X-Powered-By: PHP/8.2
```

---

## 14. Directory Enumeration

Attempts to identify directories such as:

```
/admin
/uploads
/images
/css
/js
/login
```

---

## 15. Proxy Detection

Checks whether the website is using:

- Reverse Proxy
- Forward Proxy
- CDN
- Proxy Headers

---

# Step 8: Output Report

After all tasks are completed, the script creates:

```
Information_Gathered.txt
```

The report contains all collected information in a structured format.

Example:

```
========================================
Website Footprinting
========================================

========================================
HTML Analysis
========================================

========================================
Server Technologies
========================================

========================================
Directory Enumeration
========================================

========================================
Banner Grabbing
========================================

========================================
Metadata
========================================

========================================
WAF Detection
========================================

========================================
Load Balancer Detection
========================================
```

---

# Important Note

The lab mentions:

> **You can get different outputs when performing this lab.**

This means the generated Python script and the collected results may vary depending on:

- The AI-generated code
- The target website
- Available tools and libraries
- Current website configuration

This is expected behavior.

---

# Key Takeaways

- AI tools like ChatGPT can quickly generate automation scripts.
- The generated script automates multiple website reconnaissance tasks.
- The user only needs to provide the target URL.
- The collected information is saved in `Information_Gathered.txt`.
- This approach saves time compared to manually running each reconnaissance tool.
