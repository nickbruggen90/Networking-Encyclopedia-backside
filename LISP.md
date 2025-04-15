# LISP - RFC 6830 (architecture), RFC 6831 (control plane), RFC 6832 (data plane)

### Overview:
> * LISP is a network technology and protocol designed to separate identity (who) from location (where) in IP networking. It improves routing scalability, mobility, and multihoming by decoupling endpoint identifiers (EIDs) from routing locators (RLOCs).
> * Originally developed by Cisco; now standardized by the IETF
> * Addresses core internet routing table growth, site mobility, and IPv6/IPv4 co-existence
---
### Protocol/Port(s):
> * LISP Data Plane – UDP: 4341
> * LISP Control Plane – UDP: 4342
> * Encapsulation: IP over UDP/IP (or over LISP Header when doing VXLAN-LISP overlays)
---
### Terminology/Definitions:
> * EID – the IP address of a host; identifies the endpoint but not its location in the topology
> * RLOC – the IP address of a router or LISP tunnel endpoint; identifies where a host resides in the network
> * MS (map server) – receives EID-to-RLOC registrations from xTRs and stores mapping info
> * MR (map resolver) – receives LISP map requests from xTRs and replies with EID-to-RLOC mappings from MS
> * xTR (tunnel router) – a router that acts as an Ingress Tunnel Router (ITR) and/or Egress Tunnel Router (ETR) to encapsulate/decapsulate LISP packets
> * ITR – encapsulates outgoing packets into LISP format toward RLOC
> * ETR – receives LISP packets, decapsulates, and forwards to destination EID
> * PxTR (proxy xTR) – enables communication between LISP and non-LISP sites (similar to NAT traversal)
> * Map-Request – control message sent by ITR to query mapping for an EID; resolves EID-to-RLOC
> * Map-Reply – response from MS or ETR providing the RLOC(s) for the queried EID; provides EID-to-RLOC mapping
> * Map-Register – sent from ETR to MS to register EID-to-RLOC mapping
> * Map-Notify – sent from MS to ETR to acknowledge receipt of Map-Register; successful registration
> * Map-Referral – redirects request to another MS/MR (used in delegation hierarchy)
> * LISP Database – maintains mapping tables for EID-to-RLOC relationships
> * Locator-Status-Bits (LSBs) – bits in LISP control messages that indicate reachability status of RLOCs
---
### Key Behaviors:
> * ITR stores mappings temporarily in a map-cache
> * RLOCs can be prioritized (weight-based) or failover-probed
> * Control plane and data plane are decoupled for scale and resilience
> * LISP enables multi-tenancy using Instance ID (like VRF-lite over IP)
---
### Control Plane Operation:
> * EID-to-RLOC Registration -
>   * ETR registers its EID-to-RLOC mapping with the MS via Map-Registration
>   * MS replies with Map-Notify to confirm registration
> * EID-to-RLOC Resolution -
>   * ITR receives a packet for an unknown EID
>   * ITR queries a Map Resolver via Map-Request
>   * MR (or authoritative ETR) sends a Map-Reply with the RLOC(s)
>   * ITR stores mapping in its Map-Cache and encapsulates data
---
### Control Plane Flow:
> * ITR receives traffic destined to an EID
> * ITR sends a Map-Request to the MR
> * MR replies with a Map-Reply containing the RLOC(s) for the EID
> * ITR encapsulates the packet in LISP UDP/IP Header and sends it to the appropriate RLOC
> * ETR at the destination decapsulates the LISP header and delivers the original packet to the EID
---
### LISP Control Message Types:
> * Map-Request
> * Map-Reply
> * Map-Register
> * Map-Notify
> * Map-Referral
> * Encapsulated Control Messages
---
### Data Plane Encapsulation Format:
> * Outer IP → source = ITR RLOC, destination = ETR RLOC
> * UDP → source port (random), destination = 4341
> * LISP Header → nonce, flags, instance ID (for VRFs)
> * Inner IP → source = original EID, destination = target EID
> * Inner Payload → TCP/UDP packet, etc
---
### Use Cases:
> * SD-Access fabric overlay routing
> * Multi-site data center interconnect
> * Mobile endpoints (IoT, wireless roaming)
> * IPv4/IPv6 NAT-avoidance between disjoint networks
> * Cloud on-ramp/VPN hub convergence
---
### Best Practices/Security Consideration:
> * Authentication - 
>   * LISP support SHA-256 HMAC for Map-Register and Map-Notify
>   * Protects against spoofed registrations
> * RLOC Access Control -
>   * Filter inbound traffic to RLOC to allow only trusted LISP peers
> * MS/MR Placement -
>   * Centralize for control, but use multiple for redundancy
>   * Consider delay-sensitive placement for WAN overlay
> * Use EID Prefixes -
>   * Summarize EIDs when possible to reduce mapping load
> * Avoid PxTR Unless Necessary -
>   * It reintroduces NAT-like behavior and is a single point of failure
> * Cache Tuning -
>   * Limit map-cache lifetime to balance freshness vs overhead
> * TTL Decay -
>   * Rely on RLOC-probing to age out stale mappings
---
### Common Issues and Fixes:
> * No Data Traffic?
>   * Cause?: Map-Request not resolved
>   * Fix: verify MS/MR reachability, or EID is registered
> * Packet Dropped at RLOC?
>   * Cause?: LISP not decapsulating
>   * Fix: confirm ETR configuration and UDP 4341 is open
> * Long RTT on First Packet?
>   * Cause?: cold map-cache
>   * Fix: expected behavior; use map-cache preload if supported
> * RLC Unreachable?
>   * Cause?: routing problem or blocked UDP
>   * Fix: confirm IP reachability and ACLs on RLOC IPaw
---
