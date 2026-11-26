# Router-on-a-Stick (ROAS) – Inter-VLAN Routing Lab

## Project Overview

This Cisco Packet Tracer project demonstrates **Router-on-a-Stick (ROAS)**. Seven separate VLANs are created across three switches. A single physical router interface uses VLAN-tagged subinterfaces to act as the default gateway for each VLAN and enable communication between devices in different VLANs.

## Project Objectives

- Create and name VLANs on multiple switches.
- Assign end devices to the correct access VLANs.
- Use trunk links to carry multiple VLANs between network devices.
- Configure router subinterfaces with IEEE 802.1Q VLAN tagging.
- Provide a separate default gateway for each VLAN.
- Verify inter-VLAN connectivity with ICMP ping tests.

## Network Topology

The topology contains:

- **1 Router:** Router0
- **3 Switches:** Switch0, Switch1, and Switch2
- **14 PCs:** PC0 through PC13
- **7 VLANs:** VLAN 10, 20, 30, 40, 50, 60, and 70

Router0 connects to the switching network through **GigabitEthernet0/1**. The router uses subinterfaces on this physical interface for inter-VLAN routing.

## VLAN and IP Addressing Plan

| VLAN ID | VLAN Name | Network | Subnet Mask | Default Gateway | PCs and IP Addresses |
|---:|---|---|---|---|---|
| 10 | IT | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | PC0: `192.168.10.5`, PC1: `192.168.10.6` |
| 20 | CSE | `10.0.0.0/8` | `255.0.0.0` | `10.0.0.1` | PC2: `10.0.0.2`, PC3: `10.0.0.3` |
| 30 | EEE | `172.68.10.0/24` | `255.255.255.0` | `172.68.10.1` | PC4: `172.68.10.5`, PC5: `172.68.10.6` |
| 40 | CCE | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | PC6: `192.168.20.2`, PC7: `192.168.20.3` |
| 50 | ETE | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | PC8: `192.168.30.2`, PC9: `192.168.30.3` |
| 60 | EB | `192.168.50.0/24` | `255.255.255.0` | `192.168.50.1` | PC10: `192.168.50.2`, PC11: `192.168.50.3` |
| 70 | ACAD | `192.168.60.0/24` | `255.255.255.0` | `192.168.60.1` | PC12: `192.168.60.2`, PC13: `192.168.60.3` |

> **Note:** The addressing information above is transcribed from the labels in the Packet Tracer topology. VLAN 20 uses a `/8` mask; the other listed VLANs use `/24` masks.

## Switch VLAN and Access-Port Assignments

The following access-port assignments are visible in the `show vlan brief` outputs provided for the switches.

### Switch0

| VLAN ID | VLAN Name | Access Ports |
|---:|---|---|
| 10 | IT | `Fa0/1`, `Fa0/2` |
| 20 | CSE | `Fa0/3`, `Fa0/4` |
| 30 | EEE | `Fa0/5`, `Fa0/6` |
| 40 | VLAN0040 | No access ports listed in the output |
| 50 | VLAN0050 | No access ports listed in the output |
| 60 | VLAN0060 | No access ports listed in the output |
| 70 | VLAN0070 | No access ports listed in the output |

### Switch1

| VLAN ID | VLAN Name | Access Ports |
|---:|---|---|
| 40 | CCE | `Fa0/1`, `Fa0/2` |
| 10, 20, 30, 50, 60, 70 | VLAN0010, VLAN0020, VLAN0030, VLAN0050, VLAN0060, VLAN0070 | No access ports listed in the output |

### Switch2

| VLAN ID | VLAN Name | Access Ports |
|---:|---|---|
| 50 | ETE | `Fa0/1`, `Fa0/2` |
| 60 | EB | `Fa0/3`, `Fa0/4` |
| 70 | ACAD | `Fa0/5`, `Fa0/6` |

> The screenshots show VLAN membership for access ports. They do not show the switch trunk configuration or the exact uplink interface numbers, so those details are not specified here.

## Router0 Subinterface Addressing

The `show ip interface brief` output confirms that `GigabitEthernet0/1` is up/up and has the following subinterfaces and gateway addresses:

| Router Interface | VLAN ID | IP Address |
|---|---:|---|
| `GigabitEthernet0/1.10` | 10 | `192.168.10.1` |
| `GigabitEthernet0/1.20` | 20 | `10.0.0.1` |
| `GigabitEthernet0/1.30` | 30 | `172.68.10.1` |
| `GigabitEthernet0/1.40` | 40 | `192.168.20.1` |
| `GigabitEthernet0/1.50` | 50 | `192.168.30.1` |
| `GigabitEthernet0/1.60` | 60 | `192.168.50.1` |
| `GigabitEthernet0/1.70` | 70 | `192.168.60.1` |

The parent interface `GigabitEthernet0/1` is shown as **up/up**. `GigabitEthernet0/0` is shown as administratively down and is not used for the displayed ROAS configuration.

## Example Router Configuration

The following illustrates the standard ROAS subinterface configuration using the IP addresses shown in this project. Apply it only when it matches the current router configuration.

```cisco
enable
configure terminal

interface gigabitEthernet0/1
 no shutdown

interface gigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface gigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.0.0.1 255.0.0.0

interface gigabitEthernet0/1.30
 encapsulation dot1Q 30
 ip address 172.68.10.1 255.255.255.0

interface gigabitEthernet0/1.40
 encapsulation dot1Q 40
 ip address 192.168.20.1 255.255.255.0

interface gigabitEthernet0/1.50
 encapsulation dot1Q 50
 ip address 192.168.30.1 255.255.255.0

interface gigabitEthernet0/1.60
 encapsulation dot1Q 60
 ip address 192.168.50.1 255.255.255.0

interface gigabitEthernet0/1.70
 encapsulation dot1Q 70
 ip address 192.168.60.1 255.255.255.0

end
write memory
```

## Verification Commands

Use these commands to check the VLANs, trunk links, router interfaces, and connectivity.

### On the switches

```cisco
show vlan brief
show interfaces trunk
show interfaces status
```

### On Router0

```cisco
show ip interface brief
show running-config
```

### From the PCs

Open a PC's command prompt and ping a device in another VLAN. For example:

```text
ping 192.168.30.3
ping 192.168.50.2
ping 10.0.0.2
```

Each PC should have the correct IP address, subnet mask, and default gateway from the addressing table.

## Test Results

The Packet Tracer screenshot shows multiple **Successful** ICMP PDU results between PCs belonging to different VLANs. This provides evidence that inter-VLAN communication is working in the simulation.

## Key Learning Outcomes

After completing this lab, you should understand:

- How VLANs separate devices into different Layer 2 broadcast domains.
- How access ports connect end devices to specific VLANs.
- Why trunk links are required to transport multiple VLANs over a shared link.
- How router subinterfaces and 802.1Q encapsulation provide inter-VLAN routing.
- How to verify connectivity using Cisco IOS show commands and PC ping tests.

## File

- `ROAS.pkt` – Cisco Packet Tracer project file.

