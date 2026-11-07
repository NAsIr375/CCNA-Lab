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
Router(config)# do show running-config
Router(config)# do show ip interface brief
Router(config)# exit
Router# show running-config
Router# show startup-config
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

## Next Practice

After completing this initial lab, practise the following in a separate copy of the file:

1. Set a hostname for the router.
2. Configure an IP address on a router interface.
3. Enable the interface with `no shutdown`.
4. Verify the interface using `show ip interface brief`.
5. Save the configuration using `copy running-config startup-config`.

## Author

**Md Nasir Uddin**

## License

This project is shared for educational and practice purposes.
