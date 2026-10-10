# enterprise-network-project
Cisco Packet Tracer Multi-Site Enterprise Network.
# Enterprise Multi-Site Network Design & Implementation

**Author:** Othmane Aatif  
**Tools:** Cisco Packet Tracer, Cisco IOS  
**Technologies:** VLANs, IEEE 802.1Q Trunking, Inter-VLAN Routing (Router-on-a-Stick), OSPF Routing (Area 0), Dynamic DHCP, Extended ACLs.

---

## 📌 Project Overview
This project demonstrates the design and implementation of a scalable, multi-site enterprise network infrastructure connecting a **Headquarters (HQ)** and a **Branch Office**. 

### Key Features:
- **Department Segmentation:** Isolated VLANs for IT, HR, Sales, and Management.
- **Trunking & Routing:** IEEE 802.1Q trunking with Router-on-a-Stick inter-VLAN routing.
- **Inter-Site Connectivity:** Dynamic routing between HQ and Branch using OSPF Area 0.
- **Automated IP Allocation:** Dynamic DHCP pools configured on routers for each VLAN.
- **Network Security:** Extended Access Control Lists (ACLs) to block unauthorized branch access to sensitive management servers.

---

## 🌐 Network Addressing & VLAN Schema

| Location | Department | VLAN Name | VLAN ID | Subnet / Mask | Switch Port | Gateway Interface |
| :--- | :--- | :---: | :---: | :--- | :---: | :--- |
| **HQ Site** | IT Dept | `IT_Dept` | 10 | `192.168.10.0/24` | Fa0/2 | Gig0/0.10 (`192.168.10.1`) |
| **HQ Site** | HR Dept | `HR_Dept` | 20 | `192.168.20.0/24` | Fa0/10 | Gig0/0.20 (`192.168.20.1`) |
| **HQ Site** | Management | `Management` | 99 | `192.168.99.0/24` | Fa0/5 | Gig0/0.99 (`192.168.99.1`) |
| **Branch** | Sales Dept | `Sales_Dept` | 30 | `192.168.30.0/24` | Fa0/2 | Gig0/0.30 (`192.168.30.1`) |

---

## 🔗 Interconnections & Routing

| Link Type | Source | Port | Destination | Port | Protocol / Config |
| :--- | :--- | :---: | :--- | :---: | :--- |
| **Trunk Link (HQ)** | SW1-HQ | Fa0/1 | R1-HQ | Gig0/0 | 802.1Q Trunking (VLANs 10, 20, 99) |
| **Trunk Link (Branch)** | SW2-Branch | Fa0/1 | R2-Branch | Gig0/0 | 802.1Q Trunking (VLAN 30) |
| **WAN Link** | R1-HQ | Gig0/1 | R2-Branch | Gig0/1 | `10.0.0.0/30` (OSPF Area 0) |

---

## 🔒 Security Policy (Extended ACL)

To protect the HQ Management Server (`192.168.99.10`), an Extended Access Control List was applied on `R1-HQ`:

```cisco
ip access-list extended BLOCK_SALES_SERVER
 deny ip 192.168.30.0 0.0.0.255 host 192.168.99.10
 permit ip any any
!
interface GigabitEthernet0/1
 ip access-group BLOCK_SALES_SERVER in
