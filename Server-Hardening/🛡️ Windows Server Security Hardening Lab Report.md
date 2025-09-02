# 🛡️ Windows Server Security Hardening Lab Report

## 📘 Overview
This lab demonstrates a full \*\*Windows Server security hardening process\*\* with configuration, firewall management, group policy enforcement, and PowerShell scripting.

---

## ✅ 1. Initial Configuration

- 🖥️ Windows Server successfully installed on VirtualBox/VMware
- Set static IP address and system hostname
- Enabled Remote Desktop (RDP) for remote management


![Host](file:///C:/Users/moham/OneDrive/Desktop/Host.png)
---

## 🔥 2. Windows Firewall Configuration

### 🛠️ Checked active firewall rules:
```powershell
Get-NetFirewallRule | Where-Object {$\_.Enabled -eq "True"}
```

### ⛔ Blocked unused ports (example: TCP 445):
```powershell
New-NetFirewallRule -DisplayName "Block SMB Port 445" -Direction Inbound -LocalPort 445 -Protocol TCP -Action Block
```

### 🔐 Restricted RDP to Kali Linux only:
```powershell
New-NetFirewallRule -DisplayName "Allow RDP from Kali" -Direction Inbound -Protocol TCP -LocalPort 3389 -RemoteAddress 192.168.56.102 -Action Allow
```

📄 Exported firewall rules for documentation:
```powershell
Get-NetFirewallRule | Where-Object {$\_.Enabled -eq "True"} > firewall\_rules.txt
```

![Cod1](file:///C:/Users/moham/OneDrive/Desktop/Cod1.png)
![Cod2](file:///C:/Users/moham/OneDrive/Desktop/Cod2.png)
![Cod3](file:///C:/Users/moham/OneDrive/Desktop/Cod3.png)
---

## 🔐 3. Group Policy Security Hardening

### 📂 Accessed via:
```
gpedit.msc
```

### 🔑 Password Policy:
- Minimum password length: \*\*10 characters\*\*
- Enforce password history: \*\*5 passwords\*\*
- Maximum password age: \*\*60 days\*\*
- Password complexity: \*\*Enabled\*\*

### 🔐 Account Lockout Policy:
- Lockout threshold: \*\*3 attempts\*\*
- Lockout duration: \*\*15 minutes\*\*
- Reset counter: \*\*15 minutes\*\*

✅ Applied policies with:
```powershell
gpupdate /force
```

![Cod6](file:///C:/Users/moham/OneDrive/Desktop/Cod6.png)
![Cod8](file:///C:/Users/moham/OneDrive/Desktop/Cod8.png)

![Cod9](file:///C:/Users/moham/OneDrive/Desktop/Cod9.png)

## ⚙️ 4. PowerShell Security Script

A basic auditing script was created to document service and firewall status:

```powershell
# audit\_script.ps1
Get-Service | Where-Object {$\_.Status -eq "Running"} > running\_services.txt
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, State > open\_ports.txt
Get-NetFirewallRule | Where-Object {$\_.Enabled -eq "True"} > firewall\_rules.txt
```

📂 Files generated:
- `running_services.txt`
- `open_ports.txt`
- `firewall_rules.txt`

![Cod455](file:///C:/Users/moham/OneDrive/Desktop/Cod455.png)

![Rules](file:///C:/Users/moham/OneDrive/Desktop/Rules.png)
---

## 🧾 Summary

This hardening lab demonstrated:

- 🔐 Realistic Windows Server configuration
- 🧱 Host firewall hardening using PowerShell
- ⚖️ Group Policy enforcement for password & lockout rules
- 📄 Scripted system auditing for documentation



