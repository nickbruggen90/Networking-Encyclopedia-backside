# DHCP -  RFC 2131 (IPv4), RFC 3315 (IPv6), RFC 2132 (options and message fields)

### Overview:
> * Automatically assigns IP addresses and other network configuration parameters (subnet mask, default gateway, DNS server) to a device on a network
> * DHCPv6 if often used with SLAAC or can do stateful (like DHCPv4) or even stateless DHCPv6 which will provide DNS but let SLAAC handle IP addressing
---
### Timers:
> * Lease Time – the duration for which an IP address is assigned to a client
> * Renewal Time (T1) – typically set to 50% of the lease time. At this point, the DHCP client attempts to renew its lease with the DHCP server directly
> * Rebinding Time (T2) – usually set to 87.5% of the lease time. If the renewal fails, the client will try to contact any available DHCP server to renew the lease
> * Discovery and Offer Timeouts – not standardized; DHCP clients generally wait a few seconds (often 4 - 8 seconds) for an offer after sending a Discover message before retrying
---
### Multicast:
> * DHCP Discover – DHCP clients initially use broadcast (MAC FF:FF:FF:FF:FF:FF and IP 255.255.255.255) to locate a DHCP server, as they do not yet have an IP address
> * DHCP Offer, Request, Acknowledge – these messages are usually still broadcast on the local subnet until the client has an assigned IP address
> * Multicast Considerations – although DHCP primarily uses broadcast, some advanced implementations may use multicast addressing internally within a DHCP server or in some IPv6 DHCPv6 deployments. However, in IPv4, DHCP relies on broadcast because clients initially have no IP address to send unicast messages
---
### Protocols/Port(s):
> * Server – UDP 67
> * Client – UDP 68
---
### Priorities:
> * DHCP Message Prioritization – in most networks, DHCP messages are treated as high-priority broadcast traffic during the initial configuration phase
> * QoS – DHCP traffic typically uses default DSCP values (often CS0, meaning best effort) but in some environments, administrators may choose to mark DHCP messages with a higher DSCP value to ensure timely address configuration, especially in congested networks
> * Operational Priority – since DHCP is critical for network access, ensuring its reliability and prompt processing is important. Some networks might implement QoS policies to prioritize DHCP packets to avoid delays during the address assignment process
---
### Terminology/Definitions:
> * DHCP – a network management protocol used to dynamically assign IP addresses and other network configuration parameters (such as subnet mask, default gateway, DNS servers) to devices on a network
> * DHCP Server – the device (or service) that holds a pool of IP addresses and configuration settings. It leases these addresses to DHCP clients for a define period
> * DHCP Client – any network device (PC, printer, smartphone, etc) that requests network configuration information from a DHCP server
> * DHCP Relay Agent – any network device (usually a router) that forwards DHCP messages between clients and servers when they are not on the same local subnet. It helps extend DHCP functionality across different subnet
> * Lease – a temporary allocation of an IP address to a DHCP client. The lease has a defined duration, after which the client must renew it
> * DHCP Options – additional parameters provided by the DHCP server (domain name, NTP server, boot file names) using option fields
---
> *DHCP Snooping* is a LAN switch feature that prevents untrusted ports from injecting DHCP offers. The switch builds a table of MAC-to-IP bindings from DHCP traffic on trusted interfaces. It blocks rogue DHCP servers on untrusted ports. Commonly used with DAI to prevent ARP spoofing.
---
### DHCP Process:
> * D - Discover – client broadcasts a DHCPDISCOVER
> * O - Offer – one or more DHCP servers respond with DHCPOFFER; contains an available IP address, subnet mask, lease duration and other requested options
> * R - Request – the client chooses one offer and broadcasts a DHCPREQUEST stating which servers offer it is accepting, plus a request for the IP address
> * A - Acknowledge – the chosen server finalizes the process with DHCPACK; alternatively the server may respond with DHCPNAK if the offered address is no longer valid or a conflicting issue arises
___
### DHCP Message Types:
> * DHCPDISCOVER (client) → server
> * DHCPOFFER (server) → broadcast or unicast
> * DHCPREQUEST (client) → broadcast
> * DHCPACK (server) → broadcast or unicast
> * DHCPNAK (server) → broadcast if something fails
> * DHCPDECLINE (client) → broadcast if it detects an address conflict
> * DHCPRELEASE (client) → server, releasing the IP
> * DHCPINFORM (client) → server if only needs extra config data but not an address
---
### DHCP Options:
> * Option 1 = subnet mask
> * Option 3 = default gateway (router)
> * Option 6 = DNS server(s)
> * Option 12 = clients hostname
> * Option 15 = domain name
> * Option 23 = default TTL for IP packet
> * Option 24 = timeout for discovering path MTU
> * Option 25 = list of MTU sizes to use in path MTU discovery
> * Option 26 = specifics the MTU of the interface
> * Option 28 = broadcast address on the subnet
> * Option 42 = NTP server IPs
> * Option 50 = IP the client requests (used in DHCPREQUEST)
> * Option 51 = IP address lease time in seconds
> * Option 53 = DHCP message type
> * Option 54 = server identifier
> * Option 56 = text message from the server to client
> * Option 57 = max size of a DHCP message a client can accept
> * Option 66 = TFTP server name (commonly used for IP phone or device provisioning)
> * Option 67 = bootfile name (PXE boot or phone configuration)
> * Option 69 = SMTP server
> * Option 70 = POP3 server
> * Option 71 = NNTP server
> * Option 72 = WWW server
> * Option 73 = finger server
> * Option 74 = IRC server
> * Option 81 = client FQDN
> * Option 90 = LDAP server
> * Option 150 = TFTP server (Cisco)
---
### Key Benefits:
> * Simplified Network Management – DHCP automates the assignments of IP addresses and configuration parameters, reducing the need for manual configuration and minimizes errors
> * Efficient Resource Utilization – dynamic allocation of addresses ensures that IP addresses are reused efficiently, which is particularly important in large or dynamic networks
> * Centralized Control – DHCP allows network administrators to maintain centralized control over IP address allocation, simplifying management and troubleshooting
> * Scalability – DHCP is scalable to support large networks and can be integrated with additional protocols (like DHCP Snooping) to enhance security in high-density environments
> * Reduced Administrative Overhead – automation of IP configuration and renewal processes frees up administrative resources to focus on other tasks
---
### Use Cases:
> * Enterprise Networks – automatically assign IP addresses to a large number of devices, ensuring consistent configuration across the network
> * Guest Networks – provide temporary IP addressing for guest users while enforcing restrictions using DHCP Snooping and VLAN segmentation
> * Wireless Networks – in conjunction with 802.1X and DHCP Snooping, secure wireless access and prevent unauthorized devices from obtaining network configurations
> * Data Centers and Cloud Environments – manage IP address allocation for dynamic workloads and virtual machines, enabling efficient resource utilization and simplified management
> * Use DHCP to provision customer devices, with tight controls to prevent unauthorized or rogue devices from joining the networks
---
### Best Practices/Security Considerations:
> * Keep well-defined subnets, ensuring enough address. Plan your address space wisely.
> * Reserve your gateway IP, network devices and any static hosts; use exclusion
> * Enable DHCP Snooping to block rogue devices
> * Use Option 6 to provide a correct DNS server address
> * Ensure DHCP Snooping is enabled on all access ports and VLANs. This prevents unauthorized DHCP servers from assigning IP addresses and injecting malicious configurations
> * Configure trusted ports only where legitimate DHCP servers are connected. All user-facing ports should be untrusted
> * DHCP over IPv4 is not encrypted by default, use DHCPv6 with authentication where available. For IPv4, secure the management plane to prevent interception of modification of DHCP messages
> * Implement rate limiting on DHCP traffic to minimize the impact of potential DoS attacks
> * Regularly monitor DHCP logs and the DHCP Snooping binding table to detect unusual patterns that could indicate an attack
> * Use network segmentation and ACLs to restrict DHCP traffic to authorized areas only
> * Ensure that DHCP clients receive addresses from a controlled pool. Integrate DHCP Snooping with IP Source Guard to validate that devices use the correct IP addresses associated with their MAC addresses
---
### Troubleshooting:
> * No DHCP Response/IP Address Not Assigned?
>   * Verify that the client is physically connected and the interface is enabled
> * Authentication or Filtering Problems?
>   * Verify that ports connected to legitimate DHCP servers are marked as trusted
>   * Confirm that untrusted ports are correctly configured to allow client DHCP messages
> * Monitor DHCP traffic and ensure that rate limiting is not too restrictive
> * Check Syslog messages for any DHCP-related errors or warnings
> * Ensure SNMP traps are properly configured to alert you to DHCP server issues
> * Ensure DHCP Snooping is correctly configured on the VLAN. If misconfigured, valid DHCP offers might be dropped
> * Confirm that the client can reach the DHCP server. Verify that any DHCP relay (helper address) configurations are correct
> * Check the lease time settings on the DHCP server. If the lease time is too short, frequent renewals may cause connectivity issues
> * Ensure there are enough available addresses in the DHCP pool. If the pool is exhausted, new devices cannot obtain an IP
---
### Insights:
> * Excluded ranges can overlap and inadvertently reduce your pool size to zero
> * No IP helps or a wrong IP helper leads to clients not receiving addresses
> * Ensure NTP is set, especially if the DHCP server provides option 42 (NTP server)
---
### Commands:
> *
---
