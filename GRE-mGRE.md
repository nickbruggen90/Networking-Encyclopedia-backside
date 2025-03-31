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
> 3. Decapsulation (egress) – the egress router movies the GRE and outer IP headers and forwards the original packet toward its destination
---

