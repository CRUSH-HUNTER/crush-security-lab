# Maryam Movahhed

### Cybersecurity Researcher | Web Application Security

**Independent Security Research · Vulnerability Analysis · Technical Reporting**
**Contact:** `@maryantuanet`

I am an independent cybersecurity researcher focused on web application security. My work involves investigating application behavior, analyzing security weaknesses, validating findings in controlled environments, and documenting technical evidence with practical remediation guidance.

---

## 📌 Start here

| Report | What it shows |
|---|---|
| [Full Penetration Test Report](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/full-penetration-test-report.html) | Full SQL injection walkthrough: authentication bypass, evidence, impact, remediation |
| [Authorized E-Commerce Assessment (anonymized)](https://github.com/CRUSH-HUNTER/crush-security-lab/blob/main/WebSecurity/reports/WebApplicationPenetrationTestReport.md) | Real-world style report with scope, findings table, severity, and negative results |
| [Security Test Findings](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/security-test-findings.html) | CSRF, information disclosure, IDOR, clickjacking, directory traversal, JWT `alg: none` |
| [Juice Shop Findings (Public)](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/juice_shop_findings_public.html) | Condensed summary of all lab findings |

## 🎯 Focus areas

- Web application security
- Authentication, session management, and JWT security
- Broken access control (IDOR)
- Injection vulnerabilities (SQLi, XSS, CRLF)
- Server-side request forgery (SSRF)

## 🧪 Lab reports (OWASP Juice Shop, local Docker lab)

| Report | Topic |
|---|---|
| [SQL Injection](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/sql-injection.html) | Admin login bypass and password hash extraction |
| [XSS Session Hijacking](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/xss_session_hijacking.html) | Stealing a session via XSS and cookie theft |
| [JWT Deep Dive](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/jwt_deep_dive.html) | JWT structure, payload decoding, token testing |
| [JWT Token Forgery](https://crush-hunter.github.io/crush-security-lab/WebSecurity/reports/JWT_Token_Forgery.html) | Forging tokens and its impact |
| [SSRF](https://github.com/CRUSH-HUNTER/crush-security-lab/blob/main/WebSecurity/reports/SSRF.md) | Extracting a hidden URL from a binary with `strings`, then abusing the profile image URL endpoint |
| [CRLF Injection](https://github.com/CRUSH-HUNTER/crush-security-lab/blob/main/WebSecurity/reports/CRLF-Injection.md) | Header injection, session fixation, response splitting to XSS (with PoC) |


## 📓 Study notes

Reference notes written while learning: [`WebSecurity/notes/`](WebSecurity/notes) and [`VMwareLabNotes/`](VMwareLabNotes).

---

## 🔬 Methodology

My research follows an evidence-driven approach:

- Application behavior and attack surface analysis
- Manual security testing and vulnerability validation
- Technical evidence collection and root cause identification
- Impact assessment and practical remediation recommendations
- Clear, reproducible reporting, including negative results (things tested and found safe)

## 🛠️ Tools

Burp Suite · Browser DevTools · Nmap · Hashcat · Kali Linux · Docker · VMware Workstation

## ⚠️ Responsible research

All testing is conducted in intentionally vulnerable local labs or against systems where I had **explicit written authorization**. Real-world reports are anonymized. Screenshots of cards, wallets, or accounts come from Juice Shop's **fake demo data**. I prioritize accurate, evidence-based findings over unsupported claims. Nothing here should be used against systems you do not own or have permission to test.

---

*Investigate. Validate. Document. Improve.*
