# Exercise 02 - Sniffing User Credentials using Dark-Phish

## Objective

Learn how **Dark-Phish** automates phishing attacks by creating a fake login page, capturing user credentials, and redirecting the victim to the legitimate website.

> **Note:** This technique should only be used in authorized environments for security testing and security awareness training.

---

# What is Dark-Phish?

**Dark-Phish** is a phishing automation tool that provides **ready-made templates** for popular websites such as:

* Google
* Facebook
* Instagram
* GitHub
* Twitter
* Many more

Unlike **SET's Site Cloner**, Dark-Phish already contains these templates, making phishing setup much faster.

---

# Navigate to the Project Directory

Open a terminal and become the root user:

```bash
sudo su
```

Move to the Dark-Phish project directory:

```bash
cd Dark-Phish/
```

---

# Edit the Host IP Address

Open the Python script using the Pluma text editor:

```bash
pluma dark-phish.py
```

Locate the **host** field (around line 30) and replace:

```python
host = "127.0.0.1"
```

with

```python
host = "172.19.19.18"
```

Save the file and close Pluma.

### Why is this necessary?

`127.0.0.1` (**localhost**) always refers to the **current machine only**.

If the phishing website is hosted on `127.0.0.1`, only the attacker's computer can access it.

Replacing it with the attacker's IP address allows victims on the same network to access the phishing website.

| Address        | Meaning                                              |
| -------------- | ---------------------------------------------------- |
| `127.0.0.1`    | Localhost (only this computer)                       |
| `172.19.19.18` | Attacker's network IP (accessible by other machines) |

---

# Launch Dark-Phish

Run the tool:

```bash
python3 dark-phish.py
```

---

# Select a Website Template

Choose the desired phishing template.

Example:

```text
Google
```

Dark-Phish automatically creates a fake Google login page.

---

# Choose the Hosting Method

Select:

```text
Localhost
```

Dark-Phish hosts the phishing website on the attacker's machine using the configured IP address.

---

# Generate the Phishing Link

Dark-Phish generates a phishing URL.

Example:

```text
http://172.19.19.18/...
```

This is the link that will be sent to the victim.

---

# Deliver the Phishing Link

Send the generated link through a phishing email or another communication method.

The victim believes the link leads to a legitimate Google login page.

---

# Victim Interaction

The victim:

1. Opens the phishing link.
2. Sees a fake Google login page.
3. Enters their email and password.
4. Clicks **Sign In**.

Dark-Phish captures the submitted credentials.

---

# Credential Harvesting

The captured credentials are displayed in the terminal.

Example:

```text
Email    : victim@gmail.com
Password : ********
```

---

# Redirect

After capturing the credentials, Dark-Phish automatically redirects the victim to the legitimate Google website.

This helps reduce suspicion because the victim assumes the previous login attempt simply failed.

---

# Attack Workflow

```text
Launch Dark-Phish
        ↓
Edit Host IP
        ↓
Select Website Template
        ↓
Host Fake Website
        ↓
Generate Phishing Link
        ↓
Send Link to Victim
        ↓
Victim Opens Fake Login Page
        ↓
Victim Enters Credentials
        ↓
Dark-Phish Captures Credentials
        ↓
Victim Redirected to Legitimate Website
```

---

# SET vs Dark-Phish

| SET                               | Dark-Phish                                    |
| --------------------------------- | --------------------------------------------- |
| Clones any website using its URL. | Uses built-in templates for popular websites. |
| Requires Site Cloner.             | Ready-to-use phishing templates.              |
| More flexible.                    | Faster setup for common websites.             |

Both tools perform the same objective:

* Create a fake login page.
* Trick the victim into entering credentials.
* Capture the credentials.
* Redirect the victim to the legitimate website.

---

# Key Concepts

| Concept                 | Description                                                             |
| ----------------------- | ----------------------------------------------------------------------- |
| Phishing                | Tricking users into visiting a fake website.                            |
| Credential Harvesting   | Capturing usernames and passwords entered by victims.                   |
| Localhost (`127.0.0.1`) | Refers only to the current machine.                                     |
| Attacker IP             | Makes the phishing website accessible to other machines on the network. |
| Redirect                | Sends the victim to the legitimate website after capturing credentials. |

---

# Key Takeaways

* Dark-Phish automates phishing attacks using pre-built website templates.
* The **host IP** must be changed from **`127.0.0.1`** to the **attacker's IP** so other devices can access the phishing website.
* `pluma dark-phish.py` is used to edit the configuration before launching the tool.
* Victims are lured to a fake login page using a phishing link.
* Submitted credentials are captured and displayed in the attacker's terminal.
* After harvesting credentials, the victim is redirected to the legitimate website to reduce suspicion.
* Dark-Phish simplifies phishing attacks compared to manually cloning websites using SET.
