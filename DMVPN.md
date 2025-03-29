# DMVPN - Cisco proprietary 

### Overview:
* Enables dynamic creation of IPsec tunnels; tunnels are created on-demand and torn down when not in use
* DMVPN provides a flexible, scalable solution by eliminating the need for a full mesh of static tunnels. This reduces configuration overhead and simplifies network design
---
### Terminology/Definitions:
> * DMVPN – a VPN solution that dynamically builds and tears down GRE tunnels between remote sites (spokes) over a common hub
> * mGRE – an extension of GRE that allows a single tunnel interface to support multiple endpoints. It’s a key component of DMVPN because it allows the hub to accept connections from many spokes over one interface
> * NHRP – a protocol used to resolve the public (NBMA) IP addresses of remote DMVPN endpoints, effectively mapping a tunnel (private) IP address to a physical IP address
> * IPsec Encryption – DMVPN often uses IPsec to secure GRE tunnels, ensuring data confidentiality and integrity over the public network
> * NBMA – a network type where the underlying transport (like the internet) does not support native broadcast, requiring protocols like NHRP to dynamically resolve tunnel endpoints
> * Hub – a central router that all spoke routers initially connect to. The hub facilitates tunnel establishment and can serve as a fallback path
> * Spoke – a remote router that establishes a VPN tunnel to the hub. Spokes can also dynamically form direct tunnels to other spokes when needed
> * IPsec – often used with DMVPN to encrypt the GRE tunnels, ensuring that data is transmitted securely over untrusted networks
> * Tunnel Interface – a logical interface configured for GRE/mGRE encapsulation, which is used to establish DMVPN tunnels
> * Routing Protocols – typically, dynamic routing protocols are run over DMVPN tunnels to exchange routing information dynamically
> * TLOC – 
---
### Architecture:
> * Hub and Spoke Model – all spoke routers maintain a permanent IPsec tunnel with the hub. When two spokes need to communicate, they initially send traffic to the hub. The hub then informs the initiating spoke of the remote spokes public IP via NHRP, allowing the spokes to establish a direct tunnel
> * mGRE – a single mGRE tunnel interface is configured on both hub and spoke routers. This multipoint tunnel enables dynamic establishment of direct tunnels without needing separate tunnel interfaces for each peer
> * NHRP Functionality –
>   * Registration: spokes register their public (or TLOC) IP addresses with the hub
>   * Resolution: when a spoke needs to send traffic to another spoke, it queries the hub (or directly uses NHRP) to resolve the target’s IP address
>   * Dynamic Tunnels: once the target’s IP is known, a direct spoke-to-spoke tunnel is established dynamically, bypassing the hub for subsequent traffic
> * IPsec Encryption – IPsec is typically layered over the mGRE tunnel to encrypt the traffic, providing confidentiality and integrity over public networks
---
### DMVPN Process:
> * Tunnel Establishment –
>   * A multipoint GRE (mGRE) tunnel interface is configured on the hub and all spoke routers
>   * All routers use the same tunnel network, which serves as the DMVPN “cloud”
> * NHRP Resolution –
>   * Spoke routers register their NBMA (public) IP address with the hub using NHRP
>   * When a spoke needs to communicate with another spoke, it queries the hub (or broadcasts an NHRP request) to learn the remote NBMA address
> * Dynamic Tunnel Creation –
>   * Once the NBMA address is resolved, a direct GRE tunnel (spoke-to-spoke) can be established
>   * If IPsec is enabled, these GRE tunnels are encapsulated in IPsec for secure data transport
---
### DMVPN Phases:
> * Phase 1 – The hub is the only NHRP server, and all traffic flows through the hub. Spoke-to-spoke communication is not directly supported; all traffic is relayed via the hub
> * Phase 2 – Spoke-to-spoke communication is allowed. Spokes learn the NBMA addresses of each other through the hub and can form direct GRE tunnels. Requires additional routing considerations to support direct tunnels.
> * Phase 3 – Optimizes routing and reduces the need for additional tunnel maintenance. Provides more efficient route summarization and supports dynamic spoke-to-spoke tunnels with improvised scalability. Not all features from Phase 2 are retained; check platform support and design requirements.
---
### Operational Flow:
> * Spoke Registration - upon startup, each spoke sends an NHRP registration message to the hub with its reachable IP address
> * Traffic Initiation – when a spoke (Spoke A) wants to communicate with another spoke (Spoke B), it sends the traffic to its mGRE interfaces
> * NHRP Resolution – Spoke A queries the hub (or directly uses cached NHRP information) to resolve Spoke B’s IP address
> * Direct Tunnel Establishment – with Spoke B’s IP address know, Spoke A initiates a dynamic GRE tunnel directly to Spoke B, and the traffic flows directly between them
> * Fallback to Hub – if a direct tunnel cannot be established, the traffic continues to be forwarded via the hub
---
### Key Benefits:
> * Dynamic Mesh Connectivity – DMVPN enables direct spoke-to-spoke communication, reducing latency and avoiding unnecessary traffic load on the hub
> * Simplified Management – only a single tunnel interface is required on each device (thanks to mGRE), simplifying configuration and reducing administrative overhead
> * Scalability – supports large-scale deployments with many remote sites without requiring a full mess of static tunnels
> * Cost Savings – leverages the Internet or broadband links, reducing the need for expensive MPLS circuits while maintaining secure connectivity
> * Resilience  – provides fallback options; if a direct tunnel fails, traffic can be routed via the hub
---
### Use Cases:
> * Branch Connectivity – connect multiple remote branch office securely with minimal configuration
> * Multi-Site VPNs – ideal for enterprise with geographically dispersed location that require secure, dynamic connectivity
> * Remote Access – can be extended to support secure remote access for mobile workers, with direct tunnels to each branch
> * Hybrid Networks – integrate with IPsec, MPLS and SD-WAN architectures for flexible and secure network designs
> * Disaster Recovery – provides dynamic rerouting capabilities in the event of a link or site failure, ensuring business connectivity
---
### Best Practices:
> * Ensure that all DMVPN peers use the same NHRP network ID, tunnel key, and authentication parameters
> * Use dynamic routing over DMVPN to manage tunnel endpoints and optimize spoke-to-spoke communication
> * Use IPsec to encrypt GRE tunnels to secure data across public networks.
> * Always use strong encryption and authentication methods
> * Plan for scalability by choosing the appropriate DMVPN phase (Phase 2 for direct spoke-to-spoke communication, Phase 3 for improved route summarization and scaling)
> * Regularly monitor tunnel status, NHRP bindings, and IPsec sessions.
> * Document configuration and changes to simplify troubleshooting
> * Ensure that IPsec profiles use strong cipher suites and proper key exchange methods
> * Regularly update PSK’s or consider certificate-based authentication for higher security
> * Secure NHRP messages to prevent spoofing of tunnel endpoints. Use appropriate authentication where possible
> * Limit DMVPN participation to trusted sites by controlling which devices are allowed as spokes
> * Continuously monitor DMVPN tunnels for unexpected NHRP registrations or tunnel flaps, which might indicate an attack or misconfiguration
> * Use DMVPN segmentation to isolate sensitive traffic and control the distribution of routes and policies among remote sites
---
### Troubleshooting:
> * Verify that NHRP authentication and network ID are correctly configured on all DMVPN peers
> * Ensure that the hub is reachable and that the spoke has the correct mapping for the hubs NBMA address
> * Confirm that the tunnel source interfaces are correctly configured and reachable
> * Check that the tunnel keys and crypto settings match on all devices
> * Exame routing protocol convergence over the DMVPN tunnel
> * Ensure that ISAKMP policies, pre-shared keys, and transform sets march between hub and spoke
> * DMVPN Tunnel Not Establishing?
>   * Verify tunnel interface IP addressing and that the underlying physical interface is up
>   * Check NHRP configuration and authentication; mismatches will prevent proper NHRP registration
>   * Ensure that the tunnel key and crypto settings are consistent across all peers
> * Spoke-to-Spoke Communication Failing (Phase 2)?
> * Confirm that the hub is correctly relaying NHRP requests/responses
> * Check that spokes are receiving correct NBMA address mappings from the hub
> * Review routing protocol updates to ensure proper route advertisement between spokes
>   * IPsec Tunnel Flapping?
> * Inspect ISAKMP logs for negotiation errors
> * Verify that both peers support the configured encryption and authentication methods
> * Ensure that the pre-shared keys match and that timers (such as lifetime settings) are consistent
---
### Insights:
> * The dynamic nature of DMVPN eliminates the need for a full mesh of static tunnels, significantly reducing configuration complexity
> * Often EIGRP and OSPF are ran over the DMVPN tunnel for dynamic routing
> * The effectiveness of DMVPN heavily relies on NHRP. Accurate and timely resolution of NBMA addresses is essential for tunnel establishment and direct spoke-to-spoke communication
> * Choosing the right DMVPN phase depends on your network size and requirements. Phase 2 is commonly used, but Phase 3 may offer benefits in larger networks with more complex routing requirements
> * Running dynamic routing protocols over DMVPN can greatly enhance network resiliency and efficiency. Make sure to tune routing protocols to account for DMVPBs dynamic nature
> * Testing in a lab environment before production rollout is essential. DMVPN can be sensitive to timing, NHRP resolution, and underlying transport reliability
---
### Commands:
> *
---
