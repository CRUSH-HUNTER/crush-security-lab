# 🔐 crush-security-lab

[![Security Research](https://img.shields.io/badge/Security-Research-red.svg)](https://github.com/CRUSH-HUNTER/crush-security-lab/blob/main) [![Web Security](https://img.shields.io/badge/Web-Security-orange.svg)](https://github.com/CRUSH-HUNTER/crush-security-lab/blob/main) [![OWASP](https://img.shields.io/badge/OWASP-Top%2010-black.svg)](https://github.com/CRUSH-HUNTER/crush-security-lab/blob/main)

> A personal cybersecurity lab focused on hands-on Web Application Security research, vulnerability analysis, and security testing.

---

## 🎯 Current Focus

- Web Application Security
- OWASP Top 10
- Vulnerability Research
- Authentication & Authorization
- Cross-Site Scripting (XSS)
- SQL Injection
- IDOR / Broken Access Control
- JWT & Session Security
- Security Testing with Burp Suite
- Linux-based Security Labs

---

## 🧪 Security Research

My practical research is primarily performed in controlled and intentionally vulnerable environments.

### Current Labs

| Lab                  | Focus                                                       |
| --------------------- | ------------------------------------------------------------ |
| **OWASP Juice Shop**  | Web vulnerability research and penetration-testing practice |
| **Kali Linux**        | Security testing environment                                |
| **Docker**            | Isolated vulnerable application deployment                   |
| **VMware**            | Virtualized security laboratory                              |

### Reports in this repository

Each research report aims to document the vulnerability, reproduction process, security impact, evidence, and possible remediation.

| Report | Topic |
|---|---|
| [Full Penetration Test Report](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/full-penetration-test-report.html) | Full SQLi walkthrough — authentication bypass on the Bender account |
| [SQL Injection](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/sql-injection.html) | Admin login bypass and password hash extraction via SQLi |
| [XSS Session Hijacking](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/xss_session_hijacking.html) | Stealing a user session via XSS and cookie theft |
| [JWT Deep Dive](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/jwt_deep_dive.html) | JWT structure, payload decoding, and hands-on token testing |
| [Security Test Findings](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/security-test-findings.html) | CSRF, Information Disclosure, IDOR, Clickjacking, Directory Traversal, JWT `alg: none` bypass |
| [Juice Shop Findings (Public)](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/juice_shop_findings_public.html) | Condensed summary of the Juice Shop findings |
| [TOTP Secret Explained](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/totp-secret-explained.html) | TOTP secrets and how two-factor authentication uses them |
| [Password Hash Cracking](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/password-hash-cracking.html) | Password hashing concepts and cracking techniques |
| [Hashcat Errors Report](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/hashcat-errors-report.html) | Practical errors and fixes encountered while using hashcat |
| [HTTP - URL Reference](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/http-url.html) | Reference notes on HTTP concepts and URL structure |

---

## 🛠️ Technical Skills

| Area                   | Technologies / Tools                                    |
| ----------------------- | ---------------------------------------------------------- |
| **Web Security**       | OWASP Top 10, XSS, SQL Injection, IDOR, Authentication   |
| **Security Testing**   | Burp Suite, OWASP ZAP, Browser DevTools                  |
| **Programming**        | Python, JavaScript, C                                    |
| **Web Technologies**   | HTTP, URLs, Cookies, Sessions, JWT                       |
| **Linux**              | Kali Linux, Bash, APT                                    |
| **Containers**         | Docker, Docker Compose                                   |
| **Virtualization**     | VMware Workstation                                       |
| **Security Utilities** | Hashcat, JWT tooling                                     |

---

## 📚 Learning Path

My current learning path is focused on building practical Web Security skills rather than collecting tools or completing challenges without understanding them.

### Current

- Web Application Security fundamentals
- OWASP Top 10
- Manual vulnerability discovery
- Burp Suite workflows
- Authentication and authorization testing
- Vulnerability documentation and reporting

### Next

- Advanced Web Application Security
- Business Logic vulnerabilities
- SSRF
- Advanced authentication attacks
- API Security
- Security automation with Python
- Real-world vulnerability research

---

## 📊 Research Methodology

For each vulnerability I investigate, I try to follow a structured process:

```
Reconnaissance
      ↓
Attack Surface Identification
      ↓
Hypothesis
      ↓
Manual Testing
      ↓
Proof of Concept
      ↓
Impact Analysis
      ↓
Root Cause Analysis
      ↓
Remediation
      ↓
Technical Documentation
```

The goal is not simply to reproduce a vulnerability, but to understand **why it works and how it can be prevented**.

---

## ⚠️ Ethical Use & Disclaimer

All security testing documented in this repository is performed against intentionally vulnerable applications, local laboratory environments, or systems for which testing is explicitly authorized.

No unauthorized testing or exploitation of third-party systems is intended.

The techniques documented here are provided for **educational, defensive, and authorized security research purposes**.

---

## 📈 Long-Term Goals

- Build strong foundations in Web Application Security
- Develop practical penetration-testing skills
- Improve vulnerability research and security analysis
- Participate in authorized Bug Bounty programs
- Produce high-quality technical security reports
- Contribute to the cybersecurity community
- Build a professional portfolio in Application Security

---

## 📫 Connect

**GitHub:** [CRUSH-HUNTER](https://github.com/CRUSH-HUNTER)

---

> **Learn → Test → Understand → Document → Improve**
