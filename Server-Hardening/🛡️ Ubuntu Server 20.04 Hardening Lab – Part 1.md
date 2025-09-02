# 🛡️ Ubuntu Server 20.04 Hardening Lab – Part 1

> 🔍 **Goal:** Secure a basic Ubuntu Server by creating non-root users, configuring SSH, and enabling a firewall.

---

## 🔹 Step 1: Create a New User 👤

```bash
sudo adduser secureuser
sudo usermod -aG sudo secureuser
🧠 This avoids using the `root` account for routine administration.



* * 
```
![User](file:///C:/Users/moham/OneDrive/Desktop/User.png)

## 🔹 Step 2: Disable Root SSH Login 🚫

* Edit the SSH configuration:

```
 nano /etc/ssh/sshd_config
```

* 🔧 Find and change the following line:

```
 PermitRootLogin no
```

* Restart the SSH service:

```
* sudo systemctl restart ssh
```

![Etc](file:///C:/Users/moham/OneDrive/Desktop/Etc.png)

## 🔹 Step 3: Change Default SSH Port 📮

Edit the same SSH config file:

```
* nano /etc/ssh/sshd_config
```

Change the port number (e.g., to `2222`):

```
bashCopyEditPort 2222
```

Restart the SSH service again:

```
bashCopyEditsudo systemctl restart ssh
```



## 🔹 Step 4: Enable UFW Firewall & Allow Required Ports 🔥

```
bashCopyEditsudo ufw allow 2222/tcp
sudo ufw allow 80,443/tcp
sudo ufw enable
sudo ufw status
```


![Rep44](file:///C:/Users/moham/OneDrive/Desktop/Rep44.png)


✅ **Progress Summary**

* <input disabled="" type="checkbox" checked=""> Created secure sudo user
* <input disabled="" type="checkbox" checked=""> Disabled root login via SSH
* <input disabled="" type="checkbox" checked=""> Changed default SSH port
* <input disabled="" type="checkbox" checked=""> Enabled and configured firewall with UFW

