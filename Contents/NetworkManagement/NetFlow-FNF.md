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
| **IPFIX** | IP Flow Information Export - the IETF-standardized successor to NetFlow v9, sharing similar concepts with Flexi
