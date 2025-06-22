# NetFlow/Flexible NetFlow
*Cisco proprietary / RFC 5101 and RFC 5102 (IPFIX)*

## Overview
• **NetFlow** is a Cisco proprietary network protocol designed to collect IP traffic information as it flows through routers and switches  
• Provides visibility into network usage, supports capacity planning and aids in troubleshooting and security analysis by exporting metadata about IP flows to a centralized collector  
• **Flexible NetFlow (FNF)** is an enhanced, customizable version of NetFlow that allows network administrators to define their own flow records  
• Enables selection of specific fields (IP addresses, ports, protocols, VLANs, etc.) to capture for more granular and application-specific traffic analysis  
• NetFlow provides detailed insights into network behavior that are invaluable for proactive network management and security

---

## Timers

| Timer Type | Default Value | Purpose |
|------------|---------------|---------|
| **Active Flow Timeout** | 60 seconds | Interval at which a flow is forcibly expired and exported even if it is still active |
| **Inactive Flow Timeout** | 15 seconds | Duration after which a flow is removed from the cache if no packets have been seen |
| **Template Timeout** | 60 seconds | How often template information is sent to collectors |

---

## Multicast
• **Multicast flows are recorded** like unicast but may require template customization (group IPs, incoming interfaces)  
• **Special considerations** for multicast group addresses and receiver tracking

---

## Protocols/Ports

| Protocol | Port | Usage |
|----------|------|-------|
| **NetFlow v5/v9** | UDP 2055 | Default NetFlow export |
| **IPFIX** | UDP 4739 | IETF-standardized successor to NetFlow v9 |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **NetFlow** | A Cisco-developed technology for capturing and exporting metadata about IP flows in a network |
| **Flexible NetFlow** | An advanced version of NetFlow that lets administrators create custom flow records by selecting specific fields to monitor |
| **Flow Exporter** | A network device (router or switch) that collects flow records and exports them to a NetFlow collector |
| **Collector** | A server or system that receives, aggregates and stores NetFlow data for analysis |
| **Flow Cache** | Memory on the exporter where flow records are temporarily stored before being exported |
| **Active Flow Timeout** | The interval at which a flow is forcibly expired and exported even if it is still active |
| **Inactive Flow Timeout** | The duration after which a flow is removed from the cache if no packets have been seen |
| **IPFIX** | IP Flow Information Export - the IETF-standardized successor to NetFlow v9, sharing similar concepts with Flexible NetFlow |

---

## Header/Record Structure Breakdown

### Flow Record Components

| Component Type | Description | Examples |
|----------------|-------------|----------|
| **Key Fields** | Define the flow (used for flow identification) | Source/destination IP addresses, ports, protocol, VLAN |
| **Non-Key Fields (Counters)** | Capture flow statistics | Byte/packet count, timestamps, TCP flags |
| **Counter Fields** | Track quantitative data | Byte and packet counts |
| **Timestamp Fields** | Record temporal information (optional) | Flow start and end time |

### Flexible NetFlow Customization
• **For Flexible NetFlow, you can customize which fields are included** making the flow record structure highly adaptable to your needs  
• **Template-based approach** allows different flow definitions for different use cases

---

## Key Benefits

| Benefit | Description | Value |
|---------|-------------|-------|
| **Network Visibility** | Provides granular insights into traffic flows | Performance monitoring, capacity planning, security analysis |
| **Troubleshooting** | Helps identify congestion points, misconfigurations and security anomalies | Faster problem resolution |
| **Detailed Analysis** | Supports analysis of network usage patterns | Network optimization and design improvements |
| **Customizable (FNF)** | Tailor the flow record to capture only relevant fields | Application-specific monitoring |
| **Granular Visibility** | See exactly who uses what resources and when | Resource accountability |
| **Low Overhead** | Efficient monitoring with minimal CPU impact | When properly tuned |
| **Security and Threat Detection** | Detect DDoS, exfiltration, scanning, etc. | Enhanced security posture |
| **Capacity Planning** | Track bandwidth trends over time | Smarter upgrade decisions |

---

## Use Cases

| Use Case | Application | Benefits |
|----------|-------------|----------|
| **Performance Monitoring** | Track latency, throughput and application performance in real time | End-to-end application and link visibility |
| **Capacity Planning** | Analyze traffic trends to plan for network expansion or upgrades | Data-driven infrastructure decisions |
| **Security Analysis** | Detect anomalies, DDoS attacks or unauthorized traffic flows | Identify infected hosts, anomaly detection, forensic visibility |
| **Application Troubleshooting** | Identify which applications consume the most bandwidth | Understand application interactions across the network |
| **Compliance and Audit** | Log flow data for reporting and post-event review | Regulatory compliance and forensic analysis |

---

## Architecture

### Core Components

| Component | Function | Description |
|-----------|----------|-------------|
| **Flow Exporter** | Generate flow records | Network devices that monitor traffic, aggregate packets into flow records and export them |
| **Flow Collector** | Receive and store data | Centralized server that receives, stores and analyzes flow data (SolarWinds, PRTG, ELK) |
| **Flow Analyzer** | Process and visualize | Tool or application that processes flow data to generate reports and insights |

### Operational Flow

```
Packet Inspection → Flow Identification → Cache → Export on Timeout
```

| Step | Process | Description |
|------|---------|-------------|
| **1. Packet Inspection** | Exporter examines packets | Identifies flows and creates flow records in a flow cache |
| **2. Flow Identification** | Categorization | Groups packets into flows based on key fields |
| **3. Caching** | Temporary storage | Records stored in flow cache until timeout |
| **4. Export** | Data transmission | When flows expire (active or inactive timeout), records are exported (typically via UDP) to the collector |
| **5. Analysis** | Processing | Collector aggregates and processes the data for analysis, reporting and troubleshooting |

### Configuration Scope
• **Can be configured per interface or globally** depending on monitoring requirements

---

## Flexible NetFlow Enhancements

| Enhancement | Description | Benefits |
|-------------|-------------|----------|
| **Custom Flow Records** | Define which fields to capture (VLAN IDs, MPLS labels, QoS markings, etc.) | Meet specific monitoring requirements; define unique templates for voice, MPLS, IoT |
| **Enhanced Granularity** | More detailed and relevant information | Tailored to particular applications or traffic types |
| **Multiple Templates** | Support multiple flow record templates on the same interface | Different monitoring needs on same interface |
| **Integration** | Works with QoS, NBAR2 and NetFlow-Lite | Comprehensive network monitoring |
| **MPLS and QoS Awareness** | Tag flows with DSCP, CoS, VLAN, MPLS labels | Enhanced traffic classification |

---

## Operational Overview

### Flow Processing Steps

| Step | Process | Description |
|------|---------|-------------|
| **1. Flow Record Creation** | Each flow defined by key parameters | Source IP, destination IP, protocol, ports. Packets matching these criteria are aggregated into flow records |
| **2. Flow Export** | Records sent to collector | Once a flow meets active or inactive timeout criteria, record is exported for storage and analysis |
| **3. Sampling** | Reduced processing load | In high-speed networks, packet sampling may reduce processing load by collecting statistics from a fraction |
| **4. Analysis** | Pattern identification | Collected flow records analyzed to determine traffic patterns, identify anomalies and support network planning |

### Flexible NetFlow Configuration Process

| Step | Action | Description |
|------|--------|-------------|
| **1** | **Define a flow record** | Choose key fields (IP, port, VLAN) |
| **2** | **Create a flow exporter** | Set destination collector, port, export protocol |
| **3** | **Create a flow monitor** | Binds record + exporter |
| **4** | **Apply to interface** | Attach monitor to ingress/egress direction |
| **5** | **Collector receives + parses data** | Generates traffic reports or security events |

---

## Best Practices/Security Considerations

### Data Protection
• **NetFlow data can reveal detailed information about network traffic** → Protect access to flow records and the collector with strong access controls  
• **Ensure NetFlow exporters and collectors are secured** against unauthorized access or tampering (via ACLs, secure network management practices)  
• **Protect collectors** (ACLs, segmentation, VPN across WAN)

### Performance Optimization
• **High-frequency NetFlow sampling can consume CPU and memory resources** → Monitor and adjust sampling rates or timeouts to avoid degrading device performance  
• **Adjust sampling rates** to balance between comprehensive data collection and resource utilization  
• **Monitor exporter CPU/RAM usage** → High-rate export can be taxing

### Transport Security
• **While NetFlow itself does not provide encryption** consider securing the transport (via VPN tunnels) if flow data is sent over untrusted networks  
• **Don't export over the Internet unless encrypted** (use IPsec/GRE)

### Configuration Guidelines
• **Use active and inactive timeouts** that match the network's traffic characteristics  
• **Match active/inactive timeouts** to your application behaviors  
• **Use collectors with strong parsing and alert capabilities**

### Monitoring and Documentation
• **Use automated tools and dashboards** to analyze flow data, and adjust policies as network conditions evolve  
• **Maintain thorough documentation** of your NetFlow and Flexible NetFlow settings for troubleshooting and audits  
• **Always document which interfaces/records** are being monitored and why

### Access Control
• **Restrict access to NetFlow data** to ensure proper network segmentation  
• **Ensure proper network segmentation** for collector placement

---

## Common Issues and Fixes

| Issue | Symptoms | Solution |
|-------|----------|----------|
| **No Data Received** | Collector shows no NetFlow data | Check ACLs, ports (2055, 4739) and export targets |
| **Template Missing on Collector** | Collector cannot parse flow data | Ensure exporter is sending templates periodically (template timeout) |
| **Duplicate or Missing Flows** | Inconsistent flow data | Adjust active/inactive timeouts, disable sampling for accuracy |
| **High CPU on Exporter** | Device performance degradation | Reduce flow monitor scope or enable sampling |

### Troubleshooting Process
1. **Verify network connectivity** between exporter and collector  
2. **Check port accessibility** (UDP 2055/4739)  
3. **Validate template exchange** between exporter and collector  
4. **Review timeout configurations** for flow aging  
5. **Monitor resource utilization** on exporter devices

---

## Insights

### Modern Implementation
• **FNF is the preferred method** for modern Cisco platforms - use it over legacy NetFlow v5  
• **The ability to define custom flow records** allows for tailored monitoring, especially useful in environments with complex or unique traffic patterns

### Integration Benefits
• **When combined with other monitoring systems** (SNMP, NetConf, telemetry), NetFlow gives a more complete picture of network health  
• **NetFlow + NBAR2 = app-level visibility** for comprehensive application monitoring

### Performance Considerations
• **In high-throughput networks, careful configuration** (sampling, timeout) is critical to avoid impacting device performance  
• **Sampling trade-offs** → Balance between data completeness and system performance

### Operational Reality
• **Template-based approach** in newer NetFlow versions provides flexibility but requires proper template management  
• **Collector capabilities** are critical → Choose collectors that can handle your data volume and provide meaningful analysis

---
