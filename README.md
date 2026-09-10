# 🔐 Cybersecurity Writeups

<p align="center">
  <img src="https://img.shields.io/badge/Focus-Web%20Security-red?style=for-the-badge">
  <img src="https://img.shields.io/badge/Field-Cybersecurity-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Bug%20Bounty-Learning-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Writeups-In%20Progress-yellow?style=for-the-badge">
</p>

<p align="center">
  A practical collection of cybersecurity lab writeups, vulnerability analysis,
  exploitation techniques, and security research.
</p>

---

## 📖 About This Repository

This repository documents my practical cybersecurity learning journey through authorized labs, training platforms, and security research.

Each writeup focuses on understanding:

* 🔎 How vulnerabilities are discovered
* 🧠 Why vulnerabilities exist
* 💥 How they can be exploited
* 🛡️ How they can be prevented
* 📚 What I learned from each lab

The goal is not only to solve labs, but to understand the underlying security concepts and develop a repeatable methodology.

---

## 📑 Table of Contents

* [🎯 Objectives](#-objectives)
* [🧪 Platforms](#-platforms)
* [🧩 Vulnerability Categories](#-vulnerability-categories)
* [📊 Progress Tracker](#-progress-tracker)
* [🛠️ Tools](#️-tools)
* [📂 Repository Structure](#-repository-structure)
* [📝 Writeup Methodology](#-writeup-methodology)
* [📚 Resources](#-resources)
* [⚠️ Disclaimer](#️-disclaimer)

---

## 🎯 Objectives

* Build strong practical web security skills.
* Improve vulnerability discovery and exploitation techniques.
* Develop a structured penetration testing methodology.
* Practice analyzing HTTP requests and responses.
* Improve Burp Suite skills.
* Understand common OWASP vulnerabilities.
* Document practical security knowledge.
* Build a public cybersecurity portfolio.

---

# 🧪 Platforms

## PortSwigger Web Security Academy

Practical web security labs covering a wide range of vulnerabilities.

| Category          | Status         |
| ----------------- | -------------- |
| SQL Injection     | 🚧 In Progress |
| XSS               | 🚧 In Progress |
| Authentication    | 🚧 In Progress |
| Access Control    | 🚧 In Progress |
| CSRF              | 🚧 In Progress |
| SSRF              | 🚧 In Progress |
| XXE               | 🚧 In Progress |
| File Upload       | 🚧 In Progress |
| Command Injection | 🚧 In Progress |
| API Security      | 🚧 In Progress |

---

## 🧩 Vulnerability Categories

### 💉 Injection

* SQL Injection
* NoSQL Injection
* Command Injection
* LDAP Injection
* XML Injection
* Server-Side Template Injection

### 🌐 Cross-Site Scripting

* Reflected XSS
* Stored XSS
* DOM-based XSS

### 🔐 Authentication

* Authentication bypass
* Password attacks
* Session management
* Multi-factor authentication vulnerabilities
* Password reset vulnerabilities

### 🚪 Access Control

* IDOR
* Horizontal privilege escalation
* Vertical privilege escalation
* Broken access control
* Insecure direct object references

### 🔄 Client-Side

* CSRF
* DOM-based vulnerabilities
* Client-side validation bypass

### 🖥️ Server-Side

* SSRF
* XXE
* File upload vulnerabilities
* Path traversal
* Command injection

### 🔌 API Security

* API authentication
* Authorization flaws
* Parameter manipulation
* Mass assignment
* API access control

---

# 📊 Progress Tracker

## Overall Progress

```text
PortSwigger
████████░░░░░░░░░░░░ 40%

Web Security
███████░░░░░░░░░░░░░ 35%

Bug Bounty
█████░░░░░░░░░░░░░░░ 25%
```

> Progress percentages will be updated as more labs are completed.

---

## 🏆 Completed Labs

|  # | Platform    | Lab         | Vulnerability | Difficulty |
| -: | ----------- | ----------- | ------------- | ---------- |
| 01 | PortSwigger | Example Lab | SQL Injection | Apprentice |
| 02 | PortSwigger | Example Lab | XSS           | Apprentice |

> Replace the example entries with completed labs.

---

# 🛠️ Tools

Tools commonly used during the labs and security testing:

### Web Security

* Burp Suite
* Browser DevTools
* curl
* HTTPie

### Reconnaissance

* Nmap
* Gobuster
* ffuf
* Nikto

### Exploitation

* SQLmap
* Metasploit
* Custom scripts

### Development / Scripting

* Python
* PHP
* Bash

---

# 📂 Repository Structure

```text
Cybersecurity-Writeups/
│
├── README.md
│
├── PortSwigger/
│   │
│   ├── SQL-Injection/
│   │   ├── README.md
│   │   ├── images/
│   │   └── ...
│   │
│   ├── XSS/
│   │   ├── README.md
│   │   ├── images/
│   │   └── ...
│   │
│   ├── Authentication/
│   │   └── ...
│   │
│   ├── Access-Control/
│   │   └── ...
│   │
│   ├── CSRF/
│   │   └── ...
│   │
│   ├── SSRF/
│   │   └── ...
│   │
│   ├── XXE/
│   │   └── ...
│   │
│   └── API-Security/
│       └── ...
│
├── HackTheBox/
│   └── ...
│
├── TryHackMe/
│   └── ...
│
└── Notes/
    ├── HTTP/
    ├── BurpSuite/
    ├── Recon/
    └── WebSecurity/
```

---

# 📝 Writeup Methodology

Each writeup follows a consistent methodology.

### 01 — Lab Information

Basic information about the lab:

* Platform
* Category
* Difficulty
* Status

### 02 — Objective

What the lab requires you to achieve.

### 03 — Reconnaissance

Analyze the application and identify interesting:

* Endpoints
* Parameters
* Requests
* Responses
* Cookies
* Headers
* Authentication mechanisms

### 04 — Vulnerability Analysis

Explain:

* Where the vulnerability exists
* Why it exists
* What causes it
* How the application processes the input

### 05 — Exploitation

Document the exploitation process step by step.

Example:

```http
GET /example?parameter=value HTTP/1.1
Host: example.com
```

Then explain the modification made and the resulting behavior.

### 06 — Tools

Document the tools used during the lab.

### 07 — Solution

Explain the final steps required to solve the lab.

### 08 — Lessons Learned

Document the key concepts learned from the lab.

---

# 📈 Learning Roadmap

```text
Web Fundamentals
       │
       ▼
HTTP / HTTPS
       │
       ▼
Burp Suite
       │
       ▼
OWASP Top 10
       │
       ▼
Web Vulnerabilities
       │
       ▼
Advanced Web Security
       │
       ▼
Bug Bounty
       │
       ▼
Vulnerability Research
```

---

# 📚 Resources

Useful resources used for learning and research:

* OWASP
* PortSwigger Web Security Academy
* HackerOne
* Bugcrowd
* CWE
* CVE
* Security research blogs
* Official documentation

---

# 🧠 Key Skills

Through these labs and writeups, I am continuously developing skills in:

* Web Application Security
* Vulnerability Assessment
* Penetration Testing
* Reconnaissance
* HTTP Request Analysis
* Burp Suite
* Exploitation
* Security Research
* Python Scripting
* Bug Bounty Methodology

---

# ⚠️ Disclaimer

All content in this repository is intended for **educational purposes only**.

The techniques, payloads, and methodologies documented here should only be used against systems that you own or have explicit authorization to test.

I do not take responsibility for unauthorized or malicious use of the information contained in this repository.

---

# 👨‍💻 About

This repository represents my ongoing practical journey in cybersecurity.

**Focus Areas:**

```text
Web Security
Penetration Testing
Bug Bounty
Vulnerability Research
Security Automation
```

More labs, writeups, notes, and research will be added as I continue learning.

---

<p align="center">
  <b>🔐 Learn • Hack • Document • Improve</b>
</p>
