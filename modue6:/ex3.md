# Exercise 3 - Scanning and Identifying Vulnerabilities in APIs

## Objective
Deploy **OWASP crAPI** and use **OWASP ZAP** to perform an **Active Scan** to identify API vulnerabilities.

---

# Step 1 - Start crAPI (Ubuntu)

Login:

```
Username: ubuntu
Password: toor
```

Become root:

```bash
sudo su
```

Check Docker Compose version:

```bash
docker-compose --version
```

Download the crAPI deployment file:

```bash
curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/main/deploy/docker/docker-compose.yml
```

Edit the configuration:

```bash
gedit docker-compose.yml
```

- Modify the **crapi-web** section (as instructed in the lab).
- Modify the **mailhog** section (as instructed in the lab).
- Save and close the file.

Start crAPI:

```bash
docker-compose -f docker-compose.yml --compatibility up -d
```

---

# Step 2 - Open OWASP ZAP (Parrot Security)

Launch **OWASP ZAP**.

During startup:
- Select **No, I do not want to persist this session**
- Click **Start**
- Close **Manage Add-ons** (if it appears)
- Click **No** (if prompted)

---

# Step 3 - Manual Explore

Click:

```
Manual Explore
```

↓

```
Launch Browser
```

Open:

```
http://192.168.0.19:8888
```

Create a new account (**Signup**) and **Login**.

Browse the application:
- Shop
- Vehicles
- Other available pages

> **Why?**  
> Exploring the application allows ZAP to discover more API endpoints before scanning.

---

# Step 4 - Include in Context

In ZAP:

```
Right Click Target
        ↓
Include in Context
        ↓
Default Context
```

Click **OK**.

> This enables **authenticated scanning**.

---

# Step 5 - Perform Active Scan

In ZAP:

```
Right Click Target
        ↓
Attack
        ↓
Active Scan
```

Verify the target IP and click:

```
Start Scan
```

---

# Step 6 - View Results

Navigate to:

```
Active Scan
```

Monitor the scanning progress.

After completion:

```
Alerts
```

Review the detected vulnerabilities.

Example finding:

- **Remote File Inclusion (RFI)**

If RFI is not detected:
- Add a vehicle.
- Interact with more pages.
- Run the scan again.

---

# Manual Verification

Always verify findings manually to avoid **False Positives**.

Try the payload used by ZAP and confirm whether the vulnerability is real.

---

# Commands Used

Become root:

```bash
sudo su
```

Check Docker Compose:

```bash
docker-compose --version
```

Download crAPI:

```bash
curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/main/deploy/docker/docker-compose.yml
```

Edit configuration:

```bash
gedit docker-compose.yml
```

Start crAPI:

```bash
docker-compose -f docker-compose.yml --compatibility up -d
```

---

# Important Concepts

- **Manual Explore** → Discover application pages and API endpoints.
- **Include in Context** → Enables authenticated scanning.
- **Active Scan** → Actively attacks the application to find vulnerabilities.
- **Alerts** → Displays detected security issues.
- **Remote File Inclusion (RFI)** → Allows loading files from remote sources.
- **False Positive** → Scanner reports a vulnerability that doesn't actually exist.

---

# Exam Question

**Q:** Specify the type of scan performed in this lab.

**Answer:** **Active Scan (OWASP ZAP Active Scan)**

---

# Quick Revision

- Deploy **crAPI** using Docker.
- Explore the application using **ZAP Browser**.
- **Include Target in Default Context**.
- Run **Attack → Active Scan**.
- Check **Alerts** for vulnerabilities.
- Manually verify findings to eliminate false positives.
