# Exercise 2: Identifying and Bypassing a Firewall (Revision Notes)

## Objective
Identify whether a firewall is filtering traffic and determine which ports are accessible from the attacker machine.

---

# Lab Setup

- **Attacker Machine:** Parrot Security
- **Victim Machine:** Web Server
- **Victim IP:** `172.19.19.7`

---

# Step 1: Enable Windows Firewall

**Path:**

Control Panel
→ System and Security
→ Windows Firewall
→ Turn Windows Firewall on or off
→ Turn ON Windows Firewall

### Why?

To simulate a real-world environment where the target machine is protected by a firewall.

---

# Step 2: Check Connectivity (Ping)

```bash
ping 172.19.19.7
```

### Purpose

Checks whether the target machine is reachable.

### Uses

- ICMP protocol
- Confirms the host is alive.

### Expected Result

```
64 bytes from 172.19.19.7
```

### Conclusion

- Reply received → Host is alive.
- No reply → Host may be down or ICMP may be blocked.

---

# Step 3: Perform Traceroute

```bash
traceroute 172.19.19.7
```

### Purpose

Shows the path packets take to reach the destination.

### Expected Result

No result or `* * *`

### Why?

Firewall blocks traceroute packets.

### Important

Traceroute failure **alone** does NOT prove a firewall exists.

It is only an indication.

---

# Step 4: Scan Firewall Using Nmap Firewalk

```bash
sudo nmap --script=firewalk --traceroute 172.19.19.7
```

Password

```
toor
```

---

## Purpose

Identifies

- Open ports
- Filtered ports
- Firewall behavior

---

## What Firewalk Does

Checks which packets successfully pass through the firewall.

---

## Example Output

```
PORT      STATE
21/tcp    open
80/tcp    open
135/tcp   open
139/tcp   open
445/tcp   open
3389/tcp  open
```

---

## Conclusion

- **Open** → Port is accessible.
- **Filtered** → Firewall is blocking packets.

---

# Step 5: Verify Using Hping3

```bash
sudo hping3 -S 172.19.19.7 -c 100 -p ++1
```

Password

```
toor
```

---

## Command Breakdown

### `-S`

Send TCP SYN packets.

---

### `-c 100`

Send 100 packets.

---

### `-p ++1`

Scan ports

```
1
2
3
...
100
```

---

## Purpose

Tests how the firewall responds to TCP SYN packets.

---

## Example Output

```
100 packets transmitted
2 packets received
98% packet loss
```

---

## Meaning

- 100 packets sent
- 2 packets received replies
- 98 packets received no reply

### Why?

Firewall allows only a few ports and filters the remaining ports.

---

# Important Note

Suppose Nmap shows

```
21
80
135
139
445
3389
```

as open.

Why does Hping3 receive only **2 replies**?

Because

```bash
-p ++1 -c 100
```

tests only **ports 1–100**.

Ports like

- 135
- 139
- 445
- 3389

are **outside this range**, so they are never tested.

There is **no contradiction** between Nmap and Hping3.

---

# Tool Comparison

| Tool | Purpose |
|------|---------|
| ping | Checks whether the host is alive. |
| traceroute | Finds the network path to the destination. |
| Nmap Firewalk | Finds open/filtered ports and analyzes firewall rules. |
| Hping3 | Sends custom TCP packets to verify firewall behavior. |

---

# Can We Say Firewall Exists?

Not by using only one command.

### Evidence

✔ Ping works

✔ Traceroute fails

✔ Nmap shows filtered ports

✔ Hping3 receives replies from only a few ports

Together, these indicate that the firewall is filtering traffic.

---

# Common Viva Questions

## Q1. Why do we use Ping?

To check whether the target host is reachable.

---

## Q2. Why do we use Traceroute?

To identify the path packets travel.

---

## Q3. Why use Firewalk?

To identify which ports are allowed or blocked by the firewall.

---

## Q4. Why use Hping3?

To verify firewall behavior using custom TCP SYN packets.

---

## Q5. What does "98% packet loss" mean?

Out of 100 SYN packets sent,

- only 2 received replies
- 98 were filtered or blocked.

---

## Q6. Why does Nmap show many open ports while Hping3 receives only 2 replies?

Because Hping3 tested only **ports 1–100**.

Nmap scans a much larger range of ports.

---

## Q7. Can Ping bypass a firewall?

No.

Ping only checks connectivity.

---

## Q8. Can Traceroute identify open ports?

No.

Traceroute only shows the packet path.

---

## Q9. Which command is used to identify open ports?

```
nmap --script=firewalk --traceroute <IP>
```

---

## Q10. Which command verifies firewall filtering?

```
hping3 -S <IP> -c 100 -p ++1
```

---

# Quick Revision Flow

```
Enable Firewall
        ↓
Ping
(Host Alive?)
        ↓
Traceroute
(Packet Path)
        ↓
Nmap Firewalk
(Open / Filtered Ports)
        ↓
Hping3
(Verify Firewall Filtering)
        ↓
Identify Allowed Ports
```

---

# Commands Summary

```bash
# Check connectivity
ping 172.19.19.7

# Find packet path
traceroute 172.19.19.7

# Identify firewall rules and open ports
sudo nmap --script=firewalk --traceroute 172.19.19.7

# Verify firewall filtering
sudo hping3 -S 172.19.19.7 -c 100 -p ++1
```

---

# Exam Answer (Question 7.2.1)

**Question:**
Enter the number of open ports shown in the Nmap scan.

**Answer:*8*
