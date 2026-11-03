# 📄 Web Application Penetration Test Report (Sanitized)

**Project Name:** E-Commerce Website Security Assessment (Project X)  
**Target:** `[REDACTED FOR CONFIDENTIALITY]`  
**Test Date:** October 2026  
**Tester:** CRUSH-HUNTER  
**Classification:** Confidential (Sanitized for Public Portfolio)  

---

## 1. Executive Summary

This report presents the findings of a black-box penetration test conducted on an e-commerce website (target details withheld for confidentiality) with explicit written authorization from the business owner.

**Overall Conclusion:** The web application demonstrated a **strong defense posture** against common injection vulnerabilities (such as SQL Injection and XSS) and authentication bypass attempts. The implemented Web Application Firewall (WAF) and input filters successfully blocked malicious payloads.

**However, two significant security risks (Medium to High severity) were identified at the infrastructure level:**
1. **Security Misconfiguration:** Several sensitive network services (including the database and FTP) were exposed directly to the internet.
2. **Vulnerable Components:** The web server stack (Apache/PHP) had not been updated for an extended period.

---

## 2. Scope & Methodology

*   **Scope:** Main domain and sub-paths (e.g., `/admin/`, `/products.php`).
*   **Methodology:** Manual testing based on the **OWASP Web Security Testing Guide (WSTG)**.
*   **Tools Used:** Burp Suite (Community), Nmap, Browser DevTools.
*   **Limitations:** Destructive attacks (e.g., DoS), heavy automated scanning, and mass brute-forcing were excluded in accordance with ethical rules and the agreed scope.

---

## 3. Findings Summary

| # | Vulnerability Name | Severity | Status |
| :--- | :--- | :--- | :--- |
| 1 | Exposed Network Services | **High** | ✅ Confirmed |
| 2 | Outdated Components | **Medium** | ✅ Confirmed |
| 3 | Weak CAPTCHA Implementation | **Low/Medium** | ✅ Confirmed |
| 4 | Injection & Auth Bypass Tests | - | ❌ Negative (Secure) |

---

## 4. Detailed Findings

### 🔴 Finding 1: Exposed Network Services

*   **Severity:** High (CVSS: ~7.5)
*   **Description:** During network reconnaissance, several sensitive ports were found accessible directly from the internet.
*   **Evidence:**
    *   Port **21 (FTP - Pure-FTPd):** Open.
    *   Port **22 (SSH - OpenSSH 8.0):** Open.
    *   Port **3306 (MySQL 8.0):** **Open.**
*   **Impact:** Exposing the database port (3306) allows attackers to perform brute-force attacks directly against the database. Open FTP and SSH ports also significantly increase the attack surface.
*   **Remediation:** The MySQL database and FTP service must be blocked via firewall and restricted to internal networks or specific admin IPs. SSH access should also be IP-restricted.

### 🟠 Finding 2: Outdated Components

*   **Severity:** Medium
*   **Description:** The web server infrastructure (Apache/PHP) and the administrative panel had not been updated for at least 3 years.
*   **Impact:** Outdated software versions typically contain known vulnerabilities (CVEs) that attackers can exploit (e.g., for Remote Code Execution).
*   **Remediation:** Immediately update all server components (Apache, PHP, MySQL) to the latest stable versions.

### 🟡 Finding 3: Weak CAPTCHA Implementation

*   **Severity:** Low
*   **Description:** The admin login page used a simple math CAPTCHA (e.g., `6 - 2 =`).
*   **Impact:** This type of CAPTCHA is highly vulnerable to automated attacks (e.g., OCR or scripted math solving) and can facilitate brute-force attacks on the admin panel.
*   **Remediation:** Replace the math CAPTCHA with standard services like Google reCAPTCHA or complex image-based CAPTCHAs.

---

## 5. Tests Performed & Negative Results

The following tests were conducted, and the system was found **not vulnerable**:

*   **SQL Injection:** Parameters (`id`, `cat`) and the admin login form were tested with various payloads (including Time-based `SLEEP`). The WAF and Prepared Statements prevented injection.
*   **Cross-Site Scripting (XSS):** The contact form and news parameters were tested for Reflected and Stored XSS. Inputs are properly sanitized.
*   **Host Header Injection:** Modifying the `Host` header in the password reset flow resulted in a `421 Misdirected Request`, indicating proper server configuration.
*   **Broken Access Control (IDOR):** Access to `/admin/uploads/` and `/phpmyadmin/` was tested and resulted in `403 Forbidden`.
*   **Clickjacking:** The `X-Frame-Options` header is set to `DENY`.
*   **User Enumeration:** The password reset form returned identical responses for valid and invalid emails.

---

Evidence: The Last-Modified header of static assets (e.g., fonts) dates back to 2010, indicating that the infrastructure has not been updated for over a decade."

## 6. Conclusion & Recommendations

The web application's code and business logic are in an acceptable security state, resisting common OWASP attacks. **The primary risk lies in the infrastructure and server layer.**

**Remediation Priorities:**
1. **Urgent:** Block port 3306 (MySQL) from public access via firewall.
2. **Urgent:** Update Apache and PHP to the latest versions.
3. **Medium:** Replace the math CAPTCHA with a secure alternative.
4. **Recommended:** Restrict access to FTP and SSH ports.

---

## 7. Ethical Disclaimer

All tests documented in this report were conducted with explicit written authorization from the system owner, within the framework of ethical hacking. No data was destroyed, modified, or stolen. The target's identity has been redacted to respect the client's confidentiality.
