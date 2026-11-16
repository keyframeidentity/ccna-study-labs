
# Lab 06: Static and Default Routing

## Project Overview

This lab demonstrates the configuration and verification of **static routing and default routing** across a three-router Cisco network.

The objective was to establish communication between three separate LANs using manually configured static routes, then simplify routing on the edge routers by replacing individual remote-network routes with default routes.

## Network Topology

The network consists of three Cisco routers connected in a linear topology. Each router provides connectivity to its own LAN.

```text
       LAN 1              LAN 2              LAN 3
   192.168.10.0/24    192.168.20.0/24    192.168.30.0/24
         |                  |                  |
        SW1                SW2                SW3
         |                  |                  |
         R1 --------------- R2 --------------- R3
             10.0.12.0/30      10.0.23.0/30
```

**Router Roles**

| Router | Function                                                          |
| :----- | :---------------------------------------------------------------- |
| R1     | Connects LAN 1 to R2 and uses a default route for remote networks |
| R2     | Central router connecting R1 and R3 using static routes           |
| R3     | Connects LAN 3 to R2 and uses a default route for remote networks |

## IP Addressing Scheme

### LAN Networks

| Device | Gateway Address | Network         |
| :----- | :-------------- | :-------------- |
| R1     | 192.168.10.1    | 192.168.10.0/24 |
| R2     | 192.168.20.1    | 192.168.20.0/24 |
| R3     | 192.168.30.1    | 192.168.30.0/24 |

### Router-to-Router Links

| Connection | Network   | Subnet Mask |
| :--------- | :-------- | :---------- |
| R1 ↔ R2    | 10.0.12.0 | /30         |
| R2 ↔ R3    | 10.0.23.0 | /30         |

### Router Interface Addressing

| Router | Interface | IP Address      |
| :----- | :-------- | :-------------- |
| R1     | G0/0      | 192.168.10.1/24 |
| R1     | G0/1      | 10.0.12.1/30    |
| R2     | G0/0      | 192.168.20.1/24 |
| R2     | G0/1      | 10.0.12.2/30    |
| R2     | G0/2      | 10.0.23.1/30    |
| R3     | G0/0      | 192.168.30.1/24 |
| R3     | G0/1      | 10.0.23.2/30    |

### End Devices

| Device | IP Address    | Default Gateway |
| :----- | :------------ | :-------------- |
| PC1    | 192.168.10.10 | 192.168.10.1    |
| PC2    | 192.168.20.10 | 192.168.20.1    |
| PC3    | 192.168.30.10 | 192.168.30.1    |

## Static Routing Configuration

Initially, individual static routes were configured on all three routers to provide connectivity between the remote LAN networks.

### R1 Static Routes

```cisco
ip route 192.168.20.0 255.255.255.0 10.0.12.2
ip route 192.168.30.0 255.255.255.0 10.0.12.2
```

### R2 Static Routes

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.12.1
ip route 192.168.30.0 255.255.255.0 10.0.23.2
```

### R3 Static Routes

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.23.1
ip route 192.168.20.0 255.255.255.0 10.0.23.1
```

The routing tables were verified to confirm that remote LAN networks were learned through manually configured static routes.

## Default Routing Configuration

After verifying static routing, the configurations on R1 and R3 were simplified by replacing their individual remote-network routes with default routes.

### R1 Default Route

The individual static routes were removed and replaced with:

```cisco
ip route 0.0.0.0 0.0.0.0 10.0.12.2
```

This forwards traffic destined for unknown networks toward R2.

### R3 Default Route

The individual static routes were removed and replaced with:

```cisco
ip route 0.0.0.0 0.0.0.0 10.0.23.1
```

This forwards traffic destined for unknown networks toward R2.

### R2 Routing Configuration

R2 retained its two specific static routes to reach the LANs behind R1 and R3.

This allowed R2 to act as the central router between both edge networks.

## Verification & Testing

The following Cisco IOS commands were used to verify interface status, routing tables, and connectivity.

```cisco
show ip interface brief
show ip route
show running-config
```

### Interface Verification

All router interfaces were verified as operational.

- R1 interfaces reached an up/up state.
- R2 interfaces reached an up/up state.
- R3 interfaces reached an up/up state.
- Router-to-router connectivity was verified using direct IP pings.

### Routing Table Verification

The routing tables confirmed the correct route entries.

**R1:**

- Directly connected LAN and transit networks.
- Default route via 10.0.12.2.

**R2:**

- Directly connected LAN and transit networks.
- Static route to 192.168.10.0/24 via 10.0.12.1.
- Static route to 192.168.30.0/24 via 10.0.23.2.

**R3:**

- Directly connected LAN and transit networks.
- Default route via 10.0.23.1.

Default routes appeared in the routing table with the `S*` route code.

### Connectivity Testing

The following end-to-end connectivity tests were successful:

- PC1 successfully pinged PC2.
- PC1 successfully pinged PC3.
- PC3 successfully pinged PC1.
- PC3 successfully pinged PC2.
- All three LANs maintained end-to-end connectivity.

Successful ping tests confirmed that static and default routing were functioning correctly.

## Troubleshooting

Several configuration and verification steps were completed during the lab.

| Issue                                              | Resolution                                                 |
| :------------------------------------------------- | :--------------------------------------------------------- |
| Router interfaces initially required configuration | Assigned IP addresses and enabled interfaces               |
| Router-to-router connectivity needed verification  | Tested transit interface addresses using ping              |
| Remote LAN networks required routing entries       | Configured static routes on all three routers              |
| Edge routers had multiple individual remote routes | Replaced specific routes with default routes               |
| Routing behavior needed confirmation               | Verified routing tables and tested end-to-end connectivity |

This lab reinforced the importance of correct next-hop addresses, subnet masks, routing table entries, and return paths.

## Key Takeaways

Through this lab, I gained practical experience with:

- Configuring static routes on Cisco routers.
- Understanding default routes and their purpose.
- Using next-hop addresses to forward traffic.
- Reading static routes in Cisco routing tables.
- Understanding the `S` and `S*` route codes.
- Simplifying edge-router configurations using default routes.
- Verifying router interface status.
- Troubleshooting routing and connectivity issues.
- Testing end-to-end communication across multiple LANs.

## Lab Files & Evidence

The following supporting materials are included:

- [x] Cisco Packet Tracer project file (`06-static-and-default-routing.pkt`)
- [x] Network topology screenshot
- [x] Router interface addressing screenshot
- [x] R1 static routing screenshot
- [x] R2 static routing screenshot
- [x] R3 static routing screenshot
- [x] R1 default route screenshot
- [x] R3 default route screenshot
- [x] PC1 connectivity test screenshot
- [x] PC3 connectivity test screenshot

---

**Project:** Cisco Static and Default Routing Lab
**Lab Number:** 06
**Status:** Completed
