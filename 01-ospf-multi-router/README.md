# Lab 01: Multi-Router OSPF Configuration

![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Cisco](https://img.shields.io/badge/Platform-Cisco-blue)
![Routing](https://img.shields.io/badge/Protocol-OSPF-orange)
![Area](https://img.shields.io/badge/Area-0-lightgrey)

## Project Overview

This lab demonstrates the configuration and verification of **Open Shortest Path First (OSPF)** routing across a three-router network.

The objective was to establish dynamic routing between three separate LANs using OSPF Area 0, allowing devices on different networks to communicate without manually configuring static routes.

## Network Topology

The network consists of three interconnected routers, each serving its own LAN.

```text
       LAN 1             LAN 2             LAN 3
    192.168.10.0/24   192.168.20.0/24   192.168.30.0/24
         |                 |                 |
         R1 -------------- R2 -------------- R3
         |                 |                 |
         +------- OSPF Area 0 ---------------+
```

**Router Roles**

| Router | Function |
|:---:|---|
| R1 | Connects LAN 1 to R2 |
| R2 | Central router connecting LAN 2, R1, and R3 |
| R3 | Connects LAN 3 to R2 |

All three routers participate in OSPF Area 0.

## IP Addressing Scheme

### LAN Networks

| Device | LAN Gateway | Subnet |
|:---:|---|:---:|
| R1 | 192.168.10.1 | /24 |
| R2 | 192.168.20.1 | /24 |
| R3 | 192.168.30.1 | /24 |

### Router-to-Router Links

| Connection | Network | Subnet |
|:---:|---|:---:|
| R1 ↔ R2 | 10.0.12.0 | /30 |
| R2 ↔ R3 | 10.0.23.0 | /30 |

## OSPF Configuration

OSPF was configured using Process ID 1, with all three routers participating in Area 0.

| Router | Router ID | Process ID | Area |
|:---:|:---:|:---:|:---:|
| R1 | 1.1.1.1 | 1 | 0 |
| R2 | 2.2.2.2 | 1 | 0 |
| R3 | 3.3.3.3 | 1 | 0 |

- **Routing Protocol:** OSPF
- **Process ID:** 1
- **Backbone Area:** Area 0

## Verification & Testing

The following Cisco IOS commands were used to verify interface status, OSPF neighbor relationships, routing table entries, and protocol configuration.

```bash
show ip interface brief
show ip ospf neighbor
show ip route ospf
show ip protocols
```

### Results

- OSPF neighbor relationships successfully reached the **FULL** state.
- All three routers learned remote networks through OSPF.
- OSPF routes appeared in routing tables with the `O` route code.
- PC1 successfully pinged PC3 across the network.
- End-to-end connectivity was established between all three LANs.

## Troubleshooting

Several configuration issues were identified and resolved during the lab.

| Issue | Resolution |
|---|---|
| R2 interface toward R3 was administratively down | Enabled the interface using `no shutdown` |
| Incorrect wildcard masks | Corrected wildcard masks to `0.0.0.3` for /30 networks |
| Typo in R3 OSPF network statement | Corrected the OSPF network statement |

This troubleshooting reinforced the importance of verifying interface states, subnet masks, and routing protocol configurations.

## Key Takeaways

Through this lab, I gained practical experience with:

- Configuring OSPF dynamic routing.
- Establishing and verifying OSPF neighbor adjacencies.
- Understanding router IDs and their role in OSPF.
- Using wildcard masks to identify participating networks.
- Identifying dynamically learned routes using the OSPF `O` route code.
- Troubleshooting interface and routing configuration issues.
- Verifying end-to-end connectivity across multiple networks.

## Lab Evidence

### 1. Network Topology

![OSPF Network Topology](https://private-user-images.githubusercontent.com/5049674/705976738-d108f43e-ede9-4676-9ec9-2feb85c42cb5.png)

### 2. OSPF Neighbor Verification

![OSPF Neighbor Verification](https://private-user-images.githubusercontent.com/5049674/705976535-55d06448-c90f-4729-b41f-dc956e6fd529.png)

### 3. Routing Table Verification

![OSPF Routing Table](https://private-user-images.githubusercontent.com/5049674/705956572-809916ba-eaf9-4c22-8d82-7534052c15b9.png)

### 4. Connectivity Testing

![Connectivity Testing](https://private-user-images.githubusercontent.com/5049674/705954659-c9221f85-c963-4521-9152-733f5fa00a9c.png)

---

**Lab 01 | Cisco Networking | OSPF Dynamic Routing**
