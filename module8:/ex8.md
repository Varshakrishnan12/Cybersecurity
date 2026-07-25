# Exercise 8: Abusing UAC High Integrity Level

## Objective

The objective of this lab is to understand how **User Account Control (UAC)** separates administrator privileges using Integrity Levels and how some trusted Microsoft applications automatically run with **High Integrity** without displaying a UAC prompt.

This exercise demonstrates:

- How UAC works internally
- Difference between Medium and High Integrity processes
- Access Tokens and Privileges
- Auto-Elevated Windows applications
- Observing High Integrity processes using Process Hacker
- Answering the lab question regarding mitigation policies (DEP)

---

# What is UAC?

**User Account Control (UAC)** is a Windows security feature that prevents applications from automatically obtaining administrator privileges.

Even if you log in as an Administrator, Windows does **not** use administrator privileges all the time.

Instead, Windows creates **two access tokens**.

---

# Administrator Login Process

When an Administrator logs in:

```
Administrator Login
        │
        ▼
Windows creates two access tokens
        │
        ├──────────────► Filtered Token (Medium Integrity)
        │
        └──────────────► Elevated Token (High Integrity)
```

### Filtered Token

- Used for normal work
- Medium Integrity
- Administrator privileges removed
- Used until UAC approval

### Elevated Token

- Full administrator privileges
- High Integrity
- Used after clicking **Run as Administrator**

---

# Non-Administrator Login

A standard user receives only one token.

```
User Login

↓

Medium Integrity Token

↓

Used for every process
```

---

# UAC Integrity Levels

Windows uses four Integrity Levels.

| Integrity Level | Description |
|-----------------|-------------|
| Low | Internet/browser sandbox, highly restricted |
| Medium | Standard users and filtered Administrator token |
| High | Elevated Administrator token |
| System | Windows system services |

---

# Step 1 - Login

Login to the Sales Department machine.

```
Username : Admin
Password : test@123
```

---

# Step 2 - Open Two PowerShell Windows

## Normal PowerShell

```
Windows Search

↓

PowerShell

↓

Enter
```

Runs with

```
Medium Integrity
```

---

## Administrator PowerShell

```
Windows Search

↓

PowerShell

↓

Right Click

↓

Run as Administrator
```

Accept the UAC prompt.

Runs with

```
High Integrity
```

---

# Step 3 - Open Process Hacker

Search

```
Process Hacker 2
```

Purpose:

- View running processes
- Check Integrity Levels
- View Access Tokens
- View Security Privileges
- View Mitigation Policies

---

# Step 4 - Compare Both PowerShell Processes

Locate

```
powershell.exe
```

There will be two processes.

One:

```
Medium Integrity
```

Another:

```
High Integrity
```

Open

```
Right Click

↓

Properties
```

Observe the differences.

---

# Medium PowerShell

Integrity

```
Medium
```

Privileges

```
Limited
```

Uses

```
Filtered Administrator Token
```

---

# Administrator PowerShell

Integrity

```
High
```

Privileges

```
Administrator Token
```

Contains additional privileges such as

```
SeDebugPrivilege
```

---

# Step 5 - Mitigation Policies

Inside Process Hacker Properties observe

```
Mitigation Policies
```

Examples

- DEP
- ASLR
- CFG

Observation in the lab:

Medium Integrity processes have additional mitigation protections enabled.

Administrator High Integrity processes do **not** enforce some mitigation policies.

One important observation:

```
DEP = False
```

This is the answer expected in the lab.

---

# Lab Question Answer

**Question**

For Administrator users, determine whether mitigation policies such as Data Execution Prevention (DEP) are enforced.

**Answer**

```
False
```

---

# Step 6 - Token Tab

Open

```
Properties

↓

Token
```

Compare both PowerShell processes.

---

## Medium Integrity Token

Administrator privileges

```
Disabled
```

No elevated permissions.

---

## High Integrity Token

Administrator privileges

```
Enabled
```

Several security privileges become active.

---

# Important Privilege

```
SeDebugPrivilege
```

Meaning

Allows debugging and inspection of other processes.

Typical legitimate uses:

- Debugging applications
- Troubleshooting
- Memory inspection
- System diagnostics

Because of its power, it is only available to elevated administrator processes.

---

# UAC Notification Levels

Windows supports four UAC settings.

---

## 1. Always Notify

Prompt appears

- Programs making changes
- Windows settings

Most secure option.

---

## 2. Notify Only When Programs Make Changes (Default)

Prompt appears only when applications request administrator privileges.

Windows settings by administrators usually do not trigger prompts.

---

## 3. Notify Without Secure Desktop

Same as above.

Desktop is not dimmed.

Less secure.

---

## 4. Never Notify

No UAC prompts.

Administrator always uses

```
High Integrity
```

Not recommended.

---

# Step 7 - Verify Medium Integrity

In the normal PowerShell

Run

```
net user test test /add
```

Result

```
Access Denied
```

Reason

Current process is running with

```
Medium Integrity
```

Verify by executing

```
whoami /groups
```

Observe

```
Mandatory Label

Medium Mandatory Level
```

This confirms the process is not elevated.

---

# Step 8 - Open msconfig

Run

```
msconfig
```

Observation

No UAC prompt appears.

Reason

```
Auto Elevation
```

---

# Auto Elevation

Certain trusted Microsoft applications are automatically launched with High Integrity.

Examples include

- msconfig
- mmc
- taskmgr
- compmgmt

These applications are digitally signed by Microsoft.

---

# Verify Using Process Hacker

Locate

```
msconfig.exe
```

Open

```
Properties

↓

Token
```

Observe

```
Integrity Level

↓

High
```

No UAC prompt was shown.

---

# Tools Tab

Inside

```
msconfig

↓

Tools
```

Choose

```
Command Prompt

↓

Launch
```

Observation

A Command Prompt opens with

```
High Integrity
```

without another UAC prompt because msconfig itself is already elevated.

---

# Step 9 - Explore azman.msc

Open

```
Run

↓

azman.msc
```

Authorization Manager opens.

It runs inside

```
MMC
```

---

# Verify Using Process Hacker

Locate

```
mmc.exe
```

Open

```
Properties

↓

Token
```

Observe

```
High Integrity
```

---

# Open Help

Inside Authorization Manager

```
Help

↓

Help Topics
```

The MMC Help window opens.

---

# View Source

Inside Help window

```
Right Click

↓

View Source
```

Observation

An XML file opens in

```
Notepad
```

The Notepad process also inherits

```
High Integrity
```

---

# Overall Learning

This lab demonstrates:

- How UAC separates administrator privileges
- Difference between Medium and High Integrity
- Administrator receives two access tokens
- Standard users receive one token
- High Integrity processes have additional administrator privileges
- SeDebugPrivilege becomes available after elevation
- Some Microsoft applications automatically run with High Integrity (Auto Elevation)
- Process Hacker can be used to inspect Integrity Levels, Tokens, Privileges, and Mitigation Policies

---

# Cheat Sheet

## Check Current Integrity

```powershell
whoami /groups
```

Look for

```
Medium Mandatory Level

or

High Mandatory Level
```

---

## Create Test User

```powershell
net user test test /add
```

Fails if running with Medium Integrity.

---

## Open Normal PowerShell

```
PowerShell
```

Integrity

```
Medium
```

---

## Open Elevated PowerShell

```
PowerShell

↓

Run as Administrator
```

Integrity

```
High
```

---

## Open Process Hacker

```
Process Hacker 2
```

Check

- Integrity Level
- Token
- Privileges
- Mitigation Policies

---

## Important Privilege

```
SeDebugPrivilege
```

Allows debugging and inspection of other processes.

---

## Auto Elevated Applications

```
msconfig.exe

mmc.exe

taskmgr.exe

compmgmt.msc
```

---

## UAC Integrity Levels

```
Low

↓

Medium

↓

High

↓

System
```

---

## Lab Question Answer

**Question**

Are mitigation policies such as DEP enforced for Administrator High Integrity processes?

**Answer**

```
False
```

---

# Key Interview Points

**Q1. What is UAC?**

User Account Control is a Windows security feature that prevents applications from automatically gaining administrator privileges.

---

**Q2. Why does an Administrator receive two tokens?**

To perform daily tasks with limited privileges (Medium Integrity) while using the elevated token (High Integrity) only when required.

---

**Q3. What is High Integrity?**

A security level assigned to elevated administrator processes after UAC approval.

---

**Q4. What is SeDebugPrivilege?**

A powerful administrator privilege that allows debugging and inspecting other processes.

---

**Q5. What is Auto Elevation?**

A Windows feature where trusted Microsoft applications automatically run with High Integrity without showing an additional UAC prompt.

---

**Q6. What tool is used to inspect Integrity Levels in this lab?**

```
Process Hacker
```

---

**Q7. What is the expected answer to the lab question?**

```
False
```
