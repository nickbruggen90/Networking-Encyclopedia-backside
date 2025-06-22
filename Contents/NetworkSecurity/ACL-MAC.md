# MAC ACL - Layer 2 Access Control Lists

## Overview
• MAC ACLs are L2 access control lists that filter traffic based on source or destination MAC address  
• They are used on switches (typically Catalyst platforms) to control forwarding decisions at L2 - before packets are routed  
• MAC ACLs operate on Ethernet frames, not IP packets  
• Cannot filter based on IP, TCP, or UDP fields  
• Often used to filter broadcast, multicast, unknown unicast, or specific MAC patterns

---

## MAC ACL Number Range

| ACL Type | Number Range | Layer | Filtering Capability |
|----------|--------------|-------|---------------------|
| **MAC ACL** | 700-799, 1100-1199 | Layer 2 | Source/destination MAC addresses only |

---

## Configuration Steps

### Basic MAC ACL Implementation
```
1. Define the MAC ACL using the correct number range
2. Apply to interface using: mac access-group [ACL #] in
3. Only applies to L2 ports (not routed interfaces)
```

### Requirements
• **Interface must be in switchport mode** (L2 only)  
• **Cannot be applied to routed interfaces** or SVIs  
• **Only inbound direction supported** in most implementations

---

## Use Cases

| Use Case | Description | Benefit |
|----------|-------------|---------|
| **Intra-VLAN Traffic Control** | Restricting or permitting traffic between devices within the same VLAN | Enhanced security within broadcast domains |
| **Access Port Security** | Applying granular security on access ports | Device-level access control |
| **Non-IP Protocol Filtering** | Enforcing policies for non-IP traffic (IPX, AppleTalk, control-plane MACs) | Legacy protocol management |
| **Infrastructure Protection** | Controlling access to critical network infrastructure | Network device security |

### Common Scenarios
• **IP Phone Security** → Restrict traffic to/from specific phone MAC addresses  
• **Rogue Device Prevention** → Block unauthorized MAC addresses on access ports  
• **Multicast Control** → Filter specific multicast MAC patterns  
• **Broadcast Limitation** → Control broadcast traffic flow within VLANs

---

## Best Practices/Security Considerations

### Infrastructure Security
• **Use for critical infrastructure ports** (access switches to IP phones)  
• **Combine with port security** to limit MAC addresses and enforce ACLs  
• **Use to block spoofed MACs**, rogue devices, or unauthorized multicast

### Implementation Guidelines
• **Avoid overlapping policies** with IP ACLs on routed interfaces  
• **Remember:** MAC ACLs do not apply to routed traffic or L3 SVI interfaces  
• **Test thoroughly** as MAC ACL behavior can be platform-specific  
• **Document MAC address sources** for easier troubleshooting

### Design Considerations
• **Plan for MAC address changes** (device replacements, virtualization)  
• **Consider scalability** as MAC ACLs can become unwieldy in large environments  
• **Monitor effectiveness** using show commands and logging

---

## Common Issues and Fixes

| Problem | Cause | Solution |
|---------|-------|----------|
| **ACL Not Applied** | Interface is L3 | Ensure interface is in L2 switchport mode |
| **MAC ACL Doesn't Filter IP Traffic** | MAC ACLs operate at L2 only | Use IP ACLs on routed ports instead |
| **Unexpected Behavior** | Overlapping wildcard masks | Use log or simplify rules for testing |
| **ACL Doesn't Trigger** | Wrong ACL number range | Only 700-799 and 1100-1199 are valid MAC ACLs |

### Troubleshooting Steps
1. **Verify ACL number range** → Must be 700-799 or 1100-1199
2. **Check interface mode** → `show interface [interface] switchport`
3. **Confirm ACL application** → `show mac access-group`
4. **Monitor ACL hits** → Use logging and show commands
5. **Test with simple rules** → Start with basic permit/deny for troubleshooting

### Verification Commands
• `show mac access-group [interface]` → Display applied MAC ACLs  
• `show access-lists [700-799|1100-1199]` → Show MAC ACL configuration  
• `show interface [interface] switchport` → Verify L2 interface mode
