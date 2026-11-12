# VLAN and Trunking Configuration(Static)

## Project Overview

This Packet Tracer project demonstrates VLAN segmentation and inter-switch trunking between two buildings. Each building has separate VLANs for the IT and Accounts departments. The trunk link carries VLAN 10 and VLAN 20 between the switches using IEEE 802.1Q encapsulation.

## VLAN Plan

| VLAN ID | VLAN Name | Department | Purpose |
|---|---|---|---|
| 1 | default | Default | Default VLAN; not used for the departmental access ports |
| 10 | IT | IT | IT department devices |
| 20 | ACC | Accounts | Accounts department devices |

## Building 1 – Switch Port Assignment

| VLAN | VLAN Name | Assigned Ports |
|---|---|---|
| 10 | IT | Fa0/1–Fa0/5 |
| 20 | ACC | Fa0/6–Fa0/10 |
| 1 | default | Fa0/11–Fa0/24, Gig0/1, Gig0/2 (as shown in the supplied `show vlan brief` output) |

## Building 2 – Switch Port Assignment

| VLAN | VLAN Name | Assigned Ports |
|---|---|---|
| 10 | IT | Fa0/1–Fa0/5 |
| 20 | ACC | Fa0/6–Fa0/10 |
| 1 | default | Fa0/11–Fa0/24, Gig0/2 (as shown in the supplied `show vlan brief` output) |

> **Note:** The supplied VLAN output shows the VLAN 10 name as `IT` on Building 1 and `it` on Building 2. VLAN IDs determine VLAN membership, so both refer to VLAN 10; consistent naming is recommended.

## Trunk Configuration

The supplied trunk output for both buildings reports the following:

| Setting | Value |
|---|---|
| Interface | GigabitEthernet0/1 |
| Mode | On (static trunk) |
| Encapsulation | IEEE 802.1Q |
| Status | Trunking |
| Native VLAN | 1 |
| Allowed VLANs | 10, 20 |
| Allowed and active VLANs | 10, 20 |
| VLANs forwarding and not pruned | 10, 20 |

The trunk carries traffic for VLAN 10 (IT) and VLAN 20 (Accounts). VLAN 1 is configured as the native VLAN, but it is not included in the allowed VLAN list shown in the output.

## Verification Commands

Run these commands on each switch to verify the configuration:

```text
enable
show vlan brief
show interfaces trunk
show interfaces gigabitEthernet0/1 switchport
show running-config
```

Expected trunk details:

- `GigabitEthernet0/1` is operating as a trunk.
- Encapsulation is `802.1q`.
- Native VLAN is `1`.
- Allowed VLANs are `10,20`.
- VLANs `10` and `20` are active and forwarding on the trunk.

## Important Output Check

There is one inconsistency in the supplied Building 1 output: `show vlan brief` lists `Gig0/1` under VLAN 1, while the supplied `show interfaces trunk` output identifies `Gig0/1` as a trunk. A trunk port normally does not appear as an access port under a VLAN in `show vlan brief`.

Re-run `show vlan brief` and `show interfaces trunk` on the Building 1 switch to confirm the current state of `Gig0/1`. The two command outputs should describe the same configuration. The Building 2 VLAN listing does not show `Gig0/1` under VLAN 1, which is consistent with the reported trunk status.

## Project Outcome

- Department devices are separated into VLAN 10 and VLAN 20 in both buildings.
- Each department uses the same VLAN ID in both buildings.
- The inter-switch trunk is intended to transport VLAN 10 and VLAN 20 using 802.1Q.
- VLAN and trunk status can be checked with the verification commands above.

