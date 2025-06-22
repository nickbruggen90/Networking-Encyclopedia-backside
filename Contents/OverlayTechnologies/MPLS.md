# MPLS - Multiprotocol Label Switching
*RFC 3031 (MPLS Architecture), RFC 3032 (MPLS Label Stack Encoding), RFC 3209 (RSVP-TE), RFC 5036 (LDP Version 1)*

## Overview
• MPLS is a high performance, scalable technique used in modern networks to **speed up and shape the flow of traffic**  
• MPLS works by **attaching short, fixed-length labels to packets** and using these labels to make forwarding decisions  
• **Bypasses the more complex lookups** typically associated with IP routing  
• This **label-based forwarding mechanism improves efficiency** supports traffic engineering and enables the creation of VPNs

---

## Timers

| Protocol | Timer Type | Value | Purpose |
|----------|------------|-------|---------|
| **LDP** | Hello Timer | 30 seconds | LDP hello interval |
| **LDP** | Hold Timer | 90 seconds | LDP session timeout |
| **LSP/RSVP-TE** | Refresh Interval | 30 seconds | Path state refresh |
| **LSP/RSVP-TE** | State Lifetime (Hold Timer) | 90 seconds | Path state timeout |

---

## Multicast Addresses

| Protocol | IP Version | Multicast Address | Description |
|----------|------------|-------------------|-------------|
| **LDP** | IPv4 | 224.0.0.2 | All routers group |
| **LDP** | IPv6 | FF02::2 | Link-local all-routers |

---

## Protocol/Ports

| Protocol | Port | Transport | Usage |
|----------|------|-----------|-------|
| **LDP** | 646 | TCP/UDP | Label Distribution Protocol |

---

## Priority Levels

| Field | Range | Description |
|-------|-------|-------------|
| **TC Field in Header** | 0-7 | Traffic Class for QoS priority |

---

## Terminology/Definitions

### Core MPLS Components

| Term | Definition |
|------|------------|
| **Label** | A short, fixed-length identifier (typically 20 bits) that determines the forwarding equivalence class (FEC) for a packet |
| **FEC** | Forwarding Equivalence Class - a group of packets that are forwarded in the same manner (via the same path) through the MPLS network |
| **Label Edge Router (LER)** | An MPLS router that operates at the edge of the MPLS network. Ingress LERs add labels to incoming packets, while egress LERs remove labels |
| **Label Switch Router (LSR)** | A router within the MPLS network that forwards packets solely based on the label without inspecting the packet's IP header |

### Label Distribution

| Term | Definition |
|------|------------|
| **Label Distribution Protocol (LDP)** | A protocol used to distribute labels between MPLS-enabled routers. Establishes label bindings for FECs across the MPLS domain |
| **Resource Reservation Protocol - Traffic Engineering (RSVP-TE)** | An extension of RSVP used for label distribution with traffic-engineered paths and explicit route control |
| **Label Stack** | MPLS packets may carry multiple labels (a label stack), which support hierarchical routing and enable advanced features such as VPNs |

### MPLS VPN Components

| Term | Definition |
|------|------------|
| **RD** | Route Distinguisher - uniquely identifies a route (or route instance) in the MPLS VPN to avoid conflicts from overlapping address space |
| **RT** | Route Target - acts as a policy tool to control the import and export of VPN routes between VRFs, determining which routes are shared across different VPNs |
| **MPLS VPN** | A VPN service built on MPLS, allowing service providers to segregate customer traffic using VRF instances and MPLS labels |

---

## Header Breakdown (32 bits)

### MPLS Header Structure

| Field | Size (bits) | Range/Values | Description |
|-------|-------------|--------------|-------------|
| **Label** | 20 | 0-1,048,575 | Identifies the FEC for the packet |
| **Experimental or TC (Traffic Class)** | 3 | 0-7 | Priority levels for QoS |
| **Bottom of Stack (S)** | 1 | 0 or 1 | S=0: additional label below; S=1: last label |
| **TTL** | 8 | 0-255 | Time to Live |

### Header Field Definitions

| Field | Purpose | Operation |
|-------|---------|-----------|
| **Label** | Forwarding decision | Routers use this value to determine the next hop without needing to inspect the full IP header |
| **Experimental or TC** | QoS classification | These values can be used to indicate priority levels or to carry DiffServ information. Critical for QoS in MPLS networks |
| **Bottom of Stack (S)** | Stack indication | Indicates whether the current label is the bottom (last) label in the label stack |
| **TTL** | Loop prevention | Prevents infinite loops in MPLS networks |

---

## Key Benefits

| Benefit | Description | Impact |
|---------|-------------|---------|
| **Fast Packet Forwarding** | MPLS uses fixed-length labels for forwarding | Allows routers to quickly switch packets without performing complex IP header lookups |
| **Traffic Engineering** | With protocols such as RSVP-TE | Enables administrators to define explicit paths through the network, optimizing bandwidth usage |
| **Scalability** | MPLS scales effectively across large multi-domain networks | Label-based switching model supports thousands of VPNs and numerous routing paths |
| **QoS Support** | The MPLS header's 3-bit TC field enables DSCP mapping | Allows for differentiated service treatment, maintaining performance for latency-sensitive applications |
| **VPN Support** | MPLS underpins MPLS VPN by combining VRF instances and label switching | Allows multiple customers to share common infrastructure securely and independently |
| **Legacy Integration** | MPLS can run alongside traditional IP routing | Enables gradual network upgrades and coexistence with legacy systems |
| **Simplified Management** | Abstracts underlying IP routing using labels | Simplifies management of complex networks and provides predictable traffic paths |

---

## Use Cases

### Enterprise Networks

| Application | Benefits |
|-------------|----------|
| **Internal Segmentation** | Use MPLS VPN to segment and isolate traffic between different business units or geographical locations without requiring separate physical infrastructures |
| **QoS Implementation** | MPLS traffic engineering helps maintain performance for critical applications, ensuring voice and video traffic experience minimal delay and packet loss |

### Service Provider Networks

| Application | Benefits |
|-------------|----------|
| **MPLS VPN Services** | Service providers rely on MPLS to offer VPN services to multiple customers over a shared backbone while maintaining strict isolation |
| **Traffic Engineering** | Providers use MPLS TE to optimize network paths, balance load and provide backup routes in the event of link failures |
| **Scalability and Efficiency** | MPLS allows service providers to scale networks efficiently, supporting large numbers of subscribers and high-volume traffic flows |

### Data Center Interconnects

| Application | Benefits |
|-------------|----------|
| **Efficient Interconnection** | MPLS can be used to connect data centers, ensuring high performance and reliable transport with predictable latency |
| **Virtualized Environments** | MPLS combined with VRF and MPLS VPN supports multi-tenant data center architecture, facilitating secure and isolated virtual environments |

### Mobile and Converged Networks

| Application | Benefits |
|-------------|----------|
| **Integrated Services** | MPLS plays a role in converged networks, allowing integration of voice, video and data services across a single transport mechanism |

---

## Architecture and Operation

### MPLS Forwarding Process

| Stage | Component | Operation | Description |
|-------|-----------|-----------|-------------|
| **Ingress** | LER | Label Imposition | The ingress LER examines the incoming packet and assigns it a label based on its FEC. The label is pushed onto the packet's label stack |
| **Transit** | LSR | Label Switching | Intermediate LSRs use the top label to determine the packet's next hop. They swap, push or pop labels as necessary based on pre-established label bindings |
| **Egress** | LER | Label Disposition | The egress LER removes the label from the packet (label popping) and forwards the packet to its final destination using traditional IP routing |

### Label Operations

| Operation | Description | When Used |
|-----------|-------------|-----------|
| **Push** | Add a label to the label stack | Ingress LER adding initial label |
| **Swap** | Replace current label with new label | Transit LSR forwarding |
| **Pop** | Remove label from stack | Egress LER or penultimate hop popping (PHP) |

---

## Best Practices/Security Considerations

### Authentication and Integrity
• **Ensure proper authentication and integrity checks** on protocols like LDP to prevent malicious label injection or spoofing  
• **Use features such as TCP MD5 signatures** on LDP sessions to protect label distribution from tampering  
• **Implement robust ACLs and route policies** to prevent unauthorized access to MPLS control traffic and management interfaces

### Access Control and Management
• **Restrict management access to MPLS routers** using ACLs, secure management protocols and network segmentation  
• **Ensure physical security of devices** and apply hardening best practices to reduce the risk of unauthorized configuration changes

### Data Protection
• **For MPLS VPN, consider using IPsec** to encrypt data traversing MPLS networks, particularly if the infrastructure extends over untrusted links  
• **Combine MPLS with IPsec or other security protocols** to protect data on untrusted networks

### Monitoring and Maintenance
• **Monitor MPLS forwarding tables, LDP sessions** and associated logs regularly to detect anomalies or potential security breaches  
• **Utilize verification commands and network monitoring tools** to maintain MPLS health

### Configuration Management
• **Clearly define network segments** service requirements and scalability needs  
• **Use consistent LDP or RSVP-TE configurations** across all MPLS devices  
• **Keep thorough documentation** of MPLS settings, LDP configurations and VRF instances for troubleshooting and upgrades

---

## Common Issues and Fixes

### Basic MPLS Connectivity Issues

| Issue | Symptoms | Potential Cause | Solution |
|-------|----------|-----------------|----------|
| **MPLS Labels Not Being Assigned or Advertised** | No label forwarding | LDP not enabled on interface or incorrect MPLS configuration | Ensure `mpls ip` (or `mpls ldp` in new IOS) is applied to all relevant interfaces; verify `mpls ldp router-id` is set and reachable |
| **No LDP Neighbor Relationship** | LDP adjacency fails | IP connectivity or IGP reachability issue between LSRs | Use ping, traceroute and `show mpls ldp neighbor`; confirm loopback-to-loopback reachability and matching LDP router-IDs |
| **Traffic Not Using The MPLS Path** | Packets follow IP routing | IGP or MPLS label-switched path (LSP) not set up correctly | Verify IGP (OSPF/EIGRP/IS-IS) is working correctly; use `show ip route`, `show mpls forwarding-table` |

### Label Distribution Issues

| Issue | Symptoms | Potential Cause | Solution |
|-------|----------|-----------------|----------|
| **Labels Not Showing In Forwarding Table** | Empty MPLS forwarding table | Prefixes not being advertised or no IGP route to destination | Check routing table and confirm prefixes are received; ensure labels are being advertised with `show mpls ldp bindings` |
| **Penultimate Hop Doesn't Pop The Label (PHP not working)** | Extra label in forwarding | PHP disabled or explicit null configured incorrectly | Review LDP configuration; by default, PHP is enabled, unless `mpls ldp explicit-null` is configured |

### MPLS VPN Issues

| Issue | Symptoms | Potential Cause | Solution |
|-------|----------|-----------------|----------|
| **VPNv4 Routes Not Appearing on Remote PE** | Missing VPN routes | Missing or misconfigured MP-BGP session between PEs | Ensure `BGP neighbor x.x.x.x activate` is applied under `address-family vpnv4`; verify route targets and RD settings match |
| **Packets Dropped After Label Pop** | Traffic loss at egress | Missing route to the VPN prefix in the RIB/FIB | Check BGP table and `show ip route vrf XYZ`; ensure next-hop resolution and label stack are intact |

### Infrastructure Issues

| Issue | Symptoms | Potential Cause | Solution |
|-------|----------|-----------------|----------|
| **MTU Issues with MPLS Packets** | Large packets dropped | MPLS adds 4 bytes per label; intermediate links may drop oversized frames | Ensure MTU is increased on MPLS-facing interfaces (`mtu 1504` or higher); verify with `show interfaces` |
| **Incorrect Label Stack Depth** | Wrong label count | Improper route leaking or PE misconfiguration | Use `show bgp vpnv4 unicast all` and `show mpls forwarding-table`; verify correct route-target import/export |

### General Troubleshooting Steps
• **Ensure MPLS is enabled** on the relevant interfaces  
• **Confirm that LDP is running** and that LDP neighbor relationships are established  
• **Verify there are no ACLs or firewall rules** blocking TCP/UDP port 646  
• **Check that the router-id is correctly configured** and unique  
• **Confirm VRF configurations** RD and RT values are consistent

---

## Insights

### Performance and Efficiency
• **MPLS improves packet forwarding** by simplifying lookups using labels  
• **It supports multiple services** (VPNs, TE) over a single infrastructure  
• **Combining MPLS with VRF** enables secure, segregated VPN services

### Operational Considerations
• **Consistent configurations and regular monitoring** are crucial to avoid misconfigurations that could lead to performance issues  
• **While MPLS itself doesn't encrypt data** integrating additional security measures (IPsec, etc.) and enforcing strict control over management and label distribution can secure MPLS environments

### Modern Applications
• **MPLS remains relevant** in service provider networks and enterprise WANs  
• **Integration with SD-WAN** and modern overlay technologies extends MPLS utility  
• **Traffic engineering capabilities** continue to provide value for predictable, high-performance networking

---

## Commands
*[To be completed during lab sessions]*
