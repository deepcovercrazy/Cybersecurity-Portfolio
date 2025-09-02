# 🛡️ A06 – Vulnerable and Outdated Components Assessment Report

---

## 🎯 **Objective**

Identify and analyze vulnerable or outdated software components and services running on the Linux system to assess security risks and recommend updates or mitigations.

---

## 🛠️ **Steps Performed**

### 1. 🚀 **List Running Services**

Executed the following command to list all active services on the system:

```bash
sudo systemctl list-units --type=service --state=running
```
![Rep1](Screenshots/Rep1.png)
![Rep2](Screenshots/Rep2.png)
![Rep3](Screenshots/Rep3.png)
### 2. 🔍 Check Versions of Key Services
Identified and retrieved versions of critical services:

| Service        | Version |
| -------------- | ------- |
| NetworkManager | 1.52.0  |
| lightdm        | 1.32.0  |
| ModemManager   | 1.24.0  |
| systemd        | 257.6-1 |

Commands used:

NetworkManager --version

lightdm --version

ModemManager --version

systemd --version

![Rep4](Screenshots/Rep4.png)
### 3. 📚 Review Installed System Packages
Used package management to list systemd and related libraries versions:

dpkg -l | grep systemd

(For Debian/Ubuntu-based systems)
![Rep5](Screenshots/Rep5.png)
### 4. 🕵️‍♂️ Vulnerability Analysis
Researched publicly known CVEs for the identified versions via CVE databases (e.g., MITRE CVE, NVD).

Summary:

NetworkManager 1.52.0: No critical CVEs found.

lightdm 1.32.0: Minor low-risk CVEs exist, recommended update if server exposure is high.

ModemManager 1.24.0: No significant vulnerabilities reported.

systemd 257.6-1: Some minor vulnerabilities, generally patched with system updates.

### 5. 🔄 Recommendation
Keep all system packages updated regularly:

sudo apt update && sudo apt upgrade

Use automated security auditing tools like Lynis for comprehensive system scans:

sudo apt install lynis

sudo lynis audit system

![Rep6](Screenshots/Rep6.png)

![Rep7](Screenshots/Rep7.png)

![Rep8](Screenshots/Rep8.png)
### 📌 Summary
This assessment confirms that the system’s critical components are running on reasonably recent versions with no immediate critical vulnerabilities detected. Continuous updates and regular security scans are recommended to maintain a secure environment.

