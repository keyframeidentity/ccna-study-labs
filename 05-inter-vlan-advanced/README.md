# Lab 05: Layer 3 Switching & Inter-VLAN Routing

## Project Overview

This lab demonstrates inter-VLAN routing using a Cisco multilayer switch (Layer 3 switch).

The objective was to configure a Layer 3 switch to route traffic between separate VLANs using Switched Virtual Interfaces (SVIs), eliminating the need for an external router to handle inter-VLAN communication.

## Network Topology

The network consists of a Cisco 3560 multilayer switch connected to a Cisco 2960 Layer 2 access switch.

The access switch connects four PCs across two VLANs.

### VLAN Assignments

| VLAN | Name  | Network         |
| ---- | ----- | --------------- |
| 10   | Admin | 192.168.10.0/24 |
| 20   | IT    | 192.168.20.0/24 |

### Device and Port Assignments

| Device         | Switch Port | VLAN  |
| -------------- | ----------- | ----- |
| PC1            | SW1 Fa0/1   | 10    |
| PC2            | SW1 Fa0/2   | 10    |
| PC3            | SW1 Fa0/3   | 20    |
| PC4            | SW1 Fa0/4   | 20    |
| Layer 3 Switch | Gi1/0/1     | Trunk |
| SW1            | Gi0/1       | Trunk |

## IP Addressing Scheme

### Default Gateways

| VLAN | SVI Interface | Gateway Address |
| ---- | ------------- | --------------- |
| 10   | Vlan10        | 192.168.10.1/24 |
| 20   | Vlan20        | 192.168.20.1/24 |

### End Devices

| Device | IP Address    | Subnet Mask   | Default Gateway |
| ------ | ------------- | ------------- | --------------- |
| PC1    | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| PC2    | 192.168.10.11 | 255.255.255.0 | 192.168.10.1    |
| PC3    | 192.168.20.10 | 255.255.255.0 | 192.168.20.1    |
| PC4    | 192.168.20.11 | 255.255.255.0 | 192.168.20.1    |

## Layer 3 Switch Configuration

The multilayer switch was configured with IP routing enabled.

```cisco
ip routing
```

This enables the switch to route packets between directly connected VLAN networks.

### SVI Configuration

VLAN 10:

```cisco
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
```

VLAN 20:

```cisco
interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
```

Both SVIs reached an operational state of **up/up**.

## VLAN and Trunk Configuration

VLANs 10 and 20 were created on the multilayer switch and access switch.

The connection between the Layer 3 switch and SW1 was configured as an 802.1Q trunk.

### Layer 3 Switch Uplink

```cisco
interface gigabitEthernet 1/0/1
switchport mode trunk
```

### Layer 2 Switch Uplink

```cisco
interface gigabitEthernet 0/1
switchport mode trunk
```

Both switches successfully established trunking.

VLANs 10 and 20 were verified as active and forwarding across the uplink.

## Layer 2 Access Switch

The Cisco 2960 was configured to operate as a Layer 2 access switch.

Its previous SVI gateway addresses were removed to avoid duplicate IP addresses.

The Layer 3 switch became responsible for routing between VLANs, while the 2960 handled Layer 2 switching.

## Verification & Testing

The following Cisco IOS commands were used to verify the configuration:

```cisco
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show cdp neighbors
```

### Routing Table Verification

The Layer 3 switch displayed the following directly connected networks:

```text
C 192.168.10.0/24 is directly connected, Vlan10
L 192.168.10.1/32 is directly connected, Vlan10

C 192.168.20.0/24 is directly connected, Vlan20
L 192.168.20.1/32 is directly connected, Vlan20
```

The connected routes confirmed that both VLAN networks were available for inter-VLAN routing.

### Connectivity Testing

The following tests were successful:

- PC1 successfully pinged the VLAN 10 gateway.
- PC1 successfully pinged the VLAN 20 gateway.
- PC1 successfully pinged PC3 across VLANs.
- PC2 successfully pinged PC4 across VLANs.
- Both VLANs maintained end-to-end connectivity through the Layer 3 switch.

\<badge color="success">Successful inter-VLAN routing\</badge>

## Key Takeaways

Through this lab, I gained practical experience with:

- Configuring a Cisco multilayer switch.
- Enabling Layer 3 routing with `ip routing`.
- Creating and configuring Switched Virtual Interfaces.
- Using SVIs as default gateways for separate VLANs.
- Configuring 802.1Q trunk links.
- Understanding the difference between Layer 2 and Layer 3 switching.
- Reading connected and local routes in the routing table.
- Troubleshooting duplicate gateway IP addresses.
- Verifying end-to-end connectivity between separate VLANs.

## Lab Files & Evidence

Supporting materials:

- [x] Cisco Packet Tracer project file (`05-layer3-intervlan.pkt`)
- [x] Network topology screenshot
- [x] VLAN configuration screenshot
- [x] Trunk configuration screenshot
- [x] SVI status verification screenshot
- [x] Routing table screenshot
- [x] Successful inter-VLAN ping tests

---

**Project:** Cisco Layer 3 Switching Lab
**Lab Number:** 05
**Status:** Completed
