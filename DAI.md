#DAI - Cisco proprietary

### Overview:
> * DAI is a security feature used on Cisco switches to prevent ARP spoofing and poisoning attacks.
> * DAI validates ARP packets against a trusted database, typically built by DHCP snooping.
> * DAI helps ensure that only legitimate ARP responses are allowed, protecting the network from man in the middle attacks and other ARP based threats
> * DAI is a vital component in protecting the network at the L2 level, significantly reducing the risk of ARP-based attacks.
---
### Terminology/Definitions:
> * ARP – a protocol used to map an IP address to a MAC address on a local network
> * ARP Spoofing/Poisoning – a technique in which an attacker sends falsified ARP messages to a network, associating their MAC address with the IP address of another device (usually a gateway) to intercept or manipulate traffic
> * DAI – a security feature that intercepts, validates and filters ARP packets based on trusted information (usually from a DHCP snooping database)
> * Trusted Port – an interface configured to bypass DAI checks (commonly used for uplink ports or ports connected to DHCP servers)
> * Untrusted Port – an interface in which DAI checks are enforced, meaning that ARP packets received are subject to validation
> * DAI ACL – access control listed used to allow specific static ARP entries or to define exceptions for DAI
> * DHCP Snooping – a feature that builds and maintains a database of IP-to-MAC bindings from DHCP messages; this database is essential for DAI validation
---
### DAI Process:
>* Integration with DHCP Snooping – DAI relies on the DHCP snooping binding database, which contains trusted mappings of IP addresses to MAC addresses learned from DHCP transactions. Only interfaces marked as “trusted” will populate this database; interfaces in the untrusted zone will be subject to DAI checks.
> * ARP Packet Interception and Inspection – When an ARP packet is received on an untrusted port, DAI intercepts it. The packet's IP-to-MAC mapping is compared against the DHCP snooping database (or a static DAI ACL if configured). If the ARP packet’s information matches a trusted entry, the packet is allowed; if not, it is dropped and logged as a potential risk
> * Validation Process – DAI validates both ARP requests and reply packets. For ARP replies, it checks that the sender's IP/MAC binding is correct. For ARP requests, the validation may be less strict but still verifies key fields to prevent spoofing.
> * Action on Invalid Packets – Invalid ARP packets are dropped, and an alert or log message may be generated for further investigation
---
### Key Benefits:
> * Prevention of ARP Spoofing/Poisoning – DAI verifies ARP packets against a trusted binding table (often built using DHCP Snooping). This helps prevent attackers from sending spoofed ARP messages that can lead to MITM attacks
> * Enhanced L2 Security – by inspecting ARP packets on trusted ports, DAI ensures that only legitimate ARP responses are forwarded. This protects the network from unauthorized devices and rogue ARP entries that could disrupt normal communication
> * Improved Network Integrity – DAI helps maintain the integrity of IP-to-MAC address mappings, ensuring that traffic is correctly directed and reducing the risk of traffic interception or redirection
> * Visibility and Auditing – DAI logs suspicious or invalid ARP messages, which provides network administrators with valuable information for detecting and investigating potential security breaches
---
### Use Cases:
> * Enterprise LANs – in environments where ARP spoofing could compromise sensitive data (such as corporate networks), DAI is deployed on access ports to protect end-user communications
> * Data Centers – data centers often require strict control over ARP traffic to maintain high performance and prevent attacks on critical servers and applications
> * Wireless Networks – when combined with DHCP Snooping, DAI can protect wireless networks by ensuring that only legitimate devices can communicate over LAN, helping secure the network from rogue APs or unauthorized clients
> * SP Networks – providers use DAI to secure customers traffic on shared infrastructure, preventing one customers from interfering with another by spoofing ARP messages
> * Government and Financial Institutions – networks with high-security requirements use DAI as part of a layered defense strategy to protect against ARP-based attacks, ensuring secure communication for sensitive operations
---
### Best Practices/Security Considerations:
> * Always enable DHCP snooping on all VLANs where DAI is required, and correctly mark trusted interfaces
> * Only configure trusted ports where required (uplinks, servers) to minimize exposure
> * Use ACLs to allow static ARP mappings to critical devices that do not use DHCP
> * Regularly review logs and statistics from DAI to detect potential attacks or misconfigurations
> * Periodically audit DAI and DHCP snooping settings, especially when changes occur in network topology or device roles
> * Ensure that log settings are balanced to avoid overwhelming the management system with alerts during normal network functions
> * Combine DAI with DHCP Snooping, IP Source Guard and 8021.X for a comprehensive layered approach
> * Ensure that DAI is consistently applied to all untrusted ports. Inconsistent configurations can lead to gaps in ARP protection. Be cautious when configuring DAI on trunk ports, as incorrect settings can impact multiple VLANs
---
### Common Issues and Fixes:
> * Ensure DHCP snooping is enabled and that untrusted ports are correctly configured’ without a populated binding database, DAI may drop legitimate ARP packets.
> * Verify that uplink port or ports connected to DHCP servers are correctly set to “trust”
> * Check for possible misconfigured static IP-to-MAC mappings or ACL issues. Review logs for dropped ARP packets.
> * Issue: Legitimate ARP packets are being dropped?
>   * Verify that DHCP snooping is enabled and correctly populating the binding database
>   * Ensure the affected interface is not misconfigured as untrusted if it should be trusted
>   * Review DAI ACLs for inadvertent blocking
> * Issue: No Bindings in the DHCP Snooping Database?
>   * Check DHCP server configurations and network connectivity between DHCP server and switch
>   * Verify that ports connected to DHCP servers are set to trusted
> * Issue: Inconsistent ARP Behavior Across VLANs?
>   * Ensure that DAI is enabled on the correct VLAN
>   * Cross-check VLAN assignments and trunk configurations
---
### Insights:
> * The accuracy and reliability of DAI depend on a correctly configured DHCP snooping database. Ensure both features are consistently maintained.
> * When properly implemented, DAI operates transparently. However, misconfiguration can lead to dropped traffic and connectivity issues, so testing changes in a lab environment is recommended before deployment in production
> * DAI should be part of a layered security strategy that includes features like port security, IP source guard, and network segmentation to provide comprehensive protection
---
Commands:
> *
---
