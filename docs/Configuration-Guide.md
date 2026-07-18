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

---

# Headquarters Site

The Headquarters site functions as the core of the enterprise network. It provides inter-VLAN routing, centralized Internet access, Network Address Translation (NAT/PAT), DHCP services, OSPF routing, SSH management, NTP services, and Syslog logging for the enterprise.

Devices at this site include:

- **R1** – Headquarters Router
- **S1** – Headquarters Layer 2 Switch

---

# R1 – Headquarters Router

## Purpose

R1 serves as the enterprise edge router and performs the following functions:

- Inter-VLAN routing using Router-on-a-Stick
- OSPF Area 0 routing
- DHCP services for Headquarters VLANs
- NAT/PAT for Internet access
- Default route advertisement into OSPF
- SSH remote management
- NTP Master
- Syslog logging

---

## Interface Configuration

| Interface | Purpose | IP Address |
|-----------|---------|------------|
| GigabitEthernet0/0 | ISP Connection | 209.165.200.226/30 |
| GigabitEthernet0/1.10 | VLAN 10 - Operations | 192.168.10.1/24 |
| GigabitEthernet0/1.20 | VLAN 20 - Development | 192.168.20.1/24 |
| GigabitEthernet0/1.30 | VLAN 30 - Management (Native VLAN) | 192.168.30.1/24 |
| Serial0/0/0 | WAN Link to R2 | 192.168.2.1/30 |
| Serial0/0/1 | WAN Link to R3 | 192.168.2.10/30 |

---

## Router-on-a-Stick

R1 uses IEEE 802.1Q subinterfaces to provide Layer 3 connectivity for multiple VLANs over a single physical interface.

Configured VLANs:

- VLAN 10 – Operations
- VLAN 20 – Development
- VLAN 30 – Management (Native VLAN)

---

## DHCP Configuration

R1 provides DHCP services for Headquarters users.

Configured DHCP Pools:

| Pool | Network |
|------|---------|
| POOL_10 | 192.168.10.0/24 |
| POOL_20 | 192.168.20.0/24 |

The first five IP addresses of each subnet are excluded for infrastructure devices such as routers, switches, and servers.

---

## OSPF Configuration

R1 participates in **OSPF Process 10** with the following settings:

- Router ID: **1.1.1.1**
- Area: **0**
- Passive interfaces configured on all user VLANs
- Default route advertised to the enterprise

Key configuration:

```cisco
router ospf 10
 router-id 1.1.1.1
 passive-interface GigabitEthernet0/1.10
 passive-interface GigabitEthernet0/1.20
 passive-interface GigabitEthernet0/1.30
 default-information originate
```

---

## NAT/PAT Configuration

R1 provides Internet connectivity for all enterprise networks using Port Address Translation (PAT).

- Inside Interface:
  - GigabitEthernet0/1
  - Serial0/0/0
  - Serial0/0/1

- Outside Interface:
  - GigabitEthernet0/0

PAT overload is configured on the ISP-facing interface.

---

## Remote Management

Secure remote administration is provided using SSH Version 2.

Configuration includes:

- Local administrator account
- Domain name
- RSA key generation
- SSH Version 2
- VTY access restricted to SSH

---

## Network Services

Additional enterprise services configured on R1 include:

- NTP Master
- Syslog server destination
- Static default route toward the ISP
- Default route advertisement through OSPF

---

# S1 – Headquarters Access Switch

## Purpose

S1 provides Layer 2 connectivity for Headquarters users while extending VLANs to R1 through an 802.1Q trunk.

---

## VLAN Configuration

| VLAN | Name | Purpose |
|------|------|----------|
| 10 | Ops | User Network |
| 20 | Dev | User Network |
| 30 | Mgmt | Switch Management |

---

## Access Ports

| Interface | VLAN |
|-----------|------|
| FastEthernet0/1–2 | VLAN 10 |
| FastEthernet0/3–4 | VLAN 20 |
| FastEthernet0/24 | VLAN 30 |

---

## Trunk Configuration

The uplink between S1 and R1 is configured as an IEEE 802.1Q trunk.

- Trunk Interface: **GigabitEthernet0/1**
- Native VLAN: **30**
- Allowed VLANs: **10, 20, 30**

---

## Switch Management

S1 is managed using VLAN 30.

| Setting | Value |
|---------|-------|
| Management IP | 192.168.30.2/24 |
| Default Gateway | 192.168.30.1 |

---

## Security and Management Features

S1 includes the following management features:

- SSH Version 2
- Local user authentication
- PVST enabled
- Management VLAN
- Secure VTY access using SSH

---

## Headquarters Configuration Summary

The Headquarters site provides the core enterprise services for the network.

Implemented technologies include:

- Router-on-a-Stick
- OSPF Process 10
- DHCP
- NAT/PAT
- SSH
- NTP
- Syslog
- VLAN Trunking
- PVST

With Headquarters complete, the next section documents the Branch 2 implementation.
