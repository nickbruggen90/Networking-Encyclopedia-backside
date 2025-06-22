# DHCP Snooping

## Overview
• DHCP Snooping is a security feature that helps protect against rogue DHCP servers and other DHCP-based attacks  
• By monitoring DHCP messages exchanged between clients and servers, DHCP Snooping builds a binding table (a database of IP-to-MAC mappings) and enforces policies based on trusted and untrusted interfaces  
• This binding table is also used by other security features like DAI to validate network traffic  
• DHCP Snooping is a key component in a multi-layered network security strategy. It not only prevents rogue DHCP servers but also provides vital information for subsequent security features

---

## Timers
• **DHCP Snooping binding entries can age out** if no DHCP renewal occurs  
• **Aging timer** is typically based on the lease duration configured on the DHCP server  
• **Configurable aging** may be available on a per-VLAN or global basis (platform dependent)

---

## Multicast/Broadcast
• **DHCP Discovery and Offer Messages** are typically sent as broadcast messages because the client does not yet have an IP address  
• **DHCP Snooping is focused on broadcast traffic** and does not inherently affect multicast traffic  
• **Indirect multicast support** → By securing DHCP, it indirectly supports overall network stability and proper network segmentation

---

## Protocols/Ports
• **Operates at Layer 2** → Intercepts DHCP messages (UDP 67/68) at the switching level  
• **No specific port requirements** → Works by monitoring existing DHCP traffic flow

---

## Priorities
• **Default handling** → DHCP messages are typically broadcast and handled on a best-effort basis  
• **QoS considerations** → In some implementations, QoS may prioritize DHCP traffic for faster processing

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **DHCP** | A protocol used to automatically assign IP addresses and other network configuration parameters to devices on a network |
| **DHCP Snooping** | A security feature that intercepts DHCP messages on untrusted interfaces, filters out unauthorized DHCP responses, and builds a binding table of valid client information |
| **Trusted Port** | A switch port configured to allow DHCP server responses and other DHCP messages from known, authorized sources (ports connected to legitimate DHCP servers or uplinks) |
| **Untrusted Port** | A port where DHCP clients reside. DHCP responses on these ports are subject to verification and filtering |
| **Binding Table** | A database maintained by the switch that maps client MAC addresses, IP addresses, VLAN IDs and interface information learned from DHCP transactions |
| **Rogue DHCP Server** | An unauthorized DHCP server that may provide incorrect network configurations or perform man-in-the-middle attacks |

---

## DHCP Snooping Process

### Processing Steps

| Step | Component | Description |
|------|-----------|-------------|
| **1. Message Interception** | Traffic Monitoring | The switch intercepts all DHCP messages on untrusted ports |
| | | DHCP request messages (from clients) and DHCP reply messages (from servers) are examined |
| **2. Validation and Filtering** | Security Enforcement | DHCP replies from untrusted ports are dropped to prevent unauthorized IP address assignment |
| | | Only DHCP server responses from trusted ports are accepted |
| **3. Binding Table Creation** | Database Building | When a valid DHCP transaction occurs, the switch records client information |
| | | Records: MAC address, assigned IP address, VLAN, and associated port |
| **4. Integration with Other Features** | Security Ecosystem | The binding table is used by features like DAI to validate ARP packets |
| | | May also be referenced by IP Source Guard for further port security |

### Port Behavior

| Port Type | DHCP Client Messages | DHCP Server Messages | Action |
|-----------|---------------------|----------------------|--------|
| **Trusted Port** | Allowed | Allowed | All DHCP traffic passes through, populates binding table |
| **Untrusted Port** | Allowed | **Blocked** | Client requests pass, server responses are dropped |

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Enhanced Security** | Prevents rogue DHCP servers from providing malicious configurations to clients. Blocks unauthorized devices from receiving IP addresses, reducing the risk of IP spoofing |
| **Accurate Client Binding** | Builds and maintains an accurate binding table (IP, MAC, VLAN, port), which is critical for other security features such as DAI |
| **Network Stability** | By filtering DHCP traffic, it reduces the risk of DHCP-based attacks that can lead to network instability or DoS |
| **Audit and Accountability** | Provides logs and binding information that can be used for auditing network access and troubleshooting issues related to IP address assignment |

---

## Use Cases

| Environment | Application | Security Benefits |
|-------------|-------------|-------------------|
| **Enterprise Networks** | Securing access in campus or corporate networks | Ensures only authorized DHCP servers provide IP configurations, protecting from unauthorized access |
| **Wireless Networks** | Often used with 802.1X in wireless LAN environments | Secures client connections and prevents rogue DHCP servers from assigning IP addresses to unauthorized devices |
| **Data Centers** | Dynamic IP assignments in large, high-density environments | Ensures secure IP assignments and accurate binding information for further security controls |
| **Service Providers** | Protecting subscriber networks | Ensures customer devices receive proper IP configurations and prevents cross-customer DHCP spoofing |
| **Government and High-Security Environments** | Critical security control and monitoring | Strict network access control with tight integration with other security features |

---

## Best Practices/Security Considerations

### Port Configuration
• **Only mark ports connected to legitimate DHCP servers or uplinks as trusted**  
• **Correctly marking ports** as trusted (for uplinks and known DHCP servers) or untrusted (for client access) is critical

### Traffic Control
• **Configure appropriate rate limits** on untrusted ports to mitigate DHCP-based DoS attacks  
• **Enable logging of DHCP Snooping events** to monitor for suspicious activity

### Monitoring and Maintenance
• **Periodically check the DHCP Snooping binding table** and logs for anomalies or unauthorized devices  
• **Monitor for repeated DHCP request failures** or the appearance of unexpected DHCP servers

### Integration Strategy
• **Leverage DHCP Snooping as the foundation** for other security measures like DAI and IP Source Guard  
• **Maintain clear documentation** of DHCP Snooping configurations, especially when changes occur in network topology

### Security Monitoring
• **Review binding table regularly** for unexpected entries  
• **Investigate any untrusted port** that appears to be providing DHCP responses

---

## Troubleshooting

### Common Issues and Solutions

| Issue | Potential Causes | Troubleshooting Steps |
|-------|------------------|----------------------|
| **Binding Table is Empty or Incomplete** | DHCP snooping not enabled on correct VLANs | Confirm that DHCP snooping is enabled on the correct VLANs |
| | Trusted ports not configured | Verify that ports carrying legitimate DHCP server traffic are marked as trusted |
| **Legitimate DHCP Messages Being Dropped** | Rate limits too restrictive | Check the rate limit settings on untrusted ports |
| | DHCP server on untrusted port | Ensure that the DHCP server is connected to a trusted port |
| **Rogue DHCP Server Suspected** | Unauthorized DHCP server active | Use the binding table and logs to identify unexpected DHCP server responses |
| | | Investigate and isolate any untrusted port providing DHCP responses |

### Detailed Troubleshooting Steps

#### Configuration Verification
• **Verify trusted port configuration** → Ensure all ports connected to DHCP servers or upstream switches are correctly configured as trusted  
• **Check VLAN configuration** → Ensure clients are successfully obtaining IP addresses from the authorized DHCP server  
• **Review connectivity issues** → Look for problems such as connectivity or misconfigured VLANs that might prevent DHCP transactions

#### Rate Limiting and Traffic Issues
• **Review rate limit configuration** → Adjust if the network experiences a high volume of DHCP traffic  
• **Monitor for DoS scenarios** → If the network is flooded with DHCP requests, ensure rate limiting is properly configured on untrusted ports

#### Security Validation
• **Verify port trust settings** → Ensure ports connected to legitimate DHCP servers are configured as trusted; untrusted ports should receive client messages  
• **Check VLAN enablement** → Verify that DHCP Snooping is enabled on the correct VLANs  
• **Review ACL interference** → Confirm that ACLs or other security settings are not inadvertently blocking DHCP traffic

#### Binding Table and Integration
• **Review binding table accuracy** → Ensure it only contains expected entries  
• **Verify timing** → Ensure DHCP Snooping is enabled before any DHCP messages are exchanged  
• **Check dependent features** → Ensure binding table accuracy for DAI and IP Source Guard integration

#### Performance and Logging
• **Use logging for violation detection** → Identify whether frequent DHCP Snooping violations are causing performance issues  
• **Verify feature alignment** → Check configurations of all related features to ensure they are aligned

---

## Insights

### Operational Transparency
• **When correctly configured, DHCP Snooping works in the background** without affecting normal client operations, yet it provides critical security benefits  
• **Seamless operation** → Properly implemented DHCP Snooping is invisible to end users while providing robust security

### Enterprise Implementation
• **In enterprise networks, ensuring that clients receive correct IP configurations is essential** → DHCP Snooping helps maintain integrity of this process and aids in rapid troubleshooting during network issues  
• **Foundation for layered security** → DHCP Snooping provides the binding database that enables other L2 security features

### Integration Considerations
• **Dependency relationship** → Many Layer 2 security features (DAI, IP Source Guard) depend on the DHCP Snooping binding table  
• **Timing is critical** → DHCP Snooping must be enabled and operational before dependent security features are activated

---

## Commands
*[To be completed during lab sessions]*
