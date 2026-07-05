# Task 01 - Sniffing User Credentials using SET's Site Cloner

## Objective

Learn how the **Social-Engineer Toolkit (SET)** can be used to perform a **Credential Harvesting (Phishing)** attack by cloning a legitimate website and capturing user credentials in a controlled lab environment.

> **Note:** This technique should only be used in authorized environments for security testing and awareness training.

---

# Social-Engineer Toolkit (SET)

The **Social-Engineer Toolkit (SET)** is an open-source penetration testing framework focused on **social engineering attacks**, such as phishing, credential harvesting, and website cloning.

Launch SET:

```bash
sudo su
setoolkit
```

---

# Attack Method

Navigate through the following menu:

```text
Social Engineering Attacks
     
   ↓
Website Attack Vectors
        ↓
Credential Harvester Attack Method
        ↓
Site Cloner
```

---

# Site Cloner

The **Site Cloner** downloads a legitimate website's appearance (HTML, CSS, images, etc.) and hosts a nearly identical copy on the attacker's machine.

Example:

```text
Original Website
www.moviescope.com

        ↓

Cloned Website
http://192.168.0.18
```

The victim sees a familiar login page and is more likely to trust it.

---

# POST Back IP

SET asks for the attacker's IP address.

Example:

```text
192.168.0.18
```

This is where the captured credentials are sent after the victim submits the login form.

---

# Credential Harvesting

When the victim enters:

```text
Username: alice
Password: Password123
```

SET records:

```text
Username = alice
Password = Password123
```

The credentials are displayed in the SET terminal in plain text.

---

# Phishing Email

The attacker sends an email containing a **disguised hyperlink**.

Example:

| Displayed Text                           | Actual Link           |
| ---------------------------------------- | --------------------- |
| `www.moviescope.com/account-information` | `http://192.168.0.18` |

The displayed text appears legitimate, while the hyperlink directs the victim to the cloned website.

---

# Why Redirect to the Real Website?

After capturing the credentials, SET redirects the victim to the legitimate website.

Benefits:

* Reduces suspicion.
* Victim assumes the login simply failed or the session expired.
* Makes the phishing attack less obvious.

---

# Attack Workflow

```text
Clone Legitimate Website
        ↓
Host Fake Website
        ↓
Send Phishing Email
        ↓
Victim Clicks Link
        ↓
Victim Enters Credentials
        ↓
SET Captures Username & Password
        ↓
Victim Redirected to Real Website
```

---

# Key Concepts

| Concept               | Description                                                                              |
| --------------------- | ---------------------------------------------------------------------------------------- |
| Social Engineering    | Manipulating users instead of exploiting software vulnerabilities.                       |
| Phishing              | Tricking users into visiting a fake website.                                             |
| Credential Harvesting | Capturing usernames and passwords entered by victims.                                    |
| Site Cloner           | Creates a copy of a legitimate website for phishing attacks.                             |
| Redirect              | Sends the victim to the genuine website after capturing credentials to reduce suspicion. |

---

# Key Takeaways

* SET automates phishing and credential harvesting attacks.
* The **Site Cloner** creates a convincing copy of a legitimate website.
* Victims are tricked into submitting credentials through a phishing link.
* Captured credentials are displayed in the SET terminal.
* Redirecting victims to the real website helps reduce suspicion.
* Understanding these techniques helps security professionals identify, prevent, and defend against phishing attacks.
