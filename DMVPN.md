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
