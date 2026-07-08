# Exercise 9: Web Application Vulnerability Assessment using OWASP ZAP

## Objective

Learn how to:

- Perform an automated vulnerability scan using **OWASP ZAP**
- Identify web application vulnerabilities
- Find the vulnerable parameter
- Save the POST request for SQL Injection testing in the next lab

---

# What is OWASP ZAP?

**OWASP ZAP (Zed Attack Proxy)** is an automated web application security scanner.

It automatically scans a website for common vulnerabilities such as:

- SQL Injection
- Cross-Site Scripting (XSS)
- CSRF
- Security Misconfigurations
- Information Disclosure

---

# Why use ZAP?

Instead of manually checking every page,

**ZAP automatically:**

- Crawls the website
- Finds pages and forms
- Sends test payloads
- Detects vulnerabilities
- Generates a security report

---

# Lab Steps

## Step 1: Start ZAP

Open Terminal

```bash
sudo su
zaproxy
```

---

## Step 2: Create a New Session

Choose:

```
No, I do not want to persist this session
```

Click **Start**.

---

## Step 3: Start Automated Scan

Go to:

```
Quick Start
        ↓
Automated Scan
```

Enter the target:

```text
www.luxurytreats.com
```

Click **Attack**.

---

## Step 4: ZAP Scans the Website

ZAP automatically:

- Crawls the website
- Finds pages
- Tests input fields
- Sends attack payloads
- Looks for vulnerabilities

---

## Step 5: View Alerts

Open:

```
Alerts
```

Risk levels:

| Risk | Meaning |
|------|---------|
| High | Critical vulnerability |
| Medium | Moderate vulnerability |
| Low | Minor issue |
| Informational | Information only |

Focus on **High** risk alerts.

---

## Step 6: Review Vulnerabilities

The scan identifies:

- Persistent Cross-Site Scripting (XSS)
- SQL Injection

Expand each alert to view:

- Vulnerable page
- Vulnerable parameter
- Attack payload
- Evidence

---

## Step 7: View SQL Injection Request

Select the **SQL Injection** alert.

Click:

```
Request
```

You will see the HTTP POST request that triggered the vulnerability.

Example:

```http
POST /ContactUs.aspx

email=test@test.com
comment=Hello
```

---

## Step 8: Save the POST Request

Copy the **POST request body**.

Create a file:

```text
post-request-body.txt
```

Paste the copied request into the file.

### Why save it?

The next lab uses this exact request to perform SQL Injection testing.

Instead of recreating the request manually, we simply reuse the saved request.

---

## Step 9: Modify the Vulnerable Parameter

Original:

```text
email=test@test.com
```

Replace it with:

```text
email=ZAP%27
```

`%27` is the URL-encoded value for a single quote (`'`).

A single quote is commonly used to test for SQL Injection.

---

# Complete Flow

```text
Start ZAP
      ↓
Automated Scan
      ↓
Enter Target URL
      ↓
Attack
      ↓
Website Crawling
      ↓
Find Vulnerabilities
      ↓
Open Alerts
      ↓
Select SQL Injection
      ↓
Copy POST Request
      ↓
Save as post-request-body.txt
      ↓
Modify email = ZAP%27
      ↓
Ready for Next Lab
```

---

# Important Commands

Start ZAP

```bash
sudo su
zaproxy
```

Target Website

```text
www.luxurytreats.com
```

SQL Injection Test Payload

```text
ZAP'
```

URL Encoded

```text
ZAP%27
```

---

# Key Concepts

| Term | Description |
|------|-------------|
| OWASP ZAP | Automated Web Application Vulnerability Scanner |
| Automated Scan | Automatically scans a website for vulnerabilities |
| SQL Injection | Injection attack against a database |
| Persistent XSS | Stored JavaScript execution vulnerability |
| POST Request Body | Data sent from the client to the server |
| ZAP%27 | URL-encoded SQL Injection test payload (`ZAP'`) |

---

# Exam Points

- Use **Automated Scan** in OWASP ZAP.
- Scan **www.luxurytreats.com**.
- Focus on **High Risk** alerts.
- Copy the **POST request body** of the SQL Injection request.
- Save it as **post-request-body.txt**.
- Replace the vulnerable parameter value with **`ZAP%27`**.
- **Risk Rating of SQL Injection:** **High**.
