# CCNA Study Labs

Cisco CCNA study labs, Packet Tracer topologies, configurations, and troubleshooting notes.

A hands-on collection of Cisco networking labs built with **Cisco Packet Tracer** while studying CCNA topics. Each lab includes practical configurations, verification results, and troubleshooting documentation.

## Background

I previously earned the Cisco CCNA certification and am currently working toward earning it again. This repository documents my hands-on preparation, reinforces networking fundamentals, and provides practical experience with Cisco routers, switches, and IOS configurations.

Rather than relying solely on theory, each lab is built from scratch, tested, and documented with configurations, verification commands, troubleshooting notes, and Packet Tracer project files.

## CCNA Preparation Plan

My goal is to complete the hands-on labs in this repository and then revisit Neil Anderson's CCNA Udemy course as a comprehensive review before December 7, 2026.

My preparation approach:

- Complete the planned Packet Tracer labs.
- Practice configuring and troubleshooting each technology from memory.
- Revisit Neil Anderson's CCNA course from beginning to end.
- Reinforce weaker topics with additional practice and review.
- Prepare for the CCNA exam by December 7, 2026.

## Lab Index

| Lab | Topic | Status |
|---|---|---|
| 01 | Multi-Router OSPF (Area 0) | Completed |
| 02 | VLANs, Trunking & Router-on-a-Stick | Completed |
| 03 | EtherChannel Using LACP | Completed |
| 04 | Spanning Tree Protocol (STP) | Completed |
| 05 | Layer 3 Switching & Inter-VLAN Routing | Completed |
| 06 | Access Control Lists (ACLs) and NAT/PAT | Completed |
| 07 | EIGRP Dynamic Routing | Completed |
| 08 | Static and Default Routing | Planned |
| 09 | DHCP, DNS, and IPv4 Services | Planned |
| 10 | First Hop Redundancy Protocols (FHRP) | Planned |
| 11 | WAN and VPN Technologies | Planned |
| 12 | Network Security | Planned |
| 13 | SSH and Device Management | Planned |
| 14 | Network Troubleshooting | Planned |
| 15 | Enterprise Network Capstone | Planned |

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

### Lab 04 – Spanning Tree Protocol (STP)

- Configured and verified STP across three switches.
- Changed the root bridge through bridge priority configuration.
- Examined root, designated, and alternate port roles.
- Tested STP convergence after a link failure.
- Configured PortFast and BPDU Guard.
- Triggered and recovered from a BPDU Guard violation.

### Lab 05 – Layer 3 Switching & Inter-VLAN Routing

- Configured a Cisco 3560 multilayer switch.
- Enabled Layer 3 routing using `ip routing`.
- Created VLAN 10 and VLAN 20 SVIs as default gateways.
- Configured 802.1Q trunking between Layer 2 and Layer 3 switches.
- Converted the 2960 into a Layer 2 access switch.
- Verified connected routes in the routing table.
- Successfully tested communication between separate VLANs.

### Lab 06 – Access Control Lists (ACLs) and NAT/PAT

- Configured extended ACLs to control traffic between internal networks and an external destination.
- Applied an extended ACL inbound on the router interface.
- Verified permitted and denied traffic using ACL counters.
- Configured NAT inside and outside interfaces.
- Created a standard ACL to identify internal networks eligible for translation.
- Configured PAT overload using the router's outside interface address.
- Verified NAT translations and statistics.
- Troubleshot an ACL conflict that initially blocked translated traffic.
- Tested successful and denied connectivity from multiple PCs.

### Lab 07 – EIGRP Dynamic Routing

- Configured EIGRP across three Cisco routers using Autonomous System 100.
- Advertised LAN and router-to-router networks using wildcard masks.
- Established EIGRP neighbor relationships between adjacent routers.
- Verified learned routes using the `D` route code.
- Examined EIGRP administrative distance, metrics, and next-hop addresses.
- Verified routing table entries on all three routers.
- Successfully tested bidirectional connectivity between PC1 and PC3.

## Learning Goals

- Configure and troubleshoot Cisco routers and switches.
- Understand IPv4 addressing and subnetting.
- Configure dynamic routing protocols.
- Practice VLANs, trunking, and spanning tree.
- Configure EtherChannel and link redundancy.
- Understand Layer 2 and Layer 3 switching.
- Configure access control lists and NAT/PAT.
- Understand static and default routing.
- Configure common IPv4 network services.
- Build confidence with Cisco IOS commands.
- Develop practical network troubleshooting skills.

## Lab Environment

- Cisco Packet Tracer
- Cisco 2911 routers
- Cisco 2960 switches
- Cisco 3560 multilayer switch
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
├── 05-layer3-intervlan/
├── 06-acl-nat/
├── 07-eigrp/
├── 08-static-routing/
├── 09-dhcp-dns-ipv4/
├── 10-fhrp/
├── 11-wan-vpn/
├── 12-network-security/
├── 13-ssh-device-management/
├── 14-troubleshooting/
└── 15-enterprise-capstone/
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
**Completed Labs:** 7  
**Status:** In Progress
