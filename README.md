# 🚀 Multi-National Enterprise Network Project
**A comprehensive network simulation connecting a Dubai HQ to branches in Sri Lanka, India, and Qatar.**

![Network Topology](network_topology.png)

## 📌 Project Overview
This project simulates a real-world enterprise network environment for a multi-national company ("GlobalConnect"). It demonstrates advanced networking concepts including WAN connectivity, secure VPN tunneling, dynamic routing, Voice over IP (VoIP), and centralized server management.

**Tools Used:** Cisco Packet Tracer 8.2
**Key Technologies:** OSPF, GRE Tunnels, VLANs, ROAS, DHCP, VoIP, NAT.

---

## 🏗️ Network Architecture

### 1. Headquarters (Dubai) 🇦🇪
The central hub hosting the Core Layer and Server Farm.
* **Hardware:** Cisco 2811 Router, 3560 Multilayer Switch, 2960 Access Switch.
* **VLANs:** Data (10), Voice (20), Print (30), WiFi (40), Server Farm (50).
* **Services:**
    * **Core Switching:** Inter-VLAN routing handled by the L3 Switch.
    * **Telephony:** CME (Call Manager Express) handling HQ extensions (1001-1010).
    * **Intranet:** Main DNS/Web Server (`www.globalconnect.corp`).

### 2. International Branches (Sri Lanka 🇱🇰, India 🇮🇳, Qatar 🇶🇦)
Three remote sites connected via a simulated ISP WAN.
* **Connectivity:** Each branch connects to a unique ISP Router.
* **Tunneling:** **GRE VPN Tunnels** allow private branch traffic (10.x.x.x) to reach HQ securely over the public internet.
* **Local Services:** Each branch has a local File Server.

### 3. ISP / WAN Layer 🌍
A ring topology of 4 Routers simulating the Internet.
* **Protocol:** OSPF Area 0 (Backbone).
* **Redundancy:** Ring architecture ensures connectivity even if one ISP link fails.

---

## ⚙️ Key Configurations & Features

### ✅ Dynamic Routing (OSPF)
* Implemented OSPF Area 0 across all sites.
* Used `redistribute static` at HQ to advertise the Server Farm to remote branches.
* Configured Tunnels to participate in OSPF for seamless private routing.

### ✅ Secure VPN (GRE Tunnels)
* Created virtual point-to-point links between HQ and Branches.
* **Sri Lanka Tunnel:** `172.16.0.0/30`
* **India Tunnel:** `172.16.0.4/30`
* **Qatar Tunnel:** `172.16.0.8/30`

### ✅ Voice over IP (VoIP)
* Configured `telephony-service` on all routers.
* **VLAN 20** prioritized for Voice traffic.
* IP Phones automatically register via Option 150.

### ✅ Wireless Enterprise
* WPA2-PSK Security implemented on Access Points.
* Dedicated **VLAN 40** for Wireless management.
* DHCP scopes configured to assign IPs to Pcs automatically.

---

## 📸 Usage / Testing
1.  **Web Access:** Open any PC in any branch and browse to `www.globalconnect.corp` to see the HQ Dashboard.
2.  **Ping Test:** Successful pings from **Sri Lanka PC** (10.2.10.x) to **HQ Server** (10.1.50.10).
3.  **VPN Check:** Use `tracert 10.1.50.10` from a branch to verify traffic passes through the Tunnel IP (172.16.x.x) and not the Public Internet.

---

## 👨‍💻 Credits
**Concept & Design:** MS. Sanaullah
**Original Concept Assistance:** Eng. Mohamed Saleem Sanaullah

*Project created as part of my small Networking Portfolio.*
