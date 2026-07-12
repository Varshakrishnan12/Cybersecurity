# Exercise 6 - Exploiting Broken Object Level Authorization (BOLA)

## Objective
Learn how to identify and exploit a **Broken Object Level Authorization (BOLA)** vulnerability in **OWASP crAPI** using **Burp Suite**.

---

# What is BOLA?

**BOLA (Broken Object Level Authorization)** occurs when an API does **not verify whether the logged-in user is authorized to access a specific object (ID)**.

Example:

```
User A → Vehicle ID = 101
User B → Vehicle ID = 202
```

If User A changes the request from:

```
/vehicle/101
```

to

```
/vehicle/202
```

and receives User B's data, the API is vulnerable to **BOLA**.

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

- Modify **Line 157** (as instructed in the lab).
- Modify **Mailhog (Line 237)** (as instructed in the lab).

Save and close.

Start crAPI:

```bash
sudo docker-compose -f docker-compose.yml --compatibility up -d
```

---

# Step 2 - Create a User

On **Parrot Security**, open Firefox.

Visit:

```
http://192.168.0.19:8888
```

- Sign Up
- Login

---

# Step 3 - Add a Vehicle

Click:

```
+ Add Vehicle
```

To obtain the **VIN** and **PIN**, open Mailhog:

```
http://192.168.0.19:8025
```

Open the latest email and copy:

- VIN
- PIN

Return to crAPI and verify the vehicle.

---

# Step 4 - Launch Burp Suite

Open:

```
Applications
→ Pentesting
→ Web Application Analysis
→ Burp Suite CE
```

- Temporary Project
- Use Burp Defaults
- Start Burp

Go to:

```
Proxy
```

Turn:

```
Intercept OFF
```

Click:

```
Open Browser
```

Login to crAPI again.

---

# Step 5 - Capture Vehicle Request

In Burp:

```
Proxy
    ↓
HTTP History
```

Find the request similar to:

```
GET /identity/api/v2/vehicle/vehicles
```

This request contains your **Vehicle ID**.

---

# Step 6 - Obtain Another User's Vehicle ID

In crAPI:

```
Community
```

↓

Open any post.

Switch to Burp.

In the **Response**, locate another user's:

```
VehicleID
```

Copy it.

---

# Step 7 - Exploit BOLA

Right-click the vehicle request:

```
Send to Repeater
```

In Repeater:

Replace

```
Your Vehicle ID
```

with

```
Other User's Vehicle ID
```

Click:

```
Send
```

---

# Result

If the API returns another user's vehicle information (e.g., location/details), the application is vulnerable to **BOLA**.

---

# Impact of BOLA

- Unauthorized access to other users' data.
- Data leakage.
- Data manipulation.
- Privacy violation.
- Account compromise.

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
sudo docker-compose -f docker-compose.yml --compatibility up -d
```

---

# Important URLs

crAPI:

```
http://192.168.0.19:8888
```

Mailhog:

```
http://192.168.0.19:8025
```

---

# Exam Question

**Q:** Enter the IP address used to open the Mailhog service.

**Answer:**

```
192.168.0.19
```

(Mailhog URL: `http://192.168.0.19:8025`)

---

# Quick Revision

- **BOLA** → Access another user's object by changing its ID.
- Create a new user and add a vehicle.
- Retrieve **VIN** and **PIN** from **Mailhog**.
- Capture requests using **Burp Suite**.
- Copy another user's **Vehicle ID** from the Community response.
- Replace your Vehicle ID with another user's ID in **Repeater**.
- If another user's data is returned, the API is vulnerable to **BOLA**.
