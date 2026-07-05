# Task 05 - Using Metasploit Workspaces & `db_nmap`

## Objective

Learn how to use the Metasploit database to store, organize, search, and reuse Nmap scan results during a penetration test.

---

# Why Use a Database?

Normally, `nmap` only displays scan results.

```text
Nmap Scan
     ↓
Results Displayed
     ↓
Terminal Closed
     ↓
Results Lost
```

Using **Metasploit Database**:

```text
Scan
    ↓
Store Results
    ↓
Search Anytime
    ↓
Reuse in Metasploit Modules
```

This avoids rescanning the network repeatedly.

---

# PostgreSQL

Metasploit stores scan results in a **PostgreSQL** database.

Start the database:

```bash
sudo service postgresql start
```

Initialize the Metasploit database:

```bash
sudo msfdb init
```

Launch Metasploit:

```bash
sudo msfconsole
```

Verify database connection:

```bash
db_status
```

Expected output:

```text
Connected to msf
```

---

# Workspaces

A **Workspace** is like a project folder.

It keeps scan results separated for different labs or clients.

Create a workspace:

```bash
workspace -a LPT
```

Example:

```text
Workspace
│
├── Client_A
├── Client_B
├── Lab
└── Practice
```

---

# `db_nmap`

`db_nmap` is the Nmap scanner integrated into Metasploit.

Unlike normal `nmap`, it automatically stores scan results inside the Metasploit database.

### Ping Scan (Discover Live Hosts)

```bash
db_nmap -sn 192.168.0.0/24
```

### SYN Scan (Open Ports)

```bash
db_nmap -sS 192.168.0.2-70
```

### Version Detection

```bash
db_nmap -sV 192.168.0.2-70
```

### Aggressive Scan

```bash
db_nmap -A 192.168.0.2-70
```

Performs:

* OS Detection
* Version Detection
* Default NSE Scripts
* Traceroute

---

# Viewing Stored Results

Display all discovered hosts:

```bash
hosts
```

Display all discovered services:

```bash
services
```

---

# Filtering Results

Display only selected columns:

```bash
hosts -c address,os_flavor
```

Output:

```text
192.168.0.5   Linux
192.168.0.10  Windows
```

Display only Linux hosts:

```bash
hosts -c address,os_flavor -S Linux
```

Example:

```text
192.168.0.5   Linux
192.168.0.20  Linux
```

---

# Metasploit Modules

Metasploit contains different modules such as:

* Auxiliary
* Exploits
* Payloads
* Post Exploitation
* Encoders

This lab uses the TCP Port Scanner module.

Load the module:

```bash
use auxiliary/scanner/portscan/tcp
```

---

# What is RHOSTS?

**RHOSTS = Remote Hosts (Target IP Addresses)**

Every Metasploit module needs to know **which systems it should target**.

Normally, targets are added manually:

```bash
set RHOSTS 192.168.0.5
```

---

# Using `-R`

Instead of entering target IPs manually, import them directly from the database.

```bash
hosts -c address,os_flavor -S Linux -R
```

This command:

* Retrieves Linux hosts from the database.
* Copies their IP addresses into the module's **RHOSTS** option.

Example:

```text
Database

192.168.0.5
192.168.0.20

        │
        ▼
RHOSTS

192.168.0.5
192.168.0.20
```

---

# Verify Imported Targets

```bash
show options
```

Example:

```text
RHOSTS  => 192.168.0.5 192.168.0.20
PORTS   => 1-10000
THREADS => 10
```

---

# Run the Module

```bash
run
```

The module scans every target stored in **RHOSTS**.



---

# Useful Commands

| Command                      | Purpose                    |
| ---------------------------- | -------------------------- |
| `db_status`                  | Check database connection  |
| `workspace -a LPT`           | Create a new workspace     |
| `db_nmap`                    | Scan and store results     |
| `hosts`                      | View discovered hosts      |
| `services`                   | View discovered services   |
| `hosts -c address,os_flavor` | Show selected columns      |
| `hosts -S Linux`             | Filter Linux hosts         |
| `hosts -R`                   | Import hosts into `RHOSTS` |
| `show options`               | Display module settings    |
| `run`                        | Execute the loaded module  |

---

# Workflow

```text
Start PostgreSQL
        ↓
Initialize Metasploit Database
        ↓
Launch msfconsole
        ↓
Create Workspace
        ↓
Run db_nmap Scans
        ↓
Results Stored in Database
        ↓
View Hosts & Services
        ↓
Filter Required Hosts
        ↓
Import Hosts into RHOSTS
        ↓
Run Metasploit Module
```

---

# Key Takeaways

* Learned how Metasploit integrates with PostgreSQL to store scan results.
* Understood the purpose of Workspaces for organizing projects.
* Learned the difference between `nmap` and `db_nmap`.
* Used the `hosts` and `services` commands to query stored scan data.
* Filtered hosts using `-c` (columns) and `-S` (search/filter).
* Understood that **RHOSTS** contains the target IP addresses for a Metasploit module.
* Learned how `hosts -R` automatically imports target hosts from the database into `RHOSTS`.
* Executed a Metasploit auxiliary module using targets retrieved directly from the database.

---

# Interview / Revision Notes

* **PostgreSQL** stores Metasploit scan data.
* **Workspace** organizes scan results by project or client.
* **`db_nmap`** = Nmap + automatic database storage.
* **`hosts`** displays discovered systems.
* **`services`** displays discovered services.
* **`RHOSTS`** is the list of target IP addresses used by a Metasploit module.
* **`hosts -R`** imports hosts from the database into `RHOSTS`.
* After importing targets, simply execute the module using `run`.
