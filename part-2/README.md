# 🛡️ Phase 2: Security Hardening & Architecture Upgrade

> **Project Update: Advanced Security Overhaul**
> Moving beyond basic connectivity, **Phase 2** implements a "Zero Trust" security model, military-grade encryption for WAN traffic, and high-availability architecture. This phase secures the Management Plane, Data Plane, and Control Plane against modern network threats.

---

## 🚀 Upgrade Summary matrix

| Feature Domain      | Phase 1 State (Basic)    | Phase 2 State (Hardened)   | Security Benefit                                                     |
| ------------------- | ------------------------ | -------------------------- | -------------------------------------------------------------------- |
| **Device Access**   | Telnet (Clear Text)      | **SSH v2 (Encrypted)**     | Prevents credential sniffing and Man-in-the-Middle attacks.          |
| **Authentication**  | Shared Password          | **RBAC (Admin Accounts)**  | Ensures accountability with unique user credentials (`AdminHany`).   |
| **Edge Security**   | Open / Unrestricted      | **Port Security (Sticky)** | Blocks unauthorized devices (rogue laptops) from the physical LAN.   |
| **WAN Privacy**     | GRE Tunnels (Clear Text) | **IPSec VPN (AES-256)**    | Encrypts inter-branch traffic, making it unreadable to ISPs/hackers. |
| **Traffic Control** | Full Access              | **Firewall ACLs**          | Implements "Server Guard" (Web Allowed, Ping Blocked).               |
| **Redundancy**      | Single Uplink            | **EtherChannel (LACP)**    | Provides link redundancy and 2x bandwidth at the HQ Core.            |

---

## 🛠️ Detailed Implementation

### 1. Management Plane Hardening

**Objective:** Secure routers against unauthorized administrative access.

* **Protocol:** Migrated all VTY lines from Telnet to **SSH version 2**.
* **Encryption Keys:** Generated **RSA 1024-bit** keys to secure the management channel.
* **Access Control:** Implemented **Standard ACL 10** to restrict SSH access *only* to the designated Admin PC (`10.1.10.100`).
* **Legal:** Configured `banner motd` to provide legal warning against unauthorized access.

---

### 2. Layer 2 LAN Security (Switching)

**Objective:** Prevent "Insider Attacks" and rogue device insertion at the branch level.

* **Port Security:** Configured on all Access Switch ports (`Fa0/1-10`).

* **Maximum Devices:** Locked to 2 MAC addresses (1 PC + 1 VoIP Phone).

* **Violation Mode:** `Shutdown` (Port instantly disables if a 3rd device connects).

* **Sticky Learning:** Switch dynamically learns and "locks" to the hardware address of legitimate devices.

* **Attack Mitigation:** unused ports administratively `shutdown`.

---

### 3. Network Segmentation (Firewall ACLs)

**Objective:** Enforce a "Need-to-Know" access policy for the Server Farm.

* **Server Guard Policy:** Applied **Extended ACL 101** on the HQ Router.
* ✅ **PERMIT** HTTP (TCP 80)  Allows business web applications.
* ✅ **PERMIT** DNS (UDP 53)  Allows name resolution.
* ❌ **DENY** ICMP (Ping)  Hides server presence from network scanning.

---

### 4. WAN Encryption (IPSec over GRE)

**Objective:** Secure sensitive corporate data traveling over the simulated public Internet.

* **Technology:** **IPSec (Internet Protocol Security)** in Transport Mode over GRE.

* **Cryptography Suite:**

* **Encryption:** AES (Advanced Encryption Standard).

* **Hashing:** SHA (Secure Hash Algorithm).

* **Auth:** Pre-Shared Keys (PSK).

* **Implementation:** Used **Legacy Crypto Maps** to support Cisco 2811 router architecture, mapping specific GRE traffic flows to encryption peers.

---

### 5. High Availability (EtherChannel)

**Objective:** Eliminate Single Points of Failure (SPOF) at the Headquarters.

* **Link Aggregation:** Bundled 2x FastEthernet links between **HQ Core Switch** and **HQ Access Switch**.
* **Protocol:** **LACP (Link Aggregation Control Protocol)** for dynamic negotiation.
* **Result:** If one physical cable fails, the network traffic seamlessly fails over to the second cable without dropping connections.

---

## 🧪 Security Validation Tests

### ✅ Test 1: The "Hacker" Test

* **Scenario:** An unauthorized laptop connects to a secure employee port at the Sri Lanka Branch.
* **Outcome:** Port Status changes to **Error-Disabled** (Red Light). Access is physically blocked.

---

### ✅ Test 2: The "Firewall" Test

* **Scenario:** A Branch user attempts to `ping` the HQ Server (`10.1.50.10`).
* **Outcome:** `Destination host unreachable` (Blocked by ACL).
* **Scenario:** The same user opens the Web Browser to `www.globalconnect.corp`.
* **Outcome:** Website loads successfully (Permitted by ACL).

---

### ✅ Test 3: The "Encryption" Check

* **Command:** `show crypto isakmp sa` on HQ Router.
* **Outcome:** Status shows **QM_IDLE** (Quick Mode Idle), confirming an active, encrypted VPN tunnel.

---

### 👨‍💻 Tech Stack Update

* **Simulation Tool:** Cisco Packet Tracer 8.2
* **Routing:** OSPFv2 (Backbone Area 0)
* **Switching:** VLANs (802.1Q), STP, EtherChannel (LACP)
* **Security:** SSHv2, Port-Security, IPSec, ACLs (Standard & Extended)

---

## 👨‍💻 Credits

**Concept & Network Design/Security**  
- **Eng. MS. Sanaullah**

---

<p align="center">
  <em>📁 Project developed as part of my professional Networking Portfolio</em>
</p>
