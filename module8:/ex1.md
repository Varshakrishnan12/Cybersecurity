# Exercise 1: Enumerating SMB 

## Objective

The goal of this lab is to identify whether the target system is running the SMB (Server Message Block) service and enumerate information exposed through the SMB protocol.

---

# What is SMB?

**SMB (Server Message Block)** is a network protocol mainly used by Windows systems for:

- File sharing
- Printer sharing
- Folder sharing
- Communication between computers in a Windows network

SMB commonly runs on:

- TCP Port **445** (Modern SMB)
- TCP Port **139** (Older NetBIOS over SMB)

---

# Lab Workflow

```
Target IP (192.168.0.7)
        │
        ▼
Check if SMB service is running
        │
        ▼
Port 445/139 Open?
        │
      Yes
        │
        ▼
Run SMB Enumeration
        │
        ▼
Collect Information
        │
 ├── Computer Name
 ├── Operating System
 ├── DNS Domain Name
 ├── Workgroup/Domain
 ├── FQDN
 └── SMB Details
```

---

# Step 1: Check SMB Information

Run:

```bash
nmap --script smb-os-discovery 192.168.0.7
```

### Purpose

Runs the **smb-os-discovery** NSE script to collect SMB information from the target.

Possible information:

- Operating System
- Computer Name
- DNS Domain Name
- Workgroup
- FQDN
- System Time

---

# Step 2: Run Default NSE Scripts

```bash
nmap -sC 192.168.0.7
```

### Purpose

Runs all default Nmap scripts.

This may enumerate:

- SMB
- HTTP
- FTP
- SSH
- DNS
- Other available services

The scan may take several minutes.

---

# Step 3: Verify SMB is Running

Look for:

```
139/tcp open netbios-ssn

445/tcp open microsoft-ds
```

If Port **445** is open, SMB is running.

---

# Step 4: Debug Scan

```bash
nmap -d --script smb-os-discovery 192.168.0.7
```

### Purpose

Shows debug information including:

- SMB negotiation
- Session setup
- Login attempts
- Error messages

Example:

```
STATUS_ACCESS_DENIED
```

This is normal because many modern Windows systems block anonymous SMB login.

---

# Step 5: Save XML Report

```bash
nmap -oX smbscan.xml --script smb-os-discovery 192.168.0.7
```

### Purpose

Saves scan results in XML format for documentation or GUI tools.

---

# Finding the DNS Domain Name

Look for output similar to:

```
Host script results:

Computer name: SERVER01

DNS Domain Name: company.local

FQDN: server01.company.local
```

The value after **DNS Domain Name** is the answer.

Example:

```
DNS Domain Name: company.local
```

Answer:

```
company.local
```

---

# Why do we enumerate SMB?

SMB exposes useful information about a Windows system.

By querying the SMB service, a penetration tester may discover:

- Computer Name
- Windows Version
- Domain Name
- DNS Domain Name
- Workgroup
- Fully Qualified Domain Name (FQDN)

This information helps understand the target environment before attempting further security assessments.

---

# Commands Summary

```bash
nmap --script smb-os-discovery 192.168.0.7
```

Identify operating system and SMB details.

---

```bash
nmap -sC 192.168.0.7
```

Run default NSE scripts.

---

```bash
nmap -d --script smb-os-discovery 192.168.0.7
```

Run SMB discovery with debug output.

---

```bash
nmap -oX smbscan.xml --script smb-os-discovery 192.168.0.7
```

Save results as an XML report.

---

# Key Takeaways

- SMB uses TCP ports **445** and **139**.
- First, verify whether SMB is running.
- If SMB is available, enumerate information using Nmap NSE scripts.
- SMB enumeration may reveal:
  - Computer Name
  - Operating System
  - DNS Domain Name
  - Workgroup
  - FQDN
- The **DNS Domain Name** is retrieved directly from the SMB service if it is exposed.
- Enumeration is an **information gathering** activity performed before exploitation.
