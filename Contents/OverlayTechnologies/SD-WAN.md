# SD-WAN - RFC 4301 (IPsec Architecture), RFC 6347 (DTLS)

### Overview:
> * SD-WAN is an advanced networking technology that abstracts the underlying transport networks (MPLS, broadband, LTE) to deliver virtualized, software driven WAN. It simplifies WAN management by centralizing control, automating policies and optimizing application performance across diverse network links.
> * SD-WAN solutions are widely used in brand office connectivity, multi-cloud architecture and digital transformation initiatives
> * SD-WAN represents a shift from traditional WAN architectures, offering agility, cost savings and enhanced performance
---
### Timers:
> * OMP Keepalive = 30 sec (default)
> * IPsec tunnels have configurable lifetimes and rekey intervals, which are part of the overall SD-WAN configuration (IPsec SA Lifetime)
> * BFD probes for SLA monitoring are configurable
---
### Multicast:
> * In SD-WAN, multicast traffic is usually handled as part of the overall traffic policy. Although, VXLAN-based data centers might use multicast for flooding in some configurations. SD-WAN overlays primarily use unicast replication for performance and control
---
### Protocol/Port(s):
> * OMP – UDP: 12346
> * IPsec – UDP: 500/4500
> * NTP – UDP: 123
---
### Terminology/Definitions:
> * SD-WAN Controller/Orchestrator – a centralized management platform (often cloud-based) that configures, monitors and manages the entire SD-WAN fabric through APIs and graphical interfaces
> * vManager – controller for centralized configuration and policy push
> * vSmart – distributes routing, security and application policies (OMP)
> * vBond – orchestrates initial secure control connections between devices
> * vEdge Routers/SD-WAN Edge Devices – the physical or virtual devices deployed at brand offices, data centers, or cloud environments that form the SD-WAN overlay. They encapsulate and forward traffic based on the policies provided by the controller
> * Overlay Network – a virtual network build on top of the physical underlay, where traffic is encapsulated (using IPsec, DTLS or GRE) and managed independently of the underlying transport
> * Underlay Network – the physical transport network (MPLS, broadband, LTE) that carries the encapsulated SD-WAN traffic between sites
> * Overlay Management Protocol (OMP) – a proprietary protocol, used in Cisco SD-WAN/Viptela, for distributing routing, policy and security information between the controller and edge devices
> * Transport Locator (TLOC) – a unique identifier representing a physical interface on an SD-WAN edge device. TLOCs are used in OMP to distinguish between multiple transport options for a single site
> * VPN – in SD-WAN, VPNs are used to logically segregate traffic (internet, voice, data) over the same SD-WAN fabric
---
### Header Breakdown:
> * There isn’t a single “SD-WAN header” because the encapsulation method depends on the chosen security protocol (IPsec, DTLS, etc). Instead, focus on the outer IP header, UDP/DTLS header and IPsec header.
> * IPsec Tunnels – SD-WAN often uses IPsec to secure tunnels. The IPsec encapsulation adds its own headers (ESP, AH) which include fields such as SPI, Sequence Number and more.
> * IPsec Header – uses ESP/AH for encryption and authentication
> * DTLS – In some SD-WAN solutions, DTLS is used to secure the control plane communications
> * DTLS Header – used in some control plan comms (UDP encapsulated TLS)
---
### Architecture:
> * Separation between overlay (logical tunnels) from underlay (physical WAN links). Supports dynamic path selection, QoS, ZTP, and app-aware routing
> * Centralized Orchestration – the SD-WAN controller (or orchestrator) centrally manages configuration, policy distribution and monitoring across all edge devices
> * Edge Devices – SD-WAN edge routers (vEdge) are deployed at remote sites, branches, data centers and cloud environments. They establish secure tunnels over the underlay using encapsulation methods (commonly IPsec, sometimes DTLS or GRE)
> * Overlay and Underlay Separation – the overlay network abstracts the complexities of the underlying transport (underlay), enabling a consistent application of policies regardless of the physical links used
> * Dynamic Path Control – real-time monitoring of link performance (latency, jitter, loss) allows SD-WAN to steer traffic along the best available path, ensuring optimal application performance
> * Control Plane – the OMP protocol tuns in the control plane, distributing routing and policy information from the controller to the edge devices - OMP via vSmart
> * Data Plane – the actual forwarding of encapsulated traffic access the underlay. Policies (path selection, prioritization, etc) are enforced as traffic is transmitted between edge devices. Uses secure tunnels between vEdges (IPsec/DTLS)
---
### Operational Overview:
> * Policy-Driven Connectivity – administrators define policies (application based routing, security segmentation, QoS) via the SD-WAN controller. These policies are pushed to the edge devices and dynamically enforced
> * Tunnel Establishment – edge devices (vEdge) establish secure tunnels, using IPsec/DTLS, over multiple transports. TLOCs help determine which physical link to use
> * Route and Policy Distribution – OMP distributes network topology, routing information, and policy changes in real time, enabling rapid adaptation to changing network conditions
> * Link Performance Monitoring – continuous measurement of performance metrics (latency, jitter, packet loss) helps the system make dynamic routing decisions
---
### Operational Workflow:
> * 1. ZTP Process – edge devices register via vBond
> * 2. Control plane established with vSmart (OMP, BDF, certificates)
> * 3. Tunnels formed between vEdges
> * 4. Policies (app-routing, segmentation, firewall) are pushed from vManage
> * 5. Monitoring, analytics, and logging collected via vManage
---
### Key Benefits:
> * Centralized Management – simplifies configuration, policy enforcement and monitoring across a distributed WAN
> * Cost Savings – leverages multiple types of transport (MPLS, broadband, LTE) to optimize cost and performance
> * Enhanced Application Performance – real-time monitoring and dynamic path selection optimize performance for critical applications
> * Scalability – easily scales to support large, distributed environments and multi-cloud architectures
---
### Use Cases:
> * Branch Connectivity – connect remote offices securely and efficiently using a unified SD-WAN fabric. Secure branch connectivity
> * Cloud Integration – provide seamless connectivity between on-premise networks and cloud services. Multi-cloud or hybrid cloud routing
> * Digital Transformation – enable agile, policy-driven networking that supports modern applications and business processes. MPLS + Broadband cost optimization
> * Multi-Tenant Environments – support segmented connectivity for multiple customers or business units over a single infrastructure. Segmenting business units or tenants
---
### Best Practices/Security Considerations:
> * SD-WAN solutions typically secure data plane tunnels with IPsec (or DTLS for control plane) to ensure confidentiality and integrity
> * Secure OMP sessions with proper authentication and encryption. Use certificate-based or pre-shared key mechanisms as supported by your SD-WAN solution
> * Implement strict policies via the controller to isolate traffic among different sites and customers.
> * Use VFR and firewall policies where applicable
> * Continuously monitor for anomalies using the centralized dashboard (vManage) and set up alerts for potential security breaches
> * Keep edge devices and controllers up to date with the latest security patches and firewall releases
> * Ensure a robust, high-performance underlay network to support the overlay tunnels
> * Implement strong encryption and authentication on both the control (OMP/DTLS) and data (IPsec) planes
> * Leverage the SD-WAN controller for consistent policy deployment and real-time monitoring
> * Regularly review and adjust traffic policies based on performance metrics and business requirements
> * Maintain detailed documentation of SD-WAN configurations, policy definitions and edge device settings for troubleshooting and future scaling
---
### Common Issues and Fixes:
> * Verify that underlay connectivity is stable (check physical connectivity, routing and reachability)
> * Ensure proper TLOC configuration and that the correct IP addresses are being used
> * Use debug commands for OMP to trace route and policy updates
> * Confirm that the device is properly registered with the SD-WAN controller
> * Monitor link performance metrics (latency, jitter, loss) from the vManage dashboard
> * Adjust policies based on real-time data to optimize path selection
> * Control Tunnel Down?
>   * Fix: check vBond/vSmart/vManage reachability, validate certificates and time sync
> * Data Plane Drop?
>   * Fix: check TLOC mapping and OMP updates
> * No OMP Routes?
>   * Fix: inspect OMP peering with ‘show omp peers’
> * Tunnel BFD Drops?
>   * Fix: check underlay SLA issues, jitter/loss
---
### Insights:
> * SD-WAN offers the ability to monitor network performance and dynamically select the best path is key to supporting modern, latency-sensitive applications
> * SD-WAN facilitates seamless connectivity in hybrid and multi-cloud environments and integrates well with broader SDN strategies
> * As networks become more virtualized and cloud centric, SD-WAN will continue to evolve, emphasizing even greater need for automation, security and analytic capabilities
---
### Commands
> *
---
