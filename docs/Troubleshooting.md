# Enterprise OSPF Multi-Branch Network

# Troubleshooting Guide

**Author:** Elroy Noel

**Platform:** Cisco Packet Tracer

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
