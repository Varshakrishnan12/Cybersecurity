# Exercise 2 - API Reconnaissance using AI (ShellGPT)

## Objective
Learn how to use **ShellGPT (GPT-4 powered)** to automate API reconnaissance by generating a Bash script that:
- Extracts API endpoints (routes)
- Enumerates API structure
- Identifies API functionality
- Discovers API technologies
- Saves the results into a text file

---

# Steps

## 1. Login to Parrot Security

```
Username: pentester
Password: toor
```

Become root:

```bash
sudo su
```

---

## 2. Configure ShellGPT

Run:

```bash
bash sgpt.sh
```

Enter your **AI Activation Key** (obtained from the CPENTv2 dashboard on ASPEN/iClass/ECL/ECCU).

If successful, ShellGPT will be configured.

---

## 3. Generate API Reconnaissance Script

Run the following command (**Important Prompt**):

```bash
sgpt --shell "On the target URL https://petstore.swagger.io/ perform API reconnaissance, extract API routes, enumerate API's structure, identify API functionality, and discover API technologies. Organize and display the results in clearly labeled sections for each type of information and save the results in a text file. Install all the required dependencies and libraries. Format the output like a real bash script with line breaks and \ characters."
```

> **Note:** This prompt is important for the lab.

---

## 4. Execute the Generated Script

When prompted:

```
[E] Execute
```

Type:

```text
E
```

and press **Enter**.

ShellGPT generates and executes the Bash script automatically.

---

## 5. View the Results

Open the generated output file (example):

```text
api_recon_results.txt
```

The file contains:
- API Routes (Endpoints)
- API Structure
- API Functionalities
- API Technologies

---

# Commands Used

Become root:

```bash
sudo su
```

Configure ShellGPT:

```bash
bash sgpt.sh
```

Generate API Recon Script:

```bash
sgpt --shell "On the target URL https://petstore.swagger.io/ perform API reconnaissance, extract API routes, enumerate API's structure, identify API functionality, and discover API technologies. Organize and display the results in clearly labeled sections for each type of information and save the results in a text file. Install all the required dependencies and libraries. Format the output like a real bash script with line breaks and \ characters."
```

---

# Quick Revision

- **ShellGPT** → AI-powered CLI assistant using GPT-4.
- **`bash sgpt.sh`** → Configure ShellGPT with AI activation key.
- **`sgpt --shell`** → Generates executable Bash scripts from natural language.
- Type **`E`** → Execute the generated script.
- Output is saved in **`api_recon_results.txt`** for further analysis.
