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
description WAN_LINK_TO_Dubai_ISP
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

interface Serial0/3/1
description WAN_LINK_TO_India_ISP
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
description WAN_LINK_TO_SRILANKA
 ip address 10.0.0.6 255.255.255.252
 no shutdown
exit

interface Serial0/3/1
description WAN_LINK_TO_Qatar
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
description WAN_LINK_TO_India_ISP
 ip address 10.0.0.10 255.255.255.252
 no shutdown
exit

interface Serial0/3/1
description WAN_LINK_TO_Dubai_ISP
 ip address 10.0.0.13 255.255.255.252
 clock rate 64000
 no shutdown
exit

router ospf 1
 network 10.0.0.0 0.0.255.255 area 0
 network 200.4.4.0 0.0.0.3 area 0
exit
```

---

# 🏢 ROUTER - CONFIGURATION

## 🏙️ Dubai HQ Router (HQ_Dubai_R)
```cisco
enable
configure terminal
hostname HQ_Dubai_R
```

### 1. WAN Interface (Public Internet)
```cisco
interface FastEthernet0/0
description CONNECTED_TO_Dubai_ISP
 ip address 200.1.1.2 255.255.255.252
 ip nat outside
 no shutdown
exit
```

### 2. LAN Interface (Connected to Core Switch)
```cisco
interface FastEthernet0/1
description CONNECTED_TO_CORE_L3_SWITCH
 ip address 10.1.99.1 255.255.255.252
 ip nat inside
 no shutdown
exit
```

### 🔐 3. VPN Tunnels (GRE to Branches)
```cisco
! Tunnel to Sri Lanka
interface Tunnel0
description VPN_TO_SRILANKA
 ip address 172.16.0.1 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.2.2.2
 no shutdown

! Tunnel to India
interface Tunnel1
description VPN_TO_INDIA
 ip address 172.16.0.5 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.3.3.2
 no shutdown

! Tunnel to Qatar
interface Tunnel2
description VPN_TO_QATAR
 ip address 172.16.0.9 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.4.4.2
 no shutdown
```

### 🌐 4. Routing (OSPF)
```cisco
router ospf 1
! Share Tunnel Networks
 network 172.16.0.0 0.0.255.255 area 0
 redistribute static subnets
exit
```

### 5. Static Routing for LAN
```cisco
! Route internal traffic to the Core Switch
ip route 10.1.0.0 255.255.0.0 10.1.99.2

! Default Route to Internet
ip route 0.0.0.0 0.0.0.0 200.1.1.1
```

### 🌐 6. Telephony Service (VoIP)
```cisco
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

## 🏙️ Sri lanka BR Router (BR_SL_R)
```cisco
enable
configure terminal
hostname BR_SL_R
```

### 1. WAN Interface (Public Internet)
```cisco
interface FastEthernet0/0
description CONNECTED_TO_SL_ISP
ip address 200.2.2.2 255.255.255.252
 no shutdown
 exit
```

### 2. LAN Interface (Connected to Switch)
```cisco
interface FastEthernet0/1
 no shutdown

interface FastEthernet0/1.10
description DATA_Employee PCs & Laptops
 encapsulation dot1Q 10
 ip address 10.2.10.1 255.255.255.0

interface FastEthernet0/1.20
description Voice_IP Phones (VoIP)
 encapsulation dot1Q 20
 ip address 10.2.20.1 255.255.255.0

interface FastEthernet0/1.50
description Server_File/Web/DNS Servers
 encapsulation dot1Q 50
 ip address 10.2.50.1 255.255.255.0
exit
```

### 🔐 3. VPN Tunnel to HQ
```cisco
interface Tunnel0
 ip address 172.16.0.2 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.1.1.2
no shutdown
```

### 🌐 4. Routing (OSPF)
```cisco
router ospf 1
! Share Tunnel Networks
 network 172.16.0.0 0.0.255.255 area 0
 network 10.2.0.0 0.0.255.255 area 0
 exit
```

### 5. Default Routing for LAN
```cisco
! Default Route to Internet
ip route 0.0.0.0 0.0.0.0 200.2.2.1
```

### 🌐 6. Telephony Service (VoIP)
```cisco
telephony-service
 max-ephones 5
 max-dn 5
ip source-address 10.2.20.1 port 2000
 auto assign 1 to 5
exit

ephone-dn 1
 number 2001
ephone-dn 2
 number 2002
exit
```

### 🌐 7. DHCP
```cisco
ip dhcp pool SL_DATA
 network 10.2.10.0 255.255.255.0
 default-router 10.2.10.1
 dns-server 10.1.50.10

ip dhcp pool SL_VOICE
 network 10.2.20.0 255.255.255.0
 default-router 10.2.20.1
 option 150 ip 10.2.20.1
exit
do wr
```

## 🏙️ India BR Router (BR_IND_R)
```cisco
enable
configure terminal
hostname BR_IND_R
```

### 1. WAN Interface (Public Internet)
```cisco
interface FastEthernet0/0
description CONNECTED_TO_IN_ISP
ip address 200.3.3.2 255.255.255.252
 no shutdown
 exit
```

### 2. LAN Interface (Connected to Switch)
```cisco
interface FastEthernet0/1
 no shutdown

interface FastEthernet0/1.10
 encapsulation dot1Q 10
 ip address 10.3.10.1 255.255.255.0

interface FastEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.3.20.1 255.255.255.0

interface FastEthernet0/1.50
 encapsulation dot1Q 50
 ip address 10.3.50.1 255.255.255.0
exit
```

### 🔐 3. VPN Tunnel to HQ
```cisco
interface Tunnel1
 ip address 172.16.0.6 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.1.1.2
 no shutdown
```

### 🌐 4. Routing (OSPF)
```cisco
router ospf 1
! Share Tunnel Networks
 network 172.16.0.4 0.0.0.3 area 0
 network 10.3.0.0 0.0.255.255 area 0
exit
```

### 5. Default Routing for LAN
```cisco
! Default Route to Internet
ip route 0.0.0.0 0.0.0.0 200.3.3.1
```

### 🌐 6. Telephony Service (VoIP)
```cisco
telephony-service
 max-ephones 5
 max-dn 5
 ip source-address 10.3.20.1 port 2000
 auto assign 1 to 5
exit

ephone-dn 1
 number 3001
ephone-dn 2
 number 3002
exit
```

### 🌐 7. DHCP
```cisco
ip dhcp pool IND_DATA
 network 10.3.10.0 255.255.255.0
 default-router 10.3.10.1
 dns-server 10.1.50.10

ip dhcp pool IND_VOICE
 network 10.3.20.0 255.255.255.0
 default-router 10.3.20.1
 option 150 ip 10.3.20.1
exit
do wr
```

## 🏙️ Qatar BR Router (BR_IND_R)
```cisco
enable
configure terminal
hostname BR_QAT_R
```

### 1. WAN Interface (Public Internet)
```cisco
interface FastEthernet0/0
description CONNECTED_TO_QA_ISP
ip address 200.4.4.2 255.255.255.252
 no shutdown
exit
```

### 2. LAN Interface (Connected to Switch)
```cisco
interface FastEthernet0/1
 no shutdown

interface FastEthernet0/1.10
 encapsulation dot1Q 10
 ip address 10.4.10.1 255.255.255.0

interface FastEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.4.20.1 255.255.255.0

interface FastEthernet0/1.50
 encapsulation dot1Q 50
 ip address 10.4.50.1 255.255.255.0
exit
```

### 🔐 3. VPN Tunnel to HQ
```cisco
interface Tunnel2
 ip address 172.16.0.10 255.255.255.252
 tunnel source fa0/0
 tunnel destination 200.1.1.2
 no shutdown
```

### 🌐 4. Routing (OSPF)
```cisco
router ospf 1
! Share Tunnel Networks
 network 172.16.0.4 0.0.0.3 area 0
 network 10.3.0.0 0.0.255.255 area 0
exit
```

### 5. Default Routing for LAN
```cisco
! Default Route to Internet
ip route 0.0.0.0 0.0.0.0 200.3.3.1
```

### 🌐 6. Telephony Service (VoIP)
```cisco
telephony-service
 max-ephones 5
 max-dn 5
 ip source-address 10.3.20.1 port 2000
 auto assign 1 to 5
exit

ephone-dn 1
 number 3001
ephone-dn 2
 number 3002
exit
```

### 🌐 7. DHCP
```cisco
ip dhcp pool QAT_DATA
 network 10.4.10.0 255.255.255.0
 default-router 10.4.10.1
 dns-server 10.1.50.10

ip dhcp pool QAT_VOICE
 network 10.4.20.0 255.255.255.0
 default-router 10.4.20.1
 option 150 ip 10.4.20.1
exit
do wr
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
hostname HQ_Access_SW

! --- Uplink to cor switch ---
interface GigabitEthernet0/1
description TRUNK_TO_C_SW
 switchport mode trunk
 no shutdown
 exit

! --- PC & Phone Ports ---
! Data on VLAN 10, Voice on VLAN 20
interface range FastEthernet0/1-10
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 20
 spanning-tree portfast
 no shutdown
 exit

! --- Printer Ports ---
interface range FastEthernet0/11-15
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown
 exit
```

## Sri Lanka Switch
```cisco
enable
configure terminal
hostname BR_SL_SW

! --- Uplink to Router ---
interface GigabitEthernet0/1
description TRUNK_TO_ROUTER
 switchport mode trunk
 no shutdown
 exit

! --- PC & Phone Ports ---
! Data on VLAN 10, Voice on VLAN 20
interface range FastEthernet0/1-10
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 20
 spanning-tree portfast
 no shutdown
 exit

! --- Printer Ports ---
interface range FastEthernet0/11-15
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown
 exit

! --- Local Server Port ---
interface FastEthernet0/24
 switchport mode access
 switchport access vlan 50
 no shutdown
 exit
```

## India Switch
```cisco
enable
configure terminal
hostname BR_IN_SW

! --- Uplink to Router ---
interface GigabitEthernet0/1
description TRUNK_TO_ROUTER
 switchport mode trunk
 no shutdown
 exit

! --- PC & Phone Ports ---
! Data on VLAN 10, Voice on VLAN 20
interface range FastEthernet0/1-10
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 20
 spanning-tree portfast
 no shutdown
 exit

! --- Printer Ports ---
interface range FastEthernet0/11-15
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown
 exit

! --- Local Server Port ---
interface FastEthernet0/24
 switchport mode access
 switchport access vlan 50
 no shutdown
 exit
```

## Qatar Switch
```cisco
enable
configure terminal
hostname BR_QA_SW

! --- Uplink to Router ---
interface GigabitEthernet0/1
description TRUNK_TO_ROUTER
 switchport mode trunk
 no shutdown
 exit

! --- PC & Phone Ports ---
! Data on VLAN 10, Voice on VLAN 20
interface range FastEthernet0/1-10
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 20
 spanning-tree portfast
 no shutdown
 exit

! --- Printer Ports ---
interface range FastEthernet0/11-15
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown
 exit

! --- Local Server Port ---
interface FastEthernet0/24
 switchport mode access
 switchport access vlan 50
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
