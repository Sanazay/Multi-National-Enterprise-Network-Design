# 🌐 Global Connect – Enterprise Network Configuration

> **A secure, scalable enterprise WAN design connecting a Dubai Headquarters with international branch offices using industry-standard routing and tunneling technologies.**

---

## 🗺️ Network Overview

🏢 **Dubai Headquarters**
🌍 Connected with **3 International Branches**:

* 🇱🇰 **Sri Lanka**
* 🇮🇳 **India**
* 🇶🇦 **Qatar**

🔗 Connectivity is established using **GRE Tunnels** over a **simulated ISP WAN**, ensuring **secure inter-branch communication**.

---

## 🚀 ISP (WAN) Infrastructure

* 🌐 **4 ISP Routers** simulate the **Internet Backbone**
* 🔁 **Dynamic Routing:** OSPF
* 🧭 **OSPF Area:** Backbone (**Area 0**)
* 🏗️ Designed for **redundancy, scalability, and real-world enterprise simulation**

---

# 🌍 ISP - (Service Provider) CONFIGURATION

**Role:** Simulated ISP WAN Router
**Routing Protocol:** OSPF (Area 0)
**Function:** Core connectivity between international ISPs


## ⚙️ Dubai ISP Router
```cisco
enable
configure terminal
hostname Dubai_ISP1
service password-encryption

! --- Link to Customer (Dubai HQ) ---
interface FastEthernet0/0
description WAN_LINK_TO_DUBAI_HQ
 ip address 200.1.1.1 255.255.255.252
 no shutdown
 exit

! --- Backbone Link to Sri Lanka ISP ---
interface Serial0/3/0
description CORE_LINK_TO_SRI_LANKA_ISP
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown
 exit

! --- Backbone Link to Qatar ISP ---
interface Serial0/3/1
description CORE_LINK_TO_QATAR_ISP
 ip address 10.0.0.14 255.255.255.252
 no shutdown
 exit

! --- OSPF Backbone Configuration ---
router ospf 1
 network 10.0.0.0 0.0.255.255 area 0
 network 200.1.1.0 0.0.0.3 area 0
 exit
```


## ⚙️ Sri Lanka ISP Router 
```cisco
enable
configure terminal
hostname Srilanka_ISP1

interface FastEthernet0/0
description WAN_LINK_TO_SRILANKA
 ip address 200.2.2.1 255.255.255.252
 no shutdown
 exit

interface Serial0/3/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
 exit

interface Serial0/3/1
 ip address 10.0.0.5 255.255.255.252
 clock rate 64000
 no shutdown
 exit

router ospf 1
 network 10.0.0.0 0.0.255.255 area 0
 network 200.2.2.0 0.0.0.3 area 0
 exit
```


## ⚙️ India ISP Router
```cisco
enable
configure terminal
hostname India_ISP1

interface FastEthernet0/0
description WAN_LINK_TO_INDIA
 ip address 200.3.3.1 255.255.255.252
 no shutdown
 exit

interface Serial0/3/0
 ip address 10.0.0.6 255.255.255.252
 no shutdown
 exit

interface Serial0/3/1
 ip address 10.0.0.9 255.255.255.252
 clock rate 64000
 no shutdown
 exit

router ospf 1
 network 10.0.0.0 0.0.255.255 area 0
 network 200.3.3.0 0.0.0.3 area 0
 exit
```


## ⚙️ Qatar ISP Router
```cisco
enable
configure terminal
hostname Qatar_ISP1

interface FastEthernet0/0
 description WAN_LINK_TO_QATAR
 ip address 200.4.4.1 255.255.255.252
 no shutdown
 exit

interface Serial0/3/0
 ip address 10.0.0.10 255.255.255.252
 no shutdown
 exit

interface Serial0/3/1
 ip address 10.0.0.13 255.255.255.252
 clock rate 64000
 no shutdown
 exit

router ospf 1
 network 10.0.0.0 0.0.255.255 area 0
 network 200.4.4.0 0.0.0.3 area 0
 exit
```

### 🔐 Key Technologies Used

* ✅ Cisco IOS Routing
* ✅ OSPF (Backbone – Area 0)
* ✅ GRE Tunneling
* ✅ Enterprise WAN Simulation
* ✅ ISP-Level Network Design

---


# 🏢 ROUTER - CONFIGURATION

## 🏙️ Dubai HQ Router (HQ_Dubai_R)
```cisco
enable
configure terminal
hostname HQ_Dubai_R

! WAN Interface
interface FastEthernet0/0
description CONNECTED_TO_Dubai_ISP
 ip address 200.1.1.2 255.255.255.252
 ip nat outside
 no shutdown
 exit

! LAN Interface
interface FastEthernet0/1
description CONNECTED_TO_CORE_L3_SWITCH
 ip address 10.1.99.1 255.255.255.252
 ip nat inside
 no shutdown
 exit
```

## 🔐 GRE VPN Tunnels
```cisco
interface Tunnel0
 ip address 172.16.0.1 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.2.2.2
 no shutdown

interface Tunnel1
 ip address 172.16.0.5 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.3.3.2
 no shutdown

interface Tunnel2
 ip address 172.16.0.9 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.4.4.2
 no shutdown
```

## 🌐 Routing & VoIP
```cisco
router ospf 1
 network 172.16.0.0 0.0.255.255 area 0
 redistribute static subnets
 exit

ip route 10.1.0.0 255.255.0.0 10.1.99.2
ip route 0.0.0.0 0.0.0.0 200.1.1.1

telephony-service
 max-ephones 10
 max-dn 10
 ip source-address 10.1.99.1 port 2000
 auto assign 1 to 10
 exit

ephone-dn 1
 number 1001
ephone-dn 2
 number 1002
```

---

# 🧠 CORE L3 SWITCH CONFIGURATION

## Dubai HQ L3 Switch
```cisco
enable
configure terminal
hostname HQ_Core_SW
ip routing
```

## VLANs & SVIs
```cisco
vlan 10
 name DATA_VLAN
vlan 20
 name VOICE_VLAN
vlan 30
 name PRINT_VLAN
vlan 50
 name SERVER_FARM
```

```cisco
interface vlan 10
 ip address 10.1.10.1 255.255.255.0
 no shutdown

(Same structure for VLAN 20, 30, 50)
```

---

# 🔀 SWITCH CONFIGURATION

## Dubai Switch
```cisco
enable
configure terminal
hostname HQ_DU_SW

interface GigabitEthernet0/1
description TRUNK_TO_HQ_QAT_R
 switchport mode trunk
 no shutdown
 exit
```


---
### 👨‍💼 Project Architect
**MS. Sanaullah**
---

⭐ *If you find this project useful, don’t forget to star the repository!*
🤝 *Contributions, feedback, and improvements are always welcome.*

---
