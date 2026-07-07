# Exercise 6 -- Dictionary Attack on WordPress using Burp Suite

## Objective

Find a **valid username and password** by performing a **dictionary
attack** using Burp Suite Intruder.

------------------------------------------------------------------------

## What is a Dictionary Attack?

Instead of trying random passwords, the attacker uses **predefined
wordlists** of usernames and passwords.

Example:

``` text
Usernames:
admin
john
mike

Passwords:
123456
password
prince
admin123
```

Burp tries every combination until it finds the correct one.

------------------------------------------------------------------------

## Steps

### 1. Open WordPress Login

``` text
http://www.cpent.com/wp-login.php
```

Enter dummy credentials:

``` text
Username: test
Password: guess
```

### 2. Configure Firefox Proxy

``` text
HTTP Proxy : 127.0.0.1
Port       : 8080
```

### 3. Start Burp Suite

-   Temporary Project
-   Use Burp Defaults
-   Keep **Intercept ON**

### 4. Capture Login Request

``` text
POST /wp-login.php
```

### 5. Send to Intruder

``` text
Right-click → Send to Intruder
```

### 6. Choose Attack Type

``` text
Cluster Bomb
```

**Why?** Two unknown values (username & password), so Burp tries every
username with every password.

### 7. Set Payload Positions

Mark:

``` text
test
guess
```

using **Add §**.

### 8. Load Wordlists

-   Payload Set 1 → `Usernames.txt`
-   Payload Set 2 → `Passwords.txt`

### 9. Start Attack

Click **Start Attack**.

### 10. Find Correct Credentials

Compare:

-   Response **Length**
-   **Status**

The successful login has a noticeably different response.

Valid credentials found:

``` text
Username: mike
Password: prince
```

### 11. Verify

Log in using:

``` text
mike / prince
```

Login succeeds.

------------------------------------------------------------------------

## Burp Tools Used

  Tool       Purpose
  ---------- -----------------------
  Proxy      Capture login request
  Intruder   Dictionary attack

------------------------------------------------------------------------

## Exam Flow

``` text
Configure Proxy
      ↓
Capture Login Request
      ↓
Send to Intruder
      ↓
Cluster Bomb
      ↓
Mark Username & Password
      ↓
Load Wordlists
      ↓
Start Attack
      ↓
Check Response Length
      ↓
Find Valid Credentials
      ↓
Login Successfully
```
