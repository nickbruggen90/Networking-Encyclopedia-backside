# PAgP - Cisco proprietary

### Overview:
> * Mostly used in older environments or purely Cisco environments
> * Consistency checks during negotiations automatically ensures and verifies all member links are configured identically
> * Uses LB hashes: src/dst IP, src/dst MAC, or combination of those
> * LB per frame, not per bit (Ethernet frames are atomic units and cannot be split across multiple links without breaking the protocol)
> * Cannot be mixed with LACP
> * 8 active and 8 standby links allowed
> * Uses desirable (sends PAgP packets) / auto (passive) model
---
### Timers:
> * Hello Timer = 30 sec || Dead Timer = 90 sec (x3 Hello)
> * Hello Timer Fast = 1 sec || Dead Timer = 3 sec (x3 Hello)
---
### Multicast:
> * Listening MAC = 01:00:0C:CC:CC:CC
---
### Protocols/Ports:
> * Protocol ID = 0x0104
---
### Priorities:
> * Port Priority = 0 - 255 (128 is default)  
*Lower priority takes precedence*
---
### Terminology/Definitions:
> * EtherChannel – a logical link aggregation that combines multiple physical links into one virtual interface for increased bandwidth and redundancy
> * Auto Mode – PAgP mode that waits for negotiation and does not actively send PAgP packets
> * Desirable Mode – PAgP mode that actively sends PAgP packets to negotiate an EtherChannel
> * Channel Group – a logical grouping of interfaces that form an EtherChannel, assigned a unique number per switch
> * PAgP Learn-Method – defines how MAC addresses are learned and distributed across the EtherChannel bundle
> * PAgP Timers – control how frequently PAgP packers are sent to detect link failure (default is 30 sec)
> * PAgP Neighbor – a directly connected switch participating in PAgP-based EtherChannel negotiation
> * Static EtherChannel – a manually configured EtherChannel that does not use PAgP or LACP negotiation
---
### PAgP Port Roles and States:
> * Negotiation Port – a port actively attempting to establish an EtherChannel using desirable mode
> * Listening Port – a port in auto mode, waiting for PAgP request
> * Standalone Port – a port that has not successfully negotiated an EtherChannel and remains independent
> * Bundled Port – a port successfully added to an EtherChannel group and actively forwarding traffic
> * Suspended Port – a port that failed PAgP negotiation due to a mismatch (speed, duplex, VLANs)
---
### PAgP Status Flags:
> * (P) = Protocol is PAgP
> * (U) = Bundle is up and operational
> * (S) = Standalone, not bundled
> * (I) = Individual, misconfigured ports
> * (D) = Down
----
### Header Breakdown:
*Ethernet Frame* = 
> * 56 - Preamble
> * 8 - SFD
> * 48 - Destination MAC
> * 48 - Source MAC
> * 16 - EtherType
> * variable - PAgP Payload
> * 32 - FCS  
  
*PAgP Payload* = 
> * 8 - Version
> * 48 - Device ID
> * 8 - PAgP Message Type
> * 8 - Learn-Method
> * variable - TLV
---
### Header Defintions:
> * Version – identifies the PAgP protocol version used between devices. Ensures compatibility between Cisco devices
> * Device ID – MAC address of the switch sending the PAgP frame. Used to identify if links belong to the same physical device before bundling. Prevents accidental misconfiguration or loops caused by mismatched EtherChannel groups
> * PAgP Message Type – indicates the negotiation mode being used (Auto, Desirable or Standalone). Determines if the switch actively forms an EtherChannel
>   * 00 = Standalone Mode
>   * 01 = Auto Mode
>   * 02 = Desirable Mode
> * Learn-Method – defines how MAC addresses are learned across the EtherChannel bundle. Ensures consistency across aggregated links
> * TLV Fields – Type-Length-Value fields contain additional parameters such as PAgP timers, interface settings and neighbor information
> * PAgP Flags – used for state signaling within PAgP, indicating active negotiation and channel status
> * PAgP Timer – controls the interval at which PAgP messages are exchanged (default is 30 sec)
--- 

> *Silent*: The local side can form a channel if it doesn’t hear PAgP frames from the remote side. Silent mode is useful if the remote device does not send PAgP but can still operate in a mode that appears “silent”
*Non-Silent*: The local side expects to hear PAgP frames, if none are received then the link will not join the bundle

---
### Best Practices/Security Considerations: 
> * Ensure timers, speed, duplex, VLAN settings (trunk mode/native VLAN, etc), switchport mode and STP feature settings are uniform across member interfaces
> * In larger networks or links with low fault tolerance use the normal timers
> * If one side is “On” and other side tries to negotiate, it can lead to loops or errors
> * Set port priority on to determine which port will become the active when in standby
> * Use “auto” mode initially to establish a baseline Etherchannel. Once the physical link is aggregated, you can then switch negotiation parameters. This approach provides a stable foundation and minimizes risk of misconfiguration
---
### Troubleshooting:
> * EtherChannel is not forming or is operating as individual links? Verify EtherChannel status –
>    * show etherchannel summary – refer to flags section for operational mode of links. If ports show (I) then they are not forming due to a misconfiguration mismatch
> * One switch is set to auto, and the other is also auto preventing EtherChannel formation? Ensure that at least one side is set to desirable –
>   * show spanning-tree mst – to verify operational mode of links
> * Ports are operational but are not aggregating into a bundle? Verify that all bundled interfaces have the same VLAN assignments and trunk settings –
>   * show interfaces trunk – all interfaces must be in the same VLAN (for access ports), have identical trunking modes (for trunk ports) and have matching allowed VLAN list
>   * show interfaces {interface} switchport – check if trunk encapsulation matches (for trunk ports)
> * Ports are showing as standalone instead of bundled? Ensure all EtherChannel members have links matching speed and duplex settings –
>   * show interfaces status — verify ports have identical speed and duplex
> * PAgP is not detecting the neighboring switch? Check for active PAgP neighbors
>   * show pagp neighbor – displays PAgP partner switch information, if no neighbors are detected, check for physical link issues
> * Traffic not evenly distributed across bundled links? Check the current EtherChannel load-balancing method –
>   * show etherchannel load-balance – load balancing can be based on MAC, IP or L4
---
### Common PAgP Issues and Fixes:
> * EtherChannel Not Forming?
>   * Both interfaces set to auto
> * Links in bundle are blocking?
>   * Misconfigured VLAN settings
> * Different port speeds?
>   * PAgP requires uniform speed/duplex
> * Neighbor switch not detected?
>   * Cabling issue or spanning-tree blocking
> * Uneven traffic load?
>   * Load balancing method mismatch
---
Insights:
> *
---
### Commands:
> * show etherchannel summary
>   * displays EtherChannel group number, status and protocol used
> * show pagp neighbor
>   * lists PAgP partner switch information
> * show etherchannel load-balance
>   * checks how traffic is distributed across links
> * show interfaces trunk
>   * checks if trunk configurations match
> * show spanning-tree mst
>   * verifies if STP is affecting EtherChannel links
> * debug pagp events
>   * monitors real-time PAgP negotiations
