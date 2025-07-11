# DTP - Dynamic Trunking Protocol
*Cisco proprietary*

## Overview
• DTP is a protocol used to automatically negotiate trunking on a link between two Cisco devices  
• Trunking allows multiple VLANs to traverse a single physical link, and DTP simplifies the process by dynamically determining whether a port should become a trunk or remain an access port  
• When enabled can automatically negotiate the port to be trunk or access based on the remote and local ports DTP mode  
• DTP is "plug-and-play"  
• Uses TLV format in ethernet header  
• Auto + Auto = Auto, Access + any = Access, any other combo will form a trunk  
• Auto mode is default

---

## Timers

| Timer Type | Default Value | Purpose |
|------------|---------------|---------|
| **Advertisements** | 30 sec | DTP frame transmission interval |
| **Hold Timer** | 90 sec (x3 Advertisement) | Timeout for DTP neighbor detection |

---

## Multicast
• **01:00:0C:CC:CC:CC** - DTP multicast address

---

## Header Breakdown

| Field | Size | Description |
|-------|------|-------------|
| **Destination MAC** | 6 bytes | Always 01:00:0C:CC:CC:CC |
| **Source MAC** | 6 bytes | Source device MAC address |
| **EtherType** | 2 bytes | 0x2004 - identifies the frame as DTP |
| **DTP Type** | 1 byte | i.e; negotiation or status |
| **Status** | 1 byte | Indicates the trunking state of the port |
| **DTP Domain** | Variable | Identifies administrative domain |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **DTP** | A Cisco proprietary protocol that negotiates trunking between switches by exchanging DTP frames. It determines whether a link will operate as a trunk or access port based on the configuration on each side |
| **Trunk** | A network link that carries traffic for multiple VLANs. Trunk links use tagging (typically IEEE 802.1Q) to identify which VLAN a frame belongs to |
| **Access Port** | A switch port configured to belong to a single VLAN. In an access port, no VLAN tagging is applied to the traffic |
| **Native VLAN** | The VLAN that is not tagged on a trunk link. DTP negotiates trunking without altering the native VLAN setting unless manually configured |

---

## DTP Modes

| Mode | Description |
|------|-------------|
| **Dynamic Auto** | The port will become a trunk if the neighboring port actively negotiates trunking, but it will not initiate trunking |
| **Dynamic Desirable** | The port actively negotiates to become a trunk. If the neighbor is set to auto or desirable, a trunk is formed |
| **Force Mode** | The port is forced into a trunk mode regardless of the neighbor's configuration. No DTP negotiation occurs |
| **Non-Trunk** | The port is statically configured as an access port; DTP is disabled |

---

## Architecture

### DTP Frames
• DTP operates by sending DTP frames between two connected Cisco devices  
• These frames carry information about the trunking capabilities and the desired trunking mode of each port

### Negotiation Process
• If both are configured as Dynamic Desirable, they actively negotiate and form a trunk  
• If one port is set to Dynamic Auto and the other to Dynamic Desirable, the desirable side will initiate the trunk formation  
• Ports in Force Mode bypass DTP negotiation entirely and operate as trunks  
• If both ports are set to Dynamic Auto, neither initiates trunking, and the link remains an access port

### Mode Hierarchy
• Force Mode has the highest priority; it unconditionally sets the port as a trunk  
• Dynamic Desirable actively attempts to form a trunk  
• Dynamic Auto is passive and waits for a partner that desires a trunk  
• Non-Trunk disables trunk negotiation entirely

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Simplified Trunk Configuration** | DTP automates the negotiation of trunk links, reducing manual configuration errors and simplifying network setup |
| **Dynamic Adaptation** | Enables switches to automatically adjust trunk status based on the connected device's configuration, improving network flexibility |
| **Efficient VLAN Management** | Allows a single physical link to carry traffic for multiple VLANs, which is essential in large enterprise networks |
| **Reduced Administrative Overhead** | By automating trunk negotiations, network administrators can save time during network deployment and maintenance |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Enterprise Campus Networks** | Use DTP on inter-switch links in environments where all devices are Cisco-based and where automatic trunking can simplify configuration |
| **Data Centers** | Streamline connectivity between switches in data centers by dynamically negotiating trunks for high-density VLAN environments |
| **Voice and Data Integration** | Combine with voice VLANs to allow both IP phones and data devices to operate on the same physical port without manual configuration |
| **Dynamic Environments** | In networks where topology changes frequently, DTP helps maintain proper trunk links without constant manual intervention |
| **Lab Environments** | Ideal for testing and educational setups to demonstrate how trunk negotiation works and to teach VLAN concepts |

---

## Best Practices/Security Considerations

### Configuration Guidelines
• Disable DTP in static trunk environments  
• Manually configure trunks  
• Check for consistency  
• It is best practice to disable DTP on ports where trunking is not desired

### Security Considerations
• Use port security and 802.1X alongside DTP to ensure that only authorized devices negotiate trunking and access the network  
• Disable DTP on user-facing ports to prevent unauthorized trunk formation, which can be exploited to gain access to multiple VLANs  
• Monitor DTP configurations and policies as part of your network security plan to maintain consistency across the environment

---

## Troubleshooting

### Unexpected Access Port Behavior?
• Verify that both connected devices are configured with compatible DTP modes. For instance, if one port is set to dynamic auto and the other is dynamic desirable, a trunk should form  
• If both ports are dynamic auto and no trunk is formed, consider changing one to dynamic desirable

### Trunk Not Forming?
• Check that DTP is not disabled via switchport nonegotiate on either side  
• Ensure the physical cable is intact and the interface is enabled  
• Confirm that the encapsulation type (dot1q) is supported and configured on both sides if explicitly set  
• In multi-vendor or mixed configuration environments, DTP might cause interoperability issues since it's Cisco proprietary. In such cases, manually setting the port to a trunk mode might be preferable

---

## Insights

*[To be completed during lab sessions]*

---

## Commands

*[To be completed during lab sessions]*
