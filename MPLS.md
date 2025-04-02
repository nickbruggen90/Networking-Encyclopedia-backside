# MPLS - RFC 3031 (MPLS Architecture), RFC 3032 (MPLS Label Stack Encoding), RFC 3209 (RSVP-TE), RFC 5036 (LDP Version 1)

### Overview:
> * MPLS is a high performance, scalable technique used in modern networks to speed up and shape the flow of traffic.
> * MPLS works by attaching short, fixed-length labels to packers and using these labels to make forwarding decisions - bypassing the more complex lookups typically associated with IP routing; this label based forwarding mechanism improves efficiency, supports traffic engineering and enables the creation of VPNs
---
### Timers:
> * LDP – Hello Timer = 30 sec || Hold Timer = 90 sec
> * LSP/RSVP-TE  – Refresh Interval = 30 sec || State Lifetime (hold timer) = 90 sec
---
### Multicast:
> * LDP – IPv4: 224.0.0.2 (all routers group)
> * LDP - IPv6: FF02::2 (link-local all-routers)
---
### Protocol/Port(s): 
> * TCP/UDP - 646
---
### Priority Levels:
> * TC Field in Header: 0 - 7
---
### Terminology/Definitions:
> * Label – a short, fixed-length identifier (typically 20 bits) that determines the forwarding equivalence class (FEC) for a packet
> * FEC – a group of packets that are forwarded in the same manner (via the same path) through the MPLS network
> * Label Edge Router (LER) – an MPLS router that operates at the edge of the MPLS network. Ingress LERs add labels to incoming packets, while egress LERs remove the labels before forwarding packets to their final destination
> * Label Switch Router (LSR) – a router within the MPLS network that forwards packets solely based on the label without inspecting the packets IP header
> * Label Distribution Protocol (LDP) – a protocol used to distribute labels between MPLS-enabled routers. It establishes label bindings for FECs across the MPLS domain. LDP is the most common method for distributing label bindings. It allows routers to advertise FEC-to-label mappings to their neighbors, building a consistent forwarding table across the MPLS network
> * Resource Reservation Protocol - Traffic Engineering (RSVP-TE) – an extension of RSVP that can be used for label distribution with traffic-engineered paths. In networks requiring traffic engineering, RSVP-TE can be used to establish LSPs with explicit route control, ensuring that traffic follows a predetermined path
> * Label Stack – MPLS packets may carry multiple labels (a label stack), which support hierarchical routing and enabled advanced features such as VPNs
> * RD – route distinguisher; uniquely identifies a route (or route instance) in the MPLS VPN to avoid conflicts from overlapping address space
> * RT – route target; acts as a policy tool to control the import and export of VPN routes between VRFs, effectively determining which routes are shared across different VPNs
> * MPLS VPN – a VPN service built on MPLS, allowing service providers to segregate customer traffic using VRF instances and MPLS labels
---
### Header Breakdown: 32 bits
> * 20 - Label – identifies the FEC for the packet
> * 3 - Experimental or TC (Traffic Class) – priority levels 0-7
> * 1 - Bottom of Stack(s) – S = 0: there is at least one additional label below this one
>   * S = 1: this is the last label
> * 8 - TTL
---
### Header Definitions:
> * Label – routers use this value to determine the next hop without needed to inspect the full IP header
> * Experimental or TC – these values can be used to indicate priority levels or to carry DiffServ information. Critical for QoS in MPLS networks; 0-7 priority levels
> * Bottom of Stack(s) – indicates whether the current label is the bottom (last) label in the label stack
---
### Key Benefits:
> * Fast Packet Forwarding – MPLS used fixed-length labels for forwarding, allowing routers to quickly switch packets without performing complex IP header lookups
> * Traffic Engineering – with protocols such as RSVP-TE, MPLS enables administrators to define explicit paths through the network, optimizing bandwidth usage and ensuring that high priority traffic receives preferential treatment
> * Scalability – MPLS scales effectively across large multi-domain networks. Its label-based switching model supports thousands of VPNs and numerous routing paths without excessive overhead
> * QoS – the MPLS header’s 3-bit TC field enables the mapping of DSCP values, allowing for differentiated service treatment. This helps maintain performance for latency-sensitive applications
> * VPN Support – MPLS underpins MPLS VPN by combining VRF instances and label switching. This allows multiple customers or departments to share a common infrastructure securely and independently
> * Integration with Latency Networks – MPLS can run alongside traditional IP routing and switching, enabling gradual network upgrades and coexistence with legacy systems
> * Simplified Network Management – by abstracting the underlying IP routing and using labels, MPLS simplifies the management of complex networks and provides a more predictable traffic path
---
### Use Cases:
> * Enterprise Networks –
>   * Internal Segmentation - enterprises can use MPLS VPN to segment and isolate traffic between different business units or geographical locations without requiring separate physical infrastructures
>   * QoS - MPLS traffic engineering helps maintain performance for critical applications, ensuring that voice and video traffic experience minimal delay and packet loss
> * Service Provider Networks –
>   * MPLS VPN Services - service providers rely on MPLS to offer VPN services to multiple customers over a shared backbone while maintaining strict isolation between customer networks
>   * Traffic Engineering - providers use MPLS TE to optimize network paths, balance load and provide backup routes in the event of link failures
>   * Scalability and Efficiency - MPLS allows service providers to scale their networks efficiently, supporting large numbers of subscribers and high-volume traffic flows
> * Data Center Interconnects –
>   * Efficient Interconnection - MPLS can be used to connect data centers, ensuring high performance and reliable transport of large volumes of data with predictable latency
>   * Virtualized Environments - MPLS combined with VRF and MPLS VPN supports multi-tenant data center architecture, facilitating secure and isolated virtual environments
> * Mobile and Converged Networks –
>   * Integrated Services - MPLS plays a role in converged networks, allowing for the integration of voice, video and data services across a single transport mechanism, often within the carrier and mobile networks
---
### Architecture and Operation:
> * Ingress (Label Imposition) – the ingress LER examines the incoming packet and assigns it a label based on its FEC. The label is pushed onto the packets label stack
> * Label Switching – intermediate LSRs use the top label to determine the packets next hop. They swap, push or pop labels as necessary based on pre-established label bindings
> * Egress (Label Disposition) – the egress LER removes the label from the packets (label popping) and forwards the packet to its final destination using traditional IP routing
---
### Best Practices/Security Considerations:
> * Ensure proper authentication and integrity checks on protocols like LDP to prevent malicious label injection or spoofing
> * Use features such as TCP MD5 signatures on LDP sessions to protect label distribution from tampering
> * Restrict management access to MPLS routers using ACLs, secure management protocols and network segmentation
> * For MPLS VPN, consider using IPsec to encrypt data traversing MPLS networks, particularly if the infrastructure extends over untrusted links
> * Implement robust ACLs and route policies to prevent unauthorized access to MPLS control traffic and management interfaces
> * Monitor MPLS forwarding tables, LDP sessions and associated logs regularly to detect anomalies or potential security breaches
> * Ensure physical security of devices and apply hardening best practices to reduce the risk of unauthorized configuration changes
> * Clearly define network segments, service requirements and scalability needs
> * Use consistent LDP or RSVP-TE configurations across all MPLS devices
> * Utilize verification commands and network monitoring tools to maintain MPLS health
> * Combine MPLS with IPsec or other security protocols to protect data on untrusted networks
> * Keep thorough documentation of MPLS settings, LDP configurations and VRF instances for troubleshooting and upgrades
---
### Common Issues and Fixes:
> * Ensure MPLS is enabled on the relevant interfaces
> * Confirm that LDP is running and that LDP neighbor relationships are established
> * Verify there are no ACLs or firewall rules blocking TCP/UDP port 646
> * Check that the route-id is correctly configured and unique
> * Confirm VRF configurations, RD and RT values are consistent
> * MPLS Lables Not Being Assigned or Advertised?
>   * Cause: LDP not enabled on the interface or incorrect MPLS configuration
>   * Fix?: ensure mpls ip (or mpls ldp in new IOS) is applied to all relevant interfaces; verify mpls ldp router-id is set and reachable
> * No LDP Neighbor Relationship?
>   * Cause: IP connectivity or IGP reachability issue between LSR
>   * Fix?: use ping, traceroute and show mpls ldp neighbor; confirm loopback-to-loopback reachability and matching LDP router-IDs
> * Traffic Not Using The MPLS Path?
>   * Cause: IGP or MPLS label-switched path (LSP) not set up correctly
>   * Fix?: verify IGP (OSPF/EIGRP/IS-IS) is working correctly; use show ip route, show mpls forwarding-table
> * Lables Not Showing In show mpls forwarding-table?
>   * Cause: prefixes not being advertised or no IGP route to destination
>   * Fix?: check routing table and confirm prefixes are received; ensure labels are being advertised with show mpls ldp bindings
> * Penultimate Hop Doesn’t Pop The Label (PHP not working)?
>   * Cause: PHP disabled or explicit null configured incorrectly
>   * Fix?: review LDP configuration; by default, PHP is enabled, unless mpls ldp explicit-null is configured
> * VPNv4 Routes Not Appearing on Remote PE?
>   * Cause: missing or misconfigured MP-BGP session between PEs
>   * Fix?: ensure BGP neighbor x.x.x.x activate is applied under address-family vpnv4; verify route targets and RD settings match
> * Packets Dropped After Label Pop?
>   * Cause: missing route to the VPN prefix in the RIB/FIB
>   * Fix?: check BGP table and show ip route vrf XYZ; ensure next-hop resolution and label stack are intact
> * MTU Issues with MPLS Packets?
>   * Cause: MPLS adds 4 bytes per label; intermediate links may drop oversized frames
>   * Fix?: ensure MTU is increased on MPLS-facing interfaces (mtu 1504 or higher); verify with show interfaces
> * Incorrect Label Stack Depth?
>   * Cause: improper route leaking or PE misconfiguration
>   * Fix?: use show bpg vpnv4 unicast all and show mpls forwarding-table; verify correct route-target import/export and PE-CE routing
---
### Insights:
> * MPLS improves packet forwarding by simplifying lookups using labels
> * It supports multiple services (VPNs, TE) over a single infrastructure
> * Combining MPLS with VRF enables secure, segregated VPN services
> * Consistent configurations and regular monitoring are crucial to avoid misconfigurations that could lead to performance issues
> * While MPLS itself doesn’t encrypt data, integrating additional security measures (IPsec, etc) and enforcing strict control over management and label distribution can secure your MPLS environments
---
### Commands:
> * 
---



