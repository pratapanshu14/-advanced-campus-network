# Advanced Campus Area Network Design

> **Final Year Capstone Project** — B.Tech Electronics & Communication Engineering  
> Atal Bihari Vajpayee Govt. Institute of Engineering & Technology, Shimla | 2026

---

## Overview

A production-ready dual-campus university network designed and fully implemented in **Cisco Packet Tracer 8.2**, serving **30,000+ users** across two geographically separated campuses with scalability up to **60,000 users**.

The design combines practical enterprise networking experience gained during an internship at **Scarlet Wireless India Pvt. Ltd., Mangaluru** with academic network engineering principles to deliver a secure, highly available, and fully documented campus network.

---

## Network Architecture

The network follows Cisco's **Hierarchical Three-Tier Model**:

| Layer | Devices | Role |
|-------|---------|------|
| Core | Cisco ASA 5506-X (×2) | Perimeter firewall, IPsec VPN, ISP connectivity |
| Distribution | Catalyst 3850-24PS (×4) | Inter-VLAN routing, HSRP, OSPF, EtherChannel |
| Access | Catalyst 2960-24TT (×10) | End-device connectivity, VLAN assignment |

---

## Features Implemented

- **VLAN Segmentation** — Management (10), LAN (20), WLAN (50/90), Blackhole (199)
- **Inter-VLAN Routing** — Layer 3 SVIs on Catalyst 3850 multilayer switches
- **OSPF Dynamic Routing** — Area 0 across all routing devices at both campuses
- **HSRP Redundancy** — Active/Standby gateway failover at distribution layer
- **LACP EtherChannel** — 3 Gbps aggregate uplink between distribution switches
- **Cisco ASA 5506-X Firewall** — DMZ / Inside / Outside security zones with NAT, PAT, and ACLs
- **DMZ Server Farm** — Redundant DHCP (×2), DNS, Web, Email, FTP servers
- **Site-to-Site IPsec VPN** — AES-256 / SHA / IKEv1 between HQ and Branch firewalls
- **Centralized Wireless Infrastructure** — WLC-2504 + CAPWAP-managed LAPs, 4 SSIDs per department
- **Google Cloud Integration** — Cloud service connectivity via internet path
- **Security Hardening** — SSH v2 with ACL restriction, BPDUguard, PortFast, Blackhole VLAN on all unused ports

---

## IP Addressing Scheme

### IP Address Allocation Summary

| Network Zone | Campus | IP Range | Subnet Mask | Purpose |
|--------------|--------|----------|-------------|---------|
| WLAN Management | Both | 192.168.10.0/24 | 255.255.255.0 | WLC & AP Management |
| LAN | Main | 172.16.0.0/16 | 255.255.0.0 | Main Campus wired LAN |
| LAN | Branch | 172.17.0.0/16 | 255.255.0.0 | Branch Campus wired LAN |
| WLAN | Main | 10.10.0.0/16 | 255.255.0.0 | Main Campus wireless clients |
| WLAN | Branch | 10.11.0.0/16 | 255.255.0.0 | Branch Campus wireless clients |
| DMZ Server Farm | Main | 10.20.20.0/27 | 255.255.255.224 | Servers (30 hosts) |
| Public WAN | Main | 105.100.50.0/30 | 255.255.255.252 | HQ ISP uplink |
| Public WAN | Branch | 205.200.100.0/30 | 255.255.255.252 | Branch ISP uplink |
| FWL–MLS1 | Main | 10.20.20.32/30 | 255.255.255.252 | HQ FWL to MLSW1 |
| FWL–MLS2 | Main | 10.20.20.36/30 | 255.255.255.252 | HQ FWL to MLSW2 |
| FWL–MLS1 | Branch | 10.20.20.40/30 | 255.255.255.252 | BR FWL to MLSW1 |
| FWL–MLS2 | Branch | 10.20.20.44/30 | 255.255.255.252 | BR FWL to MLSW2 |
| Google Cloud | Cloud | 8.0.0.0/8 | 255.0.0.0 | Google Cloud services |

### DMZ Server IP Assignments

| IP Address | Device | Role |
|------------|--------|------|
| 10.20.20.1 | HQ-FWL (DMZ interface) | Firewall gateway for DMZ |
| 10.20.20.5 | DHCP-1 (Primary) | Primary DHCP server |
| 10.20.20.6 | DHCP-2 (Redundant) | Secondary DHCP server |
| 10.20.20.7 | DNS | Domain Name Resolution |
| 10.20.20.8 | WEB | University web portal |
| 10.20.20.9 | EMAIL | Email services |
| 10.20.20.10 | FTP | File transfer services |

---

## VLAN Architecture

| VLAN ID | Name | Network | Purpose |
|---------|------|---------|---------|
| 10 | MANAGEMENT | 192.168.10.0/24 | Network device management |
| 20 | LAN | 172.16.0.0/16 (HQ) / 172.17.0.0/16 (BR) | Wired end-user devices |
| 50 | WLAN | 10.10.0.0/16 | Wireless clients — Main campus |
| 90 | WLAN | 10.11.0.0/16 | Wireless clients — Branch campus |
| 199 | BLACKHOLE | No routing | All unused switch ports — security only |

---

## Device Inventory

| Device | Model | Qty | Location | Role |
|--------|-------|-----|----------|------|
| HQ-FWL | Cisco ASA 5506-X | 1 | Main Campus | Perimeter firewall, DMZ, IPsec VPN |
| BRANCH-FWL | Cisco ASA 5506-X | 1 | Branch Campus | Perimeter firewall, IPsec VPN |
| HQ-MLSW1 / HQ-MLSW2 | Catalyst 3850-24PS | 2 | Main Campus | Distribution — HSRP, OSPF, EtherChannel |
| B-MLSW1 / B-MLSW2 | Catalyst 3850-24PS | 2 | Branch Campus | Distribution — HSRP, OSPF, EtherChannel |
| Faculty Switches (HQ) | Catalyst 2960-24TT | 5 | Main Campus | Access — 1 per faculty department |
| Faculty Switches (BR) | Catalyst 2960-24TT | 4 | Branch Campus | Access — 1 per faculty department |
| DMZ-SW | Catalyst 2960-24TT | 1 | Main Campus DMZ | Server farm switch |
| HQ-WLC | WLC-2504 | 1 | Main Campus | Wireless LAN Controller |
| BR-WLC | WLC-2504 | 1 | Branch Campus | Wireless LAN Controller |
| LAPs (HQ) | LAP-PT | 5 | Main Campus | 1 per faculty department |
| LAPs (BR) | LAP-PT | 4 | Branch Campus | 1 per faculty department |
| Servers | Server-PT | 6 | Main Campus DMZ | DHCP×2, DNS, Web, Email, FTP |
| ISP Routers | Cisco 2911 | 2 | Internet | HQ-ISP and Branch-ISP |
| Internet Router | Cisco 2911 | 1 | Internet | Core internet connectivity |
| Google VSW | Catalyst 2960-24TT | 1 | Cloud | Google Cloud virtual switch |

---

## Campus Structure

### Main Campus — Five Faculty Departments
- Health & Sciences
- Business
- Engineering
- Arts & Design
- Information Technology

### Branch Campus — Four Faculty Departments
- Health & Sciences
- Business
- Engineering
- Arts & Design

Each department has a dedicated Catalyst 2960-24TT access switch with PCs, printers, laptops, tablets, smartphones, and a CAPWAP-managed LAP.

---

## Security Architecture

### Firewall Zones (Cisco ASA 5506-X)

| Zone | Security Level | Purpose |
|------|---------------|---------|
| Outside | 0 | Faces ISP — untrusted |
| DMZ | 50 | Hosts public-facing servers |
| Inside | 100 | Campus LAN — fully trusted |

### IPsec VPN Configuration

| Parameter | Value |
|-----------|-------|
| Phase 1 Encryption | AES-256 |
| Phase 1 Hash | SHA |
| Authentication | Pre-shared key |
| DH Group | Group 2 |
| Phase 2 Transform | ESP-AES-256 + SHA-HMAC |
| Mode | Tunnel |
| HQ Peer | 105.100.50.2 |
| Branch Peer | 205.200.100.2 |

### Access Control
- SSH v2 restricted via ACL to NET-SEC-PC only
- All unused switch ports assigned to VLAN 199 (Blackhole) and administratively shut down
- STP PortFast and BPDUguard enabled on all access ports

---

## Wireless Infrastructure

| Parameter | Main Campus | Branch Campus |
|-----------|-------------|---------------|
| WLC | HQ-WLC (10.10.0.15) | BR-WLC (10.11.0.15) |
| Protocol | CAPWAP | CAPWAP |
| LAPs | 5 (1 per department) | 4 (1 per department) |
| SSIDs per LAP | 4 | 4 |
| SSID Names | Employees, Corporate Users, External Auditors, Guest Users | Branch variants of same |
| WLAN Network | 10.10.0.0/16 | 10.11.0.0/16 |

---

## Connectivity Test Results

All 12 connectivity tests passed in Cisco Packet Tracer 9.0:

| Test | Source | Destination | Expected Result | Status |
|------|--------|-------------|-----------------|--------|
| DHCP Allocation | HS-PC1 (VLAN 20) | DHCP Server 10.20.20.5 | IP in correct range | ✅ PASS |
| Inter-VLAN Routing | PC (VLAN 20) | Printer (VLAN 20) | Ping via SVI | ✅ PASS |
| PC to DMZ Web | BUS-PC1 | 10.20.20.8 (WEB) | HTTP reachable via FWL | ✅ PASS |
| PC to Internet | ENG-PC1 | 8.8.8.8 (Google) | Ping via NAT | ✅ PASS |
| Branch to HQ DMZ | BENG-PC1 | 10.20.20.7 (DNS) | Reachable via IPsec VPN | ✅ PASS |
| IPsec VPN Tunnel | HQ-FWL | BRANCH-FWL | IKE SA established | ✅ PASS |
| SSH Restriction | Non-admin PC | Switch VTY | Connection refused | ✅ PASS |
| SSH Allowed | NET-SEC-PC | HQ-MLSW1 VTY | Login success | ✅ PASS |
| WLC–LAP | BUS-LAP | HQ-WLC 10.10.0.15 | CAPWAP connected | ✅ PASS |
| HSRP Failover | Shutdown MLSW1 | Virtual IP | Traffic via MLSW2 | ✅ PASS |
| Cloud Access | Any PC | Google Server 8.x.x.x | Reachable via internet | ✅ PASS |
| Branch Internet | BART-PC2 | 8.8.8.8 | Ping via Branch-FWL NAT | ✅ PASS |

---

## Performance Metrics

| Metric | Value |
|--------|-------|
| OSPF Convergence Time | < 30 seconds after topology change |
| HSRP Failover Time | < 10 seconds |
| DHCP Lease Time | 24 hours (dual-server redundancy) |
| EtherChannel Bandwidth | 3 Gbps aggregate (3× GigabitEthernet) |
| CAPWAP Association Time | < 30 seconds from startup |
| Active VLANs per Campus | 5 (10 / 20 / 50 or 90 / 199) |
| Max Supported Hosts — LAN | 65,534 per /16 subnet |
| Max Supported Hosts — WLAN | 65,534 per /16 subnet |

---

## Verification Commands Used

| Command | Purpose |
|---------|---------|
| `show ip ospf neighbor` | Verify OSPF adjacencies |
| `show standby brief` | Verify HSRP Active/Standby state |
| `show etherchannel summary` | Verify LACP EtherChannel |
| `show vlan brief` | Verify VLAN assignments |
| `show crypto isakmp sa` | Verify IKE Phase 1 VPN |
| `show crypto ipsec sa` | Verify Phase 2 / ESP tunnel |
| `show ip dhcp binding` | Verify DHCP lease allocations |
| `show ip route` | Verify routing table (OSPF routes) |
| `show interfaces trunk` | Verify trunk links between switches |
| `show spanning-tree` | Verify STP port states |
| `show access-lists` | Verify ACL entries and hit counts |
| `show capwap client ap-info` | Verify LAP–WLC CAPWAP association |

---

## Tools & Technologies

| Tool / Technology | Purpose |
|-------------------|---------|
| Cisco Packet Tracer 9.0 | Network simulation and testing |
| Cisco IOS (Catalyst 3850, 2960) | Switch configuration |
| Cisco ASA OS | Firewall and VPN configuration |
| Cisco 2911 IOS | ISP and internet router configuration |
| OSPF (RFC 2328) | Dynamic routing protocol |
| HSRP (RFC 2281) | First-hop redundancy |
| LACP (IEEE 802.3ad) | Link aggregation |
| IPsec / IKEv1 | Site-to-site VPN encryption |
| CAPWAP (RFC 5415) | Wireless AP management |
| 802.1Q (IEEE) | VLAN trunking |

---

## Future Scope

| Enhancement | Description |
|-------------|-------------|
| Cisco ISE (802.1X NAC) | Port-based authentication with dynamic VLAN assignment |
| Firepower Threat Defense (FTD) | Next-gen IPS, URL filtering, malware detection |
| SD-WAN | Intelligent path selection replacing static IPsec VPN |
| IPv6 Dual-Stack | Future-proofing with OSPFv3 and DHCPv6 |
| SNMP v3 / NetFlow | Centralized monitoring and traffic analytics |

---

## Project Structure

```
advanced-campus-network/
│
├── README.md                        # This file
├── topology/
│   ├── full-dual-campus.pkt         # Cisco Packet Tracer topology file
│   ├── main-campus-dmz.png          # Main campus DMZ screenshot
│   ├── main-campus-access.png       # Main campus faculty access layer
│   ├── branch-campus.png            # Branch campus topology
│   ├── isp-vpn-tunnel.png           # ISP and IPsec VPN tunnel
│   └── google-cloud.png             # Google Cloud integration
├── configs/
│   ├── HQ-FWL.txt                   # HQ ASA firewall config
│   ├── BRANCH-FWL.txt               # Branch ASA firewall config
│   ├── HQ-MLSW1.txt                 # HQ multilayer switch 1 config
│   ├── HQ-MLSW2.txt                 # HQ multilayer switch 2 config
│   ├── B-MLSW1.txt                  # Branch multilayer switch 1 config
│   ├── B-MLSW2.txt                  # Branch multilayer switch 2 config
│   └── access-switches/             # All access switch configs
└── docs/
    └── Project_Report.pdf           # Full project report
```

---

## Author

**Anshu Pratap Giri**  
B.Tech — Electronics & Communication Engineering  
ABVGIET, Shimla | Batch 2022–2026  
CCNA 200-301 Certified  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://linkedin.com/in/your-profile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black)](https://github.com/pratapanshu14)

---

> **Internship:** Scarlet Wireless India Pvt. Ltd., Mangaluru (19 Jan – 14 May 2026)  
> **Supervisor:** Mr. Deepak  
> **University:** Himachal Pradesh Technical University, Hamirpur
