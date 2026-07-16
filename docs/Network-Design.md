# Network Design

## Executive Summary

This project demonstrates the design and implementation of a scalable multi-branch enterprise network using **Open Shortest Path First (OSPF)** as the Interior Gateway Protocol (IGP). The network interconnects a headquarters location and two branch offices while providing secure internal communication, Internet access, wireless connectivity, and centralized network management.

Each branch is segmented into multiple VLANs and uses **Router-on-a-Stick** to provide Inter-VLAN routing. OSPF dynamically exchanges routing information between all sites, eliminating the need for extensive static routing and allowing the network to scale efficiently.

Additional enterprise services including DHCP, NAT/PAT, EtherChannel (LACP), VTP, SSH, NTP, and Syslog are integrated to simulate a production-ready enterprise environment.

---

# Business Requirements

The enterprise network was designed to satisfy the following objectives:

- Connect three geographically separated branch offices.
- Provide resilient WAN connectivity between all locations.
- Segment user traffic using VLANs.
- Enable communication between VLANs through Router-on-a-Stick.
- Automate IP address assignment using DHCP.
- Provide secure Internet connectivity.
- Implement dynamic routing for simplified administration.
- Increase Layer 2 redundancy using EtherChannel.
- Support wireless users.
- Secure remote device administration.
- Centralize network monitoring and time synchronization.
- Build a scalable architecture capable of supporting future growth.

---

# Enterprise Architecture

The network consists of four primary locations:

## Headquarters (HQ)

Headquarters serves as the primary enterprise location and Internet gateway.

Primary functions include:

- OSPF routing
- Router-on-a-Stick
- DHCP services
- NAT/PAT
- Default route to the ISP
- Internet gateway

Configured VLANs

| VLAN | Purpose | Network |
|-------|----------|----------------|
| VLAN 10 | User Network | 172.16.10.0/24 |
| VLAN 20 | User Network | 172.16.20.0/24 |

---

## Branch Office 2

Branch Office 2 provides wired and wireless connectivity for remote employees.

Primary functions include:

- OSPF routing
- Router-on-a-Stick
- DHCP
- Wireless Access Point
- Enterprise wireless clients

Configured VLANs

| VLAN | Purpose | Network |
|-------|----------|----------------|
| VLAN 40 | User Network | 172.16.40.0/24 |
| VLAN 50 | User Network | 172.16.50.0/24 |
| VLAN 60 | Wireless Network | 172.16.60.0/24 |

---

## Branch Office 3

Branch Office 3 demonstrates a more advanced Layer 2 switching design using multiple switches connected through EtherChannel.

Primary functions include:

- OSPF routing
- Router-on-a-Stick
- DHCP
- EtherChannel (LACP)
- VTP
- VLAN trunking

Configured VLANs

| VLAN | Purpose | Network |
|-------|----------|----------------|
| VLAN 70 | User Network | 10.0.70.0/24 |
| VLAN 80 | User Network | 10.0.80.0/24 |
| VLAN 90 | Management / Additional Services | 10.0.90.0/24 |

---

# WAN Design

The enterprise WAN interconnects all three sites using point-to-point serial links.

OSPF provides dynamic route exchange across the WAN, allowing each branch to automatically learn remote networks without requiring extensive static routing.

Benefits of this design include:

- Faster convergence
- Simplified administration
- Automatic route advertisement
- Improved scalability

---

# Routing Design (OSPF)

Open Shortest Path First (OSPF) was selected as the enterprise Interior Gateway Protocol because it provides:

- Fast convergence
- Hierarchical routing capability
- Vendor interoperability
- Efficient SPF calculations
- Automatic topology discovery

All routers participate in the enterprise OSPF domain and advertise their directly connected LAN networks.

---

# Layer 2 Design

## VLAN Segmentation

Users are separated into individual VLANs to improve security, reduce broadcast traffic, and simplify network administration.

Benefits include:

- Broadcast containment
- Improved security
- Easier troubleshooting
- Better scalability

---

## Router-on-a-Stick

Each branch router provides Inter-VLAN routing using IEEE 802.1Q encapsulation.

This design allows multiple VLANs to share a single physical uplink while maintaining logical separation between broadcast domains.

---

## VTP

VLAN Trunking Protocol (VTP) is implemented within Branch Office 3 to simplify VLAN administration across multiple switches.

Advantages include:

- Centralized VLAN management
- Automatic VLAN synchronization
- Reduced configuration effort

---

## EtherChannel (LACP)

Branch Office 3 implements EtherChannel using the Link Aggregation Control Protocol (LACP).

Benefits include:

- Increased bandwidth
- Link redundancy
- Load balancing
- Improved availability

---

# IP Addressing Strategy

Each VLAN is assigned its own subnet.

This hierarchical addressing plan provides:

- Logical organization
- Simplified troubleshooting
- Efficient routing
- Easier future expansion

---

# DHCP Design

Each branch router functions as the DHCP server for its local VLANs.

Benefits include:

- Automatic IP assignment
- Centralized management
- Reduced administrative overhead
- Consistent client configuration

---

# NAT/PAT Design

Headquarters provides Internet connectivity through Network Address Translation (NAT/PAT).

Private enterprise addresses are translated into a public address before traffic exits toward the ISP.

This design:

- Conserves public IP addresses
- Protects internal addressing
- Enables Internet access for all branches

---

# Wireless Design

Wireless connectivity is provided within Branch Office 2 using an enterprise access point.

Wireless users receive IP addressing through DHCP and communicate securely with the wired infrastructure using the same enterprise routing architecture.

---

# Network Management

The network incorporates several management services commonly deployed in enterprise environments.

## SSH

SSH provides encrypted remote administration of routers and switches.

## NTP

Network Time Protocol synchronizes device clocks to ensure consistent timestamps for troubleshooting and logging.

## Syslog

Syslog centralizes event logging and simplifies network monitoring by recording operational messages generated by network devices.

---

# Internet Connectivity

Internet access is provided through an ISP router connected to Headquarters.

Traffic destined for external networks follows the default route toward the ISP where NAT/PAT enables communication with public resources such as the simulated web server.

---

# Scalability Considerations

The network was designed to accommodate future growth by:

- Supporting additional VLANs
- Allowing new branch offices to join the OSPF domain
- Expanding EtherChannel capacity
- Increasing wireless coverage
- Supporting additional enterprise services

---

# Design Decisions

Several enterprise design principles were applied throughout this project:

- Dynamic routing instead of static routing
- VLAN segmentation for security
- Router-on-a-Stick for efficient Inter-VLAN routing
- DHCP automation
- Centralized Internet access
- Layer 2 redundancy using EtherChannel
- Secure device management using SSH
- Time synchronization with NTP
- Centralized logging with Syslog

These design decisions improve operational efficiency while closely reflecting real-world enterprise network deployments.

---

# Design Summary

This project demonstrates the implementation of a scalable enterprise network using Cisco Packet Tracer. By combining OSPF, VLAN segmentation, Router-on-a-Stick, DHCP, NAT/PAT, EtherChannel, VTP, WLAN, SSH, NTP, and Syslog, the design provides a realistic representation of the technologies commonly deployed in modern enterprise environments.

The completed implementation emphasizes not only successful configuration but also sound network design principles, structured documentation, verification, and operational readiness.
