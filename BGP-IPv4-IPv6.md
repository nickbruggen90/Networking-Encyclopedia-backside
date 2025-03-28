# BGP - RFC 4271 (defines BGP-4), RFC 4456 (route reflectors), RFC 4364 (BGP/MPLS IP VPNs), RFC 6793 (AS numbers)

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
### Best Practices/Security Considerations:
> * Use consistent BGP configuration across all routers (proper use of AS numbers, neighbor statements and rout policies)
> * Ensure that route reflectors and confederations are configured consistency in larger networks
> * Implement prefix list, route maps and distribute lists to filter routes exchanged with neighbors
> * Filter both inbound and outbound updates to prevent propagation of invalid or suboptimal routes
> * Use attributes like Local Preference and MED appropriately to influence path selection
> * Ensure consistent application of attributes across your network to enforce desired routing policies
> * Deploy multiple BGP sessions with backup or alternative paths to ensure HA
> * Consider BGP multipath configuration to load-balance traffic across equal-cost paths
> * Configure BGP Graceful Restart to minimize disruptions during a router reload
> * Enable Route Refresh capability for dynamic re-advertisement of routes without resetting BGP sessions
> * Enabled detailed logging and SNMP traps for BGP events (sessions flaps, route changes) for proactive monitoring
> * Maintain thorough documentation of BGP configurations, policies and network design
> * Use change management procedures when modifying BGP settings to avoid unintended disruptions
> * Use BGP MD5 authentication on all BGP sessions to prevent unauthorized sessions establishment; and consider more robust security mechanisms (TCP-AO if supported) as MD5 can be vulnerable to certain attacks
> * Apply strict inbound and outbound filtering to prevent route hijacking or injection of malicious routes
> * Use prefix lists and route maps to ensure only valid, authorized routes are accepted and advertised
> * Limit BGP session peers to known IP addresses using ACLs or firewalls rules
> * Consider implementing TTL security to restrict BGP session initiation to directly connected neighbors
> * Isolate BGP routers from the general user network by placing them in dedicated management and control plane networks
> * Use VPN or OOB management for BGP configurations where possible
---
### Troubleshooting:
> * Neighbor stuck in idle or active?
>   * Often due to incorrect IP or no route to neighbor
> * Session Establishment Issues?
>   * Verify that IP connectivity exists between BGP peers
>   * Ensure that any ACLs or firewalls between peers allow TCP port 179
>   * Confirm that BGP MD5 authentication matches on both sides
>   * Check for any TTL security settings that might be causing issues
>   * Compare neighbor configuration and AS numbers on both ends
> * Routing Problems?
>   * Check prefix lists and route maps to ensure routes are correctly filtered and modified
>   * Adjust attributes via route maps if the chosen path is not optimal
>   * Look for patterns in route withdrawals and re-advertisements that might indicate unstable links or configuration issues
> * Common Pitfalls –
>   * Ensure that the correct local and remote AS numbers are configured on both peers
>   * Inconsistent prefix filtering can cause routes to be dropped. Validate your inbound/outbound policies
>   * On high-traffic networks, ensure that routers have enough resources to process BGP updates, especially if running complex policy filters
---
### Insights:
> * Network administrators often modify weight (locally) or local_pref (AS-wide) to control outbound path selection. Doing so overrides other path attributes.
> * AS_PATH prepending is used to influence inbound traffic from other ASes by making your path look longer; taking advantage of shortest AS_PATH
> * A lower MED indicates you prefer that entry point if an external AS has multiple routes into your AS
> * Rarely do you get to oldest path, RID or IP in the path selection process, but they exist to ensure that only a single best path is chosen
> * By default, BGP chooses only one best path. You can enable ECMP if routes are fully equal in attributes (weight, LP, AS_PATH, MED, etc)
> * Communities indirectly affect the best path by triggering route-map logic that sets or modifies attributes like LOCAL_PREF or MED.
> * Incorrect AS_PATH can cause suboptimal routing or black holing
---
### Commands:
> *
---
