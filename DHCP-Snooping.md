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
