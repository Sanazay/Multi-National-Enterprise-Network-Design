# 🌐 GLOBAL CONNECT ENTERPRISE NETWORK CONFIGURATION

---

### 🧑‍💼 Project Architect
**MS. Sanaullah**

---

### 🗺️ Network Topology
**Dubai HQ** connected with **3 International Branches**  
📍 Sri Lanka • 🇮🇳 India • 🇶🇦 Qatar  

Connectivity is established using **GRE Tunnels** over a **simulated ISP WAN**.

---

## 🚀 ISP (WAN) ROUTERS
These **4 ISP routers** simulate the **Internet Backbone**, running **OSPF – Area 0**.

---

## 🌍 ISP 1 — Dubai Provider Configuration

> **Role:** Simulated ISP WAN Router  
> **Routing Protocol:** OSPF (Area 0)

```
enable
configure terminal
hostname ISP1
service password-encryption

! --- Link to Customer (Dubai HQ) ---
interface FastEthernet0/0
 description WAN_LINK_TO_DUBAI_HQ
 ip address 200.1.1.1 255.255.255.252
 no shutdown
 exit

! --- Backbone Link to ISP 2 ---
interface Serial0/0/0
 description CORE_LINK_TO_ISP2
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown
 exit

! --- Backbone Link to ISP 4 ---
interface Serial0/0/1
 description CORE_LINK_TO_ISP4
 ip address 10.0.0.14 255.255.255.252
 no shutdown
 exit

! --- OSPF Backbone Routing ---
router ospf 1
 ! Advertise internal ISP links
 network 10.0.0.0 0.0.255.255 area 0
 ! Advertise Customer Public IP (Crucial for VPN Tunnels)
 network 200.1.1.0 0.0.0.3 area 0
 exit
```

---

## 🌍 ISP 1 — Dubai Provider Configuration

