# 💥 Stored XSS Exploitation Report – DVWA (Low Security)

## 📘 Summary
This report documents a successful exploitation of a Stored Cross-Site Scripting (XSS) vulnerability in the Damn Vulnerable Web Application (DVWA). The vulnerability allows execution of arbitrary JavaScript code when a stored input is rendered back to users.

---

## 🧪 Target Information

- **Target Application:** DVWA (Damn Vulnerable Web App)
- **Vulnerability Type:** Stored Cross-Site Scripting (XSS)
- **Security Level:** Low
- **Test URL:** `http://localhost/DVWA/vulnerabilities/xss_s/`

---

## 🧭 Steps to Reproduce

### 1. Navigate to XSS (Stored) Page

DVWA → XSS (Stored)


The page displays a form:

- **Name:**  
  `[ __________ ]`

- **Message:**  
  `[ __________ ]`

---

### 2. Inject JavaScript Payload

- **Name:** `attacker`
- **Message:** `<script>alert('Stored XSS')</script>`

Click on **Sign Guestbook**

---

### 3. Observation

Immediately after submission, the injected script was **stored in the guestbook** and **executed when rendered** to the page.

![stored xss alert.png](Screenshots/stored-xss-alert.png.png)

This confirms that the application does **not sanitize stored inputs before rendering**, making it vulnerable to persistent XSS attacks.

---

## ⚠️ Risk Analysis

- A malicious user could inject a script that steals session cookies, defaces the UI, or performs unauthorized actions on behalf of other users.
- Since the input is stored and displayed to all visitors, the attack persists until manually removed.

---

## 🔐 Recommendations

To mitigate Stored XSS:

- Sanitize all user input before storing it in the database.
- Escape all dynamic output when rendering HTML.
- Use Content Security Policy (CSP) headers to restrict script execution.
- Validate input length and allowed characters.

---

## 📎 Notes

This test was conducted on a local Kali Linux environment with DVWA intentionally configured for vulnerability testing. All actions are part of a legal and ethical training project.
