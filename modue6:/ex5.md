# Exercise 5 - Exploiting Insecure JSON Web Token (JWT)

## Objective

Forge (create) an **unsigned JWT** by changing the JWT algorithm from **RS256** to **none** and modifying the payload.

---

# JWT Basics

JWT consists of **3 parts**:

```
Header.Payload.Signature
```

Example:

```
xxxxx.yyyyy.zzzzz
```

- **Header** → Algorithm & Token Type
- **Payload** → User Information
- **Signature** → Verifies integrity

---

# Overall Workflow

```text
Start Juice Shop
      │
      ▼
Register User
      │
      ▼
Login
      │
      ▼
Capture Login Request (Burp)
      │
      ▼
Copy Authorization Bearer Token
      │
      ▼
Decode JWT (jwt.io)
      │
      ▼
Change Algorithm
RS256 → none
      │
      ▼
Base64URL Encode Header
      │
      ▼
Modify Payload
(Change Email)
      │
      ▼
Base64URL Encode Payload
      │
      ▼
Create New JWT
Header.Payload.
      │
      ▼
Replace Authorization Bearer
      │
      ▼
Send Request
      │
      ▼
JWT Forged Successfully
```

---

# Step 1 - Start Juice Shop (Ubuntu)

Login:

```
Password: toor
```

Run:

```bash
sudo docker run -d -p 80:3000 bkimminich/juice-shop
```

If port is busy:

```bash
service apache2 stop
```

---

# Step 2 - Open Juice Shop (Parrot)

Open:

```
http://192.168.0.19
```

Dismiss popup.

---

# Step 3 - Register

```
Account
      ↓
Login
      ↓
Not yet a customer?
```

Create a new account.

---

# Step 4 - Login

Enter

- Email
- Password

Don't click Login yet.

---

# Step 5 - Open Burp Suite

```
Applications
    ↓
Burp Suite CE
```

- Temporary Project
- Use Burp Defaults
- Start Burp

---

# Step 6 - Capture Login Request

Go to

```
Proxy
```

Turn

```
Intercept ON
```

Now click

```
Login
```

Burp captures the request.

Click

```
Forward
```

until you find

```
Authorization: Bearer <JWT Token>
```

Right Click

```
Send to Repeater
```

---

# Step 7 - Copy JWT

Go to

```
Repeater
```

Copy

```
Authorization: Bearer
```

token.

Example

```
eyJhbGc.......
```

---

# Step 8 - Decode JWT

Open

```
https://jwt.io
```

Paste the token.

You'll see

```
Header
Payload
Signature
```

---

# Step 9 - Modify Header

Original

```json
{
  "alg":"RS256",
  "typ":"JWT"
}
```

Change to

```json
{
  "alg":"none",
  "typ":"JWT"
}
```

### Why?

Normally

```
RS256
```

means

Server verifies the signature.

Changing it to

```
none
```

removes signature verification.

If the server accepts it,

it becomes vulnerable.

---

# Step 10 - Encode Header

Open

```
https://base64.guru/standards/base64url/encode
```

Paste modified header.

Click

```
Encode
```

Copy the output.

Example

```
xxxxx
```

---

# Step 11 - Modify Payload

Go back to

```
jwt.io
```

Change

```json
"email":"yourmail@gmail.com"
```

to

```json
"email":"jwtn3d@juice-sh.op"
```

Copy modified payload.

---

# Step 12 - Encode Payload

Again open

```
base64.guru
```

Encode the payload.

Copy the encoded value.

Example

```
yyyyy
```

---

# Step 13 - Create Fake JWT

Open Notepad.

Paste

```
EncodedHeader
.
EncodedPayload
.
```

Final token becomes

```
xxxxx.yyyyy.
```

Notice

```
No Signature
```

because algorithm is

```
none
```

---

# Step 14 - Replace Authorization Token

Go back to Burp Repeater.

Replace

```
Authorization: Bearer
```

with your forged JWT.

Click

```
Send
```

---

# Step 15 - Verify

Open

```
http://192.168.0.19/#/score-board
```

Challenge gets completed.

JWT successfully forged.

---


# Important Websites

Decode JWT

```
https://jwt.io
```

Base64URL Encode

```
https://base64.guru/standards/base64url/encode
```

---

# Exam Question

**Q:** What is the default signed algorithm of the JWT token in jwt.io?

**Answer:**

```
RS256
```

---

# Quick Revision

- JWT = **Header.Payload.Signature**
- Capture JWT using **Burp Suite**.
- Decode using **jwt.io**.
- Change **alg** from **RS256 → none**.
- Change the email in the payload.
- Base64URL encode Header & Payload.
- Create a new token: `Header.Payload.`
- Replace the Bearer token in Burp and send.
- If accepted, the application is vulnerable to **Insecure JWT Implementation**.
