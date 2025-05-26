# ICMP - RFC 792 (ICMPv4), RFC 4443 (ICMPv6)

### Overview:
> * ICMP is a L3 protocol used by network devices to send error messages and operational information, such as when a destination is unreachable or when a ping request is used to test reachability
> * Not used for data delivery, but assists with network diagnostics, control and path MTU discovery
> * Encapsulated directly inside IP (protocol number 1 for ICMPv4, and 58 for ICMPv6)
> * Connectionless and operates above IP, but below TCP/UDP
---
### Protocol/Port(s):
> * ICMPv4 - IP: 1
> * ICMPv6 - IP: 58
---
### Terminology/Definitions:
> * ICMP – a supporting protocol carried by IP used for diagnostics, control and error reporting
> * Type – a numeric value (8 bits) indicating the general purpose of the ICMP message
> * Code – a sub-classification of the Type field, offering additional detail
> * Checksum – a 16-bit field used to verify the integrity of the ICMP header and data
> * Payload – the data carried in an ICMP message
> * PMTUD – a process that relies on ICMP Type 3, Code 4 messages to discover the largest packet size that can traverse a path without fragmentation
> * Rate Limiting – routers may limit the number of ICMP messages sent per second to prevent misuse or DoS via ICMP
> * Glean Adjacency – occurs when the device does not a L2 address for the next hop, it uses ARP (or NS for IPv6) and may send ICMP errors if unresolved
> * Punt – a CEF action where a packet is handed off to the CPU for processing
---
### Type Vs Code:
> * Type defines the category of ICMP message
> * Code provides further detail or context within that message type - essentially a sub type
---
### ICMPv4 Types:
> * 0 - Echo Reply - echo reply to Type 8
> * 3 - Destination Unreachable
> * 5 - Redirect
> * 8 - Echo Request - standard ping request
> * 9 - Router Advertisement
> * 10 - Router Solicitation
> * 11 - Time Exceeded
> * 12 - Parameter Problem
> * 13 - Timestamp Request
> * 14 - Timestamp Reply
---
### ICMPv4 Codes:
> * Type 3 Codes -
>   * 0 = network unreachable
>   * 1 = host unreachable
>   * 2 = protocol unreachable
>   * 3 = port unreachable
>   * 4 = fragmentation needed; DF set (PMTUD)
>   * 5 = source route failed
>   * 9 = network administratively prohibited
>   * 10 = host administratively prohibited
>   * 13 = communication administratively prohibited
>   * 14 = host precedence violation
>   * 15 = precedence cutoff in effect
> * Type 5 Codes -
>   * 0 = redirect for datagram network
>   * 1 = redirect datagram for hosts
>   * 2 = redirect for TOS and network
>   * 3 = redirect for TOS and host
> * Type 11 Codes -
>   * 0 = TTL exceeded in transit (used in traceroute)
>   * 1 = fragment reassembly time exceeded
> * Type 12 Code -
>   * 0 = pointer indicates the error in the header
>   * 1 = missing required option
>   * 2 = bad length
---
### ICMPv6 Types:
> * 1 - Destination Unreachable
> * 2 - Packet Too Big
> * 3 - Time Exceeded
> * 4 - Parameter Problem
> * 128 - Echo Request - ping
> * 129 - Echo Reply - reply to type 128
> * 133 - Router Solicitation (NDP)
> * 134 - Router Advertisement (NDP)
> * 135 - Neighbor Solicitation (NDP) - similar to ARP request
> * 136 - Neighbor Advertisement (NDP) - similar to ARP reply
> * 137 - Redirect
---
### ICMPv6 Codes:
> * Type 1 Codes -
>   * 0 = no route to destination
>   * 1 = communication with destination admin. Prohibited
>   * 2 = beyond scope of source address
>   * 3 = address unreachable
>   * 4 = port unreachable
>   * 5 = source address failed ingress/egress policy
>   * 6 = reject route to destination
>   * 7 = error in source routing header
> * Type 3 Codes -
>   * 0 = hop limit exceeded in transit (TTL)
>   * 1 = fragment reassembly time exceeded
> * Type 4 Codes -
>   * 0 = erroneous header field encountered
>   * 1 = unrecognized next header type
>   * 2 = unrecognized IPv6 option encountered
---
### Behavior and Considerations:
> * ICMP is subject to rate-limiting or blocking by firewalls
> * Devices often ignore ICMP redirect message due to security concerns
> * Routers send ICMP Time Exceeded for TTL expiry (during Traceroute)
> * ICMP unreachable messages may be disabled on edge firewalls for security
> * Path MTU Discovery requires ICMP Fragmentation Needed (Type 3 Code 4)
---
### ICMPv4 Header Breakdown:
> * 8 - type –
> * 8 - code
> * 16 - checksum
> * 32 - rest of header
> * variable - payload
---
### ICMPv6 Header Differences:
> * Same structure, but expanded to support:
>   * Multicast messaging
>   * ND
>   * Mandatory use in IPv6 for call control-plane tasks
---
### Use Cases:
> * Ping to test device reachability
> * Traceroute uses TTL expiration
> * Path MTU Discovery relies on “Fragmentation Needed” messages
> * Some routing protocols use ICMP for adjacency
> * Error reporting - i.e.; destination unreachable, redirect
> * IPv6 NDP
---
### Best Practices/Security Considerations:
> * Allow ICMP selectively for tools like ping, traceroute and PMTUD
> * Use ICMP rate limiting on routers to prevent DoS vectors
> * Be cautious with ICMP redirects - often disabled for security
> * Monitor unusual ICMP patterns (large volume of unreachable or echo)
> * For IPv6, do not block ICMPv6, or features like SLAAC or NDP will break
> * Consider filtering unnecessary ICMP types at borders (echo on WAN)
> * Use ACLs or zone-based firewalls to scope allowed ICMP traffic
---
### Common Issues and Fixes:
> * Ping Fails but Routing is Fine?
>   * Cause?: ICMP Echo blocked
>   * Fix: check ACLs or firewalls
> * Traceroute Stops Mid-Path?
>   * Cause?: time exceeded or unreachable blocked
>   * Fix: enable ICMP Type 11 at intermediate devices
> * PMTUD Breaks Large File Transfer?
>   * Cause?: ICMP Fragmentation Needed blocked
>   * Fix: allow Type 3 Code 4
> * IPv6 Hosts Don’t Autoconfigure?
>   * Cause?: ICMPv6 Router Advertisement blocked
>   * Fix: allow ICMPv6 Types 133-137
> * Excessive ICMP Unreachable Messages?
>   * Cause?: routing loop or black hole
>   * Fix: verify routes and FIB entries
---
