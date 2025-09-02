## 🛡️ Penetration Testing Report: Exploiting Telnet & Samba on Metasploitable
## 🎯 Objective

* Gain unauthorized access to Metasploitable VM via Telnet and Samba vulnerabilities
* Escalate privileges to root
* Upload and execute a backdoor for persistent access
## 🛠️ Tools Used
| Tool           | Purpose                            |
| -------------- | ---------------------------------- |
| Nmap           | Network port scanning              |
| Telnet client  | Initial access to Metasploitable   |
| Metasploit     | Exploiting Samba usermap\_script   |
| msfvenom       | Generating backdoor payload        |
| wget / python3 | Uploading backdoor via HTTP server |

## 🔍 Step 1: Network Reconnaissance with Nmap
* Scanned target IP 192.168.1.184 to discover open ports

* Found port 23 (Telnet) open, which allows simple login without password protection
## 🔐 Step 2: Telnet Access
* Connected to Telnet using default credentials: msfadmin:msfadmin

* Gained basic shell access on the target machine
## ⚙️ Step 3: Privilege Escalation with Samba Usermap Script Exploit
* Used Metasploit’s exploit/multi/samba/usermap_script module

* Set RHOSTS to 192.168.1.184 and LHOST to attacker’s IP 192.168.1.182

* Exploited Samba vulnerability to open a command shell session as root

![Con](file:///C:/Users/moham/OneDrive/Pictures/Screenshots/Screenshot%20(185).png)
## 💻 Step 4: Exploring the Target System
* Ran various Linux commands to verify access and gather information:

* whoami -> confirmed root access

* uname -a -> kernel and system details

* ls -la /root -> listing root directory contents

![Who](file:///C:/Users/moham/OneDrive/Pictures/Screenshots/Screenshot%20(186).png)
## 🔄 Step 5: Uploading a Backdoor for Persistent Access
* Created a Linux Meterpreter reverse TCP backdoor using msfvenom

* Served the payload via Python’s HTTP server on attacker machine

* Downloaded and executed the backdoor on the target machine

* Listened with Metasploit exploit/multi/handler to get Meterpreter session
## 📝 Conclusion
* Successfully exploited Telnet for initial access

* Privilege escalation achieved via Samba usermap_script exploit

* Persistent access established with Meterpreter backdoor

* Demonstrated critical vulnerabilities on Metasploitable VM for learning and defense improvement