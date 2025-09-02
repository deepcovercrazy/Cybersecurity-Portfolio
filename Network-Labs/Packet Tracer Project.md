###  `cisco-packet-tracer-network-lab.md`

```
markdownCopyEdit# 🧩 Cisco Packet Tracer – Basic Network Design Lab

## 🎯 Objective

To practice and demonstrate foundational knowledge of network design and communication using **Cisco Packet Tracer**, including:
- Device configuration  
- Cable connections  
- IP assignment  
- Network testing (ping)  
- Packet flow analysis (simulation mode)

---

## 🧱 Lab Topology

### 🔧 Devices Used:
- 2x **PCs** (PC0, PC1)
- 1x **Switch**
- Copper Straight-Through cables

### 🖥️ Physical Diagram:



[ PC0 ]──┐

│

[ Switch ]

│

[ PC1 ]──┘

```



---

## 1️⃣ Step One: Connect Devices via FastEthernet

- Open **Cisco Packet Tracer**
- Drag and drop:
  - 2 PCs
  - 1 Switch
- Use **Copper Straight-Through** cables:
  - PC0 (FastEthernet0) → Switch (FastEthernet0/1)
  - PC1 (FastEthernet0) → Switch (FastEthernet0/2)

![Packet1](Screenshots/Packet1.png)
---

## 2️⃣ Step Two: Configure Static IP Addresses

### PC0 Configuration:
```

IP Address: 192.168.1.10

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.1.1

```

### PC1 Configuration:
```

IP Address: 192.168.1.11

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.1.1

```

➡️ Configuration done via:
- PC → Desktop Tab → IP Configuration

![Packet23](Screenshots/Packet23.png)

![Packet22](Screenshots/Packet22.png)

---

## 3️⃣ Step Three: Test Network Connectivity

Use the command prompt inside each PC:

### On PC0:
```bash
ping 192.168.1.11
```

### On PC1:

```
ping 192.168.1.10
```
✅ Expected Result: All pings are **successful** — the network is functioning correctly.

![Packet3](Screenshots/Packet3.png)




* * *

## 4️⃣ Step Four: Observe Traffic in Simulation Mode

### 🎥 Simulation Steps:

- Switch to **Simulation Mode**
- Use `ping` from PC0 to PC1
- Watch the packet flow:

    - ARP request/response
    - ICMP Echo request/reply

### 🔍 Packet Types Observed:

- ARP (Address Resolution Protocol)
- ICMP (Ping)

🧠 This step helped in visualizing how data flows across a network and how Layer 2 & Layer 3 protocols interact.

![Packet2](Screenshots/Packet2.png)

* * *

## ✅ Summary of What Was Learned

| Key Concepts | Description |
| --- | --- |
| Network Design | Creating a functional LAN topology |
| IP Configuration | Assigning static IPs to end devices |
| Packet Analysis | Viewing packet movement using Simulation Mode |
| Connectivity Testing | Validating design with `ping` commands |


* * *

> 
> 
> 💡 *“This hands-on lab demonstrated how to design and troubleshoot a basic LAN environment. It built foundational skills essential for real-world networking and more advanced labs involving routing, VLANs, and network security.”*
> 

```
yamlCopyEdit
```