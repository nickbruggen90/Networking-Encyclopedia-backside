# NetFlow/Flexible NetFlow - Cisco proprietary / RFC 5101 and RFC 5102 (IPFIX)

### Overview:
> * NetFlow is a Cisco proprietary network protocol designed to collect IP traffic information as it flows through routers and switches. It provides visibility into network usage, supports capacity planning and aids in troubleshooting and security analysis by exporting metadata about IP flows to a centralized collector
> * Flexible NetFlow (FNF) is an enhanced, customizable version of NetFlow that allows network administrators to define their own flow records, selecting which fields (such as IP addresses, ports, protocols and VLANs, etc) to capture. This flexibility enables more granular and application-specific traffic analysis
> * NetFlow provided detailed insights into network behavior that are invaluable for proactive network management and security 
---
### Timers:
> * Active Flow Timeout = 60 sec
> * Inactive Flow Timeout = 15 sec
> * Template Timeout = 60 sec
---
### Multicast:
> * Multicast flows are recorded like unicast but may require template customization (group IPs, incoming interfaces)
---
### Protocol/Port(s):
> * UDP: 2055 (default NetFlow v5/v9 export)
>   * IPFIX – UDP: 4739
---
### Terminology/Definitions:
> * NetFlow – a Cisco-developed technology for capturing and exporting metadata about IP flows in a network
> * Flexible NetFlow – an advanced version of NetFlow that lets administrators create custom flow records by selecting specific fields to monitor
> * Flow – a network device (router or switch) that collects flow records and exports them to a NetFlow collector
> * Collector – a server or system that receives, aggregates and stores NetFlow data for analysis
> * Flow Cache – memory on the exporter where flow records are temporarily stores before being exported
> * Active Flow Timeout – the interval at which a flow is forcibly expired and exported even if it is still active
> * Inactive Flow Timeout – the duration after which a flow is removed from the cache if no packets have been seen
> * IP Flow Information Export (IPFIX) – the IETF-standardized successor to NetFlow v9, sharing similar concepts with Flexible NetFlow
---
### Header/Record Structure Breakdown:
> * Key Fields – define the flow, such as source/destination IP addresses, ports, protocol and VLAN, etc
> * Non-Key Fields (counters) – byte/packet count, timestamps, TCP flags, etc
> * Counter Fields – capture statistics like byte and packet counts
> * Timestamp Fields (optional) – may record the start and end time of a flow
> * *For Flexible NetFlow, you can customize which fields are included, making the flow record structure highly adaptable to your needs*
---
### Key Benefits:
> * Network Visibility – provides granular insights into traffic flows for performance monitoring, capacity planning and security analysis
> * Troubleshooting – helps identify congestion points, misconfigurations and security anomalies
> * Supports detailed analysis of network usage patterns, aiding in optimization and network design
> * Customizable (Flexible NetFlow) – tailor the flow record to capture only the fields that are more relevant to your monitoring needs
> * Granular visibility – see exactly who is uses what resources and when
> * Customizable (FNF) – tailor data collection per app, interface or security use case
> * Low Overhead – efficient monitoring with minimal CPU hit (when properly tuned)
> * Security and Threat Detection – DDoS, exfiltration, scanning, etc
> * Capacity Planning – track bandwidth trends over time for smarter upgrades
---
### Use Cases:
> * Performance Monitoring – track latency, throughput and application performance in real time; end to end application and link visibility
> * Capacity Planning – analyze traffic trends to plan for network expansion or upgrades
> * Security Analysis – detect anomalies, DDoS attacks or unauthorized traffic flows; identify infected hosts, anomaly detection, forensic visibility
> * Application Troubleshooting – identify which applications are consuming the most bandwidth and how they interact across the network
> * Compliance and Audit – log flow data for reporting and post-event review
---
### Architecture:
> * Components –
>   * Flow Exporter - the network device that monitors traffic, aggregates packets into flow records and exports them; devices that generate flow records (routers, switches, etc)
>   * Flow Collector - a centralized server that receives, stores and analyzes flow data; aggregates exported data (SolarWinds, PRTG, ELK)
>   * Flow Analyzer - a tool or application that processes flow data to generate reports and insights; parses and visualizes traffic patterns
> * Operation –
>   * The exporter examines packets, identifies flows and creates flow records in a flow cache
>   * When flows expire (active or inactive timeout), records are exported (typically via UDP) to the collector
>   * The collector aggregates and processes the data for analysis, reporting and troubleshooting
> * Flow Process –
>   * packet inspection → flow identification → cache → export or timeout
>   * *Can be configured per interface or globally*
---
### Flexible NetFlow Enhancements:
> * Custom Flow Records – administrators can define which fields to capture (VLAN IDs, MPLS labels, QoS markings, etc) to meet specific monitoring requirements; define unique templates for voice, MPLS, IoT, etc
> * Enhanced Granularity – provides more detailed and relevant information tailored to particular applications for traffic types
> * Multiple Templates – supports multiple flow record templates on the same interface
> * Integration – can work with QoS, NBAR2 and NetFlow-Lite
> * MPLS and QoS Awareness – tag flows with DSCP, CoS, VLAN, MPLS labels
---
### Operational Overview:
> * Flow Record Creation – each flow is defined by a set of key parameters (source IP, destination IP, protocol, ports). As packets matching these criteria traverse the device, they are aggregated into flow records
> * Flow Export – once a flow meets the active or inactive timeout criteria, the record is exported to a collector for storage and analysis
> * Sampling – in high-speed networks, packet sampling may be used to reduce the processing load. Instead of capturing every packet, the exporter collects statistics from a fraction
> * Analysis – collected flow records are analyzed to determine traffic patterns, identify anomalies and support network planning and security monitoring
>   * 1. Define a flow record – choose key fields (IP, port VLAN)
>   * 2. Create a flow exporter – set destination collector, port, export protocol
>   * 3. Create a flow monitor – binds record + exporter
>   * 4. Apply to interface – attach monitor to ingress/egress direction
>   * 5. Collector receives + parses data – generates traffic reports or security events
---
### Best Practices/Security Considerations:
> * NetFlow data can reveal detailed information about network traffic. Protect access to flow records and the collector with strong access controls
> * Ensure that NetFlow exporters and collectors are secured against unauthorized access or tampering (via ACLs, secure network management practices)
> * High-frequency NetFlow sampling can consume CPU and memory resources. Monitor and adjust sampling rates or timeouts to avoid degrading device performance
> * While NetFlow itself does not provide encryption, consider securing the transport (via VPN tunnels) if flow data us sent over untrusted networks
> * Adjust sampling rates to balance between comprehensive data collection and resource utilization
> * Use active and inactive timeouts that match the networks traffic characteristics
> * Restrict access to NetFlow data to ensure proper network segmentation
> * Use automated tools and dashboards to analyze flow data, and adjust policies as network conditions evolve
> * Maintain thorough documentation of your NetFlow and Flexible NetFlow settings for troubleshooting and audits
> * Protect collectors (ACLs, segmentation, VPN is across WAN)
> * Don’t export over the Internet unless encrypted (use IPsec/GRE)
> * Match active/inactive timeouts to your application behaviors
> * Monitor exporter CPU/RAM usage; high-rate export can be taxing
> * Use collectors with strong parsing and alert capabilities
> * Always document which interface/records are being monitored and why
---
### Common Issues and Fixes:
> * No Data Received?
>   * Fix: check ACLs, ports (2055, 4639) and export targets
> * Template Missing on Collector?
>   * Fix: ensure exporter is sending periodically (template timeout)
> * Duplicate or Missing Flows?
>   * Fix: adjust active/inactive timeouts, disable sampling for accuracy
> * High CPU on Exporter?
>   * Fix: reduce flow monitor scope or enable sampling
---
### Insights:
> * The ability to define custom flow records allows for tailored monitoring, which is especially useful in environments with complex or unique traffic patterns
> * When combined with other monitoring systems (SNMP, NetConf, telemetry), NetFlow gives a more complete picture of network health
> * In high-throughput networks, careful configuration (sampling, timeout) is critical to avoid impacting device performance
> * FNF is the preferred method for modern Cisco platforms - use it over legacy NetFlow v5
> * NetFlow + NBAR2 = app-level visibility
---
### Commands:
> *
---
