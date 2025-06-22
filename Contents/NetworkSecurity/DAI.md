# DAI - Dynamic ARP Inspection
*Cisco proprietary*

## Overview
• DAI is a security feature used on Cisco switches to prevent ARP spoofing and poisoning attacks  
• DAI validates ARP packets against a trusted database, typically built by DHCP snooping  
• DAI helps ensure that only legitimate ARP responses are allowed, protecting the network from man-in-the-middle attacks and other ARP-based threats  
• DAI is a vital component in protecting the network at the L2 level, significantly reducing the risk of ARP-based attacks

---

## Timers
• **DAI is event-driven** → No specific timers; responds to ARP packet events as they occur

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **ARP** | A protocol used to map an IP address to a MAC address on a local network |
| **ARP Spoofing/Poisoning** | A technique in which an attacker sends falsified ARP messages to a network, associating their MAC address with the IP address of another device (usually a gateway) to intercept or manipulate traffic |
| **DAI** | A security feature that intercepts, validates and filters ARP packets based on trusted information (usually from a DHCP snooping database) |
| **Trusted Port** | An interface configured to bypass DAI checks (commonly used for uplink ports or ports connected to DHCP servers) |
| **Untrusted Port** | An interface on which DAI checks are enforced, meaning that ARP packets received are subject to validation |
| **DAI ACL** | Access control lists used to allow specific static ARP entries or to define exceptions for DAI |
| **DHCP Snooping** | A feature that builds and maintains a database of IP-to-MAC bindings from DHCP messages; this database is essential for DAI validation |

---

## DAI Process

### Process Flow

| Step | Description | Action |
|------|-------------|---------|
| **1. Integration with DHCP Snooping** | DAI relies on the DHCP snooping binding database | Trusted mappings of IP addresses to MAC addresses learned from DHCP transactions |
| **2. ARP Packet Interception** | When an ARP packet is received on an untrusted port | DAI intercepts and inspects the packet |
| **3. Validation Process** | Compare packet's IP-to-MAC mapping against trusted database | Check against DHCP snooping database or static DAI ACL |
| **4. Decision Making** | Determine if packet should be allowed or dropped | If mapping matches trusted entry → allow; if not → drop |
| **5. Action on Invalid Packets** | Invalid ARP packets are dropped | Alert or log message generated for investigation |

### Validation Details
• **ARP Replies** → Checks that the sender's IP/MAC binding is correct  
• **ARP Requests** → Validation may be less strict but still verifies key fields to prevent spoofing  
• **Database Sources** → DHCP snooping database (primary) or static DAI ACL entries (exceptions)

### Port Behavior
• **Trusted Ports** → Bypass DAI checks, populate DHCP snooping database  
• **Untrusted Ports** → Subject to DAI validation checks

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Prevention of ARP Spoofing/Poisoning** | DAI verifies ARP packets against a trusted binding table, preventing attackers from sending spoofed ARP messages that can lead to MITM attacks |
| **Enhanced L2 Security** | By inspecting ARP packets on untrusted ports, DAI ensures that only legitimate ARP responses are forwarded, protecting from unauthorized devices and rogue ARP entries |
| **Improved Network Integrity** | DAI helps maintain the integrity of IP-to-MAC address mappings, ensuring that traffic is correctly directed and reducing the risk of traffic interception or redirection |
| **Visibility and Auditing** | DAI logs suspicious or invalid ARP messages, providing network administrators with valuable information for detecting and investigating potential security breaches |

---

## Use Cases

| Environment | Application | Benefits |
|-------------|-------------|----------|
| **Enterprise LANs** | Deployed on access ports to protect end-user communications | Prevents ARP spoofing in environments where it could compromise sensitive data |
| **Data Centers** | Strict control over ARP traffic | Maintains high performance and prevents attacks on critical servers and applications |
| **Wireless Networks** | Combined with DHCP Snooping | Protects wireless networks by ensuring only legitimate devices can communicate, securing against rogue APs or unauthorized clients |
| **Service Provider Networks** | Secures customer traffic on shared infrastructure | Prevents one customer from interfering with another by spoofing ARP messages |
| **Government and Financial Institutions** | Part of layered defense strategy | Protects against ARP-based attacks in high-security environments, ensuring secure communication for sensitive operations |

---

## Best Practices/Security Considerations

### Core Configuration Requirements
• **Always enable DHCP snooping** on all VLANs where DAI is required, and correctly mark trusted interfaces  
• **Only configure trusted ports where required** (uplinks, servers) to minimize exposure  
• **Use ACLs to allow static ARP mappings** to critical devices that do not use DHCP

### Monitoring and Maintenance
• **Regularly review logs and statistics** from DAI to detect potential attacks or misconfigurations  
• **Periodically audit DAI and DHCP snooping settings** especially when changes occur in network topology or device roles  
• **Ensure log settings are balanced** to avoid overwhelming the management system with alerts during normal network functions

### Layered Security Implementation
• **Combine DAI with DHCP Snooping, IP Source Guard and 802.1X** for a comprehensive layered approach  
• **Ensure consistent application** to all untrusted ports; inconsistent configurations can lead to gaps in ARP protection  
• **Be cautious when configuring DAI on trunk ports** as incorrect settings can impact multiple VLANs

---

## Common Issues and Fixes

### General Troubleshooting
• **Ensure DHCP snooping is enabled** and that untrusted ports are correctly configured; without a populated binding database, DAI may drop legitimate ARP packets  
• **Verify that uplink ports or ports connected to DHCP servers** are correctly set to "trusted"  
• **Check for possible misconfigured static IP-to-MAC mappings** or ACL issues. Review logs for dropped ARP packets

### Specific Issues and Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| **Legitimate ARP packets being dropped** | DHCP snooping not enabled or binding database not populated | Verify DHCP snooping is enabled and correctly populating the binding database |
| **Interface misconfiguration** | Affected interface misconfigured as untrusted when it should be trusted | Ensure proper trusted/untrusted port configuration |
| **ACL blocking** | DAI ACLs inadvertently blocking legitimate traffic | Review DAI ACLs for inadvertent blocking |
| **No bindings in DHCP snooping database** | DHCP server configuration or connectivity issues | Check DHCP server configurations and network connectivity between DHCP server and switch |
| **Trusted port misconfiguration** | Ports connected to DHCP servers not set to trusted | Verify that ports connected to DHCP servers are set to trusted |
| **Inconsistent ARP behavior across VLANs** | DAI not enabled on correct VLAN or VLAN assignment issues | Ensure DAI is enabled on the correct VLAN; cross-check VLAN assignments and trunk configurations |

---

## Insights

### Implementation Dependencies
• **The accuracy and reliability of DAI depend on a correctly configured DHCP snooping database** → Ensure both features are consistently maintained  
• **When properly implemented, DAI operates transparently** → However, misconfiguration can lead to dropped traffic and connectivity issues

### Deployment Strategy
• **Testing changes in a lab environment is recommended** before deployment in production  
• **DAI should be part of a layered security strategy** that includes features like port security, IP source guard, and network segmentation to provide comprehensive protection

### Operational Considerations
• **Monitor DAI statistics and logs regularly** to ensure proper operation and detect potential security events  
• **Maintain consistent configuration across the network** to avoid security gaps or operational issues

---

## Commands
*[To be completed during lab sessions]*
