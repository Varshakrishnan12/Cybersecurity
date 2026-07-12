# Exercise 8 - Exploiting Hidden API Functionality Exposure

## Objective

Identify hidden/undocumented API functionality in **DVWS** and gather additional information about the application using **Swagger UI** and **Burp Suite**.

---

# Overall Workflow

```text
Start DVWS
      │
      ▼
Open Swagger UI (/api-docs)
      │
      ▼
Execute /api/v1/info
      │
      ▼
Capture GET Request
      │
      ▼
Send Request to Repeater
      │
      ▼
Configure Target
(Host & Port)
      │
      ▼
Modify GET Request
(As shown in the lab)
      │
      ▼
Send Request
      │
      ▼
Receive Application Information
      │
      ▼
Record Content-Length
```

---

# Step 1 - Start DVWS (Ubuntu)

Login

```
Username : ubuntu
Password : toor
```

Become root:

```bash
sudo su
```

Move to DVWS directory:

```bash
cd dvws-node
```

Start DVWS:

```bash
docker-compose up
```

---

# Step 2 - Open Burp Suite (Parrot)

Open:

```
Burp Suite
```

Go to

```
Proxy
```

↓

Turn

```
Intercept OFF
```

↓

Click

```
Open Browser
```

---

# Step 3 - Open Swagger Documentation

In Burp Browser open:

```
http://192.168.0.19:8080/api-docs/
```

Swagger UI opens.

---

# Step 4 - Forward Requests

Switch to Burp.

Click

```
Forward
```

until all requests are completed.

---

# Step 5 - Execute API

In Swagger,

Locate

```
GET /api/v1/info
```

Click

```
Try it out
```

↓

Click

```
Execute
```

This generates a GET request.

---

# Step 6 - Forward Again

Switch to Burp.

Click

```
Forward
```

until every request is completed.

---

# Step 7 - Capture the GET Request

Go to

```
Proxy
```

↓

```
HTTP History
```

Locate the request:

```
GET /api/v1/info
```

Right Click →

```
Send to Repeater
```

---

# Step 8 - Configure Target

Go to

```
Repeater
```

Click the **Pencil Icon** beside **Target**.

Configure:

```
Host : 192.168.0.19

Port : 8080
```

Click

```
OK
```

---

# Step 9 - Modify the Request

In the **Raw Request** section,

Delete the existing request.

Paste the **modified GET request exactly as shown in the lab screenshot**.

> **Note:** Do not create your own request. Use the request provided in the lab screenshot.

---

# Step 10 - Send Request

Click

```
Send
```

The server returns detailed information about the application.

---

# Step 11 - Record Content-Length

Look at the **modified request**.

Find:

```http
Content-Length: <value>
```

Record that value.

This is the answer for the lab question.

---

# Commands Used

Become root:

```bash
sudo su
```

Move to DVWS:

```bash
cd dvws-node
```

Start DVWS:

```bash
docker-compose up
```

---

# Important URLs

Swagger UI

```
http://192.168.0.19:8080/api-docs/
```

API Used

```
GET /api/v1/info
```

---

# Tanglish Explanation

### Step 1

DVWS application start panrom.

---

### Step 2

Burp browser open panrom.

---

### Step 3

Swagger UI open panrom.

Purpose:

```
Application expose panniruka APIs paaka.
```

---

### Step 4

`/api/v1/info`

execute panrom.

Purpose:

```
GET request generate aaganum.
```

---

### Step 5

Burp HTTP History la

```
GET /api/v1/info
```

capture aagum.

---

### Step 6

Request ah Repeater ku anuprom.

Reason:

```
Request modify panna.
```

---

### Step 7

Target Host

```
192.168.0.19
```

Port

```
8080
```

set panrom.

---

### Step 8

Current request delete pannitu,

**Lab screenshot la kudutha modified GET request ah exactly paste panrom.**

---

### Step 9

Send click panrom.

Server application pathi extra information return pannum.

---

### Step 10

Modified request la iruka

```
Content-Length
```

value note pannrom.

---

# Exam Question

**Q:** Record the Content-Length of the modified request.

**Answer:**

Look at the modified request:

```http
Content-Length: 21654
```

The number after **Content-Length** is the answer.

---

# Quick Revision

- Start DVWS.
- Open Burp Browser.
- Open Swagger (`/api-docs`).
- Execute `GET /api/v1/info`.
- Capture request in HTTP History.
- Send to Repeater.
- Configure Host = `192.168.0.19`, Port = `8080`.
- Replace request with the modified GET request shown in the lab.
- Click **Send**.
- Record the **Content-Length** value.
