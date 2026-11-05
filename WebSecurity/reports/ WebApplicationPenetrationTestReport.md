# Web Application Penetration Test Report 

**Target:** E-Commerce Website Security Assessment (Project X)

**Date:** November 2026

**Tester:** CRUSH-HUNTER

**Methodology:** OWASP Web Security Testing Guide (WSTG)

---

## 1. Executive Summary

A manual security assessment was performed on `Project X` based on the
OWASP WSTG framework. The application demonstrated resistance to SQL Injection
and User Enumeration attacks, protected by input filtering / WAF mechanisms.
Two low-to-medium severity findings were identified (weak admin panel CAPTCHA
and missing CSP header), along with one unconfirmed finding (potential XSS in
the feedback form). Several network services were also found exposed at the
hosting infrastructure level.

## 2. Scope & Limitations

- **Scope:** Main domain `Project X` and its public pages
- **Method:** Manual testing with Burp Suite and Nmap
- **Limitations:** No heavy automated scanning, mass brute-force, or destructive
  attacks were performed. Admin panel access was out of scope; dependent
  findings are reported as "unconfirmed."

## 3. Findings Summary

| # | Finding | Severity | Status |
|---|---|---|---|
| 1 | Exposed network services (hosting-level) | Medium | Confirmed |
| 2 | Weak math CAPTCHA on admin panel | Low | Confirmed |
| 3 | Potential Stored XSS in Feedback form | Informational | Unconfirmed |
| 4 | Missing Content-Security-Policy header | Low | Confirmed |
| 5 | SQL Injection in `id` parameter | — | Negative (Safe) |
| 6 | User Enumeration (login & forgot password) | — | Negative (Safe) |
| 7 | Information disclosure via 500 error | — | Negative (Safe) |
| 8 | Directory listing on `/admin/uploads/gallery/` | — | Negative (Safe) |

## 4. Detailed Findings

### Finding 1 — Exposed Network Services
**Severity:** Medium

An `Nmap` scan was performed:

\`\`\`
nmap -sV --top-ports 20 dreamskitchen.co.in
\`\`\`

Results:

\`\`\`
21/tcp   open  ftp     Pure-FTPd
22/tcp   open  ssh     OpenSSH 8.0 (protocol 2.0)
53/tcp   open  domain
80/tcp   open  http    Apache httpd
110/tcp  open  pop3    Dovecot pop3d
111/tcp  open  rpcbind
143/tcp  open  imap    Dovecot imapd
443/tcp  open  ssl/http Apache httpd
993/tcp  open  ssl/imap Dovecot imapd
995/tcp  open  ssl/pop3 Dovecot pop3d
1723/tcp open  tcpwrapped
3306/tcp open  mysql   MySQL 8.0.46-cll-lve
\`\`\`

The MySQL version string (`cll-lve`) indicates the server is hosted on a
CloudLinux/cPanel shared hosting environment. This suggests the open ports are
likely a result of the hosting provider's configuration rather than a direct
misconfiguration by the site owner.

**Impact:** Direct exposure of MySQL (3306) to the internet increases the
attack surface and allows direct brute-force attempts against the database.
FTP and SSH are similarly at risk if weak credentials are used.

**Remediation:** Restrict access to port 3306 to specific IPs or internal
networks via firewall rules. Follow up with the hosting provider.

### Finding 2 — Weak CAPTCHA on Admin Panel
**Severity:** Low
**Location:** `/admin/index.php`

The admin login form uses a simple math CAPTCHA (e.g., "6 − 2 = ?"). The login
error message ("Invalid username or password") is identical for both invalid
username and invalid password cases — no user enumeration via this vector.

**Impact:** Simple math CAPTCHAs are trivially solvable by scripts and provide
no real protection against automated brute-force attacks.

**Remediation:** Replace with reCAPTCHA or an equivalent solution, combined
with login attempt rate limiting.

### Finding 3 — Potential Stored XSS in Feedback Form
**Severity:** Informational — Unconfirmed
**Location:** `/feedback` (fields: `subject`, `message`)

The following payload was submitted:

\`\`\`
subject=<b>text</b>
message=<script>Hello</script>
\`\`\`

Values were accepted without visible filtering or error. The response page
(`thank-you.php`) does not reflect these values, so Reflected XSS is ruled
out. Since access to the admin panel (the likely display location for this data)
was out of scope, Stored XSS could not be confirmed or ruled out.

**Positive note:** The form uses randomized anti-CSRF tokens (hashed field
names).

**Remediation:** Internal review by the team with admin panel access; apply
output encoding to all displayed user-submitted data.


### Finding 4 — Open HTTP Methods (PUT / DELETE / PATCH)
**Severity:** High (Potential — Unconfirmed)

Requests with various HTTP methods against the root domain returned:

\`\`\`
GET      -> 200
OPTIONS  -> 200
PUT      -> 200
DELETE   -> 200
PATCH    -> 200
TRACE    -> 405 Method Not Allowed
\`\`\`

TRACE is correctly disabled (protection against Cross-Site Tracing). However,
a `200` response for PUT, DELETE, and PATCH requires deeper investigation: in
many PHP/mod_rewrite configurations, all requests are routed to a central
file regardless of method, and a `200` does not necessarily mean the file
write/delete operation actually executed.

**Testing limitation:** Confirming this definitively (sending a harmless file
via PUT and reading it back via GET) required explicit client authorization,
which was **requested and declined**. This check was therefore halted at this
stage.

**Impact (if actually exploitable):** Worst case, arbitrary file upload or
deletion on the server (including web shell upload) and potential Remote Code
Execution. Until confirmed or ruled out, this should be treated as **High**
risk.

**Remediation:** Disable unnecessary HTTP methods (PUT, DELETE, PATCH, TRACE)
at the web server configuration level, except where explicitly required.
Final confirmation of this finding by an internal team with a safe test
environment is strongly recommended.

### Finding 5 — .env File Present in Public Path
**Severity:** Informational

A request to `/.env` returned `403 Forbidden` (not `404`), indicating the
file **exists** on the server but direct access is blocked.

**Impact:** No immediate risk under current configuration, but the file's
presence in the public web root could lead to sensitive data exposure
(database credentials, API tokens) if server configuration changes or a
bypass is discovered.

**Remediation:** Move sensitive configuration files outside the web root
instead of relying solely on web-server-level blocking rules.

## Additional Negative Results
- **Clickjacking (active test):** An HTML page with an iframe targeting the
  site was built; the site failed to load inside the iframe, confirming
  `X-Frame-Options: Deny` works in practice, not just in theory.
- **TRACE method:** Blocked with `405 Method Not Allowed`; Cross-Site Tracing
  risk ruled out.

## Strength — TLS/SSL Configuration
SSL Labs assessment returned an **A+** rating. The server supports TLS 1.3
and long-duration HSTS (`max-age=31536000`), with a valid certificate
(RSA 2048, SHA256) and no weak keys or revocation issues.


### Finding 6 — Missing Content-Security-Policy Header
**Severity:** Low

Response headers lacked a CSP header, though the following were correctly
configured:

\`\`\`
Strict-Transport-Security: max-age=31536000
X-Frame-Options: Deny
Cache-Control: no-store, no-cache, must-revalidate
\`\`\`

**Remediation:** Add a Content-Security-Policy header as an additional
defense layer against XSS.

## 5. Negative Test Results (Verified Safe)

- **SQL Injection:** The `id` parameter in `newsdetail.php` showed no change
  in Content-Length with a single quote (`id=4'`, both 10346 bytes).
  Boolean-based payloads (`AND 1=1--`, `AND 1=2--`) were blocked with
  `400 Bad Request` by input filtering/WAF. Not exploitable.
- **User Enumeration (Login):** Identical error message for invalid
  username/password.
- **User Enumeration (Forgot Password):** Identical message ("Enter correct
  email id") for valid and invalid emails.
- **Information Disclosure:** The 500 error page on `/admin/uploads/`
  contains no technical details (no stack trace, no file paths).
- **Directory Listing:** `/admin/uploads/gallery/` returns `403 Forbidden`.
- **phpMyAdmin:** Default path `/phpMyAdmin` not found (`404`).

## 6. Conclusion & Remediation Priorities

The application shows acceptable security posture at the application-logic
level. The primary risk lies in infrastructure and network service
configuration.

1. **Urgent:** Restrict MySQL (3306) access via firewall to specific IPs
2. **Medium:** Internal review of the Feedback form to confirm/rule out Stored XSS
3. **Low:** Replace math CAPTCHA with reCAPTCHA
4. **Low:** Add Content-Security-Policy header

---

*Prepared by CRUSH-HUNTER- — November 2026 — based on the OWASP Web Security
Testing Guide (WSTG)*
data) was out of scope, Stored XSS could not be confirmed or ruled out.
