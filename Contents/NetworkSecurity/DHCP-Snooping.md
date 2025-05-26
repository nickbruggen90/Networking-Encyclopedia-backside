# DHCP Snooping

### Overview:
> * DHCP Snooping is a security feature that helps protect against rogue DHCP servers and other DHCP-based attacks
> * By monitoring DHCP messages exchanged between clients and servers, DHCP Snooping builds a binding table (a database of IP-to-MAC mappings) and enforces policies based on trusted and untrusted interfaces
> *This binding table is also used by other security features like DAI to validate network traffic
> * DHCP Snooping is a key component in a multi-layered network security strategy. It not only prevents rogue DHCP servers but also provides vital informations for subsequent security features
---
### Timers:
> DHCP Snooping binding entries can age out if no DHCP renewal occurs; the aging timer is typically based on the lease duration configured on the DHCP server (if the switch supports aging configurable on a per-VLAN or global basis)
---
### Multicast:
> * DHCP Discovery and Offer Messages are typically sent as a broadcast message because the client does not yet have an IP address
> * DHCP Snooping is focused on broadcast traffic. It does not inherently affect multicast traffic; however, by securing DHCP, it indirectly supports overall network stability and proper network segmentation
---
### Protocols/Port(s):
> *
---
### Priorities:
> In some implementations, QoS may prioritize DHCP traffic, but by default, DHCP messages are typically broadcast and handled on a best-effort basis
---
### Terminology/Definitions:
> * DHCP – a protocol used to automatically assign IP addresses and other network configuration parameters to devices on a network
> * DHCP Snooping – a security feature that intercepts DHCP messages on untrusted interfaces, filters out unauthorized DHCP responses, and builds a binding table of valid client information
> * Trusted Port – a switch port configured to allow DHCP server responses and other DHCP messages from known, authorized sources (ports connected to legitimate DHCP servers or uplinks)
> * Untrusted Port – a port where DHCP clients reside. DHCP responses on these ports are subject to verification and filtering
> * Binding Table – a database maintained by the switch that maps client MAC addresses, IP addresses, VLAN IDs and interface information learned from DHCP transactions
> * Rogue DHCP Server – an unauthorized DHCP server that may provide incorrect network configurations or perform man-in-the-middle attacks
---
### DHCP Snooping Process:
> * Message Interception –
>   * The switch intercepts all DHCP messages on untrusted ports
>   * DHCP request messages (from clients) and DHCP reply messages (from server) are examined
> * Validation and Filtering –
>   * DHCP replies from untrusted ports are dropped to prevent unauthorized IP address assignment
>   * Only DHCP server responses from trusted ports are accepted
> * Binding Table Creation –
>   * When a valid DHCP transaction occurs, the switch records the client’s MAC address, assigned IP address, VLAN and the associated port in the DHCP Snooping binding table
> * Integration with Other Security Features –
>   * The binding table is used by features like DAI to validate ARP packets
>   * It maye also be referenced by IP Source Guard for further port security
---
### Key Benefits:
> * Enhanced Security – prevents rogue DHCP servers from providing malicious configurations to clients. Blocks unauthorized devices from receiving IP addresses, reducing the risk of IP spoofing
> * Accurate Client Binding – builds and maintains an accurate binding table (IP, MAC, VLAN, port), which is critical for other security features such as DAI
> * Network Stability – by filtering DHCP traffic, it reduces the risk of DHCP-based attacks that can lead to network instability or DoS
> * Audit and Accountability – provides logs and binding information that can be used for auditing network access and troubleshooting issues related to IP address assignment
---
### Use Cases:
> * Enterprise Networks – securing access in campus or corporate networks by ensuring that only authorized DHCP servers provide IP configurations, protecting the network from unauthorized access
> * Wireless Networks – often used in wireless LAN environments (with 802.1X) to secure client connections and prevent rogue DHCP servers from assigning IP addresses to unauthorized devices
> * Data Centers – ensuring that dynamic IP assignments in large, high-density environments remain secure and that binding information is accurate for further security controls
> * Service Providers – protecting subscriber networks by ensuring that customer device receive proper IP configurations and preventing cross-customer DHCP spoofing or misconfigurations
> * Government and High-Security Environments – critical for environments where network access must be strictly controlled and monitored, with tight integration with other security features
---
### Best Practices/Security Considerations:
> * Only mark ports connected to legitimate DHCP servers or uplinks as trusted
> * Configure appropriate rate limits on untrusted ports to mitigate DHCP-based DoS attacks
> * Periodically check the DHCP Snooping binding table and logs for anomalies or unauthorized devices
> * Leverage DHCP Snooping as the foundation for other security measures like DAI and IP Source Guard
> * Maintain clear documentation of DHCP Snooping configurations, especially when changes occur in network topology
> * Correctly marking ports as trusted (for uplinks and known DHCP servers) or untrusted (for client access) is critical. Misconfiguration can either allow rogue DHCP activity or block legitimate traffic
> * Enable logging of DHCP Snooping events to monitor for suspicious activity, such as repeated DHCP request failures or the appearance of unexpected DHCP servers
---
### Troubleshooting:
> * Binding Table is Empty or Incomplete?
>   * Confirm that DHCP snooping is enabled on the correct VLANs
>   * Verify that ports carrying legitimate DHCP server traffic are marked as trusted
> * Legitimate DHCP Messages Being Dropped?
>   * Check the rate limit settings on untrusted ports to ensure they are not too restrictive
>   * Ensure that the DHCP server is connected to a trusted port
> * Rogue DHCP Server Suspected?
>   * Use the binding table and logs to identify unexpected DHCP server responses
>   * Investigate and isolate any untrusted port that appears to be providing DHCP responses
> * Verify that all ports are connected to DHCP servers or upstream switches are correctly configured as trusted
> * Ensure clients are successfully obtaining IP addresses from the authorized DHCP server
> * Look for issues such as connectivity problems or misconfigured VLANs that might prevent DHCP transactions from completing
> * Review and adjust the rate limit configuration if the network experiences a high volume of DHCP traffic
> * Esure that ports connected to legitimate DHCP servers are configured as trusted. Untrusted ports should receive client messages
> * Verify that DHCP Snooping is enabled on the correct VLANs and that there are no misconfigurations blocking DHCP messages
> * Confirm that ACLs or other security settings are not inadvertently blocking DHCP traffic
> * Review the DHCP Snooping binding table to ensure it only contains expected entries
> * Ensure that DHCP Snooping is enabled before any DHCP messages are exchanged, so that the binding table is correctly populated
> * If the network is flooded with DHCP requests (perhaps a DoS scenario), esure that rate limiting is properly configured on untrusted ports
> * Use logging to identify whether frequent DHCP Snooping violations are causing performance issues
> * Ensure that the binding table created by DHCP Snooping is accurate, as DAI and IP Source Guard rely on this data
> * If clients are experiencing issues even though DHCP seems to be working, check the configurations of all related features to ensure they are aligned
---
### Insights:
> * When correctly configured, DHCP Snooping works in the background without affecting normal client operations, yet it provides critical security benefits
> * In enterprise networks, ensuring that clients receive correct IP configurations is essential. DHCP Snooping helps maintain integrity of this process and aids in rapid troubleshooting during network issues.
---
### Commands:
> *
---
