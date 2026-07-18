# Enterprise OSPF Multi-Branch Network

# Troubleshooting Guide

**Author:** Elroy Noel

**Platform:** Cisco Packet Tracer

---

# Router-on-a-Stick Troubleshooting

## Issue 1 – VLAN Connectivity

### Symptom

Devices connected to different VLANs were unable to communicate with each other.

### Cause

Inter-VLAN routing depends on correctly configured router subinterfaces, 802.1Q encapsulation, and an operational trunk between the router and switch.

### Resolution

Verified:

- Subinterface IP addressing
- 802.1Q encapsulation
- Native VLAN configuration
- Switch trunk status

Verification Commands:

```text
show interfaces trunk
show running-config
show vlan brief
```

### Verification

Successfully confirmed communication between all local VLANs.

**Status:** ✅ Resolved

---

# DHCP Troubleshooting

## Issue 1 – DHCP Scope Verification

### Objective

Confirm that clients received addresses from the correct DHCP pool.

### Resolution

Verified:

- Network statement
- Default gateway
- DNS server
- Excluded addresses

Testing confirmed clients received valid IP addressing within their assigned VLAN.

**Status:** ✅ Operational

---

# NAT/PAT Troubleshooting

## Issue 1 – Internet Connectivity

### Objective

Verify that Headquarters translated internal enterprise addresses to the public interface.

### Resolution

Confirmed:

- Inside interfaces
- Outside interface
- PAT overload configuration
- Static default route to ISP

Verification Commands:

```text
show running-config
show ip route
```

All branch networks successfully reached the simulated ISP through Headquarters.

**Status:** ✅ Operational

---

# Switch VLAN Troubleshooting

## Issue 1 – VLAN Verification

### Objective

Confirm VLAN assignments matched the enterprise design.

Verification Commands:

```text
show vlan brief
show interfaces status
```

Verified:

- Correct VLAN membership
- Operational access ports
- Management VLANs

**Status:** ✅ Operational

---

# Trunk Verification

### Objective

Confirm VLAN traffic traversed trunk links correctly.

Verification Command

```text
show interfaces trunk
```

Verified:

- IEEE 802.1Q encapsulation
- Native VLAN
- Active VLANs
- Forwarding state

**Status:** ✅ Operational
**Routing Protocol:** OSPF Process 10

**Documentation Version:** 1.0

---

## Table of Contents

- Troubleshooting Objectives
- OSPF Issues
- Router-on-a-Stick Issues
- DHCP Issues
- NAT/PAT Issues
- Switching Issues
- EtherChannel Issues
- PVST Verification
- General Troubleshooting Methodology
- Lessons from the Implementation

---

# Troubleshooting Objectives

Enterprise networks rarely operate perfectly during the initial implementation. Throughout this project, multiple configuration issues were identified, isolated, and resolved using Cisco IOS verification commands.

This document summarizes the troubleshooting process used during the implementation of the Enterprise OSPF Multi-Branch Network and highlights the techniques used to restore normal network operation.

The troubleshooting methodology focused on:

- Identifying the symptoms
- Isolating the affected devices
- Verifying the configuration
- Applying corrective actions
- Confirming successful operation through verification testing

---

# OSPF Troubleshooting

## Issue 1 – Missing Default Route Advertisement

### Symptom

Branch routers successfully exchanged internal OSPF routes but did not receive a default route for Internet access.

### Cause

The Headquarters router had a static default route configured, but OSPF was not advertising it into the routing domain.

### Resolution

Configured:

```cisco
router ospf 10
 default-information originate
```

### Verification

Verified using:

- show ip route
- show ip protocols

Branch routers successfully learned:

```
O*E2 0.0.0.0/0
```

**Status:** ✅ Resolved

---

## Issue 2 – OSPF Route Verification

### Symptom

Needed to confirm every enterprise subnet had been learned correctly.

### Resolution

Used:

```text
show ip route
```

Verified:

- Headquarters networks
- Branch 2 networks
- Branch 3 networks
- WAN links
- Default route

**Status:** ✅ Verified

---

## Issue 3 – Neighbor Adjacency Verification

### Objective

Confirm stable OSPF adjacencies across all WAN links.

### Verification Command

```text
show ip ospf neighbor
```

Confirmed:

- R1 ↔ R2
- R2 ↔ R3
- R3 ↔ R1

All neighbors reached the **FULL** state.

**Status:** ✅ Operational

---

# EtherChannel Troubleshooting

## Issue 1 – EtherChannel Formation

### Objective

Verify successful Layer 2 aggregation between Branch 3 distribution switches.

### Resolution

Verified:

- LACP configuration
- Channel-group membership
- Port-channel operational status

Verification Command

```text
show etherchannel summary
```

Confirmed:

- Port-channel1 operational
- LACP active
- Member interfaces successfully bundled

**Status:** ✅ Operational

---

# PVST Verification

## Issue 1 – Spanning Tree Operation

### Objective

Confirm a loop-free Layer 2 topology while maintaining redundant links.

### Resolution

Verified:

- Root Bridge election
- Root Port selection
- Designated Ports
- Alternate/Blocking Ports

Verification Command

```text
show spanning-tree
```

Results confirmed that redundant links transitioned to the appropriate forwarding or blocking state, preventing switching loops while preserving redundancy.

**Status:** ✅ Operational

---

# VTP Verification

## Objective

Confirm successful VLAN synchronization within the Branch 3 switching environment.

### Resolution

Verification Command

```text
show vtp status
```

Confirmed:

- VTP Domain: **nexgent**
- Client mode operation on access switches
- VLAN database synchronized across participating switches

**Status:** ✅ Operational

---

# SSH Troubleshooting

## Objective

Verify secure remote management across all routers and switches.

### Resolution

Verification Command

```text
show ip ssh
```

Confirmed:

- SSH Version 2 enabled
- Local user authentication configured
- Secure VTY access available
- Telnet disabled

**Status:** ✅ Operational

---

# General Troubleshooting Methodology

Throughout this project, a structured troubleshooting methodology was followed to isolate and resolve issues efficiently.

The process consisted of five steps:

1. Identify the problem by observing symptoms and collecting information.
2. Verify the current configuration using Cisco IOS show commands.
3. Isolate the affected device, interface, or protocol.
4. Apply the corrective configuration.
5. Validate the solution through verification commands and end-to-end connectivity testing.

This systematic approach reduced troubleshooting time and ensured that changes resolved the root cause without introducing additional issues.

---

# Lessons from the Implementation

Completing this project reinforced several important networking principles:

- Verify physical connectivity before investigating higher-layer protocols.
- Confirm Layer 2 operation before troubleshooting Layer 3 routing.
- Validate OSPF neighbor relationships before analyzing routing tables.
- Use verification commands after every configuration change.
- Implement changes incrementally and verify each step before proceeding.
- Maintain clear documentation throughout the implementation process.

Following these practices improved both the reliability of the network and the efficiency of the troubleshooting process.

---

# Troubleshooting Summary

During implementation, routing, switching, and enterprise services were successfully validated and any identified issues were resolved using Cisco IOS verification commands.

The troubleshooting process confirmed successful operation of:

- OSPF Process 10
- Router-on-a-Stick
- VLAN Segmentation
- IEEE 802.1Q Trunking
- DHCP
- NAT/PAT
- SSH
- NTP
- Syslog
- EtherChannel (LACP)
- PVST
- VTP

All identified issues were resolved, resulting in a stable and fully operational Enterprise OSPF Multi-Branch Network.

---

# Overall Result

# ✅ SUCCESS

The troubleshooting methodology successfully identified, isolated, corrected, and verified all configuration issues encountered during the implementation of the Enterprise OSPF Multi-Branch Network.
