# Router-Interface-IP-Configuration & Routing

A Cisco Packet Tracer lab (`Router-Interface-IP-Configuration & Routing.pkt`) that demonstrates:

- Assigning IP addresses to router interfaces
- Bringing interfaces up with `no shutdown`
- Adding static routes
- Saving the configuration and verifying it after a reload

---

## Lab Overview

| Item | Details |
|------|---------|
| Tool | Cisco Packet Tracer |
| Devices | 2 x Cisco 2911 routers (Router 0, Router 1) |
| IOS | C2900 Universal K9, Version 15.1(4)M5 |
| Routing type | Static routing |

---

## IP Addressing Table

### Router 0

| Interface | IP Address | Subnet Mask | Status |
|-----------|-----------|-------------|--------|
| GigabitEthernet0/0 | 192.168.10.1 | 255.255.255.0 | up / up |
| GigabitEthernet0/1 | 192.168.30.1 | 255.255.255.0 | up / up (after `no shutdown`) |
| GigabitEthernet0/2 | 145.12.200.1 | - | up / up |

### Router 2

| Interface | IP Address | Subnet Mask | Status |
|-----------|-----------|-------------|--------|
| GigabitEthernet0/0 | 192.168.30.3 | 255.255.255.0 | up / up (after `no shutdown`) |
| GigabitEthernet0/1 | 10.10.10.1 | - | up / up |
| GigabitEthernet0/2 | 192.168.20.1 | - | up / up |

> Router 0 `Gig0/1` (192.168.30.1) and Router 1 `Gig0/0` (192.168.30.3) are on the same `192.168.30.0/24` network.

---

## Configuration Steps

### Router 0

**1. Bring up GigabitEthernet0/1**

```
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# no shutdown
```

**2. Verify and save**

```
Router(config-if)# do show ip interface brief
Router(config-if)# do write memory
```

**3. Static routes**

```
Router(config)# ip route 10.0.0.0 255.0.0.0 10.10.10.4
Router(config)# ip route 10.0.0.0 255.0.0.0 10.10.10.3
Router(config)# ip route 10.0.0.0 255.0.0.0 10.10.10.2
Router(config)# do write memory
```

### Router 2

**1. Enable GigabitEthernet0/0 and set its IP**

```
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# no shutdown
Router(config-if)# ip address 192.168.30.3 255.255.255.0
Router(config-if)# do write memory
```

**2. Static routes**

```
Router(config)# ip route 192.168.20.0 255.255.255.0 192.168.20.2
Router(config)# ip route 192.168.20.0 255.255.255.0 192.168.20.3
Router(config)# ip route 192.168.20.0 255.255.255.0 192.168.20.4
Router(config)# do write memory
```

---

## Verification Commands

```
show ip interface brief
show ip route
show running-config
ping <destination-ip>
```

After `write memory` and `reload`, the interface IPs are loaded from NVRAM and remain the same, which confirms the configuration was saved.

---

## Errors Encountered & Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `% Invalid input detected at '^' marker` | Typo in command (`inty`, stray `\`, `iup`) | Retype the command correctly |
| `% Incomplete command` | `ip address` was entered without a subnet mask | Use `ip address 192.168.30.3 255.255.255.0` |
| `%Inconsistent address and mask` | `10.10.10.0` with mask `255.0.0.0` has host bits set | Use the network address `10.0.0.0 255.0.0.0` (or `10.10.10.0 255.255.255.0`) |
| `Translating "wr\"... Name lookup aborted` | Stray `\` after `wr` made IOS treat it as a hostname | Type `wr` (or `write memory`) without the backslash |
| Gig0/1 `up / down` | Interface not enabled properly / link not yet negotiated | `no shutdown`, then wait for the line protocol to come up |

---

## Notes & Suggestions

- **Router 0 static routes:** the next hops `10.10.10.2/3/4` are not on any network directly connected to Router 0, so these routes will not appear in `show ip route`. A next hop must be reachable through a connected interface (for example `192.168.30.3` on Router 1).
- **Router 1 static routes:** `192.168.20.0/24` is already directly connected on Router 1 (`Gig0/2`), so the static route to it is redundant.
- **Suggested routes for end-to-end connectivity:**

  ```
  ! Router 0
  ip route 192.168.20.0 255.255.255.0 192.168.30.3
  ip route 10.10.10.0 255.255.255.0 192.168.30.3

  ! Router 1
  ip route 192.168.10.0 255.255.255.0 192.168.30.1
  ```

- Multiple static routes to the same destination with different next hops provide backup/load-sharing paths.
- Always run `write memory` (or `copy running-config startup-config`) before `reload`.

---

## How to Open

1. Install Cisco Packet Tracer.
2. Open `Router-Interface-IP-Configuration & Routing.pkt`.
3. Open each router's CLI tab and run the verification commands above.

