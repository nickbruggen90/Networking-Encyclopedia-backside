# GRE/mGRE -  RFC 2784 (GRE), RFC 2890 (GRE extension for IPsec)

### Overview:
> * GRE is a tunneling protocol developed to encapsulate a wide variety of network layer protocols inside virtual point-to-point links over an IP network. GRE creates a virtual tunnel between two endpoints, allowing encapsulated packets to traverse intermediate networks without modification.
> * mGRE extends GRE by allowing a single GRE tunnel interface to support multiple endpoints. This is particularly useful in dynamic environments – such as DMVPN deployments – where multiple remote sites (spokes) can connect to a single hub without requiring an individual GRE tunnel for each remote site.
>   * GRE’s ability to encapsulate various protocols makes it a foundational technology for many overlay networks
>   * mGRE simplifies complex topologies by allowing dynamic tunnel endpoints, especially in DMVPN deployments
---
### Timers:
> * GRE itself is a stateless tunneling protocol and does not have inherent timers. However, if GRE is used as part of a larger solution like DMVPN, then related timers (NHRP, OMP keepalives) apply.
---
### Multicast:
> * GRE/mGRE can encapsulate multicast traffic. In DMVPN scenario with GRE, multicast traffic might be flooded or replicated using dynamic mechanisms (often leveraging unicast replication with NHRP) to optimize performance.
---
### Terminology/Definitions:
> * GRE – a tunneling protocol that encapsulates network layer packets in a GRE header, allowing them to be forwarded over an IP network
> * mGRE – an extension of GRE that supports multiple tunnel endpoints on a single interface, enabling dynamic peer-to-peer tunnel establishment
> * GRE Tunnel Endpoint – the router or device at either end of a GRE tunnel responsible for encapsulating/decapulsating packets
> * DMVPN – a popular application of mGRE that uses GRE encapsulation with additional protocols (like NHRP) to enable dynamic, scalable VPNs
---
### GRE Architecture: 
> * Standard GRE creates a virtual point-to-point link between two endpoints. A GRE tunnel encapsulates packets with additional GRE header plus an outer IP header for transmission.
---
### mGRE Architecture: 
> * mGRE extends GRE by allowing one tunnel interface to establish tunnels to multiple remote endpoints. This reduces configuration overhead and is essential for scalable overlay networks. In deployments such as DMVPN, mGRE works in tandem with protocols like NHRP to dynamically learn and map remote tunnel endpoints.
---
### GRE Operational Overview:
> 1. Encapsulation (ingress) – the ingress router receives a packet, encapsulates it with a GRE header and then adds an outer IP header with the tunnel endpoints IP addresses
> 2. Forwarding – the packet is routed through the underlay network based on the outer IP header
> 3. Decapsulation (egress) – the egress router removes the GRE and outer IP headers and forwards the original packet toward its destination
---
### mGRE Operational Overview: 
> * With mGRE, a single tunnel interface is configured to accept GRE encapsulated packets from multiple remote endpoints. The router uses additional mechanisms (like NHRP) to dynamically determine which remote endpoint to use for a given traffic flow.
---
### Connecting Two Private Networks Over The Internet:
> * Criteria:
>   * Tunnel source and destination must be specified as an IP address or interface number (but they cannot be identical)
>   * The physical interfaces (or loopback) must not be in the same subnet (could cause a loop)
>   * The IP address assigned to the tunnel interfaces must be in the same subnet (dynamic routing protocols expect adjacent interfaces to be on the same subnet)
>   * A route to the tunnel destination must exist on the local router
>   ** The interface state must be up/up

1. GRE tunnels encapsulate private IP packets within public IP headers. This allows two private networks to communicate over the public internet.  
2. The tunnel source and destination IP addresses must be publicly routable so that intermediate routers can deliver packets between two tunnel endpoints. Usually, these are the public IPs of the routers (like ISP-facing interfaces).  
3. GRE supports multicast, broadcast and non-IP protocols, which makes it ideal for routing protocols (like OSPF, EIGRP) to run across the tunnel.  
4. GRE does not provide encryption, authentication or integrity by itself. For secure transmission, GRE is often combined with IPsec (used in DMVPN Phase 3, for example).  
5. GRE uses IP protocol number 47 (not TCP or UDP) in the IP header. This is important for ACLs, NAT, and firewall configuration.  
6. GRE is stateless, there is no inherit keepalive, unless configured manually (or via extensions like NHRP in mGRE/DMVPN).
7. GRE adds overhead (24 bytes typically), so MSS/MTU tuning may be needed to avoid fragmentation over the Internet.  
8. Both ends need proper tunnel source and tunnel destination IP configured to bring up the tunnel.  
9. GRE can carry traffic between overlapping private subnets, but NAT or VRF separation may be required depending on topology.  
---
### Header Breakdown: 4 to 8 bytes
> * 16 - Flags and Versions
> * 16 - Protocol Type (0x0800 for IPv4, 0x86DD for IPv6)

#### Optional Fields = 	
> * 16 - Checksum - triggered by flag C
> * 16 - Reserved (for Checksum) - triggered by flag C
> * 32 - Key - triggered by flag K
> * 32 - Sequence Number - triggered by flag S
---
### Header Definitions:
> * Flags –
>   * C (Checksum Present): 1 bit
>   * R (Routing Present): 1 bit
>   * K (Key Present): 1 bit
>   * S (Sequence Number Present): 1 bit
>   * S (Strict Source Routing): 1 bit
>   * Recur (Recursion Control): 3 bits
>   * A (Acknowledgement Present): 1 bit
>   * Version (typically 0): 3 bits
> * Protocol Type – Indicates the type of payload encapsulated
> * Checksum – optional checksum of the GRE payload
> * Key – key used to identify the GRE tunnel
> * Sequence Number – indicates ordering of GRE packets
> * *mGRE uses the same header format; however, mGRE’s key may be used to differentiate among multiple tunnel endpoints*
---
### Key Benefits:
> * Protocol Agnosticism – GRE can encapsulate a wide variety of protocols, making it versatile for different network applications
> * Simplified WAN Connectivity – GRE tunnels enable connectivity between geographically separated networks
> * Dynamic Multipoint Connectivity (mGRE) – mGRE simplifies configurations in environments like DMVPN by supporting multiple endpoints on a single tunnel interface
> * Interoperability – GRE is widely supported and integrates with various security and tunneling solutions
---
### Use Cases:
> * Site-to-Site Connectivity – standard GRE tunnels are often used to interconnect brand offices
> * DMVPN Deployments – mGRE is a key component of DMVPN, enabling dynamic spoke-to-spoke tunnel establishment
> * Protocol Transport – GRE can encapsulate non-IP protocols or multicast traffic, making it useful in specialized scenarios
> * Overlay Networks – GRE/mGRE can form the basis of an overlay network, decoupling the data plane from the physical infrastructure
---
### Best Practices/Security Considerations:
> * GRE does not provide encryption or authentication by itself. To secure GRE tunnels, combine with IPsec or similar security protocols
> * If using tunnel keys, ensure they are consistently configured to prevent unauthorized tunnel establishment
> * Limit management access to tunnel interfaces via ACLs and secure management protocols
> * Monitor tunnel interfaces and debug outputs to detect anomalies or potential security breaches
> * Ensure tunnel source/destination IP addresses, keys and mGRE settings are consistently configured across all devices
> * Always secure GRE tunnels using IPsec when transmitting sensitive data
> * Regularly verify tunnel interfaces, use debug commands judiciously and review logs for any anomalies
> * Lab your GRE and mGRE configuration to ensure compatibility and performance under expected network loads
> * Maintain clear documentation of GRE/mGRE settings and integration with related protocols (like NHRP in DMVPN)
---
### Insights:
> * Since GRE lacks native security features, combining it with encryption (IPsec, etc) is crucial in untrusted environments
> * While GRE is relatively simple to configure, mGRE adds flexibility for multipoint connectivity but may require additional protocols (like NHRP) for optimal operation
---
### Commands:
> * 
---

