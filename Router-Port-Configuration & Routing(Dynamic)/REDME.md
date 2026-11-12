# Router-Port-Configuration & Routing(Dynamic Routing)

This README explains how to configure **IPv4 Static Routing** between the **GEC Branch** and the **Chawkbazar Branch** shown in the Cisco Packet Tracer topology. This lab uses static routes only; RIP and other dynamic routing protocols are not required.

## 1. Network Topology and IP Addressing

### GEC Branch

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| Router0 | G0/1 (LAN) | 192.168.10.1 | 255.255.255.0 | — |
| PC0 | Fa0 | 192.168.10.2 | 255.255.255.0 | 192.168.10.1 |
| PC1 | Fa0 | 192.168.10.3 | 255.255.255.0 | 192.168.10.1 |
| PC2 | Fa0 | 192.168.10.4 | 255.255.255.0 | 192.168.10.1 |

**GEC LAN Network:** `192.168.10.0/24`

### Router-to-Router Link

| Device | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| Router0 | G0/0 | 192.168.20.3 | 255.255.255.0 |
| Router1 | G0/0 | 192.168.20.4 | 255.255.255.0 |

**Inter-router Network:** `192.168.20.0/24`

### Chawkbazar Branch

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| Router1 | G0/1 (LAN) | 10.10.10.1 | 255.0.0.0 | — |
| PC8 | Fa0 | 10.10.10.2 | 255.0.0.0 | 10.10.10.1 |
| PC7 | Fa0 | 10.10.10.3 | 255.0.0.0 | 10.10.10.1 |
| PC6 | Fa0 | 10.10.10.4 | 255.0.0.0 | 10.10.10.1 |

**Chawkbazar LAN Network:** `10.0.0.0/8` (because the subnet mask is `255.0.0.0`).

> **Important:** With an IP address such as `10.10.10.1` and a subnet mask of `255.0.0.0`, the network address is `10.0.0.0/8`, not `10.10.10.0/8`.

## 2. Router0 Configuration — GEC Branch

Open the CLI of Router0 and enter the following commands:

```cisco
enable
configure terminal

interface gigabitEthernet 0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface gigabitEthernet 0/0
 ip address 192.168.20.3 255.255.255.0
 no shutdown
 exit

! Dynamic route to the Chawkbazar LAN
ip route 0.0.0.0 0.0.0.0 192.168.20.4

end
copy running-config startup-config
```

**Explanation:** To reach the `10.0.0.0/8` network, Router0 forwards packets to the next-hop router at `192.168.20.4` (Router1).

## 3. Router1 Configuration — Chawkbazar Branch

Open the CLI of Router1 and enter the following commands:

```cisco
enable
configure terminal

interface gigabitEthernet 0/1
 ip address 10.10.10.1 255.0.0.0
 no shutdown
 exit

interface gigabitEthernet 0/0
 ip address 192.168.20.4 255.255.255.0
 no shutdown
 exit

! Static route to the GEC LAN
ip route 0.0.0.0 0.0.0.0 192.168.20.3

end
copy running-config startup-config
```

**Explanation:** To reach the `192.168.10.0/24` network, Router1 forwards packets to the next-hop router at `192.168.20.3` (Router0).

## 4. Configure the PCs

For each PC, open **Desktop → IP Configuration** and enter the appropriate IP address, subnet mask, and default gateway.

- **GEC PCs:** Use addresses from `192.168.10.2` to `192.168.10.4`, subnet mask `255.255.255.0`, and default gateway `192.168.10.1`.
- **Chawkbazar PCs:** Use addresses from `10.10.10.2` to `10.10.10.4`, subnet mask `255.0.0.0`, and default gateway `10.10.10.1`.

## 5. Verify the Configuration

On both routers, check the interface status:

```cisco
show ip interface brief
```

The interfaces used in this topology should show **up/up** under Status and Protocol.

Next, check the routing table:

```cisco
show ip route
```

Router0 should show a static route (`S`) similar to this:

```text
S 10.0.0.0/8 [1/0] via 192.168.20.4
```

Router1 should show a static route (`S`) similar to this:

```text
S 192.168.10.0/24 [1/0] via 192.168.20.3
```

## 6. Ping Tests

First, test the inter-router connection from Router0 to Router1:

```cisco
ping 192.168.20.4
```

Then test the connection from Router1 to Router0:

```cisco
ping 192.168.20.3
```

From PC0, test connectivity to PC6:

```text
ping 10.10.10.4
```

Also test in the reverse direction, from PC6 to PC0:

```text
ping 192.168.10.2
```

If the pings succeed, the two branch networks can communicate. The first ping may time out while ARP resolves the destination's MAC address. Run the ping again if necessary.

## 7. Troubleshooting Checklist

1. Confirm that the two router interfaces connected to each other are in the same network: `192.168.20.3/24` and `192.168.20.4/24`.
2. Make sure the required router interfaces are enabled with `no shutdown`.
3. Confirm that Router0 uses `192.168.20.4` as the next hop for the Chawkbazar network.
4. Confirm that Router1 uses `192.168.20.3` as the next hop for the GEC network.
5. Verify that every PC has the correct default gateway.
6. Check `show ip route` and confirm that the remote network appears with the `S` route code.
7. If RIP was configured earlier and you want this lab to use static routing only, remove the RIP configuration:

```cisco
configure terminal
no router rip
end
```

> Use `no router rip` only if RIP was previously configured on the router and you want to remove it for this static-routing lab.

---

**Lab Topic:** Static Routing Between Two Branch Networks  
**Branches:** GEC Branch ↔ Chawkbazar Branch  
**Routing Method:** IPv4 Static Routing

