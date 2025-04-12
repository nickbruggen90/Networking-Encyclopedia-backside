VXLAN - RFC 7348

### Overview:
> * VXLAN is an overlay networking technology designed to address scalability and segmentation challenges in large data center or cloud environments
> * VXLAN encapsulates L3 Ethernet frames with UDP packets, allowing the extension of L2 networks over a L3 infrastructure. This enables the creation of logical (virtual) networks that can span across data centers and remote locations
> * Integrates with modern automation and SDN frameworks
--
### Protocol/Port(s): 
> * UDP - 4789 (destination port)
---
### Priorities:
> * VXLAN does not define its own priority field; but DSCP/CoS values can be applied to the outer UDP/IP headers for underlay QoS. Proper mapping should be configured to maintain service class guarantees for overlay traffic.
---
### Terminology/Definitions:
> * VXLAN – an overlay protocol that encapsulates Ethernet frames in UDP packets to extend L2 networks over a L3 infrastructure
> * VNI – a 24-bit identifier used to uniquely define a VXLAN segment or virtual network, supporting up to 16 million VNIs
> * VTEP – the device (switch, router or hypervisor) that encapsulates and decapsulates VXLAN traffic. VTEPs perform the mapping between traditional MAC addresses and VNIs
> * BGP EVPN – a control plane mechanism often used within VXLAN to distribute MAC and IP address reachability information, enabling dynamic VTEP mapping and improved scalability
> * NVE – network virtualization edge; Cisco term for logical interface on a VTEP that handles encapsulation/decapsulation of VXLAN packets
> * Type-5 Route – EVPN route types used to carry IP prefix reachability info in BGP EVPN (important in routed VXLAN networks)
> * Type-2 Route – EVPN route type used to carry MAC and IP address bindings for hosts/endpoints
---
### Architecture/Conceptual Overview:
> * VTEP Functionality –
>   * Encapsulating: Ethernet frames from local VXLAN segment by adding a VXLAN header, UDP header and IP header
>   * Decapsulation: incoming VXLAN packets to extract the original Ethernet frame and deliver it to the appropriate endpoint
> * Underlay Network – the underlay is typically a standard IP network that provides connectivity between VTEPs
> * Control Plane (optional) –
>   * Flood-and-Learn: in basic VXLAN deployments, a data plane mechanism (flood-and-learn) is used to build MAC address tables
>   * BGP EVPN: for scalable and efficient environments, VXLAN often uses BGP EVPN as the control plane to advertise MAC/IP/VTEP mappings and avoid unnecessary flooding
---
### Header Breakdown: 
```
+----------------------------+
| Outer Ethernet          |
| Outer IP Header        |  → Source = local VTEP | Destination = remote VTEP
| UDP Header              |  → Destination Port = 4789
| VXLAN Header (8B)  |
| Original Ethernet        |
+---------------------------+
```
> * VXLAN encapsulation adds several layers to the original Ethernet frame –
>   * Original Ethernet frame
>   * 8 - VXLAN Header
>   * 8 - UDP Header
>   * 20 (IPv4), 40 (IPv6) - Outer IP Header
>   * Outer Ethernet header
>   * 8 - Flags – Bit 0: l flag must be set to indicate the presence of a valid VNI
>   * 24 - Reserved – must be zero
>   * 24 - VNI – uniquely identifies the VXLAN segment
>   * 8 - Reserved - must be zero
---
### Key Benefits:
> * Scalability – supports up to 16 million logical networks through VNIs, overcoming VLAN limitations; compared to the 4096 VLAN limit
> * Network Segmentation – provides flexible segmentation for multi-tenant and data center environments
> * Data Center Interconnect – enables the extension of L2 domains over L3 infrastructure
> * Control Plane Efficiency – BGP EVPN enhances scalability by eliminating the need for flood-to-learn in large environments
> * Operational Simplicity – leverages existing IP networks for overlay transport and integrates with automation tools
---
### Use Cases:
> * Data Center Fabric – VXLAN is widely deployed in data centers to support large-scale, multi-tenant environments.
> * Cloud Integration – connects virtualized workloads across geographically dispersed data centers
> * Network Virtualization – allows service providers and enterprises to offer isolated virtual networks over a common physical infrastructure
> * Multi-Tenant Environments – provides secure segmentation and isolation for different customers or business units
---
### Best Practices/Security Considerations:
> * Ensure that VNIs are uniformly configured across all VTEPs
> * Optimize the underlay network for low latency and high reliability, as it directly impacts VXLAN performance
> * Secure both the control and data planes using proper authentication, encryption and ACLs
> * Regularly monitor NVE interfaces, BGP EVPN sessions and VXLAN forwarding tables. Use built-in verification commands and logging to identify issues early
> * Maintain thorough documentation of VXLAN configuration and consider automating deployment and monitoring with tools like Cisco DNA Center or custom scripts
> * VXLAN encapsulation does not inherently encrypt traffic. To secure sensitive data integrate IPsec or MACsec on the underlay network
> * Protect BGP EVPN sessions using appropriate authentication (MD5 or TCP-AO)
> * Implement route filtering to ensure only authorized VTEP information is accepted
> * Limit access to the NVE interfaces and VTEP configurations using ACLs and secure management practices
> * Ensure multicast groups used for VXLAN flooding are restricted to authorized devices and segments of the network
> * Monitor VXLAN and BGP EVPN logs for anomalies
> * Use network management tools to continuously assess the security posture of both the overlay and underlay
---
### Common Issues and Fixes:
> * NVE Interface Down?
>   * Fix: Verify the source interface (typically a loopback) is up; confirm that the NVE interface is enabled
> * Missing VXLAN Forwarding Entries?
>   * Fix: Check if BGP EVPN is correctly distributing MAC/IP mappings; ensure that VNIs are correctly configured on all relevant VTEPs
> * Connectivity Issues?
>   * Fix: Verify the underlay network reachability between VTEPs; check UDP port 4789 is not blocked by firewalls
> * VXLAN Tunnel Not Forming?
>   * Cause?: VTEP source loopback not advertised in IGP/BGP
>   * Fix: ensure the loopback used for NVE source-interface is reachable via the underlay
> * BGP EVPN Failing?
>   * Cause?: ASN mismatch, route policies or missing route types
>   * Fix: use show bgp 12vpn evpn to validate advertised routes and peer status
> * MAC Address Duplication?
>   * Cause?: duplicate VM or host config
>   * Fix: check for VM mobility misconfigurations or MAC spoofing
> * Flooding Instead of Unicast?
>   * Cause?: BGP EVPN not configured or failing
>   * Fix: validate route exchange; fallback to flood-and-learn indicates a control plane issue
---
### Insights:
> * VXLAN has fundamentally changed data center design by overcoming VLAN limitations and enabling flexible, scalable network overlays
> * VXLAN integrates well with SDN frameworks, allowing for centralized policy management and automated provisioning
> * While early VXLAN deployments relied on flood-to-learn, modern implementations using BGP EVPN offer enhanced scalability and efficiency
> * As networks become more virtualized and cloud-driven, VXLAN continues to play a crucial role in bridging traditional and virtual network paradigms
---
### Commands:
> *
---
