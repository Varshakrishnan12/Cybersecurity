# Task 04 - Automating Penetration Testing Using Bash Scripting

## Objective

Learn how Bash scripting can automate repetitive penetration testing tasks such as reconnaissance, service discovery, password auditing, and FTP login.

---

## What is Bash?

* Bash (Bourne Again SHell) is the default command-line shell in Linux.
* It interprets and executes Linux commands.

## What is Bash Scripting?

* A Bash script (`.sh`) is a file containing multiple Linux commands.
* Commands are executed sequentially.
* Used to automate repetitive tasks.

Example:

```bash
bash pentest.sh
```

---

## FTP (File Transfer Protocol)

* Used to transfer files between computers over a network.
* Default port: **21**
* Requires authentication using a **username** and **password**.

### FTP Port Open

If **Port 21 is open**, it means:

* An FTP service is running.
* The server is accepting FTP connections.

---

## Bash Script Workflow

```text
Input IP Range
      ↓
Find Live Hosts (Nmap)
      ↓
Identify FTP (Port 21) Open
      ↓
Display FTP Servers
      ↓
Select Target IP
      ↓
Run Hydra Dictionary Attack
      ↓
Obtain Credentials
      ↓
Login to FTP Server
```

---

## Commands Learned

### Discover Live Hosts

```bash
nmap -sn <IP_RANGE>
```

### Scan FTP Port

```bash
nmap -p 21 <TARGET_IP>
```

### Search Matching Lines

```bash
grep "Up" out.txt
```

### Extract Specific Field

```bash
cut -d " " -f2
```

### Run Hydra

```bash
hydra -L users.txt -P passwords.txt ftp://<TARGET_IP>
```

### Login to FTP

```bash
ftp <TARGET_IP>
```

---

## Hydra

Hydra performs a **dictionary attack** by trying username/password combinations from supplied wordlists.

Example:

```
admin : 123456
admin : password
john  : admin
jason : green ✅
```

> Hydra does **not** generate random usernames or passwords. It uses the wordlists provided by the tester.

---

## Dictionary Attack vs Brute Force

| Dictionary Attack             | Brute Force                        |
| ----------------------------- | ---------------------------------- |
| Uses predefined wordlists     | Tries every possible combination   |
| Faster                        | Slower                             |
| Common in penetration testing | Used when no wordlist is available |

---

## Important Commands

```bash
bash script.sh
grep
cut
nmap
hydra
ftp
```

---

# Recap

* Learned the purpose of Bash scripting and automation.
* Understood how Bash scripts execute Linux commands sequentially.
* Learned how to discover live hosts using Nmap.
* Learned to identify FTP services running on Port 21.
* Used `grep` and `cut` to process command output.
* Understood FTP authentication using usernames and passwords.
* Learned how Hydra performs dictionary attacks using wordlists.
* Understood the difference between dictionary attacks and brute-force attacks.
* Learned how multiple penetration testing tasks can be automated using a single Bash script.
