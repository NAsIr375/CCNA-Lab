# Basic Router Configuration

A beginner-level Cisco Packet Tracer lab for exploring a router's CLI and checking its configuration with Cisco IOS show commands.

## Lab Overview

- **Platform:** Cisco Packet Tracer
- **Device:** Cisco Router
- **Topic:** Basic Router CLI and configuration verification
- **IP addressing:** Not configured yet
- **Routing:** Not configured

> **Note:** This is an initial router configuration practice file. No IP address has been assigned to the router interfaces, so this lab does not demonstrate end-to-end network connectivity.

## Objectives

- Access privileged EXEC mode.
- Enter global configuration mode.
- View the current running configuration.
- Check interface IP addresses and interface status.
- Understand how to run an EXEC command from configuration mode by using `do`.

## Commands Practiced

```text
Router> enable
Router# configure terminal
Router(config)# hostname R01
R01(config)# do show running-config
R01(config)# enable password 12364
R01(config)# service password-encryption (for md-7 encryption)
R01(config)# enable secret 123654 (foe md-5 encryption)
R01(config)# do wr
R01(config)# do show ip interface brief
R01(config)# exit
R01# show running-config
R01# show startup-config

console port command:
R01(config)# username Nasir secret(for password) 123654
R01(config)# line console 0
R01(config-line)# login local
R01(config-line)# do wr

```

### Important Command

```text
Router(config)# do show running-config
```

The `do` keyword allows you to run an EXEC-mode command while you are in configuration mode. The shortcut below can also be used:

```text
Router(config)# do sh run
```

## How to Open the Lab

1. Open Cisco Packet Tracer.
2. Select **File → Open**.
3. Open `basic-router-configuration.pkt`.
4. Click the router and open the **CLI** tab.
5. Try the commands listed above.

fig`.

## Author

**Md Nasir Uddin**

## License

This project is shared for educational and practice purposes.
