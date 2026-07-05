# Task 02 - Sniffing User Credentials using SET's QRCode Generator Attack Vector

## Objective

Learn how attackers can combine **SET's QRCode Generator** with **Site Cloner** to deliver a phishing website through a QR code and capture user credentials in a controlled lab environment.

> **Note:** This technique is for authorized security testing and awareness training only.

---

# What is QR Code Phishing (Quishing)?

**Quishing** (QR Code Phishing) is a phishing technique where a malicious URL is embedded inside a QR code instead of a traditional hyperlink.

When the victim scans the QR code, they are redirected to a phishing website.

---

# Attack Components

* **QRCode Generator** → Generates a QR code containing the malicious URL.
* **Site Cloner** → Creates a clone of a legitimate website.
* **Credential Harvester** → Captures usernames and passwords entered by the victim.

---

# Attack Setup

Generate a QR code pointing to the attacker's machine:

```text
http://192.168.0.18
```

The generated QR code is stored in:

```text
/root/.set/reports/qrcode_attack.png
```

Next, use **Credential Harvester → Site Cloner** to clone the legitimate website.

Example:

```text
Original Website
www.moviescope.com

        ↓

Cloned Website
http://192.168.0.18
```

---

# Delivery Method

Instead of sending a phishing link directly, the attacker sends an email containing the malicious QR code as an attachment.

Victim actions:

1. Open the email.
2. Scan the QR code (or use Google Lens).
3. Open the extracted website.

---

# Credential Harvesting

The QR code redirects the victim to the cloned login page.

Victim enters:

```text
Username
Password
```

SET records the credentials in the terminal and then redirects the victim to the legitimate website to reduce suspicion.

---

# Attack Workflow

```text
Generate QR Code
        ↓
Clone Legitimate Website
        ↓
Host Fake Website
        ↓
Send QR Code via Email
        ↓
Victim Scans QR Code
        ↓
Victim Opens Fake Website
        ↓
Victim Enters Credentials
        ↓
SET Captures Credentials
        ↓
Victim Redirected to Real Website
```

---

# Difference from Site Cloner Attack

| Site Cloner Attack            | QR Code Attack                       |
| ----------------------------- | ------------------------------------ |
| Victim clicks a phishing link | Victim scans a QR code               |
| Link embedded in email        | QR code embedded in email or poster  |
| Redirects to cloned website   | Redirects to the same cloned website |

The credential harvesting process remains the same; only the delivery method changes.

---

# Key Concepts

| Concept              | Description                                                             |
| -------------------- | ----------------------------------------------------------------------- |
| Quishing             | Phishing using QR codes instead of hyperlinks.                          |
| QRCode Generator     | Creates a QR code containing the phishing URL.                          |
| Site Cloner          | Clones a legitimate website.                                            |
| Credential Harvester | Records submitted usernames and passwords.                              |
| Redirect             | Sends the victim to the legitimate website after capturing credentials. |

---

# Key Takeaways

* SET can generate malicious QR codes for phishing campaigns.
* QR codes provide an alternative delivery method to traditional phishing links.
* Victims are redirected to a cloned website after scanning the QR code.
* Submitted credentials are captured by the Credential Harvester.
* Redirecting victims to the legitimate website helps reduce suspicion.
* Security awareness should include verifying QR codes before scanning them and checking the destination URL before entering credentials.
