# Enterprise OSPF Multi-Branch Network

## Configuration Guide

**Author:** Elroy Noel  
**Platform:** Cisco Packet Tracer  
**Routing Protocol:** OSPF Process 10  
**Network Type:** Enterprise Multi-Branch Network  
**Documentation Version:** 1.0

---

# Table of Contents

- Project Overview
- Technologies Implemented
- Network Objectives
- Enterprise Topology
- Configuration Order
- IP Addressing Plan
- VLAN Design
- Headquarters Site
- Branch 2 Site
- Branch 3 Site
- ISP Router
- Enterprise Services
- Design Summary

---

# Project Overview

This project demonstrates the design, implementation, and verification of a multi-site enterprise network using Cisco IOS devices in Cisco Packet Tracer.

The enterprise consists of a Headquarters location, two branch offices, and an Internet Service Provider (ISP). Dynamic routing is implemented using OSPF Process 10, while Router-on-a-Stick provides inter-VLAN routing at each site. Additional enterprise services including DHCP, NAT/PAT, SSH, NTP, Syslog, EtherChannel, VTP, and PVST are integrated to create a realistic business network.

The objective of this project is to demonstrate the deployment of an enterprise network that provides secure connectivity, centralized Internet access, dynamic routing, and resilient Layer 2 switching while following networking best practices.

---

# Technologies Implemented

This project includes the following Cisco technologies:

- OSPF Process 10
- Router-on-a-Stick
- IEEE 802.1Q VLAN Trunking
- VLAN Segmentation
- DHCP
- NAT/PAT
- SSH Version 2
- Network Time Protocol (NTP)
- Syslog
- EtherChannel (LACP)
- VLAN Trunking Protocol (VTP)
- Per-VLAN Spanning Tree (PVST)

---

# Network Objectives

The primary objectives of this project were to:

- Build a scalable enterprise network.
- Implement dynamic routing using OSPF.
- Separate user traffic through VLAN segmentation.
- Provide automatic IP addressing using DHCP.
- Centralize Internet access through Headquarters.
- Secure remote device management using SSH.
- Synchronize network devices using NTP.
- Centralize logging through Syslog.
- Provide redundant Layer 2 connectivity using EtherChannel.
- Prevent switching loops using PVST.

---

# Enterprise Topology

The enterprise network consists of:

| Site | Devices |
|-------|---------|
| Headquarters | R1, S1 |
| Branch 2 | R2, S2, Wireless Access Point |
| Branch 3 | R3, S3A, S3B, S3C |
| Internet | ISP Router |

Headquarters functions as the enterprise edge by providing centralized Internet connectivity through NAT/PAT while advertising the default route to the remaining enterprise routers using OSPF.

Branch 2 and Branch 3 provide local user access while relying on Headquarters for Internet connectivity.

---

# Configuration Order

The network was implemented using the following sequence:

1. Configure hostnames
2. Configure interface IP addressing
3. Create VLANs
4. Configure trunk links
5. Configure Router-on-a-Stick
6. Configure OSPF Process 10
7. Configure DHCP
8. Configure NAT/PAT
9. Configure SSH
10. Configure Syslog
11. Configure NTP
12. Configure EtherChannel
13. Configure VTP
14. Verify end-to-end connectivity

---

# IP Addressing Plan

| Network | Purpose |
|----------|---------|
| 192.168.10.0/24 | Headquarters Operations |
| 192.168.20.0/24 | Headquarters Development |
| 192.168.30.0/24 | Headquarters Management |
| 172.16.40.0/24 | Branch 2 Operations |
| 172.16.50.0/24 | Branch 2 Development |
| 172.16.60.0/24 | Branch 2 Management |
| 172.16.150.0/24 | Branch 2 Wireless |
| 10.0.70.0/24 | Branch 3 Operations |
| 10.0.80.0/24 | Branch 3 Development |
| 10.0.90.0/24 | Branch 3 Management |

WAN Networks

| Link | Network |
|------|---------|
| R1-R2 | 192.168.2.0/30 |
| R2-R3 | 192.168.2.4/30 |
| R1-R3 | 192.168.2.8/30 |
| R1-ISP | 209.165.200.224/30 |

---

# VLAN Design

| VLAN | Name | Purpose |
|------|------|----------|
| 10 | Ops | Headquarters Operations |
| 20 | Dev | Headquarters Development |
| 30 | Mgmt | Headquarters Management |
| 40 | Ops | Branch 2 Operations |
| 50 | Dev | Branch 2 Development |
| 60 | Mgmt | Branch 2 Management |
| 70 | Ops | Branch 3 Operations |
| 80 | Dev | Branch 3 Development |
| 90 | Mgmt | Branch 3 Management |
| 150 | WLAN | Wireless Network |

Each site uses a dedicated management VLAN to isolate infrastructure management traffic from user data traffic while simplifying device administration.
