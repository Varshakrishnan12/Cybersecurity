
# Exercise 3 : HTTP Tunneling to Bypass Firewalls Using HTTPort

---

# Objective

Learn how attackers bypass firewall restrictions using **HTTP Tunneling**.

Normally:

- FTP uses Port 21
- HTTP uses Port 80

If the firewall blocks FTP (21) but allows HTTP (80), attackers can encapsulate (tunnel) FTP traffic inside HTTP traffic to communicate through the firewall.

---

# Scenario

Machines

| Machine | Purpose |
|----------|---------|
| FTP Server | Runs HTTHost |
| Windows Server 2022 | Runs HTTPort |
| Sales Department | FTP Server (172.19.19.17) |

---

# Overall Flow

```
FTP Client
(Windows Server)

        │
        │ FTP Request
        ▼

HTTPort
(Converts FTP → HTTP)

        │
        │ HTTP Port 80
        ▼

Firewall
(Allows HTTP)

        │
        ▼

HTTHost
(Removes HTTP Tunnel)

        │
        │ FTP
        ▼

Sales Department FTP Server
172.19.19.17
```

---

# Step 1
## Login to FTP Server

Password

```
test@123
```

---

## Why?

This machine will host **HTTHost**, which acts as the tunnel server.

Without HTTHost, tunneling cannot occur.

---

# Step 2
## Stop IIS Services

Open

```
Control Panel
→ Windows Tools
→ Services
```

Stop

- World Wide Web Publishing Service
- IIS Admin Service

---

## Why?

IIS already occupies **Port 80**.

HTTHost also needs Port 80.

Only one application can listen on a port.

Before

```
Port 80
│
└── IIS
```

After stopping IIS

```
Port 80
│
└── HTTHost
```

---

# Step 3
## Start HTTHost

Open

```
Desktop
→ HTTHost
→ htthost.exe
```

If Security Warning appears

Click

```
Run
```

---

## Why?

HTTHost acts as the HTTP tunnel server.

It receives HTTP packets and converts them back into FTP traffic.

---

# Step 4
## Configure HTTHost

Go to

```
Options Tab
```

Leave all settings default.

Set

```
Personal Password

magic
```

Enable

```
✔ Revalidate DNS Names

✔ Log Connections
```

Click

```
Apply
```

---

## Why?

The password authenticates HTTPort.

Only clients knowing this password can establish the tunnel.

---

# Step 5
## Verify HTTHost

Application Log should display

```
Listener:
Listening at 0.0.0.0:80
```

---

## Meaning

HTTHost is successfully listening on Port 80.

Now HTTP traffic can enter the tunnel.

---

# Step 6
## Login to Windows Server 2022

Password

```
Pa$$w0rd
```

---

## Why?

This machine acts as the FTP client.

HTTPort will be installed here.

---

# Step 7
## Enable Windows Firewall

Go to

```
Control Panel

↓

Windows Defender Firewall

↓

Turn Windows Firewall On
```

---

## Why?

To simulate a protected network.

---

# Step 8
## Create Outbound Rule

Go to

```
Advanced Settings

↓

Outbound Rules

↓

New Rule
```

Choose

```
Port
```

Click Next

Choose

```
All Remote Ports
```

Click Next

Choose

```
Block Connection
```

Finish.

Name

```
Port 21 Blocked
```

---

## Why?

This rule blocks outgoing FTP traffic.

Now direct FTP communication becomes impossible.

---

# Step 9
## Modify Rule

Open

```
Properties

↓

Protocols and Ports

↓

Specific Remote Port

21
```

Apply

OK

---

## Why?

Instead of blocking every port,

only FTP Port 21 is blocked.

---

# Step 10
## Disable Rule

Right-click

```
Disable Rule
```

---

## Test FTP

```
ftp 172.19.19.17
```

---

## Expected

FTP asks

```
Username
```

Connection successful.

---

## Why?

Firewall rule is disabled.

Nothing blocks FTP.

---

# Step 11
## Enable Rule

Right-click

```
Enable Rule
```

Again run

```
ftp 172.19.19.17
```

---

## Expected

FTP connection blocked.

---

## Why?

Firewall blocks Port 21.

Direct FTP communication is impossible.

---

# Step 12
## Install HTTPort

Install

```
HTTPort 3.SNFM
```

Complete the installation wizard.

---

## Why?

HTTPort converts FTP packets into HTTP packets.

---

# Step 13
## Configure HTTPort

Proxy Tab

Proxy Server

```
172.19.19.9
```

Port

```
80
```

Bypass Mode

```
Remote Host
```

Remote Host

```
172.19.19.9
```

Port

```
80
```

Password

```
magic
```

---

## Why?

HTTPort connects to HTTHost through HTTP.

Password authenticates the tunnel.

---

# Step 14
## Create Port Mapping

Port Mapping

↓

Add

Rename

```
ftp old home
```

Configure

```
Local Port

21

Remote Host

172.19.19.17

Remote Port

21
```

---

## Why?

HTTPort now knows

Whenever local FTP requests arrive,

forward them to

```
172.19.19.17:21
```

through the HTTP tunnel.

---

# Step 15
## Start HTTPort

Go back

```
Proxy Tab

↓

Start
```

---

## Meaning

Tunnel becomes active.

---

# Step 16
## Test Direct FTP

```
ftp 172.19.19.17
```

---

## Expected

Blocked.

---

## Why?

Firewall blocks Port 21.

---

# Step 17
## Test Tunnel

```
ftp 127.0.0.1
```

---

## Why localhost?

You are NOT connecting directly to the FTP server.

You connect to HTTPort.

HTTPort forwards traffic through HTTHost.

```
FTP Client

↓

127.0.0.1

↓

HTTPort

↓

HTTP

↓

HTTHost

↓

FTP Server
```

---

## Expected

Username prompt appears.

---

# Step 18
## Login

Username

```
Admin
```

Password

```
test@123
```

---

## Meaning

Tunnel works successfully.

Firewall has been bypassed.

---

# Step 19
## Create Folder

```
mkdir Test
```

---

## Why?

Verify write access.

---

# Step 20
## Verify

Login to Sales Department

Go to

```
C:\FTP
```

Folder

```
Test
```

should exist.

---

# Why HTTP Tunneling Works?

Firewall

```
Allows HTTP

Blocks FTP
```

Instead of sending

```
FTP
```

HTTPort converts it into

```
HTTP
```

Firewall thinks

```
Normal Web Traffic
```

Allows it.

HTTHost converts it back into FTP.

---

# Complete Communication Flow

```
FTP Client

↓

HTTPort

↓

HTTP Packet

↓

Firewall

↓

HTTHost

↓

FTP Packet

↓

FTP Server
```

---

# Difference Between HTTHost and HTTPort

| HTTHost | HTTPort |
|----------|----------|
| Server | Client |
| Receives HTTP | Sends HTTP |
| Converts HTTP → FTP | Converts FTP → HTTP |
| Runs on FTP Server | Runs on Windows Server |

---

# Important Ports

| Port | Service |
|------|---------|
| 21 | FTP |
| 80 | HTTP |

---

# Important IP Addresses

| IP | Purpose |
|-----|----------|
| 172.19.19.9 | HTTHost Machine |
| 172.19.19.17 | FTP Server |
| 127.0.0.1 | HTTPort (Localhost) |

---

# Interview Questions

## Why stop IIS?

To free Port 80 for HTTHost.

---

## Why block Port 21?

To simulate firewall restrictions.

---

## Why use HTTP?

HTTP is usually allowed through firewalls.

---

## Why use localhost?

HTTPort listens locally.

FTP client connects to HTTPort.

HTTPort forwards traffic through the tunnel.

---

## Why use HTTHost?

To receive HTTP traffic and convert it back into FTP.

---

## Why use HTTPort?

To convert FTP traffic into HTTP traffic.

---

## Why is Personal Password required?

To authenticate the tunnel connection.

---

## Which tab contains Personal Password?

**Options Tab**

---

## Why does ftp 172.19.19.17 fail?

Firewall blocks Port 21.

---

## Why does ftp 127.0.0.1 work?

Traffic is tunneled over HTTP (Port 80), which the firewall allows.

---

# One-Line Revision

```
FTP blocked
↓

HTTPort converts FTP → HTTP

↓

Firewall allows HTTP

↓

HTTHost converts HTTP → FTP

↓

FTP Server

↓

Firewall bypassed
```

---

# Commands Used

```
ftp 172.19.19.17

ftp 127.0.0.1

mkdir Test
```

---

# Lab Question 7.3.1

**Question**

Identify the tab in HTTHost where the Personal Password is configured.

**Answer**options
