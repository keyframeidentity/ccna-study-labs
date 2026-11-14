# CCNA Lab 02 – VLANs, Trunking & Inter-VLAN Routing

## Objective
Configure VLANs on two Layer 2 switches, establish 802.1Q trunk links, and implement inter-VLAN routing using router-on-a-stick.

## Topology
- 1 Cisco router (R1)
- 2 Cisco 2960 Layer 2 switches (SW1 and SW2)
- 4 end-user PCs

### VLAN Configuration

| VLAN | Name | Network | Default Gateway |
|---|---|---|---|
| 10 | ADMIN | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |

### Switch Port Assignments

Both switches:
- Fa0/1 – VLAN 10 (ADMIN)
- Fa0/2 – VLAN 20 (IT)

Trunk connections:
- SW2 G0/1 ↔ R1 G0/0
- SW2 G0/2 ↔ SW1 G0/1

## Router Configuration

R1 uses subinterfaces on G0/0 for inter-VLAN routing.

```cisco
interface GigabitEthernet0/0
 no ip address
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

## Trunk Configuration

Trunk links use 802.1Q and allow VLANs 10 and 20.

```cisco
switchport mode trunk
switchport trunk allowed vlan 10,20
```

## Verification

Commands used:

```cisco
show vlan brief
show interfaces trunk
show ip interface brief
show running-config
show ip route
```

### Results
- VLANs 10 and 20 created successfully.
- Access ports assigned to the correct VLANs.
- Trunk links established.
- VLANs 10 and 20 allowed across trunk links.
- Router subinterfaces operational.
- PCs successfully reached their default gateways.
- Inter-VLAN connectivity verified with successful ping tests.

## Troubleshooting Notes

An initial connectivity issue was caused by an incorrect PC IP configuration. Correcting the IP address resolved the issue.

The Layer 2 switches did not have management SVI addresses configured. Switch-to-router management pings were not part of this lab's requirements.

## Files
- `CCNA-Lab-02-VLANs-Trunking-InterVLAN-Routing.pkt` – Packet Tracer project
- `screenshots/` – Lab topology and verification evidence

## Skills Demonstrated
- VLAN creation and assignment
- Access port configuration
- 802.1Q trunking
- VLAN trunk allowed lists
- Router-on-a-stick configuration
- Inter-VLAN routing
- Network troubleshooting and verification
