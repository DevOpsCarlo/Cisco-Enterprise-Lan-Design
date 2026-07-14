# Cisco-Enterprise-Lan-Design

# Enterprise LAN Infrastructure: Inter-VLAN Routing, VLSM, and STP Security

## 📌 Project Overview
This project demonstrates the design and implementation of a secure, subnetted enterprise Local Area Network (LAN) using Cisco infrastructure. It showcases advanced Layer 2 and Layer 3 networking concepts, including Variable Length Subnet Masking (VLSM), Inter-VLAN routing via a Distribution Switch (DSW), and Spanning Tree Protocol (STP) hardening techniques to ensure network reliability and security.

## 🛠️ Technologies & Features Demonstrated
*   **Layer 3 Switching / Inter-VLAN Routing:** Configured Switch Virtual Interfaces (SVIs) on the Distribution Switch (DSW) for efficient routing between departments.
*   **VLSM Subnetting:** Optimized IP address allocation to minimize host wastage across multiple VLANs.
*   **STP Toolkit Security:** Hardened the Access Layer (ASW) using **PortFast** for immediate host connectivity and **BPDU Guard** to prevent unauthorized switch attachments and loop injections.
*   **Static Routing / IP Route:** Implemented predictable static routing paths to simulate upstream or WAN connectivity.

## 📐 Network Topology & Design



### 📊 VLSM Subnetting Scheme
The network uses the base network `192.168.1.0/24` (or your chosen subnet), partitioned via VLSM to accommodate varying host requirements:

| VLAN ID | Department| Required Hosts | Subnet Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| VLAN 10 | Engineering | 45 | 192.168.1.0 | /26 (255.255.255.192) | 192.168.1.62 |
| VLAN 20 | HR | 10 | 192.168.1.64 | /27 (255.255.255.224) | 192.168.1.94 |
| VLAN 30 | SALES | 12 | 192.168.1.96 | /27 (255.255.255.224) | 192.168.1.126 |

---

## 💻 Configuration Highlights

### 1. Access Layer Security (ASW1)
Configured edge ports to ensure rapid user connection while strictly enforcing topology boundaries using the STP Toolkit.
```text
interface range FastEthernet 0/1 - 4
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
```
*Rationale: PortFast bypasses the 30-second STP listening/learning state for immediate assignment on end-user PCs. BPDU Guard error-disables the port immediately if a rogue switch is plugged into these ports, preventing loop generation.*

### 2. Core/Distribution Routing (DSW1)
Enabled routing globally and established SVIs to handle traffic moving between different subnets.
```text
ip routing
!
interface Vlan 10
 ip address 192.168.1.62 255.255.255.192
!
interface Vlan 20
 ip address 192.168.1.94 255.255.255.224
!
interface Vlan 30
 ip address 192.168.1.126 255.255.255.224
```

---

## 🔍 Verification & Testing
To confirm structural integrity, the following Cisco IOS verification commands were executed:
*   `show vlan brief`: Verified all edge ports are successfully tracking their assigned active VLANs.
*   `show spanning-tree summary`: Confirmed PortFast and BPDU Guard are globally or interface-actively monitoring the network edge.
*   `show ip route`: Verified the routing table on the DSW populated all connected SVI networks and static paths correctly.
*   **End-to-End Ping Tests:** Executed successful cross-VLAN ICMP pings between hosts to validate Inter-VLAN functionality.

## 🚀 How to Run This Project
1. Clone this repository.
2. Open the included `.pkt` (Packet Tracer).
3. Review the sanitized text configuration files in the `/configs` directory.
