
# Lab 07: EIGRP Dynamic Routing Configuration

## Project Overview

This lab demonstrates the configuration and verification of **Enhanced Interior Gateway Routing Protocol (EIGRP)** across a three-router network.

The objective was to establish dynamic routing between three separate LANs using EIGRP Autonomous System (AS) 100, allowing devices on different networks to communicate without manually configuring static routes.

## Network Topology

The network consists of three interconnected Cisco routers, each serving its own LAN.

```text
       LAN 1             LAN 2             LAN 3
    192.168.10.0/24   192.168.20.0/24   192.168.30.0/24
         |                 |                 |
        SW1               SW2               SW3
         |                 |                 |
         R1 -------------- R2 -------------- R3
         |                 |                 |
         +-------- EIGRP AS 100 -------------+
```

**Router Roles**

| Router | Function |
| :----- | :------- |
| R1 | Connects LAN 1 to R2 |
| R2 | Central router connecting LAN 2, R1, and R3 |
| R3 | Connects LAN 3 to R2 |

All three routers participate in EIGRP AS 100.

## IP Addressing Scheme

### LAN Networks

| Device | LAN Gateway | Subnet |
| :----- | :---------- | :----- |
| R1 | 192.168.10.1 | /24 |
| R2 | 192.168.20.1 | /24 |
| R3 | 192.168.30.1 | /24 |

### Router-to-Router Links

| Connection | Network | Subnet Mask |
| :--------- | :------ | :---------- |
| R1 ↔ R2 | 10.0.12.0 | /30 |
| R2 ↔ R3 | 10.0.23.0 | /30 |

### PC Addressing

| Device | IP Address | Default Gateway |
| :----- | :--------- | :-------------- |
| PC1 | 192.168.10.10 | 192.168.10.1 |
| PC2 | 192.168.20.10 | 192.168.20.1 |
| PC3 | 192.168.30.10 | 192.168.30.1 |

## EIGRP Configuration

EIGRP was configured using Autonomous System number 100 on all three routers.

### R1 Configuration

```cisco
router eigrp 100
 network 192.168.10.0 0.0.0.255
 network 10.0.12.0 0.0.0.3
 no auto-summary
```

### R2 Configuration

```cisco
router eigrp 100
 network 192.168.20.0 0.0.0.255
 network 10.0.12.0 0.0.0.3
 network 10.0.23.0 0.0.0.3
 no auto-summary
```

### R3 Configuration

```cisco
router eigrp 100
 network 192.168.30.0 0.0.0.255
 network 10.0.23.0 0.0.0.3
 no auto-summary
```

**Routing Protocol:** EIGRP  
**Autonomous System:** 100  
**Route Code:** D  
**Administrative Distance:** 90

## Verification & Testing

The following Cisco IOS commands were used to verify EIGRP neighbor relationships, learned routes, and routing protocol operation.

```bash
show ip eigrp neighbors
show ip route eigrp
show ip protocols
show ip interface brief
```

### EIGRP Neighbor Verification

R2 successfully established EIGRP neighbor relationships with both R1 and R3.

| Router | Neighbor Address | Interface |
| :----- | :--------------- | :-------- |
| R2 | 10.0.12.1 | GigabitEthernet0/1 |
| R2 | 10.0.23.2 | GigabitEthernet0/1/0 |

Both neighbor relationships were established successfully with zero queued EIGRP packets.

### Routing Table Verification

EIGRP successfully distributed remote network information across all three routers.

**R2 Learned Routes**

- 192.168.10.0/24 through R1.
- 192.168.30.0/24 through R3.

**R1 Learned Routes**

- 192.168.30.0/24 through R2.

**R3 Learned Routes**

- 192.168.10.0/24 through R2.
- 192.168.20.0/24 through R2.
- 10.0.12.0/30 through R2.

Routes appeared in the routing table with the `D` route code, identifying them as EIGRP-learned routes.

### Connectivity Testing

End-to-end connectivity was verified using ICMP ping tests.

- PC1 successfully pinged PC3.
- PC3 successfully pinged PC1.
- Communication between separate LANs was established through dynamically learned EIGRP routes.

## Troubleshooting

During the lab, an addressing issue was encountered while configuring the point-to-point router link.

| Issue | Resolution |
| :---- | :--------- |
| Attempted to assign 10.0.12.3 to a /30 interface | Identified the address as the broadcast address and assigned a valid host address |
| EIGRP neighbor relationships needed verification | Used `show ip eigrp neighbors` to confirm both adjacent routers |
| Remote network reachability needed confirmation | Verified learned routes and tested connectivity between PC1 and PC3 |

This troubleshooting reinforced the importance of understanding subnet boundaries, usable host addresses, and EIGRP neighbor relationships.

## Key Takeaways

Through this lab, I gained practical experience with:

- Configuring EIGRP dynamic routing.
- Understanding EIGRP Autonomous System numbers.
- Using wildcard masks in EIGRP network statements.
- Establishing and verifying EIGRP neighbor relationships.
- Understanding EIGRP route codes and administrative distance.
- Reading dynamically learned routes in the routing table.
- Understanding EIGRP metrics and next-hop addresses.
- Troubleshooting subnet addressing issues.
- Verifying end-to-end connectivity across multiple LANs.

---

**Project:** Cisco EIGRP Dynamic Routing Lab  
**Lab Number:** 07  
**Status:** Completed
