# ACL - Access Control Lists
*RFC 2644 (default deny behavior), RFC 3704 (BCP38 for source filtering)*

## Overview
• ACLs are rule-based filters used in routers, switches, and firewalls to permit or deny traffic based on L3 and L4 headers  
• They are fundamental for security, traffic control, QoS, and policy enforcement

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **Permit** | Allows traffic that matches the rule |
| **Deny** | Blocks traffic that matches the rule |
| **Implicit Deny** | Hidden rule at the end of every ACL that blocks unmatched traffic |
| **Wildcard Mask** | Inverse subnet mask used in ACLs |
| **Line Number** | Sequence number used to organize/edit named ACLs |
| **Access-Group** | Command used to apply an ACL to an interface or line |
| **Access-Class** | Applies ACL to VTY (remote access) sessions |
| **Established** | Keyword used in extended ACLs to permit return TCP traffic with ACK/RST |

---

## Types of ACLs

| ACL Type | Number Range | Filtering Capability | Use Case |
|----------|--------------|---------------------|----------|
| **Standard ACL** | 1-99, 1300-1999 | Source IP address only | Basic source-based filtering |
| **Extended ACL** | 100-199, 2000-2699 | Source/destination, protocol, port, flags | Granular traffic control |
| **Named ACL** | N/A | Same as numbered but with names | More flexible, supports editing and resequencing |
| **IPv6 ACL** | N/A | IPv6 traffic filtering | IPv6 environments (separate syntax: `ipv6 access-list`) |
| **Reflexive ACL** | N/A | Stateful return traffic | Permit return traffic dynamically |
| **Time-Based ACL** | N/A | Time-restricted filtering | ACLs active only during defined time periods |
| **MAC ACL** | N/A | L2 filtering | Switch or firewall L2 traffic control |
| **Control Plane ACL** | N/A | CPU/control plane protection | CoPP/CPPr implementations |
| **VACL** | N/A | Intra-VLAN filtering | Traffic filtering within VLANs (Cisco-specific) |

---

## Process/Evaluation Logic

### ACL Processing Order
```
1. Evaluated top-down, first match wins
2. If no match is found, traffic is implicitly denied
3. Applied to interfaces, VTY lines, control plane, and VPNs
```

### Processing Flow
```
Packet → ACL Rule 1 → Match? → [Yes] Apply Action (Permit/Deny)
            ↓
         [No] Continue
            ↓
        ACL Rule 2 → Match? → [Yes] Apply Action (Permit/Deny)
            ↓
         [No] Continue
            ↓
        Implicit Deny → Drop Packet
```

---

## Best Practices/Security Considerations

### Placement Guidelines
• **Place standard ACLs** as close to the destination as possible (since they filter only on source)  
• **Place extended ACLs** as close to the source as possible (more granular filtering)

### Configuration Best Practices
• **Use named ACLs** for better readability and easier management  
• **Always include a final permit ip any any** for logging-only ACLs, or to avoid accidental black holes during testing  
• **Document ACLs** with remark statements  
• **Use object-groups or prefix-lists** for scalability in modern platforms  
• **Avoid overly broad deny ip any any rules** on user-facing interfaces

### Security Implementations
• **Use ACLs on VTY lines** to restrict remote access  
• **Pair DHCP Snooping with ACLs** (IP Source Guard, etc.)  
• **Combine with uRPF or BCP38** (RFC 3704) to prevent spoofed IPs  
• **Implement CoPP/CPPr** using ACLs to rate-limit/control CPU-bound traffic  
• **Filter inbound traffic** before it enters sensitive zones (firewalls, internal VLANs)

---

## Common Issues and Fixes

| Problem | Cause | Solution |
|---------|-------|----------|
| **ACL Blocks Legitimate Traffic** | Rule order or wildcard mask wrong | Use `log` keyword, review order |
| **Cannot Access Router via SSH** | ACL applied to VTY or interface | Use `show access-lists`, `show run` |
| **Return Traffic Dropped** | Missing `established` keyword or stateful mechanism | Use reflexive ACLs or inspect/CBAC |
| **IPv6 ACL Not Working** | Not applied correctly or missing implicit permit | Always end with `permit ipv6 any any` for testing |

### Troubleshooting Steps
1. **Check ACL configuration** → `show access-lists [name/number]`
2. **Verify ACL application** → `show run interface [interface]`
3. **Monitor ACL hits** → Use `log` keyword and check logs
4. **Test connectivity** → Systematic testing from source to destination
5. **Review rule order** → Remember first match wins principle
