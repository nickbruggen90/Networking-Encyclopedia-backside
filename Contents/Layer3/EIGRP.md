# EIGRP - Enhanced Interior Gateway Routing Protocol
*Cisco proprietary*
*RFC 7868 (EIGRP)*

## Overview
• **Is an advanced distance vector protocol**, incorporating features of both distance vector and link-state protocols  
• **EIGRP only sends routing updates when there's a change**, and those updates are only sent to the affected neighbors (rather than flooding the entire network)  
• **Uses Diffusing Update Algorithm (DUAL)** to ensure loop-free, rapid convergence and efficient routing computation  
• **Considers factors like bandwidth, delay, load and reliability** to compute best path  
• **Supports VLSM and route summarization**  
• **Summarization can happen at any interface**  
• **Uses unequal-cost load balancing**; allowing traffic to be load balanced over routes with different metrics

---

## Timers

| Interface Type | Hello Timer | Hold Timer |
|----------------|-------------|------------|
| **LAN/Ethernet** | 5 sec | 15 sec |
| **WAN/Serial** | 60 sec | 180 sec |

---

## Multicast

| IP Version | Address | Usage |
|------------|---------|-------|
| **IPv4 Multicast** | 224.0.0.10 | EIGRP hello and update messages |
| **IPv6 Multicast** | ff02::A | EIGRP hello and update messages |

---

## Protocol/Port(s)

| Protocol | Number | Usage |
|----------|--------|-------|
| **IP** | 88 | EIGRP packet transmission |

---

## Priorities

| Parameter | Value | Description |
|-----------|-------|-------------|
| **Administrative Distance (Internal)** | 90 | Routes learned from EIGRP neighbors in same AS |
| **Administrative Distance (External)** | 170 | Routes redistributed into EIGRP |
| **Administrative Distance (Summary)** | 5 | When you manually configure a summary under an interface |
| **Composite Metric** | Bandwidth + Delay | Primary calculation (by default) |
| **Hop Limit** | Default 100, Maximum 255 | Maximum hop count |

---

## K-Values

| K-Value | Metric Component | Default | Weight | Purpose |
|---------|------------------|---------|---------|---------|
| **K1** | Bandwidth | 1 | Enabled | Slowest link bandwidth in path |
| **K2** | Load | 0 | Disabled | Current utilization of link |
| **K3** | Delay | 1 | Enabled | Cumulative delay of path |
| **K4** | Reliability | 0 | Disabled | Reliability of the path |
| **K5** | MTU | 0 | Disabled | Maximum Transmission Unit |

**Default Formula:** Metric = (K1 × Bandwidth + K3 × Delay) × 256

---

## Active/Passive Router

| State | Description |
|-------|-------------|
| **Passive** | Working router, EIGRP is not searching for an alternative |
| **Active** | Querying neighbors for a new route if the route fails and no FS is available |

---

## Terminology

| Term | Definition |
|------|------------|
| **Feasible Distance (FD)** | The lowest calculated metric from the local router to the destination. The best metric to reach the prefix from the local router's perspective |
| **Reported Distance (RD)** | The metric that the neighbor advertises as its distance from the destination. Neighbor's best metric to the prefix |
| **Successor** | The primary next-hop with the lowest metric (FD); installed into routing table |
| **Feasible Successor (FS)** | Backup next-hop that meets the feasible condition (its RD < current FD) |
| **Neighbor Table** | Lists directly connected EIGRP neighbors |
| **Topology Table** | Holds all learned routes and their metrics (FD, RD) |
| **Routing Table** | The best (successor) routes from the topology table |

---

## EIGRP Packet Types

| Packet Type | Purpose | Description |
|-------------|---------|-------------|
| **Hello** | Neighbor discovery | Discover and maintain neighbor relationships, forms adjacency |
| **Update** | Route advertisement | Advertise route changes to neighbors (unicast or multicast) |
| **Query** | Route request | Request route information when a destination becomes unreachable and has no FS |
| **Reply** | Query response | Respond to query packets with route information |
| **Acknowledgement** | Reliable delivery | Acknowledge receipt of update/query/reply packets to ensure reliable delivery |

---

## EIGRP Stub Routing

### Overview
• **Stub routers do not forward traffic between external neighbors**. They typically only have one path to the core  
• **EIGRP stubs reduce query scopes**, helping with faster convergence and less overhead in hub-and-spoke topologies

### Common Stub Options

| Option | Description |
|--------|-------------|
| **receive-only** | Receives routes, does not advertise anything |
| **connected** | Only advertises connected routes |
| **summary** | Only advertises summary routes, if configured |
| **static** | Only advertises static routes |

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Rapid Convergence** | EIGRP typically converges faster than many other routing protocols, minimizing downtime when network changes occur. DUAL helps ensure optimal route selection and quick recovery from link failure |
| **Efficient Resource Utilization** | Uses incremental updates, sending only changes rather than the entire routing table, reducing bandwidth usage and CPU load. Supports load balancing over equal-cost and unequal-cost paths, which optimizes resource usage and traffic distribution |
| **Scalability and Flexibility** | EIGRP can support large networks with relatively simple configuration. It allows for hierarchical routing designs and can be tuned using metrics like bandwidth, delay, reliability, load and MTU |
| **Easy Configuration and Maintenance** | EIGRP's configuration is generally straightforward, and its automatic route summarization (in classful environments) simplifies management. It's well-integrated in Cisco environments, with extensive documentation and support |
| **Hybrid Routing Protocol** | EIGRP combines the benefits of both distance-vector and link-state protocols, providing efficient routing decisions with lower complexity |
| **Robust Metric Calculation** | EIGRP's composite metric allows administrators to influence path selection with granular control over various parameters |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Enterprise Networks** | Suitable for medium to large enterprise networks where fast convergence and efficient load balancing are critical. Often used in environments where the network topology is dynamic, and quick adaptation to changes is needed |
| **Campus and Branch Networks** | Ideal for campus networks where multiple pathways exist, and load balancing across them improves overall network performance. Can be used to interconnect branch offices with rapid failover in case of link outages |
| **SP Environments** | Employed by service providers for internal routing, especially in Cisco-centric networks. Its scalability and efficient resource usage make it a good fit for complex multi-tier architecture |
| **Mixed Routing Environments** | In scenarios where multiple routing protocols are deployed, EIGRP can coexist with protocols like OSPF and BGP. Its flexibility in metric configuration makes it adaptable to networks with diverse link characteristics |

---

## Best Practices/Security Considerations

### Configuration Management
• Use a well planned IP addressing scheme and consistent configuration parameters  
• Regularly check neighbor relationships and topology changes  
• Use route summarization at network boundaries to reduce routing overhead  
• Only use auto-summarization in simple, contiguous networks; use manual summarization  
• Maintain updated network diagrams and configuration notes  
• Use EIGRP authentication  
• Avoid adjusting K-Values except when absolutely necessary

### Security Hardening
• Use route maps, distribute lists or prefix lists to filter incoming and outgoing EIGRP updates. This limits the spread of incorrect or malicious routing information  
• Implement ACLs on interfaces to restrict which devices can become EIGRP neighbors  
• Secure the control plane by isolating routing traffic from general user traffic, using management VLANs and proper network segmentation  
• Regularly monitor EIGRP neighbor relationships and routing table updates to quickly identify anomalies  
• Enable logging and SNMP traps for EIGRP events, such as authentication failures or unexpected route changes  
• Keep network device firmware updated and address any vulnerabilities in EIGRP implementations  
• Regularly review and audit routing configurations and policies to ensure compliance with security best practices

---

## Troubleshooting

### Common Issues
• **AS number mismatch**  
• **K-Values mismatch**  
• **Auth mismatch**  
• **Passive interface** (no adjacency on that interface)  
• **Incorrect network statements**

---

## Insights

### Operational Considerations
• **Stub configs in EIGRP reduce unnecessary queries**  
• **Summarization at the core/distribution reduces LSA and route churn**  
• **Neighbor Requirements:** same subnet, same AS number, matching K-Values (metric weights), and authentication settings

---

## Commands

*[To be completed during lab sessions]*
