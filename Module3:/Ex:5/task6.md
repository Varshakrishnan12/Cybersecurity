# Task 06 - Scanning & Building a Target Database

## Objective

Learn how to organize scan results into a **Target Database** for vulnerability analysis, exploitation, and final reporting.

---

# Scanning Methodology

Perform scans in the following order:

| Scan                | Command                              | Purpose                                                  |
| ------------------- | ------------------------------------ | -------------------------------------------------------- |
| Live Host Discovery | `nmap -sn <target>` *(Older: `-sP`)* | Identify live hosts                                      |
| SYN Scan            | `nmap -sS <target>`                  | Discover open ports                                      |
| Version Detection   | `nmap -sV <target>`                  | Identify service versions                                |
| Aggressive Scan     | `nmap -A <target>`                   | OS detection, version detection, NSE scripts, traceroute |

---

# Export Scan Results

Save scan results in XML format:

```bash
nmap -A <target> -oX scan.xml
```

Convert XML to an HTML report:

```bash
xsltproc -o ~/scanresults.html /usr/share/nmap/nmap.xsl scan.xml
```

**Why XML?**

* Structured and machine-readable.
* Easy to import into tools.
* Can be converted into HTML for better readability.

---

# Target Database

A Target Database organizes scan results and helps prioritize targets during a penetration test.

## Suggested Fields

| Field           | Description                           |
| --------------- | ------------------------------------- |
| Host/IP         | Target IP address or hostname         |
| OS              | Operating system                      |
| Open Ports      | Relevant open ports                   |
| Services        | Service name and version              |
| Vulnerabilities | Identified CVEs or weaknesses         |
| Exploit         | Related exploit/module (if available) |
| Notes           | Additional observations               |
| Priority        | High / Medium / Low                   |

---

# Example

| Host/IP      | OS      | Open Ports | Services        | Priority |
| ------------ | ------- | ---------- | --------------- | -------- |
| 192.168.0.5  | Linux   | 22,80      | SSH, Apache 2.4 | Medium   |
| 192.168.0.20 | Windows | 445        | SMB             | High     |

---

# Why Build a Target Database?

* Organizes scan results.
* Avoids reviewing raw Nmap output repeatedly.
* Helps prioritize high-value targets.
* Maps vulnerabilities to possible exploits.
* Serves as the foundation for the final penetration testing report.

---

# Workflow

```text
Discover Live Hosts
        ↓
Scan Open Ports
        ↓
Identify Services
        ↓
Enumerate Target
        ↓
Export Scan Results (XML)
        ↓
Convert to HTML
        ↓
Analyze Findings
        ↓
Create Target Database
        ↓
Prioritize Targets
        ↓
Exploit & Report
```

---

# Key Takeaways

* Follow a structured scanning methodology (`-sn` → `-sS` → `-sV` → `-A`).
* Save scan results in **XML** for easy processing.
* Convert XML reports to **HTML** for better readability.
* Maintain a **Target Database** instead of relying on raw scan output.
* Use the database to prioritize targets, track vulnerabilities, and support the final penetration testing report.
