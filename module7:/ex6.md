# Exercise 6 – Performing WAF Fingerprinting (Revision)

## Objective
Detect the Web Application Firewall (WAF) protecting a website.

---

## Tools Used

- Nmap
- WhatWaf

---

## Step 1: Become Root

```bash
sudo su
```

Password:

```
toor
```

**Why?**

Some security tools require root privileges.

---

## Step 2: Detect WAF using Nmap

```bash
nmap --script http-waf-detect certifiedhacker.com
```

**Purpose**

- Detect WAF
- Detect IDS/IPS

**Example Output**

```
WAF Detected
IDS/IPS Detected
```

---

## Step 3: Identify WAF using WhatWaf

```bash
whatwaf -u http://certifiedhacker.com
```

**Purpose**

Identify the WAF vendor.

**Example Output**

```
ModSecurity
Open Source WAF
```

---

# Command Breakdown

### Nmap

Network scanner.

### http-waf-detect

Nmap script to detect WAF and IDS/IPS.

### WhatWaf

Tool used to fingerprint WAF.

### -u

Specifies the target URL.

---

# Tool Difference

| Nmap | WhatWaf |
|------|----------|
| Detects WAF/IDS | Identifies WAF vendor |
| General scanner | Dedicated WAF fingerprinting tool |

---

# Lab Flow

```
Target Website
      ↓
Nmap Scan
      ↓
Detect WAF/IDS
      ↓
WhatWaf Scan
      ↓
Identify WAF Vendor
```

---

# Commands

```bash
sudo su

nmap --script http-waf-detect certifiedhacker.com

whatwaf -u http://certifiedhacker.com
```

---

# Viva Questions

### What is WAF?

Protects web applications from attacks like SQL Injection and XSS.

### Why use Nmap?

To detect WAF and IDS/IPS.

### Why use WhatWaf?

To identify the WAF vendor.

### What does `-u` mean?

Specifies the target URL.

---

# Exam Answer

**Question 7.6.1**

**Option used to specify URL in WhatWaf:**

```
-u
```
````
