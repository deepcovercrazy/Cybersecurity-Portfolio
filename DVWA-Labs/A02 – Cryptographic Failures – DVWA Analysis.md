# 🔐 A02 – Cryptographic Failures – DVWA Analysis

> 🔎 This report documents the discovery and analysis of common cryptographic vulnerabilities in the Damn Vulnerable Web Application (DVWA) during manual testing in a controlled Kali Linux environment.

---

## 📁 Environment

| Component           | Value                      |
|--------------------|----------------------------|
| OS                 | Kali Linux                 |
| Target App         | DVWA (Low Security Level)  |
| Testing Tools      | Firefox + Burp Suite       |
| Interception Proxy | Burp Suite (Community)     |

---

## 1️⃣ Password Hashing Analysis

By accessing the DVWA database through MySQL, we retrieved the stored password hashes from the `users` table:

```sql
SELECT user, password FROM users;
```
🔍 Observation:

| Username | Password Hash                    |
| -------- | -------------------------------- |
| admin    | 5f4dcc3b5aa765d61d8327deb882cf99 |
| gordonb  | e99a18c428cb38d5f260853678922e03 |
| 1337     | 8d3533d75ae2c3966d7e0d4fcc69216b |

These are MD5 hashes, which are:

❌ Outdated and insecure

❌ Not salted

❌ Easily cracked using rainbow tables or online hash databases

DVWA configuration file showing database settings

![DVWA ID](Screenshots/DVWA%20ID.png)

## 2️⃣ Cleartext Login Transmission
While submitting the login form using Burp Suite interception, we captured the following HTTP request:

POST /DVWA/login.php HTTP/1.1
Host: 127.0.0.1
Content-Type: application/x-www-form-urlencoded

username=admin&password=password&Login=Login

🔍 Findings:
❌ Credentials are sent in cleartext (no encryption)

❌ Communication occurs over HTTP, not HTTPS

❌ Vulnerable to Man-in-the-Middle (MitM) attacks

![Burp](Screenshots/Burp.png)

✅ Mitigation: Always enforce HTTPS and use TLS to encrypt sensitive data during transmission.

## 3️⃣ Insecure Session Cookie
After successful login, the application sets the following cookie:

Set-Cookie: PHPSESSID=072856bbb613c602293cd4f47c31cba0; path=/

🔍 Security Review:

| Attribute  | Present | Risk                             |
| ---------- | ------- | -------------------------------- |
| `Secure`   | ❌       | Can be transmitted over HTTP     |
| `HttpOnly` | ❌       | Accessible via JavaScript (XSS)  |
| `SameSite` | ❌       | No protection against CSRF       |
| `Entropy`  | Medium  | Potentially guessable session ID |

![Admin](Screenshots/Admin.png)

![systeminfo](Screenshots/systeminfo.png)
## 🧠 Summary of Findings

| Vulnerability                    | Status |
| -------------------------------- | ------ |
| Weak password hashing (MD5)      | ✅    |
| Login transmitted over HTTP      | ✅    |
| Plaintext credentials in transit | ✅     |
| Insecure session cookie          | ✅    |

✅ Recommendations
🔐 Replace MD5 with a strong algorithm like bcrypt or Argon2

🌐 Enforce HTTPS and implement HSTS

🍪 Set cookie attributes: Secure, HttpOnly, and SameSite=Strict

🧰 Use session expiration, rotation, and CSRF protection

📅 Date: July 2025

🧪 Report by: M.M

🔬 Lab: DVWA (Localhost - Kali Linux)
