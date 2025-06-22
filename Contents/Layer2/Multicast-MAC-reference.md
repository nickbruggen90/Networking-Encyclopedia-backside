# Multicast and Special MAC Address Reference

## IPv4 Multicast Addresses

### General Network Functions

| IPv4 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **224.0.0.1** | 01:00:5E:00:00:01 | All hosts on a local subnet |
| **224.0.0.2** | 01:00:5E:00:00:02 | All routers on a local subnet |

### Routing Protocol Multicast

| IPv4 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **224.0.0.5** | 01:00:5E:00:00:05 | OSPF Hello packets |
| **224.0.0.6** | 01:00:5E:00:00:06 | OSPF DR/BDR communication |
| **224.0.0.9** | 01:00:5E:00:00:09 | RIPv2 updates |
| **224.0.0.10** | 01:00:5E:00:00:0A | EIGRP updates |
| **224.0.0.13** | 01:00:5E:00:00:0D | PIM |

### First Hop Redundancy Protocols

| IPv4 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **224.0.0.18** | 01:00:5E:00:00:12 | VRRPv2 |
| **224.0.0.19** | 01:00:5E:00:00:13 | HSRPv1 |
| **224.0.0.102** | 01:00:5E:00:00:66 | GLBP |

### Other IPv4 Protocols

| IPv4 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **224.0.0.22** | 01:00:5E:00:00:16 | IGMP |

---

## IPv6 Multicast Addresses

### General Network Functions

| IPv6 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **FF02::1** | 33:33:00:00:00:01 | All IPv6 nodes |
| **FF02::2** | 33:33:00:00:00:02 | All IPv6 routers |

### Routing Protocol Multicast (IPv6)

| IPv6 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **FF02::5** | 33:33:00:00:00:05 | OSPFv3 Hello packets |
| **FF02::6** | 33:33:00:00:00:06 | OSPFv3 DR/BDR communication |
| **FF02::9** | 33:33:00:00:00:09 | RIPng (RIP for IPv6) |
| **FF02::A** | 33:33:00:00:00:0A | EIGRP for IPv6 |
| **FF02::D** | 33:33:00:00:00:0D | PIM |

### IPv6 Specific Functions

| IPv6 Multicast Address | MAC Address | Protocol/Usage |
|------------------------|-------------|----------------|
| **FF02::12** | 33:33:00:00:00:12 | VRRPv3 (for IPv6) |
| **FF02::16** | 33:33:00:00:00:16 | MLD (Multicast Listener Discovery) |
| **FF02::66** | 33:33:00:00:00:66 | GLBP |

---

## IEEE Standard Layer 2 Protocols

### Spanning Tree and Network Management

| MAC Address | Protocol/Usage |
|-------------|----------------|
| **01:80:C2:00:00:00** | STP (Spanning Tree Protocol) |
| **01:80:C2:00:00:01** | IEEE 802.1X (Port-based Network Access Control) |
| **01:80:C2:00:00:02** | LACP (Link Aggregation Control Protocol) |
| **01:80:C2:00:00:03** | LLDP (Link Layer Discovery Protocol) |
| **01:80:C2:00:00:04** | 802.1ak (Multiple Registration Protocol) |
| **01:80:C2:00:00:08** | PTPv2 (Precision Time Protocol version 2) |

---

## Cisco Proprietary Layer 2 Protocols

### Discovery and Management Protocols

| MAC Address | Protocol/Usage |
|-------------|----------------|
| **01:0C:CD:01:00:00** | CDP (Cisco Discovery Protocol) |
| **01:0C:CD:02:00:00** | VTP (VLAN Trunking Protocol) |
| **01:0C:CD:04:00:00** | DTP (Dynamic Trunking Protocol) |
| **01:0C:CD:05:00:00** | PAgP (Port Aggregation Protocol) |
| **01:0C:CD:07:00:00** | UDLD (Unidirectional Link Detection) |

---

## Special Broadcast Address

| MAC Address | Usage |
|-------------|-------|
| **FF:FF:FF:FF:FF:FF** | Ethernet broadcast address |

---

## Address Format Patterns

### IPv4 Multicast to MAC Mapping
• **IPv4 Multicast Range:** 224.0.0.0 to 239.255.255.255  
• **MAC Address Pattern:** 01:00:5E:xx:xx:xx  
• **Mapping:** Last 23 bits of IPv4 multicast address map to last 23 bits of MAC address

### IPv6 Multicast to MAC Mapping
• **IPv6 Multicast Prefix:** FF00::/8  
• **MAC Address Pattern:** 33:33:xx:xx:xx:xx  
• **Mapping:** Last 32 bits of IPv6 multicast address map to last 32 bits of MAC address

### IEEE Reserved Addresses
• **IEEE 802 Range:** 01:80:C2:00:00:xx  
• **Usage:** IEEE standard protocols (STP, LACP, LLDP, etc.)

### Cisco Proprietary Range
• **Cisco OUI:** 01:0C:CD:xx:xx:xx  
• **Usage:** Cisco proprietary protocols (CDP, VTP, DTP, etc.)

---

## Quick Reference Notes

### Multicast Address Types
• **224.0.0.x** → Well-known IPv4 multicast (local network scope)  
• **FF02::x** → Link-local IPv6 multicast  
• **01:00:5E** → IPv4 multicast MAC prefix  
• **33:33** → IPv6 multicast MAC prefix

### Protocol Families
• **Routing Protocols:** OSPF, RIP, EIGRP, PIM  
• **Redundancy Protocols:** HSRP, VRRP, GLBP  
• **Discovery Protocols:** CDP, LLDP  
• **Management Protocols:** LACP, STP, 802.1X

### Common Troubleshooting
• **Check multicast filtering** on switches for these well-known addresses  
• **Verify IGMP snooping** configuration for proper multicast forwarding  
• **Monitor protocol-specific traffic** using these addresses for debugging  
• **Ensure proper VLAN configuration** for multicast protocol operation
