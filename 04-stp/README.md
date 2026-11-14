
# Lab 04: Spanning Tree Protocol (STP)

## Project Overview

This lab demonstrates the configuration, operation, and verification of **Spanning Tree Protocol (STP)** in a redundant Layer 2 switching environment.

The objective was to understand how STP prevents switching loops, elects a root bridge, determines port roles, and maintains network connectivity when redundant links fail.

The lab was performed using Cisco 2960 switches in Cisco Packet Tracer.

## Network Topology

The network consists of three interconnected Layer 2 switches arranged in a triangle.

This topology creates redundant paths between switches, allowing STP to prevent Layer 2 loops while maintaining backup connectivity.

**Switch Roles**

| Switch | Function |
| :--- | :--- |
| SW1 | Configured root bridge |
| SW2 | Secondary switching path |
| SW3 | Redundant switching path and BPDU Guard testing |

All switches participate in VLAN 1 using Per-VLAN Spanning Tree (PVST).

## STP Root Bridge Election

Initially, SW2 was elected as the root bridge because it had the lowest Bridge ID.

The initial election demonstrated how STP uses bridge priority and MAC address to determine the root bridge.

### Root Bridge Configuration

SW1 was intentionally configured with a lower priority to become the root bridge.

```cisco
configure terminal
spanning-tree vlan 1 priority 4096
end
```

### Verification

The following command was used to verify the root bridge election:

```cisco
show spanning-tree
```

**Results:**

- SW1 successfully became the root bridge.
- Configured priority was 4096.
- Displayed priority was 4097 due to the VLAN 1 system ID extension.
- All active SW1 ports became designated forwarding ports.
- SW2 and SW3 recalculated their root and designated port roles.

## STP Port Roles and States

The lab demonstrated the following STP port roles:

| Port Role | Purpose |
| :--- | :--- |
| Root Port | Best path toward the root bridge |
| Designated Port | Forwarding port for a network segment |
| Alternate Port | Backup path that remains blocked to prevent loops |

### Verification

The following command was used to examine STP port roles and states:

```cisco
show spanning-tree
```

**Results:**

- SW1 operated as the root bridge.
- SW2 selected its root port toward SW1.
- SW3 selected Fa0/2 as its root port.
- SW3 Fa0/1 became an alternate blocking port.
- Redundant paths remained available without creating switching loops.

## STP Failover Testing

A link failure test was performed on SW3 to verify that STP could maintain connectivity through a redundant path.

### Test Procedure

1. Identified Fa0/1 as the alternate blocking port.
2. Shut down the active root port, Fa0/2.
3. Observed STP select Fa0/1 as the new root port.
4. Verified the port transitioned through the Listening state.
5. Waited for the port to reach Forwarding.
6. Restored Fa0/2 and verified the original topology returned.

### Results

- Fa0/2 was initially the root forwarding port.
- After the link failure, Fa0/1 became the root port.
- The root path cost increased from 19 to 38 during failover.
- Fa0/1 successfully transitioned from alternate blocking to root forwarding.
- Restoring Fa0/2 returned the network to its original STP topology.

This test demonstrated classic 802.1D STP convergence and redundant path recovery.

## PortFast and BPDU Guard

PortFast and BPDU Guard were configured on an unused SW3 access port.

### Configuration

```cisco
configure terminal
interface fa0/4
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable
end
```

### PortFast

PortFast allows an access port connected to an end device to transition directly to forwarding rather than waiting through the traditional STP listening and learning stages.

### BPDU Guard

BPDU Guard protects access ports by placing them into an error-disabled state if they receive a Spanning Tree BPDU.

### Verification and Testing

A second switch was connected to SW3 Fa0/4 to simulate an unauthorized switch connection.

The following messages confirmed BPDU Guard activation:

```text
%SPANTREE-2-BLOCK_BPDUGUARD:
Received BPDU on port FastEthernet0/4 with BPDU Guard enabled. Disabling port.

%PM-4-ERR_DISABLE:
bpduguard error detected on 0/4, putting 0/4 in err-disable state
```

**Results:**

- Fa0/4 detected a BPDU from the connected switch.
- BPDU Guard automatically disabled the interface.
- The interface entered an error-disabled state.
- The test switch was disconnected.
- Fa0/4 was recovered using `shutdown` and `no shutdown`.

## Troubleshooting and Recovery

| Issue or Test | Resolution |
| :--- | :--- |
| Initial root bridge election | Changed SW1 priority to 4096 |
| Redundant link blocking | Verified expected alternate port behavior |
| Active root port failure | Observed STP select the backup path |
| BPDU Guard activation | Disconnected the second switch and recovered Fa0/4 |
| STP topology restoration | Restored Fa0/2 and verified original port roles |

## Key Takeaways

Through this lab, I gained practical experience with:

- Understanding STP root bridge elections.
- Configuring bridge priority.
- Identifying root, designated, and alternate ports.
- Understanding forwarding and blocking states.
- Preventing Layer 2 switching loops.
- Observing STP convergence after link failures.
- Verifying redundant path failover.
- Configuring PortFast on access ports.
- Configuring and testing BPDU Guard.
- Recovering interfaces from an error-disabled state.

## Lab Files & Evidence

The following supporting materials are included:

- [x] Cisco Packet Tracer project file (`04-stp.pkt`)
- [x] Network topology screenshot
- [x] Initial root bridge election screenshot
- [x] Alternate port blocking screenshot
- [x] SW1 root bridge configuration screenshot
- [x] STP failover screenshot
- [x] BPDU Guard activation screenshot
- [x] PortFast and BPDU Guard configuration screenshot

## Project Summary

**Project:** Cisco Spanning Tree Protocol Lab  
**Lab Number:** 04  
**Technology:** Cisco PVST / IEEE 802.1D STP  
**Platform:** Cisco Packet Tracer  
**Status:** Completed
