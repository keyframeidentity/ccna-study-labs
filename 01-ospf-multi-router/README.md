Lab 01: Multi-Router OSPF

<ins>Status: Completed</ins>

Objective

Build a three-router network using OSPF Area 0 and establish connectivity between three separate LANs.

Network Topology

R1: Connects LAN 1 to R2.

R2: Connects LAN 2, R1, and R3.

R3: Connects LAN 3 to R2.

All three routers participate in OSPF Area 0.

IP Addressing

LAN Networks

R1: 192.168.10.1/24

R2: 192.168.20.1/24

R3: 192.168.30.1/24

Router-to-Router Links

R1 ↔ R2: 10.0.12.0/30

R2 ↔ R3: 10.0.23.0/30

OSPF Configuration

Router

Router ID

Area

R1

1.1.1.1

0

R2

2.2.2.2

0

R3

3.3.3.3

0

OSPF Process ID: 1

Routing Area: 0

Verification

Commands used:

show ip interface brief
show ip ospf neighbor
show ip route ospf
show ip protocols

Results:

OSPF neighbor relationships reached FULL.

All routers learned remote networks through OSPF.

PC1 successfully pinged PC3 across the network.

Troubleshooting

Issues encountered:

R2 interface toward R3 was administratively down. Fixed with no shutdown.

Incorrect wildcard masks were corrected to 0.0.0.3 on /30 networks.

A typo in R3's OSPF network statement was corrected.

What I Learned

How OSPF establishes neighbor relationships.

How router IDs identify routers.

How wildcard masks select networks.

How to identify OSPF routes using the O route code.

How to troubleshoot routing and interface problems.

Lab Files

Packet Tracer project and topology screenshots will be added here.
