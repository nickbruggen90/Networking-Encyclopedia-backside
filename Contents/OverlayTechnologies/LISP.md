# LISP - Locator/ID Separation Protocol
*RFC 6830 (architecture), RFC 6831 (control plane), RFC 6832 (data plane)*

## Overview
• LISP is a network technology and protocol designed to **separate identity (who) from location (where)** in IP networking  
• It improves routing scalability, mobility, and multihoming by **decoupling endpoint identifiers (EIDs) from routing locators (RLOCs)**  
• **Originally developed by Cisco** → Now standardized by the IETF  
• **Addresses core internet routing table growth** site mobility, and IPv6/IPv4 co-existence

---

## Protocol/Ports

| Component | Protocol/Port | Usage |
|-----------|---------------|-------|
| **LISP Data Plane** | UDP 4341 | Encapsulated data packets |
| **LISP Control Plane** | UDP 4342 | Control messages (Map-Request, Map-Reply, etc.) |
| **Encapsulation** | IP over UDP/IP | Or over LISP Header when doing VXLAN-LISP overlays |

---

## Terminology/Definitions

### Core Components

| Term | Definition |
|------|------------|
| **EID** | Endpoint Identifier - the IP address of a host; identifies the endpoint but not its location in the topology |
| **RLOC** | Routing Locator - the IP address of a router or LISP tunnel endpoint; identifies where a host resides in the network |
| **MS (Map Server)** | Receives EID-to-RLOC registrations from xTRs and stores mapping information |
| **MR (Map Resolver)** | Receives LISP map requests from xTRs and replies with EID-to-RLOC mappings from MS |

### Tunnel Router Components

| Term | Definition |
|------|------------|
| **xTR (Tunnel Router)** | A router that acts as an Ingress Tunnel Router (ITR) and/or Egress Tunnel Router (ETR) to encapsulate/decapsulate LISP packets |
| **ITR** | Ingress Tunnel Router - encapsulates outgoing packets into LISP format toward RLOC |
| **ETR** | Egress Tunnel Router - receives LISP packets, decapsulates, and forwards to destination EID |
| **PxTR (Proxy xTR)** | Enables communication between LISP and non-LISP sites (similar to NAT traversal) |

### Control Messages

| Term | Definition |
|------|------------|
| **Map-Request** | Control message sent by ITR to query mapping for an EID; resolves EID-to-RLOC |
| **Map-Reply** | Response from MS or ETR providing the RLOC(s) for the queried EID; provides EID-to-RLOC mapping |
| **Map-Register** | Sent from ETR to MS to register EID-to-RLOC mapping |
| **Map-Notify** | Sent from MS to ETR to acknowledge receipt of Map-Register; successful registration |
| **Map-Referral** | Redirects request to another MS/MR (used in delegation hierarchy) |

### Database and Status

| Term | Definition |
|------|------------|
| **LISP Database** | Maintains mapping tables for EID-to-RLOC relationships |
| **Locator-Status-Bits (LSBs)** | Bits in LISP control messages that indicate reachability status of RLOCs |

---

## Key Behaviors

### Operational Characteristics
• **ITR stores mappings temporarily in a map-cache** for performance optimization  
• **RLOCs can be prioritized (weight-based) or failover-probed** for load balancing and redundancy  
• **Control plane and data plane are decoupled** for scale and resilience  
• **LISP enables multi-tenancy** using Instance ID (like VRF-lite over IP)

---

## Control Plane Operation

### EID-to-RLOC Registration Process

| Step | Actor | Action | Description |
|------|-------|--------|-------------|
| **1** | **ETR** | Registration | ETR registers its EID-to-RLOC mapping with the MS via Map-Registration |
| **2** | **MS** | Acknowledgment | MS replies with Map-Notify to confirm registration |

### EID-to-RLOC Resolution Process

| Step | Actor | Action | Description |
|------|-------|--------|-------------|
| **1** | **ITR** | Unknown EID | ITR receives a packet for an unknown EID |
| **2** | **ITR** | Query | ITR queries a Map Resolver via Map-Request |
| **3** | **MR/ETR** | Response | MR (or authoritative ETR) sends a Map-Reply with the RLOC(s) |
| **4** | **ITR** | Cache and Forward | ITR stores mapping in its Map-Cache and encapsulates data |

---

## Control Plane Flow

### End-to-End Communication Process

| Step | Process | Description |
|------|---------|-------------|
| **1** | **Traffic Reception** | ITR receives traffic destined to an EID |
| **2** | **Mapping Request** | ITR sends a Map-Request to the MR |
| **3** | **Mapping Response** | MR replies with a Map-Reply containing the RLOC(s) for the EID |
| **4** | **Encapsulation** | ITR encapsulates the packet in LISP UDP/IP Header and sends it to the appropriate RLOC |
| **5** | **Decapsulation** | ETR at the destination decapsulates the LISP header and delivers the original packet to the EID |

---

## LISP Control Message Types

| Message Type | Purpose |
|--------------|---------|
| **Map-Request** | Query for EID-to-RLOC mapping |
| **Map-Reply** | Response containing RLOC information |
| **Map-Register** | Register EID-to-RLOC mapping |
| **Map-Notify** | Acknowledge registration |
| **Map-Referral** | Redirect to another MS/MR |
| **Encapsulated Control Messages** | Control messages sent through data plane |

---

## Data Plane Encapsulation Format

### LISP Packet Structure

| Layer | Component | Details |
|-------|-----------|---------|
| **Outer IP** | Header | Source = ITR RLOC, Destination = ETR RLOC |
| **UDP** | Header | Source port (random), Destination = 4341 |
| **LISP Header** | Control | Nonce, flags, instance ID (for VRFs) |
| **Inner IP** | Header | Source = original EID, Destination = target EID |
| **Inner Payload** | Data | TCP/UDP packet, etc. |

### Encapsulation Flow
```
Original Packet → LISP Encapsulation → Transport → LISP Decapsulation → Original Packet
     EID              RLOC routing              EID
```

---

## Use Cases

| Use Case | Application | Benefits |
|----------|-------------|----------|
| **SD-Access Fabric** | Overlay routing in campus networks | Simplified fabric management, mobility support |
| **Multi-site Data Center Interconnect** | DCI between geographically dispersed data centers | Scalable site interconnection |
| **Mobile Endpoints** | IoT, wireless roaming scenarios | Seamless mobility without IP address changes |
| **IPv4/IPv6 NAT-avoidance** | Between disjoint networks | Clean protocol transition and interoperability |
| **Cloud On-ramp/VPN Hub** | Convergence solutions | Simplified cloud connectivity |

---

## Best Practices/Security Considerations

### Authentication and Security
• **LISP supports SHA-256 HMAC** for Map-Register and Map-Notify messages  
• **Protects against spoofed registrations** and unauthorized mapping updates  
• **Filter inbound traffic to RLOC** to allow only trusted LISP peers

### Infrastructure Design
• **MS/MR Placement** → Centralize for control, but use multiple for redundancy  
• **Consider delay-sensitive placement** for WAN overlay deployments  
• **Use EID Prefixes** → Summarize EIDs when possible to reduce mapping load

### Operational Guidelines
• **Avoid PxTR Unless Necessary** → It reintroduces NAT-like behavior and is a single point of failure  
• **Cache Tuning** → Limit map-cache lifetime to balance freshness vs overhead  
• **TTL Decay** → Rely on RLOC-probing to age out stale mappings

### Security Implementation
• **RLOC Access Control** → Implement proper filtering for LISP control traffic  
• **Authentication configuration** → Use HMAC for all control plane messages  
• **Monitoring and logging** → Track mapping registrations and unusual patterns

---

## Common Issues and Fixes

| Issue | Symptoms | Potential Cause | Solution |
|-------|----------|-----------------|----------|
| **No Data Traffic** | Packets not forwarding through LISP | Map-Request not resolved | Verify MS/MR reachability, confirm EID is registered |
| **Packet Dropped at RLOC** | Traffic reaches RLOC but doesn't proceed | LISP not decapsulating | Confirm ETR configuration and UDP 4341 is open |
| **Long RTT on First Packet** | Initial packet has high latency | Cold map-cache | Expected behavior; use map-cache preload if supported |
| **RLOC Unreachable** | Control plane connectivity issues | Routing problem or blocked UDP | Confirm IP reachability and ACLs on RLOC IPs |

### Detailed Troubleshooting Steps

#### Control Plane Issues
1. **Verify MS/MR registration** and reachability  
2. **Check Map-Register/Map-Notify** message exchange  
3. **Validate Instance ID** consistency across sites  
4. **Review authentication** configuration and keys

#### Data Plane Issues
1. **Confirm UDP 4341 accessibility** between RLOCs  
2. **Verify LISP encapsulation/decapsulation** configuration  
3. **Check routing** between RLOC addresses  
4. **Validate firewall rules** for LISP traffic

#### Performance Issues
1. **Monitor map-cache** hit rates and aging  
2. **Check RLOC probing** and reachability detection  
3. **Review network latency** between LISP components  
4. **Optimize mapping** refresh intervals

---

## Architecture Benefits

### Scalability Improvements
• **Routing table growth mitigation** → EID-to-RLOC separation reduces core routing table size  
• **Improved convergence** → Control plane changes don't affect data plane forwarding  
• **Multi-tenancy support** → Instance ID enables VRF-like segmentation

### Mobility and Flexibility
• **Seamless mobility** → EID remains constant while RLOC changes  
• **Multihoming support** → Multiple RLOCs per EID for redundancy  
• **Protocol transition** → IPv4/IPv6 interoperability through tunnel endpoints

### Operational Advantages
• **Traffic engineering** → RLOC selection based on policies and metrics  
• **Load balancing** → Multiple RLOCs with priority and weight  
• **Failure detection** → RLOC probing and LSB status indication

---
