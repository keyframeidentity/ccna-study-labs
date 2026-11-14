
# Lab 03: EtherChannel Configuration Using LACP

## Project Overview

This lab demonstrates the configuration and verification of **EtherChannel** between two Cisco switches using the **Link Aggregation Control Protocol (LACP)**.

The objective was to combine two physical switch links into one logical connection, increasing available link capacity and providing redundancy if one physical link fails.

## Network Topology

The network consists of two Cisco 2960 switches connected through two FastEthernet links.

```text
        SW1                         SW2
   +------------+              +------------+
   |            | Fa0/23       |            |
   |            |==============|            |
   |            | Fa0/24       |            |
   |            |==============|            |
   |            |              |            |
   +------------+              +------------+
          \                       /
           \_____ Port-Channel 1_/
                   LACP
```

**Physical Connections**

| Switch | Interface | Connected To |
| :----- | :-------- | :----------- |
| SW1    | Fa0/23    | SW2 Fa0/23   |
| SW1    | Fa0/24    | SW2 Fa0/24   |

**Logical Interface:** Port-channel 1

## EtherChannel Configuration

LACP was used to negotiate the EtherChannel between the two switches.

| Switch | LACP Mode | Role                         |
| :----- | :-------- | :--------------------------- |
| SW1    | Active    | Initiates LACP negotiation   |
| SW2    | Passive   | Responds to LACP negotiation |

Both physical interfaces were assigned to channel-group 1.

### Trunk Configuration

Port-channel 1 was configured as an 802.1Q trunk.

| Setting         | Configuration |
| :-------------- | :------------ |
| Port-channel    | 1             |
| Encapsulation   | 802.1Q        |
| Switchport Mode | Trunk         |
| Allowed VLANs   | 10, 20        |
| Native VLAN     | 1             |

## Verification & Testing

The following Cisco IOS commands were used to verify EtherChannel operation, trunking, and spanning-tree status.

```bash
show etherchannel summary
show interfaces trunk
show spanning-tree vlan 10
show spanning-tree vlan 20
show interfaces status
```

### EtherChannel Results

The final EtherChannel summary showed:

```text
Po1(SU)  LACP  Fa0/23(P) Fa0/24(P)
```

**Results:**

- Port-channel 1 successfully reached the operational state.
- LACP successfully negotiated between SW1 and SW2.
- Both physical interfaces were bundled into the logical EtherChannel.
- Both physical links were operational and participating in the channel.

### Trunk Verification

Port-channel 1 successfully operated as an 802.1Q trunk.

- VLAN 10 was allowed.
- VLAN 20 was allowed.
- Both VLANs were active on the trunk.

### Spanning Tree Verification

Spanning Tree Protocol recognized Port-channel 1 as a logical interface.

- VLAN 10: Po1 reached Designated Forwarding.
- VLAN 20: Po1 reached Root Forwarding.

## Redundancy Testing

A physical link failure was simulated by shutting down SW2 Fa0/23.

During the test, the EtherChannel summary showed:

```text
Po1(SU)  LACP  Fa0/23(D) Fa0/24(P)
```

**Results:**

- Fa0/23 went down.
- Fa0/24 remained bundled and operational.
- Port-channel 1 remained up.
- Connectivity through the remaining physical link was maintained.

Fa0/23 was subsequently restored, and both physical interfaces returned to the bundled state.

## Troubleshooting

Several configuration issues were identified and resolved during the lab.

| Issue                                                           | Resolution                                                                        |
| :-------------------------------------------------------------- | :-------------------------------------------------------------------------------- |
| Existing PAgP configuration conflicted with LACP                | Removed the old channel configuration before rebuilding the EtherChannel          |
| Stale Port-channel 1 configuration remained                     | Removed the existing logical interface and verified the channel-group was cleared |
| Physical interface Fa0/22 appeared in the initial configuration | Confirmed Fa0/23 and Fa0/24 were the intended connected links                     |
| EtherChannel members initially showed individual status         | Reconfigured both switches with compatible LACP modes                             |

This troubleshooting reinforced the importance of checking existing EtherChannel configurations and ensuring both switches use compatible negotiation protocols.

## Key Takeaways

Through this lab, I gained practical experience with:

- Understanding EtherChannel and link aggregation.
- Configuring LACP using active and passive modes.
- Combining multiple physical interfaces into one logical interface.
- Configuring Layer 2 EtherChannel trunking.
- Allowing specific VLANs across a trunk.
- Understanding EtherChannel status codes.
- Verifying spanning-tree forwarding over a port-channel.
- Testing link redundancy by simulating a physical link failure.
- Troubleshooting incompatible PAgP and LACP configurations.

## Lab Files & Evidence

The following supporting materials are included:

- [x] Cisco Packet Tracer project file (.pkt)
- [x] Network topology screenshot
- [x] EtherChannel summary verification screenshot
- [x] Trunk verification screenshot
- [x] Spanning-tree verification screenshot
- [x] Link failure and redundancy test screenshot

---

**Project:** Cisco EtherChannel LACP Lab
**Lab Number:** 03
**Status:** Completed
