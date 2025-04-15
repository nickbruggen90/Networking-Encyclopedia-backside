# MAC ACL -

### Overview:
> * MAC ACLs are L2 access control lists that filter traffic based on source or destination MAC address. They are used on switches (typically Catalyst platforms) to control forwarding decisions at L2 - before packets are routed.
> * MAC ACLs operate on Ethernet frames, not IP packets
> * Cannot filter based on IP, TCP, or UDP fields
> * Often used to filter broadcast, multicast, unknown unicast, or specific MAC patterns
---
### MAC ACL Number Range:
> * 700 - 799, 1100 - 1199
---
### Configuration Steps:
> * Define the MAC ACL using the correct number range
> * Apply to interface using mac access-group [ACL #] in
> * Only applies to L2 ports (not routed interfaces)
---
### Use Cases:
> * Restricting or permitting traffic between devices within the same VLAN
> * Applying granular security on access ports
> * Enforcing non-IP traffic policies (IPX, AppleTalk, control-plane MACs)
---
### Best Practices/Security Considerations:
> * Use for critical infrastructure ports (access switches to IP phones)
> * Combine with port security to limit MAC address and enforce ACLs
> * Use to block spoofed MACs, rogue devices, or unauthorized multicast
> * Avoid overlapping policies with IP ACLs on routed interfaces
> * MAC ACLs do not apply to routed traffic or L3 SVI interfaces
---
### Common Issues and Fixes:
> * ACL Not Applied?
>   * Cause?: interface is L3
>   * Fix: ensure interface is in L2 switchport mode
> * MAC ACL Doesn’t Filter IP Traffic?
>   * Cause?: MAC ACLs operator at L2 only
>   * Fix: use IP ACLs on routed ports instead
> * Unexpected Behavior?
>   * Cause?: overlapping wildcard masks
>   * Fix: use log or simplify rules for testing
> * ACL Doesn’t Trigger?
>   * Cause?: wrong ACL number range
>   * Fix: only 700 - 799 and 1100 - 1199 are valid MAC ACLs
---
