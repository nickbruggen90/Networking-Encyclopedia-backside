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
> *DHCP Snooping* is a LAN switch feature that prevents untrusted ports from injecting DHCP offers. The switch builds a table of MAC-to-IP bindings from DHCP traffic on trusted interfaces. It blocks rogue DHCP servers on untrusted ports. Commonly used with DAI to prevent ARP spoofing.
