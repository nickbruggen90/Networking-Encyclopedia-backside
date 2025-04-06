# OSPF IPv4 - RFC 2328 (OSPFv2, IPv4)

### Overview
* A link-state IGP protocol used within an AS  
* Builds a complete map of the network (LSDB) and calculates shortest path to each destination  
* OSPF scales well in large enterprise networks thanks to its use of areas, reducing the size of LSDBs  
* Priority and RID (loopback IP takes precedence or highest active IP) are used to elect DR/BDR  
---
### Timers 
> * Hello Timer *broadcast and P2P* = 10 sec || Dead Timer = 40 sec (x4 Hello)  
> * Hello Timer *NBMA, serial, Frame Relay* = 30 sec || Dead Timer = 120 sec (x4 Hello)  
> * LSA Refresh Timer = 1800 sec (30 min)
> * LSA Retransmission Timer = 5 sec  
> * LSA MaxAge Timer = 3600 sec (60 min)  
---
### Multicast:
> * IPv4 Multicast (OSPFv2)
> * All OSPF Routers: 224.0.0.5
> * DR/BDR Routers: 224.0.0.6
> * IPv6 Multicast (OSPFv3)
> * All OSPF Routers: FF02::5
> * All DR/BDR Routers: FF02::6
---
### Protocol/Port:
> * IP: 89
---
### Priorities:
> * Administrative Distance: 110
---
### Terminology/Definitions:
> * Router ID (RID) – a unique 32-bit number that identifies each OSPF router (often the highest IP on an active interface)
> * LSA – packets used by OSPF to exchange routing and topology information
> * LSDB – a collection of LSAs that form a map of the network
> * Adjacency – a relationship between two OSPF routers that exchange routing information
> * DR – acts as  a central point in exchanging LSAs; generates network LSAs
> * BDR – takes over if DR fails
> * Internal Router – generates router link LSAs
> * ABR – a router that connects one or more OSPF areas to the backbone. It maintains separate LSDBs for each area it’s connected to and summarize routing information between areas, acting as a gateway for inter-area traffic
> * ASBR – a router that redistributes external routes (from outside the OSPF domain) into the OSPF process. It can exist within any OSPF area and is responsible for originating Type 5 LSAs or Type 7 LSAs
---
### OSPF Packet Types:
> * Hello (1) – establishes and maintains neighbor relationships
> * BDB (2) – summarizes LSDB; used during the initial exchange
> * LSR (3) – requests more detailed LSA information
> * LSU (4) – sends requested LSAs to neighbors
> * LSAck (5) – confirms receipt of LSAs
---
### Adjacency States:
> * Down – no information is being exchanged
> * Init – a router has received a Hello but two-way communication is not established; the local RID is not listed in the neighbors Hello
> * 2-way – bidirectional communication is confirmed; on multi-access networks(broadcast networks) this state is used to elect DR/BDR
> * ExStart – the start of database exchange; router decides if either M or S will describe their LSDB
> * Exchange – routers exchange DBD packets summarizing their LSDB
> * Loading – routers request more missing or new LSAs via LSRs and respond with LSUs
> * Full – adjacency is fully established; databases are synchronized
---
### Network Types:
> * Broadcast (Ethernet) – elects DR/BDR to reduce flooding; all other routers are DROTHER
> * NBMA (frame relay) – refers to network segments that allow multiple devices to communicate over a shared infrastructure but do no support native broadcast or multicast traffic
> * Point-to-Point – no DR/BDR needed; simplified adjacency, one neighbor per interface; uses Type 1 LSA; does not appear as a separate LSA in the LSDB, it is recorded as a direct connection between two routers
> * Point-to-Multipoint – treats each subinterface as a separate adjacency, no DR/DBR on each link
> * Loopback – OSPF sees loopbacks as a stub host unless configured otherwise
---
> * *DB/BDR Election:* Routers in a broadcast or NBMA segment choose DR/BDR based on highest OSPF priority. Priority 0 means the router is ineligible to be DR/BDR. A tie results in the highest RID winning. DR is responsible for flooding LSAs to others in the segment; BDR takes over if DR fails. Once elected, DR/BDRs do not preempt; so set OSPF priorities carefully.
---
### Areas / Hierarchical Design:
> * Backbone (Area 0) – core area; all non-backbone areas must connect to Area 0 (directly or via virtual links); acts as the central hub for inter-area route exchange
> * Non-Backbone Areas (normal areas) – regular areas that can contain any LSA types (inter-area and intra-area routes)
> * Stub Area – does not receive external (Type 5) LSAs; uses a default route to the ABR; the ABR injects a default route to Type 3 LSA for everything external
> * Totally Stubby Area (Cisco proprietary) – further restricts LSAs (blocks Type 5 and Type 3); allowing only a default route and intra-area routes. The ABR sends a single default route
> * NSSA – similar to stub area but allows a limited form of external routes via Type 7 LSAs injected by an ASBR inside the NSSA
> * Totally NSSA – a variant that restricts summary LSAs from other areas in addition to external routes
> * Virtual Links – used to connect non-contiguous Area 0 segments over a non-backbone area; configured between ABRs that share a common non-backbone area
---
### OSPF LSA Types: (Types 1-5, 7,8 is IPv4)
> * Type 1 (Router LSA) – generated by every router; contains information about the routers directly connected interfaces; used in Point-to-Point connections; flooded within the local area only
> * Type 2 (Network LSA) – generated by DR on broadcast (Ethernet) and NBMA networks; describes all routers connected to the network; generated only on multi-access networks; OSPF does not generate Type 2 LSA when only one device is present; flooded within the area
> * Type 3 (Summary LSA) – generated by ABRs; advertises inter-area routes; summarization typically happens here
> * Type 4 (ASBR Summary LSA) – generated by ABR; advertises a route to an ASBR
> * Type 5 (External LSA) – generated by ASBR; used to advertise routes redistributed into OSPF from other routing protocols like BGP or static routes. Flooded throughout OSPF unless blocked by stub rules
> * Type 7 LSA – used in NSSA areas to advertise external routes; converted to Type 5 by ABR when crossing into the backbone
---
### OSPF Route Types and Prioritization:  
> O → O IA → N1 → E1 → N2 → E2
> * O – routes learned within the same OSPF area. These are considered the most trustworthy and preferred because they are internal to the area and require no inter-area routing.
> * O IA – routes learned from another area via an ABR. Still trusted, by slightly less than intra-area routes
> * N1 – external routes learned from a NSSA where Type 1 cost is used. The metric includes both the external cost and the internal OSPF cost to reach the advertising ASBR
> * E1 – routes from outside the OSPF domain redistributed into OSPF by an ASBR. Like N1, the metric includes both the internal OSPF cost and the external cost
> * N2 – external routes from an NSSA where only the external metric is considered, ignoring internal OSPF cost to the ASBR
> * E2 – default type for routes redistributed into OSPF. Metric reflects only the external cost, regardless of the internal OSPF cost to the ASBR. Least preferred.
---
### OSPF Authentication Options:
> * Null Auth – no auth; default
> * Simple Password – a plain-text password
> * MD5 Auth – provides integrity and authenticity
> * OSPFv3 (IPv6) – auth is handled by IPsec layer rather than with OSPF itself
---
### Header Breakdown & Definitions:
#### OSPF Common Header =  
> * | Version | Type | Packet Length | RID | Area ID | Checksum | AuType | Authentication |
>   * 1 - Version (2 for IPv4, 3 for IPv6)  
>   * 1 - Type (1 = Hello, 2 = DBD, 3 = LSR, 4 = LSU, 5 = LSAck)  
>   * 2 - Packet Length (header + payload length)  
>   * 4 - RID (unique ID of router sending packet)  
>   * 4 - Area ID (area ID which packet belongs; 0.0.0.0 for Area 0)  
>   * 2 - Checksum  
>   * 2 - AuType (0 = no auth, 1 = plain-text, 2 = MD5 or cryptographic)  
>   * 8 - Authentication (password, MD5 or zeroed)  
*OSPFv3 for IPv6 auth is handled differently*  

```
OSPF Common Header Definitions:
```

#### OSPF Hello Packet (Type 1) = 
Purpose: Hello packets discover neighbors, elect DR/BDR on broadcast/NBMA networks and ensure two-way communication; maintains adjacencies
> * | Common Header | Network Mask | Hello Int | Options | Router Prio. | Dead Int | DR | BDR | Neighbor ID |
>   * 4 - Network Mask (subnet mask of sending interface)
>   * 2 - Hello Interval
>   * 1 - Options (bitmask describing OSPF capabilities)
>   * 1 - Router Priority (used in DR/DBR election)
>     * 1 = default 
>     * 0 = ineligible
>   * 4 - Dead Interval 
>   * 4 - DR (IP of current DR on segment; 0.0.0.0 if none)
>   * 4 - BDR (IP of current DBR; 0.0.0.0 if none)
>   * variable - Neighbor IDs

```
OSPF Hello Packet (Type 1) Header Definitions:
> * Neighbor IDs – list of RIDs from which Hellos have been seen on this interface within the Dead interval
```

#### OSPF DBD Packet (Type 2) = 
Purpse: DBD packets provide a high-level summary of each LSA so neighbors can check what they have or are missing. Summaries of LSDB content to compare databases.
> * | Common Header | Interface MTU | Options | DBD | DD Sequence Number | LSA Headers |
>   * 2 - Interface MTU
>   * 1 - Options (same optional capability bits as in Hello)
>   * 1 - DBD *flags*
>     * I (initial) - set if this is the first DBD in a series
>     * M (more) - more DBD packets to follow
>     * MS (master/slave) - indicates M/S			 
>   * 4 - DD Sequence Number
>   * variable - LSA Headers  

```
OSPF DBD Packet (Type 2) Header Definitions:
> * DBD Sequence Number - sequence number for the master/slave relationship to ensure DBD packets are processed in the correct order; maintained by Master
> * LSA Headers - a list of LSA headers that describe what is in the routers LSBD
```

#### OSPF LSR Packet (Type 3) =
Purpose: A neighbor uses LSR to ask for specific details of LSAs that it has determined missing or out of date. Requests missing or newer LSAs.
> * |  Common Header | LSA Requests |
>   * variable - LSA Requests
>   * 4 - LSA Type
>   * 4 - LS ID
>   * 4 - Advertising Router

```
OSPF LSR Packet (Type 3) Definitions:
> * LSA Requests –  one or more entries, each specifying an LS Type, LS ID and Advertising Router that the router wants from its neighbor
> * LSA Type – ????
> * Link State ID – ????
> * Advertising Router – RID of who originated the LSA
```

#### OSPF LSU Packet (Type 4) = 
Purpose: Flood updated link-state info to neighbors when topology changes or in response to LSRs. The actual LSAs being sent out/flooded.
> * | Common Header | Number of LSAs | LSAs |
>   * 4 - Number of LSAs
>   * variable - LSAs

```
OSPF LSU Packet (Type 4) Definitions:
> * Number of LSAs –  how many LSAs are in the packet
> * LSAs – each LSA includes the 20-byte header + body fields
```

#### OSPF LSAck Packet (Type 5) = 
Purpose: Ensures reliability of LSA flooding. If no LSAck is received within a certain time, the LSU is retransmitted. Acknowledgement to confirm LSUs were received.
> * | Common Header | Number of LSAs | LSA Headers |
>   * 4 - Number of LSAs
>   * variable - LSA Headers

```
OSPF LSAck Packet (Type 5) Definitions:
> * Number of LSAs – how many LSAs are being acknowledged
> * LSA Headers – contains the headers of each LSA that are being acknowledged
```
---

### Best Practices:
> * Limit number of routes per area (<50 for stability)
> * Check for network type mismatch
> * Manually set RID
> * Summarize routes to ABRs to reduce LSA flooding
> * Keep backbone area contiguous ????
> * Use virtual links to connect an area to Area 0 when direct adjacency is not possible (design best to avoid using virtual links)
> * Adjust timers in point-to-point networks for faster convergence
> * Use LSA throttling features to manage rapid link transition
> * Apply OSPF directly to the interface, instead of using network command in OSPF configuration
---

### Troubleshooting:
> * Too many routers in a single area will slow down convergence
---

### Insights:
> * If adjacency is stuck in Init or Down, ensure both sides have the same timers
> * Routers in different areas in the same subnet won't form adjacencies
> * Duplicate RIDs will cause adjacency or database instability
> * Authentication mismatch will not form adjacency
> * If one router is configured as a stub but neighbor is not the adjacency will fail
> * OSPF multi-area design minimizes flooding and LSA overhead
> * OSPF network types (broadcast, NBMA, point-to-point) can impact LSA exchanges and neighbor relations
> * Stub, totally stubby and NSSA provide route-filtering strategies
> * Neighbors must be in the same area to form adjacency
---

### Commands:
> *
---
