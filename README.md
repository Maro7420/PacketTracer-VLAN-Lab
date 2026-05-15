<div align="center">

```
██████╗  █████╗  ██████╗██╗  ██╗███████╗████████╗    ████████╗██████╗  █████╗  ██████╗███████╗██████╗
██╔══██╗██╔══██╗██╔════╝██║ ██╔╝██╔════╝╚══██╔══╝    ╚══██╔══╝██╔══██╗██╔══██╗██╔════╝██╔════╝██╔══██╗
██████╔╝███████║██║     █████╔╝ █████╗     ██║           ██║   ██████╔╝███████║██║     █████╗  ██████╔╝
██╔═══╝ ██╔══██║██║     ██╔═██╗ ██╔══╝     ██║           ██║   ██╔══██╗██╔══██║██║     ██╔══╝  ██╔══██╗
██║     ██║  ██║╚██████╗██║  ██╗███████╗   ██║           ██║   ██║  ██║██║  ██║╚██████╗███████╗██║  ██║
╚═╝     ╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚══════╝   ╚═╝           ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝╚══════╝╚═╝  ╚═╝
```

### *Troubleshooting IPv4 & IPv6 Connectivity Across a Multi-VLAN Enterprise LAN*

[![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)](https://www.netacad.com/)
[![Networking](https://img.shields.io/badge/Networking-IPv4%20%26%20IPv6-00ADEF?style=for-the-badge)]()
[![VLANs](https://img.shields.io/badge/VLANs-10%20%26%2020-orange?style=for-the-badge)]()
[![SSH](https://img.shields.io/badge/Remote%20Access-SSH-brightgreen?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)]()

---

> **"In networking, the difference between a working topology and a broken one is often a single misconfigured byte."**

</div>

---

## 📌 Project Overview

This lab simulates a **real-world enterprise LAN troubleshooting scenario** built in Cisco Packet Tracer. After a network update, multiple devices across two VLANs were left misconfigured — causing full connectivity loss. The objective was to diagnose each fault, correct the configurations, and restore end-to-end communication using both IPv4 and IPv6 dual-stack addressing.

---

## 🖥️ Network Topology

```
                        [ Cloud / ISP ]
                              |
                          10.0.0.1/30
                              |
                           Router0
                    Gi0/0 (Trunk: VLAN 10, 20)
                    ├── 192.168.10.1  (VLAN 10 Gateway)
                    └── 192.168.20.1  (VLAN 20 Gateway)
                              |
               ┌──────────────┴──────────────┐
               │                             │
           Switch1                        Switch2
         (VLAN 10)                       (VLAN 20)
         /   |   \                      /    |    \
       PC0  PC1  PC2         Web Server  DB Server  PC3  PC4  PC5
  192.168.10.x            192.168.20.10  192.168.20.1
```

| Device | Role | VLAN | IP Address |
|---|---|---|---|
| Router0 | Core Router | Trunk | 192.168.10.1 / 192.168.20.1 |
| Switch1 | Access Switch | VLAN 10 | — |
| Switch2 | Access Switch | VLAN 20 | — |
| PC0, PC1, PC2 | End Hosts | VLAN 10 | DHCP → 192.168.10.x |
| PC3, PC4, PC5 | End Hosts | VLAN 20 | DHCP → 192.168.20.x |
| Web Server | Application Server | VLAN 20 | 192.168.20.10 |
| DB Server | Database Server | VLAN 20 | 192.168.20.1 |
| Cloud/ISP | External Gateway | — | 10.0.0.1 /30 |

---

## 🎯 Objectives

- Troubleshoot and correct misconfigured **IP addressing and subnetting** across all devices
- Reconfigure **VLAN assignments** on both access switches
- Restore the **Router-on-a-Stick** trunk configuration on Router0
- Set up **IPv4 & IPv6 dual-stack** addressing throughout the topology
- Enable and verify **SSH remote access** to Router0 for secure management
- Confirm **end-to-end connectivity** between all PCs, servers, and the external cloud

---

## 🛠️ Skills Demonstrated

| Skill | Description |
|---|---|
| **Network Troubleshooting** | Diagnosed misconfigured IPs, VLANs, and gateways |
| **IPv4 Subnetting** | Corrected addressing across VLAN 10 and VLAN 20 subnets |
| **IPv6 Configuration** | Implemented dual-stack on router sub-interfaces and end devices |
| **VLAN Management** | Configured trunk and access ports across multilayer switches |
| **SSH Setup** | Enabled encrypted remote access to the router |
| **DHCP Verification** | Confirmed dynamic IP assignment for all end hosts |
| **Ping & Traceroute Testing** | Verified full connectivity with diagnostic tools |

---

## 📂 Files

```
PacketTracer-Troubleshooting-Challenge/
├── topology.pkt         # Cisco Packet Tracer lab file
├── screenshots/         # Ping tests & topology screenshots
└── README.md
```

---

## 🔧 Key Configurations

### Router0 — Sub-interface (Router-on-a-Stick)

```bash
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

### Switch — VLAN & Trunk Configuration

```bash
# Access port (VLAN 10 example)
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

# Trunk port to Router
interface FastEthernet0/24
 switchport mode trunk
```

### SSH Remote Access

```bash
ip domain-name lab.local
crypto key generate rsa modulus 1024
username admin privilege 15 secret cisco
line vty 0 4
 transport input ssh
 login local
```

---

## ✅ Results

| Test | Result |
|---|---|
| PC0 → PC1 (VLAN 10 intra-VLAN) | ✅ Success |
| PC0 → PC3 (inter-VLAN routing) | ✅ Success |
| PC0 → Web Server | ✅ Success |
| PC3 → DB Server | ✅ Success |
| SSH to Router0 | ✅ Success |
| IPv6 dual-stack connectivity | ✅ Success |
| Internet (Cloud) reachability | ✅ Success |

---

## 📚 Concepts Covered

- **Router-on-a-Stick** — Single physical interface serving multiple VLANs via sub-interfaces
- **802.1Q Trunking** — Carrying multiple VLANs over a single link
- **Dual-Stack Networking** — Running IPv4 and IPv6 simultaneously
- **DHCP** — Dynamic Host Configuration Protocol for automatic IP assignment
- **SSH** — Secure Shell for encrypted remote device management

---

## 🚀 How to Open

1. Install **Cisco Packet Tracer** — [Download from NetAcad](https://www.netacad.com/resources/lab-downloads)
2. Clone this repository:
```bash
git clone https://github.com/Maro7420/PacketTracer-Troubleshooting-Challenge.git
```
3. Open `topology.pkt` in Packet Tracer
4. Explore the configurations or attempt the troubleshooting challenge yourself!

---

<div align="center">

**Made with 🔌 and a deep love for clean network configs**

[⭐ Star this repo](https://github.com/Maro7420/PacketTracer-Troubleshooting-Challenge) · [🐛 Report Issue](https://github.com/Maro7420/PacketTracer-Troubleshooting-Challenge/issues)

---

*"Packets don't lie — the config does."*

👨‍💻 **Author:** Marwan Osama

</div>
