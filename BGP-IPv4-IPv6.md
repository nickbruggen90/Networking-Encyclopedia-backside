BGP - RFC 4271 (defines BGP-4), RFC 4456 (route reflectors), RFC 4364 (BGP/MPLS IP VPNs), RFC 6793 (AS numbers)

### Overview:
> * Path-Vector protocol
> * Supports IPv4 and IPv6
> * Can handle hundreds of thousands (or millions) of routes; using the BGP table with all candidate paths. And injecting the best path into the router's RIB
---
### Timers:
> * Hold Timer = 180 sec || Keepalive Timer = 60 sec (⅓ of Hold) || Connect Retry = 120 sec 
> * MRAI eBGP = 30 sec || MRAI iBGP = 5 sec
     
> * eBGP default TTL is 1, meaning neighbors must be directly connected (unless using ebpg-multihop)
> * iBGP default TTL is 255
---
### Multicast:
> * BGP does not use multicast/broadcast; instead each sessions uses unicast (point-to-point) TCP between two peers
---
### Protocol/Port(s)
> * TCP: 179
---
### Terminolgy/Definitions:
> * Aggregate-Address – summarizes multiple prefixes into one
> * Atomic Aggregate – used with route aggregation
> * AGGREGATOR – info on who performed aggregation
> * COMMUNITY – tags a route with certain properties (no-export, internet, custom numeric)
> * Route Dampening – penalizes unstable routes (flaps) by increasing a “penalty” and potentially suppressing the route advertisement temporarily
> * NLRI (Network Layer Reachability Information) – ???
> * Confederations – breaks larger ASs into smaller sub-ASs
---
### BGP Flavors:
> * iBGP can be used internally where more granular routing control is needed; used within large enterprises or service providers, and within the same AS.
> * By default, iBGP neighbors do not re-advertise iBGP learned routes to other iBGP neighbors. A full mesh topology is required with an AS, or the use of route reflectors or confederations.
> * eBGP is the routing protocol of the internet – used between different ASs for inter-domain routing
---
### Path Selection: (Cisco's 11-Steps Process, in order)
> * Weight (Cisco specific) – default weight is 0, higher weight is preferred. Applies only to local routers; not exchanged with other routers.
> * LOCAL_PREF (iBGP only) – default is 100, higher is preferred. An exit point (among multiple exit points) out of an AS; propagated throughout the same AS. Exchanged among iBGP neighbors; not sent to eBGP peers.
> * shortest AS_PATH – sequence of AS numbers the route has traversed . Used for loop avoidance (if a router sees its own AS in the AS_PATH, it will discard the route) and path preference. Shorter is preferred by default if the local_pref and weight are not set.
> * NEXT_HOP – IP address to reach the advertised prefix; uses lowest IGP metric when multiple paths
> * ORIGIN – how the route was originally injected IGP < EGP < Incomplete Used lower in the selection list
> * MED (Multi-Exit Discriminator) – suggests to external neighbors how to enter your AS (when multiple entrances exist). Only compared when the neighboring AS is the same for both routes. Lower is preferred but not always honored by default (depends on configurations)
---
### BGP Features:
> * Route Reflectors (RR) – is a route that can reflect routes between iBGP clients, reducing the need for direct iBGP sessions among all routers. Solved the iBGP full mesh problem.
> * Confederations – splits the AS into multiple sub-AS systems, each with their own iBGP mesh but collectively appear as a single AS to external neighbors. Another solution to iBGP full mesh
> * BGP Peer Groups – a way to group neighbors that share the same outbound policy. Updates are generated once and replicated to all peers in the group – reducing CPU overhead
---
### BGP Message Types:
> * Open – first message after TCP connection; initiates a BGP session. Negotiates parameters such as: AS number, hold time, BGP version, RID
> * Keepalive – sent every 60 sec by default to maintain connection; confirmation neighbor is still alive.
> * Update – conveys NLRI (advertises new routes) or withdraws unfeasible routes
> * Notification – indicates errors (malformed packet, hold timer expired), and by default causes BGP to close the session immediately
---
### Neighbor States:
> * Idle – initial state or error state
> * Connect – attempting or waiting to establish TCP connection
> * Active – ???
> * OpenSent – exchange of open messages and waiting for keepalives
> * OpenConfirm – ???
> * Established – routing information (NLRI) can be exchanged
Requirements:
*Neighbors must be manually configured (BGP does not dynamically discover peers)*  
*AS numbers must match the expected relationship (iBGP vs eBGP)*  
*Peers must be able to reach each other over TCP 179*
---

