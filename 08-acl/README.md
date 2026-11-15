# Lab 08: Access Control Lists (ACLs) & NAT Configuration

## Project Overview

This lab demonstrates the configuration and verification of **Access Control Lists (ACLs)** and **Network Address Translation (NAT)** on a Cisco router.

The objective was to configure Port Address Translation (PAT) to allow multiple internal LANs to access an external network while using ACLs to control which individual devices could reach a simulated ISP server.

The lab also demonstrates how ACL placement affects traffic filtering when NAT is configured.

## Network Topology

The network consists of two internal VLANs connected through a multilayer switch to an edge router and a simulated ISP router.

```text
       VLAN 10 - ADMIN                 VLAN 20 - IT
       192.168.10.0/24                192.168.20.0/24

          PC1                            PC2       PC3
     192.168.10.10                 192.168.20.10  .11
             \                         /          /
              \                       /          /
                    SW1 - Layer 3 Switch
                    VLAN 10 / VLAN 20
                             |
                     192.168.100.0/30
                             |
                       R1 - Edge Router
                    NAT / PAT / ACL Filtering
                             |
                      203.0.113.0/30
                             |
                       R2 - ISP Router
                             |
                   198.51.100.10
                    Loopback0 Server
```

**Router Roles**

| Device | Function |
| :--- | :--- |
| SW1 | Layer 3 switching and inter-VLAN routing |
| R1 | Edge routing, NAT/PAT, and ACL enforcement |
| R2 | Simulated ISP router |
| PC1 | VLAN 10 client |
| PC2 | VLAN 20 client |
| PC3 | VLAN 20 client used for ACL testing |

## IP Addressing Scheme

### Internal LAN Networks

| Device | IP Address | Subnet | Gateway |
| :--- | :--- | :--- | :--- |
| PC1 | 192.168.10.10 | /24 | 192.168.10.1 |
| PC2 | 192.168.20.10 | /24 | 192.168.20.1 |
| PC3 | 192.168.20.11 | /24 | 192.168.20.1 |
| SW1 VLAN 10 | 192.168.10.1 | /24 | — |
| SW1 VLAN 20 | 192.168.20.1 | /24 | — |

### Router Connections

| Connection | Interface | IP Address |
| :--- | :--- | :--- |
| SW1 ↔ R1 | SW1 Gi0/1 | 192.168.100.2/30 |
| SW1 ↔ R1 | R1 Gi0/0 | 192.168.100.1/30 |
| R1 ↔ R2 | R1 S0/0/0 | 203.0.113.2/30 |
| R1 ↔ R2 | R2 S0/0/0 | 203.0.113.1/30 |
| ISP Test Server | R2 Loopback0 | 198.51.100.10 |

## NAT and PAT Configuration

R1 was configured to translate traffic from both internal LAN networks using PAT.

### NAT Interface Roles

| Interface | NAT Configuration |
| :--- | :--- |
| GigabitEthernet0/0 | Inside |
| Serial0/0/0 | Outside |

### NAT Access List

```cisco
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
```

### PAT Configuration

```cisco
interface gigabitEthernet 0/0
 ip nat inside

interface serial 0/0/0
 ip nat outside

ip nat inside source list 1 interface serial 0/0/0 overload
```

PAT allowed multiple internal devices to share R1's public-facing address, `203.0.113.2`.

## Extended ACL Configuration

An extended ACL named `BLOCK-PC3` was configured to restrict ICMP traffic from selected internal devices to the simulated ISP server.

```cisco
ip access-list extended BLOCK-PC3
 deny icmp host 192.168.20.11 host 198.51.100.10
 deny icmp host 192.168.10.10 host 198.51.100.10
 permit ip any any
```

The ACL was applied inbound on R1's internal interface:

```cisco
interface gigabitEthernet 0/0
 ip access-group BLOCK-PC3 in
```

### ACL Policy

| Source Device | Destination | Action |
| :--- | :--- | :--- |
| PC1 - 192.168.10.10 | 198.51.100.10 | Deny ICMP |
| PC2 - 192.168.20.10 | 198.51.100.10 | Permit |
| PC3 - 192.168.20.11 | 198.51.100.10 | Deny ICMP |
| Other permitted traffic | Any | Permit |

The ACL filters traffic before NAT translation, allowing individual internal source addresses to be identified.

## Verification & Testing

The following Cisco IOS commands were used to verify ACL operation, NAT/PAT functionality, and interface configuration.

```bash
show access-lists
show ip nat statistics
show ip nat translations
show ip interface gigabitEthernet 0/0
show ip interface serial 0/0/0
```

### Extended ACL Verification

The final ACL counters showed:

- PC3 deny statement: 4 matches.
- PC1 deny statement: 4 matches.
- Permit statement: 8 matches.

The counters confirmed that traffic from the two restricted devices matched their respective deny statements while permitted traffic continued through the router.

### NAT/PAT Verification

NAT statistics showed:

- Inside interface: GigabitEthernet0/0.
- Outside interface: Serial0/0/0.
- 40 NAT hits.
- 50 NAT misses.
- Dynamic translations were created during testing.

The NAT translation table also showed ICMP translations using `203.0.113.2` as the inside global address.

### Connectivity Testing

| Test | Expected Result | Actual Result |
| :--- | :--- | :--- |
| PC1 → ISP server | Blocked | Successful restriction |
| PC2 → ISP server | Allowed | Successful connectivity |
| PC3 → ISP server | Blocked | Successful restriction |
| Internal VLAN communication | Allowed | Successful connectivity |

## Troubleshooting

Several configuration issues were identified and resolved during the lab.

| Issue | Resolution |
| :--- | :--- |
| PC1 continued reaching the ISP server | Corrected ACL placement and rule ordering |
| Standard ACL blocked all PAT clients | Removed the outbound WAN ACL because all clients shared the same translated public IP |
| New deny statements were placed after permit rules | Rebuilt the extended ACL with deny statements before the permit statement |
| PC2 could not reach the ISP server | Removed the obsolete standard ACL from the WAN interface |
| Individual device filtering was required | Applied the extended ACL inbound on R1's internal interface before NAT |

This troubleshooting reinforced the importance of understanding ACL processing order and the relationship between NAT translation and packet filtering.

## Key Takeaways

Through this lab, I gained practical experience with:

- Configuring standard and extended ACLs.
- Understanding implicit and explicit ACL permit/deny behavior.
- Applying ACLs inbound and outbound on router interfaces.
- Configuring NAT and Port Address Translation.
- Using wildcard masks to identify internal networks.
- Understanding inside and outside NAT interfaces.
- Verifying dynamic NAT translations and statistics.
- Understanding how NAT affects source addresses seen by outbound ACLs.
- Filtering individual devices before NAT translation.
- Troubleshooting ACL rule ordering and traffic restrictions.

## Lab Files & Evidence

The following supporting materials are included:

- [x] Cisco Packet Tracer project file (.pkt)
- [x] Network topology screenshot
- [x] Extended ACL verification screenshot
- [x] NAT statistics screenshot
- [x] PC2 successful connectivity screenshot
- [x] PC1 and PC3 blocked connectivity screenshots
- [x] ACL interface application screenshot
- [x] NAT translation screenshot

---

**Project:** Cisco ACL and NAT Lab  
**Lab Number:** 08  
**Status:** Completed
