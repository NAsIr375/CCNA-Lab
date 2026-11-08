# Router-Interface-IP-Configuration

## Project Overview

This Packet Tracer lab demonstrates how to configure IP addresses on three router GigabitEthernet interfaces, enable the interfaces, verify their status, and save the running configuration.

## Packet Tracer File

`Router-Interface-IP-Configuration.pkt`

## Interface IP Addressing

| Interface | IP Address | Subnet Mask | Network (based on mask) |
|---|---|---|---|
| GigabitEthernet0/0 | `192.168.10.1` | `255.255.255.0` | `192.168.10.0/24` |
| GigabitEthernet0/1 | `10.10.20.1` | `255.0.0.0` | `10.0.0.0/8` |
| GigabitEthernet0/2 | `145.12.200.1` | `255.255.0.0` | `145.12.0.0/16` |

> **Subnet mask note:** The network column is calculated from the exact IP addresses and subnet masks used in this lab. For example, `10.10.20.1 255.0.0.0` belongs to the `10.0.0.0/8` network, not `10.10.20.0/24`.

## Configuration Commands

### 1. Enter Privileged EXEC and Global Configuration Mode

```text
enable
configure terminal
```

### 2. Configure GigabitEthernet0/0

```text
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
```

### 3. Configure GigabitEthernet0/1

```text
interface gigabitEthernet 0/1
ip address 10.10.20.1 255.0.0.0
no shutdown
exit
```

### 4. Configure GigabitEthernet0/2

```text
interface gigabitEthernet 0/2
ip address 145.12.200.1 255.255.0.0
no shutdown
exit
```

### 5. Verify Interface Status

```text
do show ip interface brief
```

Expected result after the interfaces are connected and operational:

```text
Interface              IP-Address      Status    Protocol
GigabitEthernet0/0     192.168.10.1    up        up
GigabitEthernet0/1     10.10.20.1      up        up
GigabitEthernet0/2     145.12.200.1    up        up
```

### 6. Save the Configuration

```text
do write
```

Alternatively, from privileged EXEC mode:

```text
copy running-config startup-config
```

## Important Notes

- `no shutdown` enables an interface administratively.
- An interface normally shows `up/up` when it is enabled and has an active physical connection to another powered-on device or network.
- The command `no shutydown` is misspelled and invalid. Use `no shutdown`.
- `do` allows an EXEC command, such as `show` or `write`, to be run from configuration mode.
- This lab configures interface IP addresses only. It does not configure static routes, dynamic routing protocols, DHCP, or NAT.

## Verification 

 IP addresses assigned to all three interfaces.
 All three interfaces enabled with `no shutdown`.
 Interface status checked with `show ip interface brief`.
 Configuration saved to startup-config.

## Learning Outcomes

After completing this lab, you should be able to:

1. Enter router configuration modes.
2. Configure IP addresses and subnet masks on router interfaces.
3. Enable router interfaces using `no shutdown`.
4. Verify interface IP addresses and operational status.
5. Save the router configuration.

