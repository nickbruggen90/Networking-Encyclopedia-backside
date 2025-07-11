# Port Security
*IEEE 802.1X*

## Overview
• **Port Security is a L2 security feature available on Cisco switches** that restricts access to a switch port based on the source MAC addresses of the connected devices  
• **It enforces a policy on a per-port basis** to ensure that only authorized devices can send traffic through a particular port  
• **Port Security works well alongside IEEE 802.1X for port-based authentication**, DHCP Snooping to build a trusted DHCP binding table, and DAI for further L2 security; a layered approach provides defense in depth

---

## Timers

### Aging Timer
• **Port security supports an aging timer for dynamically learned MAC addresses**. You can configure either:

| Aging Type | Description |
|------------|-------------|
| **Absolute Aging** | All learned MACs are removed after a fixed period regardless of inactivity |
| **Inactivity Aging** | A MAC address is removed if no traffic is seen from that address within the specified time |

---

## Multicast
• **Port Security primarily examines source MAC addresses in Ethernet frames**. It does not typically treat multicast addresses differently, since multicast frames use the sender's unicast MAC as the source  
• **Multicast destination addresses are not considered by port security**

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **Port Security** | A feature that restricts which MAC addresses can connect to a switch port |
| **Secure MAC Address** | A MAC address that is allowed on a port. It can be statically configured, dynamically learned or "sticky" |
| **Sticky Secure MAC** | A dynamically learned MAC address that is saved into the running configuration, making it persistent across reboots if saved |
| **Dynamic Secure MAC** | Learned automatically and stored in the switch's secure MAC table |
| **Static Secure MAC** | Manually configured and does not change unless edited |
| **Violation Mode** | The action taken when an unauthorized MAC is detected |
| **Access Port** | A switch port configuration to carry traffic for a single VLAN, this is where port security is most effective |
| **802.1X** | An IEEE standard for port-based network access control that authenticates devices via credentials rather than solely by MAC address. 802.1X uses EAP for authenticating devices on the network. It offers stronger, identity-based security compared to basic MAC filtering |
| **802.1Q** | Specifies VLAN tagging and includes a 3-bit CoS field used for L2 traffic prioritization, which can complement Port Security in a layered security design |
| **Aging** | Dynamically learned secure MAC addresses can be aged out after a specified timeout period if inactive. Aging can be configured as either absolute (fixed time after learning) or inactivity-based (time since last activity) |

---

## Violation Modes

### Cisco supports three primary modes:

| Mode | Action | Logging | Description |
|------|--------|---------|-------------|
| **Protect** | Drop frames | No | Drops frames from unknown MAC addresses without logging violations |
| **Restrict** | Drop frames | Yes | Drops frames and logs violations (and may send SNMP traps) |
| **Shutdown** | Err-disable port | Yes | Puts the port into an err-disabled state upon violation |

---

## Header Considerations
• **Port Security operates at L2 by inspecting the Ethernet frame header**  
• **It only checks the source MAC address against the configured secure MAC list**  
• **It does not parse IP or TCP headers**  
• **This simplicity enables fast hardware-based filtering**

---

## Access Control Mechanism Comparisons

### Port Security vs 802.1X
• **Port Security** restricts access based on MAC addresses and is simple to configure, but MAC addresses can be spoofed  
• **802.1X** authenticates devices with credentials (passwords, certificates), providing stronger security but requiring additional infrastructure (RADIUS servers) and supplicant support on endpoints

### Port Security vs DHCP Snooping
• **DHCP Snooping** monitors DHCP messages to build a binding table and prevent rogue DHCP servers from assigning IPs  
• **While DHCP Snooping doesn't prevent device connection**, it supports other features like DAI and IP Source Guard, complementing Port Security

### Port Security vs MAC ACLs
• **MAC ACLs** filter traffic based on MAC addresses but operate statelessly  
• **Port Security** dynamically learns MAC addresses and provides violation tracking, making it easier to manage per-port security

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Enhanced Security** | Restricts unauthorized devices from accessing the network, thus reducing potential attack vectors |
| **Simplified Administration** | With sticky MAC learning, Port Security can automatically populate allowed MAC addresses, reducing manual configuration effort |
| **Real-Time Alerts** | Violation logging and SNMP traps help network administrators identify and respond to security incidents promptly |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Enterprise Access Ports** | Protect individual user ports in office environments, ensuring that only the intended device (or devices) can connect |
| **Public Areas** | In spaces like conference rooms, labs or dormitories, where unauthorized access is a concern, Port Security limits connections to authorized devices |
| **Critical Infrastructure** | On ports connecting to critical network devices (IP phones, printers or servers), Port Security ensures that only the expected hardware is present |
| **Colocation and Data Centers** | To enforce strict device access policies on ports where only specific hardware is allowed, preventing accidental or malicious misconnections |

---

## Best Practices/Security Considerations

### Configuration Guidelines
• Administratively shut down all unused ports to prevent unauthorized access  
• Configure ports as access ports ensuring that Port Security applies correctly  
• For stronger security, especially in enterprise environments, use 802.1X for device/user authentication. Port Security can act as a secondary safeguard or fallback  
• Clearly document which ports have Port Security enabled and what the allowed MAC addresses are. This is crucial for troubleshooting and maintenance  
• Regularly review violation logs and SNMP traps to detect unauthorized access attempts  
• In environments where devices frequently change (conference rooms, etc), consider using dynamic learning with appropriate aging settings, or use 802.1X if possible

### Security Hardening
• Port Security limits the number of devices per port, helping to prevent unauthorized access and potential network breaches  
• By limiting the number of MAC addresses learned on a port, Port Security reduces the risk of CAM table overflow attacks, which can lead to network-wide traffic flooding  
• Use strict violation modes (shutdown or restrict) in environments where security is paramount. However, in environments requiring high availability (critical workstations, etc), consider using restrict mode to avoid complete port shutdown while still logging violations  
• Ensure that violation logs, SNMP traps and Syslog messages are monitored so that security incidents can be quickly detected and addressed

### Operational Management
• Ensure that all access ports have Port Security enabled, especially in high-security areas  
• Periodically review secure MAC address lists and violation logs. Use automation tools or SNMP to monitor and alert on anomalies  
• Maintain clear records of which MAC addresses are allowed on which ports, and update the documentation as devices change  
• Where possible, use IEEE 802.1X for stronger authentication, with Port Security as an additional layer  
• Validate configurations in a lab environment before deploying them in production, especially if integrating with multiple security features  
• In environments with frequent device changes, consider using dynamic learning with appropriate aging settings instead of static or sticky configurations

---

## Troubleshooting

### Port in Err-Disable State?
• Check the violation count and logs to see which MAC triggered the violation  
• Verify if the authorized device is connected  
• If the device is legitimate (moved to another port), update the configuration (remove the old secure MAC, then allow the new one)

### Unexpected Violations?
• Ensure that the port is configured in access mode and not inadvertently negotiating as a trunk  
• Check that sticky MAC addresses have been saved to the running configuration if persistence is required  
• Use the command show port-security to identify if there are multiple MAC addresses learned, or if the violation mode is set too restrictively for the environment

### MAC Address Aging Issues?
• If using dynamic learning (not sticky) with aging enabled, verify that the aging timer is appropriate for the environment  
• For sticky addresses, remember that aging is not supported on many Cisco platforms, so if a device is removed, the MAC remains, until manually cleared

---

## Insights

### Operational Reality
• **Port Security is simple to configure and operate**, but its static nature can sometimes cause issues in dynamic environments (guest access, BYOD, etc). Evaluate your environment and choose the appropriate mechanism  
• **Monitoring and prompt response to violations is essential**. An err-disabled port can affect network availability, so having automated recovery or clear procedures is important

### Modern Implementation
• **As networks become more dynamic and device mobility increases**, many organizations are moving toward identity-based access (802.1X or NAC) rather than relying solely on MAC-based controls  
• **However, Port Security remains a valuable tool** for smaller networks or as an additional safeguard in larger deployments

---

## Commands

*[To be completed during lab sessions]*
