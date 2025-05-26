# ACL - RFC 2644 (default deny behavior), RFC 3704 (BCP38 for source filtering)

### Overview:
> * ACLs are rule-based filters used in routers, switches, and firewalls to permit or deny traffic based on L3 and L4 headers. They are fundamental for security, traffic control, QoS, and policy enforcement
---
### Terminology/Definitions:
> * Permit – allows traffic that matches the rule
> * Deny – blocks traffic that matches the rule
> * Implicit Deny – hidden rule at the end of every ACL that blocks unmatched traffic
> * Wildcard Mask – inverse subnet mask used in ACLs
> * Line Number – sequence number used to organize/edit named ACLs
> * Access-Group – command used to apply an ACL to an interface or line
> * Access-Class – applies ACL to VTY (remote access) sessions
> * Established – keyword used in extended ACLs to permit return TCP traffic with ACK/RST
---
### Types of ACLs:
> * Standard ACL – (1 - 99, 1300 - 1999) - filters only on source IP address
> * Extended ACL – (100 - 199, 2000 - 2699) - filters on source/destination, protocol, port, flags
> * Named ACL – more flexible, supports editing and resequencing
> * IPv6 ACL – separate syntax (ipv6 access-list)
> * Reflexive ACL – stateful ACLs that permit return traffic dynamically
> * Time-Based ACL – ACLs that are active only during defined time periods
> * MAC ACL – L2 filtering (on switches or firewalls)
> * Control Plane ACL (CoPP/CPPr) – protect CPU/control plane device
> * VACL – traffic filtering within VLANs (Cisco-specific)
---
### Process/Evaluation Logic:
> * 1. Evaluated top-down, first match wins
> * 2. If no match is found, traffic is implicitly denied
> * 3. Applied to interfaces, vty lines, control plane, and VPNs
---
### Best Practices/Security Considerations:
> * Place standard ACLs as close to the destination as possible (since they filter only on source)
> * Place extended ACLs as close to the source as possible (more granular filtering)
> * Use named ACLs for better readability and easier management
> * Always include a final permit ip any any for logging-only ACLs, or to avoid accidental black holes during testing
> * Document ACLs with remark statements
> * Use object-groups or prefix-lists for scalability in modern platforms
> * Avoid overly broad deny ip any any rules on user-facing interfaces
> * Use ACLs on VTY lines to restrict remote access
> * Pair DHCP Snooping with ACLs (IP Source Guard, etc)
> * Combine with uRPF or BCP38 (RFC 3704) to prevent spoofed IPs
> * Implement CoPP/CPPr using ACLs to rate-limit/control CPU-bound traffic
> * Filter inbound traffic before it enters sensitive zones (firewalls, internal VLANs)
---
### Common Issues and Fixes:
> * ACL Blocks Legit Traffic?
>   * Cause?: rule order or wildcard mask wrong
>   * Fix: use log keyword, review order
> * Cannot Access Router via SSH?
>   * Cause?: ACL applied to VTY or interface
>   * Fix: use show access-lists, show run
> * Return Traffic Dropped?
>   * Cause?: missing established keyword or stateful mechanism
>   * Fix: use reflexive ACLs or inspect/CBAC
> * IPv6 ACL Not Working?
>   * Cause?: not applied correctly or missing implicit permit
>   * Fix: always end with permit ipv6 any any for testing
---

