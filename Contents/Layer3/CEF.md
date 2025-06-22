# CEF - Cisco Express Forwarding
*Cisco proprietary*

## Overview
• Primary high performance packet forwarding mechanism in Cisco; can be hardware or software based  
• Replaced older switching mechanisms like process-switching  
• Routing is precomputed into hardware, enabling wire-speed packet forwarding with minimal CPU overhead; CPU is mostly used for control-plane tasks  
• CEF is scalable and can handle large routing tables without significant performance degradation; CEF maintains a stable FIB and adjacency table  
• L3 relies on TCAM, while L2 relies on CAM

---

## Evolution

### Switching Evolution Timeline
```
Process Switching → Fast Switching → CEF
(CPU-intensive    (cached, partial   (hardware-based,
 per packet)       CPU-based)        precomputed)
```

### CEF Operation Flow
The IP routing table (RIB) is built by routing protocols. CEF then copies the best routes from the RIB into the FIB for fast forwarding decisions. CEF then checks the FIB for the longest prefix match to obtain the next-hop info. Then checks the adjacency table for L2 details. Forwards packets with minimal CPU involvement.

**On ASIC-based devices** (Catalyst switches, ASR) CEF data is in the hardware tables, enabling line-rate forwarding.

---

## Timers
CEF is designed for fast, hardware forwarding, so it does not have explicit "timers" in the same sense as protocols like BGP or IPsec. However, aspects such as adjacency aging may apply → adjacency entries might be refreshed or removed based on underlying protocol updates (ARP or NDP refresh intervals)

---

## Multicast
• **Multicast Forwarding** → CEF supports multicast traffic by maintaining special FIB entries and adjacency lists for multicast groups. Multicast forwarding in CEF uses algorithms like RPF to determine correct multicast paths  
• **Protocol Support** → CEF works with various L3 protocols (IPv4, IPv6, MPLS) and ensures that multicast as well as unicast traffic is switched efficiently in hardware

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **CEF** | A Cisco proprietary IP switching technology that uses a precomputed FIB and Adjacency Table for fast packet forwarding |
| **FIB** | Hardware-optimized copy of routing table specifically for looking up longest-prefix match; mirrors the routing table's best paths. FIB is updated automatically whenever the routing or ARP tables change |
| **Adjacency Table** | Stores L2 next-hop info (MAC, encapsulation details) for each FIB entry (along with rewrite info) |
| **IP Routing Table** | The routing table maintained by the control plane; CEF uses a FIB derived from this table |
| **CEF Switch** | A Cisco router or switch that implements CEF for hardware-based packet switching |
| **CEF Accounting** | Optional to track packets/bytes in the FIB (can add overhead) |
| **CEF with NAT** | CEF is NAT-aware; you must ensure consistent inside/outside setup or risk partial software switching |
| **Multicast Forwarding** | The process by which CEF handles multicast traffic, using specialized entries to forward packets to multiple destinations efficiently |

### Adjacency Types

| Adjacency Type | Description |
|----------------|-------------|
| **ARP Adjacency** | Standard next-hop with known MAC, after ARP is resolved |
| **Glean Adjacency** | When a router knows the interface is connected but triggers an ARP request to determine the specific host's MAC |
| **Null Adjacency** | Used for routes to null0; packets are dropped |
| **Discard Adjacency** | Similar to null but might generate ICMP unreachable |
| **Punt Adjacency** | For traffic that must be handled by the CPU (routing protocol packets, control plane traffic, local traffic) |

---

## CEF Operations

### Processing Steps
1. **Route Learned** → A route is installed into the routing table by some protocol  
2. **FIB Update** → The route's next-hop is added to the FIB with the best-match prefix stored in a tree-based or trie-based structure  
3. **ARP Table** → If the next-hop is on a directly connected subnet the router obtains the next-hop MAC from the ARP table  
4. **Adjacency Table** → The FIB entry references an adjacency for the next-hop, which includes L2 rewrite info (MAC, VLAN tag, encapsulation)  
5. **Forwarding** → When a packet arrives, the router does a CEF lookup on the destination IP in the FIB, finds the adjacency, applies the rewrite and sends it out the correct interface

---

## Load Balancing

| Load Balancing Type | Method | Characteristics |
|---------------------|--------|-----------------|
| **Per-Destination LB** | Hash of source+destination IP or other fields | Packets to a specific source/destination pair always take the same path |
| **Per-Packet LB** | Round-robin approach | Sends each packet on a different path; can cause out-of-order packets in different latency paths |

### Polarization Issue
**Problem:** Multiple devices each doing the same hashing might converge traffic onto one path  
**Solution:** Adjust hashing fields (src/dst) or seeds

---

## CPU vs Hardware

| Platform Type | FIB/Adjacency Storage | Performance |
|---------------|----------------------|-------------|
| **Hardware-based platforms** (Catalyst switches, high-end boxes) | FIB and adjacency data downloaded to hardware ASICs | Line-rate forwarding |
| **Software-based routers** (small platforms) | FIB/adjacency stored in memory | Fast lookup compared to process switching |

---

## Architecture

### Core Components

**Precomputation**  
• The control plane computes routing information and builds a FIB  
• The FIB contains entries for destination prefixes along with the next-hop and interface details

**Adjacency Table**  
• For each FIB entry, the Adjacency Table stores the necessary L2 information for forwarding packets

**Hardware Forwarding**  
• When a packet arrives, the router uses CEF-enabled hardware (ASICs) to look up the destination address in the FIB, retrieve the corresponding adjacency, and forward the packet without CPU intervention

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **High Performance** | CEF is designed for speed and scalability, enabling routers to forward packets at very high rates with minimal CPU involvement |
| **Efficient Resource Utilization** | By using precomputed FIB and Adjacency Tables, CEF reduces processing overhead and improves overall network performance |
| **Scalability** | CEF supports large networks with extensive routing tables, making it ideal for enterprise and SP environments |
| **Improved Stability** | Hardware-based forwarding provides consistent performance even under heavy traffic loads |
| **Integration with Advanced Features** | Works seamlessly with MPLS, QoS and other technologies, enabling robust, multi-service network designs |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Enterprise Networks** | Used in core routers and aggregation switches to support high-speed routing with minimal latency |
| **Data Centers** | CEF's efficiency is critical in data centers where large amounts of traffic must be forwarded rapidly and reliably |
| **SP Networks** | Enables high-performance routing across large networks with numerous customers and extensive routing tables |
| **Routing Protocol Intensive Environments** | In networks where rapid route convergence and dynamic routing updates are common, CEF ensures that packet forwarding remains efficient |

---

## Best Practices/Security Considerations

### Performance Optimization
• **Use CEF in conjunction with QoS and MPLS** for effective traffic engineering. CEF's hardware-based switching supports high-speed forwarding necessary for advanced QoS policies  
• **Ensure consistent routing protocol configuration** which feeds the FIB. Misconfigurations in the control plane can lead to incorrect FIB entries and suboptimal forwarding

### Scalability Management
• **Design routing architecture** so that the FIB does not become overloaded  
• **Use route summarization and efficient address planning** to keep the FIB size manageable

### Security Implementation
• **Secure the control plane** (which builds the FIB) using access controls, firewalls and proper routing protocol authentication  
• **Implement IP Source Guard and DAI** to prevent spoofed packets from being injected into the network or misdirecting CEF forwarding  
• **Keep firmware up-to-date** to protect against vulnerabilities in the ASICs or CEF implementation

### Monitoring and High Availability
• **Use network management systems** to monitor for sudden changes in the FIB or adjacency table that might indicate a routing attack  
• **Design for HA** in the control and data planes. Redundant routers and failover mechanisms ensure that if one device's CEF encounters issues, traffic can be rerouted without significant disruption

---

## Troubleshooting

### Common Issues
• **Packets not forwarding correctly** → Check for route flaps or misconfigurations in your routing protocols, which can cause temporary inconsistencies in the FIB  
• **CEF debugging** → Although CEF is hardware-based and typically doesn't produce extensive debug output, some platforms offer debugging commands to track packet forwarding issues. Use these commands with caution in production environments

---

## Insights
*[To be completed as learning progresses]*

---

## Commands
*[To be completed during lab sessions]*
