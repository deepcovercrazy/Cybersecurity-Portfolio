# 🔐 Security Audit Report – Internal Lab Network (Kali + Metasploitable)

---

## 🖥️ 1. Network Overview

This audit was conducted on a virtual lab environment composed of:

- 🧱 **Kali Linux (Attacker VM)** – used for penetration testing and analysis.
- 🎯 **Metasploitable 2 (Target VM)** – a vulnerable Linux-based server used for security training.
- 🌐 **Virtual Network** – isolated host-only network enabling controlled exploitation.

---

## 🛡️ 2. Security Controls Reviewed

### 📘 Password Policy

- Minimum 12 characters with complexity rules (uppercase, lowercase, digit, symbol).
- Passwords must be changed every 90 days.
- MFA is enforced where applicable.
- Password history: last 5 passwords cannot be reused.

### 📱 BYOD Policy

- Devices must have screen lock, antivirus, encryption, and remote wipe enabled.
- Rooted/jailbroken devices are prohibited.
- Only connect via VPN or secure Wi-Fi.
- Unauthorized app installations are not allowed.

---

## 📊 3. Risk Assessment (Risk Register)

| Risk ID | Asset             | Threat                      | Vulnerability                         | Likelihood | Impact | Risk Level | Mitigation                          |
|---------|-------------------|-----------------------------|----------------------------------------|------------|--------|-------------|--------------------------------------|
| R1      | Metasploitable VM | Unauthorized Access         | Default credentials                    | High       | High   | Critical    | Change default creds, restrict SSH   |
| R2      | Metasploitable VM | Privilege Escalation        | Unpatched local exploits               | High       | High   | Critical    | System patching, access controls     |
| R3      | Virtual Network   | Packet Sniffing / MITM      | No encryption (e.g. Telnet, FTP)       | Medium     | High   | High        | Use isolated network, Wireshark      |
| R4      | Kali Reports      | Information Leakage         | Plain-text storage of credentials      | Medium     | Medium | Medium      | Encrypt reports and credentials      |
| R5      | Kali VM           | Malware Infection           | Untrusted scripts/tools                | Medium     | High   | High        | Verify tool integrity                |
| R6      | SSH Access Logs   | Brute Force / Recon Attempts| No IP restrictions                     | Medium     | Medium | Medium      | Rate-limiting, Fail2Ban, IP whitelisting |

---
![Sc3](file:///C:/Users/moham/OneDrive/Desktop/Sc3.png)
## 🧾 4. Log Review (SSH Logs)

* Logs were collected from `journalctl` on the Kali VM.

### ✅ Successful Login:
```log
Accepted password for testuser from ::1 port 49274 ssh2
session opened for user testuser
```
### ❌ Failed Login Attempts:
```Failed password for testuser from ::1 port 48332 ssh2
authentication failure; PAM error: PAM_AUTHINFO_UNAVAIL
```
### 🛠️ SSH Service Activity:
```
Server listening on 0.0.0.0 port 22
Server listening on :: port 22
```
🔎 These logs indicate that SSH is running publicly on all interfaces and accepting password-based logins — which increases attack surface in real environments.
## 📌 6. Recommendations

* 🔒 Change all default credentials on lab machines.
* 🔥 Restrict SSH to specific IP ranges or local access only.
* 🛡️ Disable password-based authentication; use SSH keys.
* 👁️ Set up logging with real-time alerting (e.g., Fail2Ban).
* 📁 Encrypt sensitive reports and credential files.
* 🧽 Clear temporary logs/files after testing.
* 📚 Keep all systems up-to-date and patch known exploits.
## ✅ Conclusion

This audit covered multiple key aspects of information security:

* Identification of critical vulnerabilities
* Log review and incident evidence
* Implementation of basic security policies
* Risk documentation via Risk Register

> 
> 
> 🔐 While this environment was a controlled lab, the process reflects real-world practices used in internal audits, penetration tests, and compliance revie

