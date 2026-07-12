# Exercise 9 - Exploiting XPath Injection Vulnerabilities

## Objective

Exploit an **XPath Injection** vulnerability in DVWS by modifying an API request and using an **encoded XPath payload** to retrieve sensitive information.

---

# What is XPath Injection?

XPath is used to query **XML** data, similar to how SQL queries databases.

Example:

```text
//user[username='admin']
```

If user input is not validated, an attacker can inject XPath expressions to bypass filters or extract data.

---

# Overall Workflow

```text
Open DVWS
      │
      ▼
Login/Register
      │
      ▼
Capture API Request
(/api/v2/release/0.0.1)
      │
      ▼
Send to Repeater
      │
      ▼
Modify URL
(Add XPath Payload)
      │
      ▼
Encode Payload (Hex)
      │
      ▼
Send Request
      │
      ▼
Server Returns Sensitive Data
(User Credentials)
```

---

# Step 1 - Open Burp Browser

Login:

```
Username : attacker
Password : toor
```

Launch:

```
Burp Suite
```

Go to:

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

# Step 2 - Open DVWS

Visit:

```
http://192.168.0.19:8080
```

If you don't have an account:

- Register
- Login

---

# Step 3 - Forward Requests

Switch to Burp.

Click

```
Forward
```

until all requests are completed.

---

# Step 4 - Capture API Request

Go to:

```
Proxy
```

↓

```
HTTP History
```

Locate:

```
GET /api/v2/release/0.0.1
```

Right Click →

```
Send to Repeater
```

---

# Step 5 - Modify the Request

Original value:

```text
0.0.1
```

Replace it with the XPath Injection payload:

```text
0.0.1' or 1=1 or 'a'='a
```

---

# Step 6 - Encode the Payload

The lab requires the payload to be **Hex Encoded**.

Encode the payload exactly as demonstrated in the lab screenshot.

Replace the original value with the **encoded payload**.

> **Note:** Use the encoded payload shown in the lab image.

---

# Step 7 - Send the Request

Click:

```
Send
```

---

# Step 8 - Observe the Response

The server returns:

- Configuration contents
- User credentials
- Root user information

This confirms a successful XPath Injection.

---

# Important Payload

Original:

```text
0.0.1
```

Injected:

```text
0.0.1' or 1=1 or 'a'='a
```

---

# Exam Question

**Q:** Input the encoded payload as demonstrated in the lab.

**Answer:**

Use the **Hex Encoded version** of:

```text
0.0.1' or 1=1 or 'a'='a
```

exactly as shown in the lab screenshot.

---

# Quick Revision

- Open DVWS.
- Login.
- Capture `GET /api/v2/release/0.0.1`.
- Send to Repeater.
- Replace `0.0.1` with `0.0.1' or 1=1 or 'a'='a`.
- Hex encode the payload.
- Send the request.
- Retrieve configuration and user credentials.
