# 🚀 Mulualem Workshop Enterprise Network Infrastructure

A multi-VLAN enterprise network architecture designed, configured, and verified using Cisco Packet Tracer. This project demonstrates end-to-end network deployment including Inter-VLAN routing, automated IP address management (DHCP), local domain name resolution (DNS), and granular traffic filtering via Extended Access Control Lists (ACLs).

---

## 📌 Architecture Overview

[ Router (Cisco 2911) ]
(Router-on-a-Stick Routing)
|
| Trunk Link (802.1Q)
|
[ Switch (Cisco 2960) ]
/             |            
/              |             
VLAN 10 (Admin)   VLAN 20 (Ops)    VLAN 30 (Guest)
192.168.1.0/24   192.168.2.0/24   192.168.3.0/24
[DNS Server]     [Workstations]    [Visitor PCs]

## 🛠️ Network Specifications & Addressing Plan

| VLAN ID | Subnet Name | Subnet Range | Gateway IP | Key Static Hosts |
| :--- | :--- | :--- | :--- | :--- |
| VLAN 10 | ADMIN | 192.168.1.0/24 | 192.168.1.1 | DNS Server: 192.168.1.100<br>Office Printer: 192.168.1.10 |
| VLAN 20 | OPERATIONS | 192.168.2.0/24 | 192.168.2.1 | Dynamic DHCP Clients |
| VLAN 30 | GUEST | 192.168.3.0/24 | 192.168.3.1 | Dynamic DHCP Clients |

---

## ⚡ Automated Services & Security Policies

### 1. Dynamic IP Allocation (DHCP)
- Host: Router (Mulualem-Router)
- Pools Configured:
  - ADMIN_POOL (192.168.1.0/24, Default Router: 192.168.1.1)
  - OPERATIONS_POOL (192.168.2.0/24, Default Router: 192.168.2.1)
  - GUEST_POOL (192.168.3.0/24, Default Router: 192.168.3.1)
- Excluded Addresses: 192.168.1.1 – 192.168.1.50 (Reserved for gateways, server, and static infrastructure).

### 2. Domain Name Resolution (DNS)
- Host: Dedicated PT-Server (192.168.1.100)
- Domain Mapped: mulualemworkshop.local $\rightarrow$ 192.168.1.100

### 3. Extended Access Control List (ACL Security)
- Policy Name: GUEST_SECURITY (Applied in on sub-interface GigabitEthernet0/0.30)
- Rules:
  1. PERMIT Guest subnets (192.168.3.0/24) to access DNS Server (192.168.1.100) for domain resolution.
  2. DENY Guest subnets (192.168.3.0/24) from reaching sensitive Admin devices (192.168.1.0/24).
  3. PERMIT all remaining traffic.

---

## 📋 Verification Results

- Inter-VLAN Communication: Verified 0% packet loss across internal subnets.
- DHCP Leases: Confirmed automatic allocation of IP, Gateway, and DNS settings across client workstations.
- DNS Resolution: Client terminal pings to mulualemworkshop.local successfully resolved to 192.168.1.100.
- ACL Enforcement: Pings from Guest devices to 192.168.1.100 succeeded, while pings to Admin endpoints (192.168.1.10) were actively rejected with Destination Host Unreachable.

---

## 📂 Repository Contents
- mulualem_workshop_network.pkt: The complete Cisco Packet Tracer simulation file.
- README.md: Project documentation
