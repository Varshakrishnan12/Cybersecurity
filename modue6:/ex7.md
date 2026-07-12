# Exercise 7 - Exploiting Broken Function Level Authorization (BFLA)

## Objective
Learn how to identify and exploit a **Broken Function Level Authorization (BFLA)** vulnerability by manipulating API requests using **Burp Suite**.

---

# What is BFLA?

**BFLA (Broken Function Level Authorization)** occurs when a user can access **restricted functions (such as admin functions)** without having the required privileges.

### Difference Between BOLA and BFLA

| BOLA | BFLA |
|------|------|
| Changes **Object ID** | Changes **Function/Endpoint/HTTP Method** |
| Access another user's data | Access unauthorized functionality (Admin functions) |

---

# Overall Workflow

```text
Login to crAPI
      │
      ▼
Go to Profile
      │
      ▼
Upload a Video
      │
      ▼
Rename the Video
      │
      ▼
Capture PUT Request in Burp
      │
      ▼
Send to Repeater
      │
      ▼
Change PUT → OPTIONS
      │
      ▼
Discover Allowed HTTP Methods
      │
      ▼
Change OPTIONS → DELETE
      │
      ▼
Server says:
"Admin Function"
      │
      ▼
Modify URL
user → admin
      │
      ▼
DELETE Works
      │
      ▼
Change Video ID
      │
      ▼
Delete Another User's Video
      │
      ▼
BFLA Confirmed
```

---

# Step 1 - Login

Open Firefox.

Visit:

```
http://192.168.0.19:8888
```

Login with the account created in the previous exercise.

---

# Step 2 - Upload a Video

Go to:

```
Profile
```

↓

```
My Personal Video
```

↓

```
Upload Video
```

Upload any **.mp4** file (or a dummy video).

---

# Step 3 - Change Video Name

Click:

```
Change Video Name
```

Rename it:

```
Video1
```

Click **Change**.

---

# Step 4 - Capture Request

Open Burp Suite.

```
Proxy
```

↓

```
Intercept OFF
```

↓

```
Open Browser
```

Login again.

Rename the video once more.

Burp captures a request like:

```http
PUT /identity/api/v2/user/video
```

Find the **PUT** request in:

```
Proxy
    ↓
HTTP History
```

Right-click →

```
Send to Repeater
```

---

# Step 5 - Discover Allowed HTTP Methods

In Repeater, change:

```http
PUT
```

to

```http
OPTIONS
```

Click **Send**.

The response lists the allowed HTTP methods.

Example:

```
GET
POST
PUT
DELETE
OPTIONS
```

**Total allowed methods in this lab:**

```
5
```

---

# Step 6 - Test DELETE

Change

```http
OPTIONS
```

to

```http
DELETE
```

Leave the body empty.

Click **Send**.

Server response:

```
This is an admin function.
Try to access the admin API.
```

This tells us **DELETE is restricted to administrators**.

---

# Step 7 - Exploit BFLA

Modify the URL.

Example:

Before:

```
/user/
```

After:

```
/admin/
```

Click **Send**.

The server performs the delete operation even though we are a normal user.

This confirms **Broken Function Level Authorization (BFLA)**.

---

# Step 8 - Delete Another User's Video

Notice the numeric ID in the endpoint.

Example:

```
/admin/video/1
```

Change it to:

```
/admin/video/5
```

Click **Send**.

The application deletes **User 5's video**.

This confirms that unauthorized administrative functionality is accessible.

---

# Why This Works

The application checks **who you are** (authentication) but **fails to check whether your role is allowed to perform the function**.

A normal user should **never** be able to access an admin-only endpoint.

---

# Commands / Tools Used

- Firefox
- Burp Suite
- Burp Proxy
- HTTP History
- Repeater

---

# Exam Question

**Q:** Determine the number of HTTP methods permitted in crAPI.

**Answer:**

```
5
```

---

# Quick Revision

- **BFLA** → Unauthorized access to restricted functions.
- Upload and rename a video.
- Capture the **PUT** request.
- Change **PUT → OPTIONS** to discover allowed methods.
- Change **OPTIONS → DELETE**.
- Server reveals it's an admin function.
- Modify **user → admin** in the endpoint.
- Change the video ID to another user's ID.
- Successfully deleting another user's video confirms **BFLA**.

---

# Easy Memory Trick

### BOLA

```
Change the ID
```

↓

Access another user's **data**.

### BFLA

```
Change the Function
```

↓

Access an **admin operation**.

**Remember:**

- **BOLA = Object**
- **BFLA = Function**
