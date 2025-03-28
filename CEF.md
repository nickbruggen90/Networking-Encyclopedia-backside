# CEF - Cisco proprietary

### Overview
> * Primary high performance packet forwarding mechanism in Cisco; can be hardware or software based
> * Replaced older switching mechanisms like process-switching
> * Routing is precomputed into hardware, enabling wire-speed packing forwarding with minimal CPU overhead; CPU is mostly used for control-plane tasks
> * CEF is scalable and can handle large routing tables without significant performance degradation; CEF maintains a stable FIB and adjacency table
> * L3 relies on TCAM, while L2 relies on CAM
---
### Evolution:
*process switching (cpu-intensive per packet) → fast switching (cached, partial CPU-based) → CEF*  
The IP routing table (RIB) is built by routing protocols. CEF then copies the best routes from the RIB into the FIB for fast forwarding decisions. CEF then checks the FIB for the longest prefix match to obtain the next-hop info. Then checks the adjacency table for L2 details. Forwards packets with minimal CPU involvement.
>    * On ASIC-based devices (Catalyst switches, ASR) CEF data is in the hardware tables, enabling line-rate forwarding.
---
