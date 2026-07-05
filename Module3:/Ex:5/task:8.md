# Task 08 - Automating Network Scanning using AI-Powered Tools

## Objective

Learn how AI can be used to generate a Python script that automates **Attack Surface Mapping** by performing multiple network scanning tasks.

---

# What is Attack Surface Mapping?

Attack Surface Mapping is the process of identifying all possible entry points into a target system or network.

It includes:

* Discovering live hosts
* Identifying open ports
* Detecting running services
* Finding service versions
* Identifying the operating system

---

# AI's Role

Instead of manually writing Python code or executing multiple Nmap commands, an AI tool (e.g., Microsoft Copilot or ChatGPT) can generate a Python automation script based on a prompt.

The AI **does not perform the scan**; it generates the code that performs the scan.

---

# Workflow

```text
Provide Prompt to AI
        ↓
Generate Python Script
        ↓
Save as Attack_Surface_Mapping.py
        ↓
Run the Script
        ↓
Enter Target Network/IP
        ↓
Perform Multiple Scans
        ↓
Save Results to a Text File
```

---

# Scanning Tasks Performed

| Task                      | Purpose                                            |
| ------------------------- | -------------------------------------------------- |
| ICMP Ping Scan            | Discover live hosts                                |
| ARP Ping Scan             | Discover live hosts on the local network           |
| UDP Ping Scan             | Check host availability using UDP                  |
| TCP Ping Scan             | Check host availability using TCP                  |
| TCP Connect Scan          | Identify open TCP ports using a full TCP handshake |
| SYN (Half-Open) Scan      | Detect open ports with a stealthier scan           |
| UDP Scan                  | Identify open UDP ports                            |
| Xmas Scan                 | Identify port states using FIN, PSH, and URG flags |
| SCTP INIT Scan            | Scan SCTP services                                 |
| Service Version Detection | Identify running services and their versions       |
| OS Detection              | Identify the target operating system               |

---

# Running the Script

Execute the Python script:

```bash
python3 Attack_Surface_Mapping.py
```

When prompted, enter the target:

```text
192.168.0.0/24
```

The script performs all configured scans automatically and stores the results in a text file.

---

# Benefits of Automation

* Reduces manual effort.
* Executes multiple scans in sequence.
* Saves scan results automatically.
* Produces consistent and repeatable results.
* Speeds up the reconnaissance phase of a penetration test.

---

# Comparison

| Manual Scanning                  | AI Automation               |
| -------------------------------- | --------------------------- |
| Run each Nmap command separately | Execute one Python script   |
| Manual result collection         | Automatic result collection |
| More time-consuming              | Faster and repeatable       |

---

# Key Takeaways

* AI can generate Python scripts to automate reconnaissance tasks.
* Automation combines multiple scanning techniques into a single workflow.
* Attack Surface Mapping helps identify hosts, open ports, services, service versions, and operating systems.
* The generated script simplifies reconnaissance by executing scans sequentially and saving the results automatically.
* AI assists in code generation, while the Python script performs the actual scanning.
