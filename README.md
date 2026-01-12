# 🌐 Multi-National Enterprise Network Project
### *GlobalConnect – Enterprise-Grade Network Simulation*

> **A comprehensive enterprise network design connecting a Dubai Headquarters with international branches in •Sri Lanka 🇱🇰 • India 🇮🇳 • Qatar 🇶🇦**


<p align="center">
  <img src="network_topology.png" alt="GlobalConnect Network Topology (pic will be update)" width="800">
</p>

---

## 📌 Project Overview
This project simulates a **real-world multi-national enterprise network** for **GlobalConnect**, showcasing modern networking principles and best practices.  
It is designed to reflect **enterprise-level WAN architecture, secure communications, and centralized service management**.

**🛠 Tools Used**  
- Cisco Packet Tracer **8.2**

**🔑 Key Technologies**  
- OSPF  
- GRE VPN Tunnels  
- VLANs & ROAS  
- DHCP  
- VoIP  
- NAT  

---

## 🏗️ Network Architecture

### 🏢 1. Headquarters — Dubai 🇦🇪
The **central hub** hosting the Core Layer and Enterprise Server Farm.

**Hardware**
- Cisco **2811 Router**
- Cisco **3560 Multilayer Switch**
- Cisco **2960 Access Switch**

| Category | Devices |
|--------|--------|
| Network Devices | Cisco 2811 Router, Cisco 3560 Multilayer Switch, Cisco 2960 Access Switch |
| End User Devices | PCs, Laptops |
| Communication | IP Phones, Analog Phones |
| Peripheral Devices | Printers |
| Servers | DNS, Web, File, DHCP Servers |


**VLAN Design**
| VLAN | Purpose |
|-----:|---------|
| 10 | Data |
| 20 | Voice |
| 30 | Print |
| 50 | Server Farm |

**Core Services**
- **Inter-VLAN Routing:** Handled by the Layer-3 switch  
- **Telephony:** CME (Call Manager Express) managing HQ extensions *(1001–1010)*  
- **Intranet Services:** Central DNS & Web Server  
  - `www.globalconnect.corp`

---

### 🌏 2. International Branches  
**Sri Lanka 🇱🇰 • India 🇮🇳 • Qatar 🇶🇦**

Remote branch offices connected securely to HQ via a simulated ISP WAN.

- **Connectivity:** Each branch connects through a dedicated ISP router  
- **Tunneling:**  
  - **GRE VPN Tunnels** enable private internal traffic *(10.x.x.x)* over public WAN links  
- **Local Services:**  
  - Each branch hosts a local **File Server**

---

### 🌍 3. ISP / WAN Layer
A **4-router ring topology** simulating the Internet backbone.

- **Routing Protocol:** OSPF **Area 0 (Backbone)**
- **High Availability:** Ring design ensures resilience during link failures

---

## ⚙️ Key Configurations & Features

### ✅ Dynamic Routing (OSPF)
- OSPF Area 0 deployed across HQ, Branches, and ISP routers  
- `redistribute static` configured at HQ to advertise Server Farm networks  
- GRE Tunnel interfaces participate in OSPF for seamless private routing

---

### 🔐 Secure VPN Connectivity (GRE)
Virtual point-to-point tunnels connecting HQ and Branches:

| Branch | Tunnel Network |
|------|----------------|
| Sri Lanka | `172.16.0.0/30` |
| India | `172.16.0.4/30` |
| Qatar | `172.16.0.8/30` |

---

### ☎️ Voice over IP (VoIP)
- `telephony-service` configured on all routers  
- **VLAN 20** prioritized for voice traffic  
- IP Phones auto-register using **DHCP Option 150**

---

### 📶 Enterprise Network
- **WPA2-PSK** secured Access Points   
- DHCP scopes automatically assign IP addresses

---

## 🧪 Usage & Testing

1. **Web Access Test**  
   - From any branch PC, browse:  
     👉 `www.globalconnect.corp`

2. **Connectivity Test**  
   - Successful ping from **Sri Lanka PC** `(10.2.10.x)`  
     to **HQ Server** `(10.1.50.10)`

3. **VPN Verification**  
   - Run:
     ```bash
     tracert 10.1.50.10
     ```
   - Confirm traffic flows through **Tunnel IPs (172.16.x.x)** instead of public WAN links

---

## 👨‍💻 Credits

**Concept & Network Design**  
- **Eng. MS. Sanaullah**

---

<p align="center">
  <em>📁 Project developed as part of my professional Networking Portfolio</em>
</p>
