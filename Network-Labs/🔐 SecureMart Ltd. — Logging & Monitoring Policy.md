# 🔐 SecureMart Ltd. — Logging & Monitoring Policy  
\*Author: moh (Mohammad Mohammadi)\*  
\*Date: July 2025\*  

---

## 🎯 Objective  
To ensure that all critical system, application, and security events across SecureMart Ltd.'s infrastructure are logged, monitored, and analyzed to support threat detection, incident response, and compliance.

---

## 🏢 Company Background  
\*\*SecureMart Ltd.\*\* is a UK-based e-commerce company managing customer data, financial transactions, and sensitive product inventory information through a cloud-based platform and internal IT infrastructure.

---

## 📜 Scope  
This policy applies to:

- 🔹 Linux systems (Ubuntu, Kali)
- 🔹 Windows Server environments
- 🔹 Web and database servers
- 🔹 Security devices (firewalls, IDS)
- 🔹 Cloud services (Azure/AWS logging integrations)

---

## ⚖️ Compliance Frameworks  
This policy aligns with:

- ✅ [ISO/IEC 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) — Clause 12.4
- ✅ [NIST SP 800-53 Rev. 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) — AU family controls
- ✅ [CIS Controls v8](https://www.cisecurity.org/controls) — Control 8: Audit Log Management
- ✅ [PCI-DSS v4.0](https://www.pcisecuritystandards.org/) — Req. 10: Track and monitor all access to system components

---

## 🔍 What to Log  
| Event Type            | Example Logs                          | Importance      |
|----------------------|----------------------------------------|-----------------|
| User authentication  | Login success/failure, sudo activity  | High            |
| File access/mods     | `/etc/passwd` changes, uploads        | Critical        |
| Network activity     | Inbound SSH, unusual outbound traffic | High            |
| Application events   | Web requests, database queries        | Medium          |
| Security alerts      | IDS/IPS detections, virus scans       | Critical        |

---

## 🛠️ Tools & Configuration  
### 🔧 Linux (Ubuntu/Kali)
- `auditd`: For file monitoring and system calls
- `syslog`: Centralized logging
- `logrotate`: Retention management

### 🪟 Windows Server
- Event Viewer (Application, Security, System logs)
- PowerShell Logging
- Group Policy for log size/retention

---

## 🧪 Sample Configuration Snippet

### 📄 `audit.rules` (Linux Auditd)
```bash
-w /etc/passwd -p wa -k user-change
-w /var/log/ -p rwxa -k log-access
```
🪟 Windows PowerShell (Enable Log)

```
Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -Name "EnableScriptBlockLogging" -Value 1
```
📡 Centralized Logging (Optional)
Use Wazuh, Splunk, or Graylog for log collection, parsing, and alerting.

Configure remote agents on endpoints and send to a central log server.

⏳ Retention Policy

| Log Type            | Retention Period | Rotation |
| ------------------- | ---------------- | -------- |
| Authentication Logs | 12 months        | Weekly   |
| Application Logs    | 6 months         | Daily    |
| System Logs         | 12 months        | Weekly   |
| IDS Logs            | 18 months        | Monthly  |


## 🚨 Incident Response Integration

- Alerts triggered on:
> 
> 5 failed login attempts in 2 minutes
>
- Access to restricted directories
- Execution of suspicious binaries
- All alerts forwarded to **SIEM dashboard** and security team email.
## 🧩 Review and Audit
Quarterly audit log review

Random sample verification

Logs must be immutable (write-once policy where possible)

## 🧠 Analyst Notes
“Good logging is boring logging — if it’s exciting, you waited too long.”
— moh


## ✅ Summary
Implementing a robust logging and monitoring policy ensures:

✔️ Accountability

✔️ Threat detection

✔️ Compliance

✔️ Incident response readiness

Prepared by moh 🧠
Cyber Security Engineer & SIEM Enthusiast

