# 🛡️ SecureMart Ltd. – Logging and Monitoring Policy

> 📌 \*\*Document Version:\*\* 1.0  
> 🏢 \*\*Organization:\*\* SecureMart Ltd.  
> 📆 \*\*Date:\*\* July 2025  
> 🔐 \*\*Policy Owner:\*\* Cybersecurity Analyst (Moh!)

---

## 🎯 1. Purpose

The purpose of this policy is to define the logging and monitoring strategy used within SecureMart Ltd. to detect, respond to, and prevent security incidents through effective audit trail management and log review.

---

## 🧱 2. Scope

✅ Applies to:  
- All Windows Server and Linux-based infrastructure  
- Application servers and network devices  
- Security tools such as auditd and Windows Event Viewer

---

## 📚 3. Policy Statements

### 🔍 3.1 Log Collection

- All systems must have logging enabled for key activities, including:
  - User login/logout events (Success/Failure)
  - Privilege escalation and sudo commands
  - System configuration changes
  - Application start/stop events
  - File access on critical directories

- \*\*Linux Systems (Kali Linux):\*\*  
  Use `auditd` with custom rules such as:
  ```bash
  sudo auditctl -a always,exit -F arch=b64 -S execve -k exec\_log
  - **Windows Systems:**

Enable auditing via Group Policy and monitor:

    - `Event ID 4624` – Successful logon
    - `Event ID 4625` – Failed logon
    - `Event ID 4688` – Process creation

* * *
![Win1](file:///C:/Users/moham/OneDrive/Desktop/Win1.png)
### 🕒 3.2 Log Retention

- 🔐 Security logs must be retained for **minimum 90 days**.
- Backups must be encrypted and stored securely.
- Rotations must be scheduled weekly on non-critical hours.

* * *

### ⚠️ 3.3 Alerts and SIEM Integration

- Logs from critical systems must be forwarded to a central SIEM (e.g. Wazuh or Splunk).
- Alerts should be generated for:

    - Multiple failed login attempts
    - Unexpected privilege escalation
    - Unauthorized access to sensitive files

* * *

### 👥 3.4 Roles & Responsibilities

| Role | Responsibility |
| --- | --- |
| SysAdmin | Ensure log configuration and backup |
| Security Analyst | Monitor logs, investigate anomalies |
| Compliance Officer | Review logging policies quarterly |



* * *

## ✅ 4. Compliance & Enforcement

Failure to comply with this policy may result in disciplinary action and will be treated as a violation of SecureMart's information security standards.

* * *

## 📌 5. Review Schedule

| Reviewed By | Review Date | Next Review |
| --- | --- | --- |
| Security Analyst | July 2025 | Jan 2026 |

