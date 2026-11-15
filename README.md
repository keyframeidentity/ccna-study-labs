# ccna-study-labs

Cisco CCNA study labs, Packet Tracer topologies, configurations, and troubleshooting notes.

A hands-on collection of Cisco networking labs built with **Cisco Packet Tracer** while studying CCNA topics. Each lab includes practical configurations, verification results, and troubleshooting documentation.

Background

I previously earned the Cisco CCNA certification and am currently working toward earning it again. This repository documents my hands-on preparation, reinforces networking fundamentals, and provides practical experience with Cisco routers, switches, and IOS configurations.

Rather than relying solely on theory, each lab is built from scratch, tested, and documented with configurations, verification commands, troubleshooting notes, and Packet Tracer project files.

CCNA Preparation Plan

My goal is to complete the hands-on labs in this repository and then revisit Neil Anderson's CCNA Udemy course as a comprehensive review before December 7, 2026.

My preparation approach:

Complete the planned Packet Tracer labs.

Practice configuring and troubleshooting each technology from memory.

Revisit Neil Anderson's CCNA course from beginning to end.

Reinforce weaker topics with additional practice and review.

Prepare for the CCNA exam by December 7, 2026.

The combination of hands-on lab work and a full course review will help reinforce both practical configuration skills and exam knowledge.

## Lab Index

| Lab | Topic | Status |
| :--- | :--- | :--- |
| 01 | Multi-Router OSPF (Area 0) | Completed |
| 02 | VLANs, Trunking & Inter-VLAN Routing | Completed |
| 03 | EtherChannel Using LACP | Completed |
| 04 | Spanning Tree Protocol (STP) | Planned |
| 05 | Access Control Lists (ACLs) and NAT | Planned |
| 06 | DHCP, DNS, and IPv4 Services | Planned |
| 07 | Static Routing | Planned |
| 08 | EIGRP | Planned |
| 09 | First Hop Redundancy Protocols (FHRP) | Planned |
| 10 | WAN and VPN Technologies | Planned |
| 11 | Network Security | Planned |
| 12 | SSH and Device Management | Planned |
| 13 | Network Troubleshooting | Planned |
| 14 | Enterprise Network Capstone | Planned |

## Completed Labs

### Lab 01 – Multi-Router OSPF

- Configured OSPF across three routers.
- Established neighbor adjacencies in Area 0.
- Verified dynamically learned routes.
- Tested end-to-end connectivity between separate LANs.

### Lab 02 – VLANs, Trunking & Inter-VLAN Routing

- Created and assigned VLANs.
- Configured 802.1Q trunk links.
- Implemented router-on-a-stick inter-VLAN routing.
- Verified connectivity between VLANs.

### Lab 03 – EtherChannel Using LACP

- Configured EtherChannel between two Cisco 2960 switches.
- Established LACP negotiation using active and passive modes.
- Configured an 802.1Q trunk over Port-channel 1.
- Verified spanning-tree forwarding.
- Tested redundancy by shutting down one physical link while maintaining the EtherChannel.

Lab 04 – Spanning Tree Protocol (STP)

Configured and verified STP across three switches.
Changed the root bridge through bridge priority configuration.
Examined root, designated, and alternate port roles.
Tested STP convergence after a link failure.
Configured PortFast and BPDU Guard.
Triggered and recovered from a BPDU Guard violation.

Lab 05 – Layer 3 Switching & Inter-VLAN Routing
Configured a Cisco 3560 multilayer switch.
Enabled Layer 3 routing using ip routing.
Created VLAN 10 and VLAN 20 SVIs as default gateways.
Configured 802.1Q trunking between Layer 2 and Layer 3 switches.
Converted the 2960 into a Layer 2 access switch.
Verified connected routes in the routing table.
Successfully tested communication between separate VLANs.

## Learning Goals

- Configure and troubleshoot Cisco routers and switches.
- Understand IPv4 addressing and subnetting.
- Configure dynamic routing protocols.
- Practice VLANs, trunking, and spanning tree.
- Configure EtherChannel and link redundancy.
- Understand access control lists and NAT.
- Build confidence with Cisco IOS commands.
- Develop practical network troubleshooting skills.

## Lab Environment

- Cisco Packet Tracer
- Cisco 2911 routers
- Cisco 2960 switches
- End devices (PCs)
- Cisco IOS CLI

## Repository Structure

Each lab is organized into its own directory containing the Packet Tracer project, documentation, and supporting evidence.

```text
ccna-study-labs/
├── ospf/
├── 02-vlan-trunking/
├── 03-etherchannel/
├── 04-stp/
├── 05-acl-nat/
├── 06-dhcp-dns-ipv4/
└── ...
```

## Approach

Each lab is built hands-on from scratch and documented with:

- Network topology
- IP addressing plan
- Device configurations
- Verification commands and results
- Troubleshooting notes
- Lessons learned
- Packet Tracer project files and screenshots

These labs are primarily for personal CCNA study, practical networking experience, and demonstrating hands-on Cisco networking skills.

---

**Project:** CCNA Study Labs  
**Platform:** Cisco Packet Tracer  
**Status:** In Progress
