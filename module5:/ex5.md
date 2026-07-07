````md
# Exercise 5: Exploiting Broken Object-Level Authorization (BOLA) – OWASP Juice Shop

## Objective
Learn how to identify and exploit a **Broken Object-Level Authorization (BOLA)** vulnerability using **Burp Suite**.

---

# What is BOLA?

**Broken Object-Level Authorization (BOLA)** occurs when an application **does not verify whether the logged-in user is authorized to access a specific object**.

The application trusts the object ID sent by the client.

Example:

```
GET /rest/basket/1
```

If changing it to

```
GET /rest/basket/2
```

returns another user's basket, then the application is vulnerable.

---

# Simple Example

Imagine Gmail.

```
https://mail.com/message/123
```

If you change

```
123 → 124
```

and you can read another person's email,

that is **Broken Object-Level Authorization**.

The server should verify

> "Does this message belong to the logged-in user?"

Instead it only checks

> "Does message 124 exist?"

This is BOLA.

---

# Lab Architecture

Client

```
Browser
```

↓

Frontend

```
Angular
```

↓

Backend

```
Node.js + Express
```

↓

REST API

```
/rest/*
```

↓

Database

```
SQLite
```

---

# Lab Workflow

## Step 1 — Start Juice Shop

On Ubuntu

```bash
sudo docker run -d -p 80:3000 bkimminich/juice-shop
```

If port 80 is occupied

```bash
sudo service apache2 stop
```

Verify

```bash
docker ps
```

---

## Step 2 — Open Juice Shop

From Parrot browser

```
http://192.168.0.19
```

Dismiss the warning message.

---

## Step 3 — Create Account

Go to

```
Account
    ↓
Login
    ↓
Not yet a customer?
```

Register

Login

Logout

---

# Why logout?

The next objective is to capture the login request using Burp Suite.

---

# Step 4 — Start Burp Suite

Applications

```
Pentesting

    Web Application Analysis

        BurpSuite CE
```

Use

- Temporary Project
- Burp Defaults

Start Burp

---

# Step 5 — Open Burp Browser

Proxy

```
Open Browser
```

Visit

```
http://192.168.0.19
```

---

# Step 6 — Observe HTTP Requests

Turn

```
Intercept ON
```

Every request will stop inside Burp before reaching the server.

Forward them.

Notice:

Every click generates HTTP requests.

Example

```
GET /
GET /rest/products
GET /assets
POST /rest/user/login
```

---

# Step 7 — Capture Login Request

Turn Intercept ON.

Login using your account.

Burp captures

```
POST /rest/user/login
```

Forward until login completes.

---

# Observation

The login request is visible in plaintext.

You can inspect

- headers
- body
- cookies
- JWT token

Burp acts as a man-in-the-middle proxy.

---

# Step 8 — Add Products

Turn interception OFF.

Reload page.

Turn interception ON again.

Add

- Product 1
- Product 2
- Product 3

Forward every request.

Observe

```
POST /api/BasketItems
```

and

```
GET /rest/basket
```

---

# Step 9 — View HTTP History

Go to

```
Proxy

↓

HTTP History
```

Search for

```
GET /rest/basket
```

Status

```
200 OK
```

Right Click

```
Send to Repeater
```

---

# Why Repeater?

Repeater allows sending the same request repeatedly after modifying it.

Perfect for testing authorization.

---

# Step 10 — Send Request

Go to

```
Repeater
```

Click

```
Send
```

Response

```
Your basket
```

---

# Step 11 — Modify Basket ID

Original request

```
GET /rest/basket/1
```

Change

```
1
```

to

```
2
```

Click

```
Send
```

---

# Result

Instead of

```
403 Forbidden
```

or

```
401 Unauthorized
```

the server returns

```
Basket of User 2
```

You successfully accessed another user's basket.

---

# Why did it work?

Because the server trusted the URL.

It only checked

```
Does basket 2 exist?
```

It never checked

```
Does basket 2 belong to this logged-in user?
```

Missing authorization check = BOLA.

---

# Expected Secure Behaviour

When requesting

```
GET /rest/basket/2
```

Server should verify

```
Current User
```

↓

Owns

```
Basket 2?
```

If

NO

↓

Return

```
403 Forbidden
```

---

# Vulnerable Flow

```
User Login
      │
      ▼
Requests

/rest/basket/1

      │

Change URL

/rest/basket/2

      │

Server only checks

Basket exists?

      │

Returns basket

❌ Authorization NOT checked
```

---

# Secure Flow

```
User Login
      │
      ▼

Requests

/rest/basket/2

      │

Server checks

Does basket belong to user?

      │

YES ───► Return basket

NO ───► 403 Forbidden
```

---

# Burp Suite Components Used

### Proxy

Captures requests.

---

### Intercept

Stops requests before reaching server.

---

### HTTP History

Shows every HTTP request made.

---

### Repeater

Modify and resend requests.

Useful for

- BOLA
- SQL Injection
- XSS
- IDOR testing

---

# Why this vulnerability is dangerous

Attackers can access

- Orders
- Shopping carts
- User profiles
- Invoices
- Medical records
- Bank details

Simply by changing an ID.

---

# How Developers Prevent BOLA

Never trust object IDs from users.

Always verify

```
Object belongs to authenticated user.
```

Example (pseudo code)

```java
if (basket.userId != currentUser.id)
{
    return 403;
}
```

Never return data based only on

```
basketId
```

---

# Scoreboard

Enable Juice Shop scoreboard

Visit

```
http://192.168.0.19/#/score-board
```

This unlocks the challenge progress page.

---

# Key Takeaways

- BOLA = Broken Object-Level Authorization.
- The vulnerability occurs when object ownership is **not validated**.
- Burp Suite Proxy captures requests.
- HTTP History helps identify interesting API endpoints.
- Repeater is used to modify and resend requests.
- Changing `/rest/basket/1` to `/rest/basket/2` exposed another user's basket.
- Proper authorization should return **403 Forbidden** for unauthorized access.
- BOLA is one of the most critical **OWASP API Security Top 10** vulnerabilities because it can expose sensitive user data.

---

# Exam Tips

Remember this sequence:

```
Start Juice Shop
      ↓
Register/Login
      ↓
Burp Proxy
      ↓
Capture Requests
      ↓
Add Products
      ↓
HTTP History
      ↓
GET /rest/basket
      ↓
Send to Repeater
      ↓
Change Basket ID
      ↓
Send Again
      ↓
Access Other User's Basket
      ↓
BOLA Confirmed
```
````
