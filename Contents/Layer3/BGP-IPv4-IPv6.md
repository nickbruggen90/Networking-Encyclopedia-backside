# BGP - Border Gateway Protocol
*RFC 4271 (defines BGP-4), RFC 4456 (route reflectors), RFC 4364 (BGP/MPLS IP VPNs), RFC 6793 (AS numbers)*

## Overview
• **Path-Vector protocol**  
• **Supports IPv4 and IPv6**  
• **Can handle hundreds of thousands (or millions) of routes** using the BGP table with all candidate paths and injecting the best path into the router's RIB

---

## Timers

| Timer Type | Default Value | Description |
|------------|---------------|-------------|
| **Hold Timer** | 180 sec | Maximum time to wait for Keepalive before declaring neighbor down |
| **Keepalive Timer** | 60 sec (⅓ of Hold) | Frequency of Keepalive messages |
| **Connect Retry** | 120 sec | Time to wait before retrying TCP connection |
| **MRAI eBGP** | 30 sec | Minimum Route Advertisement Interval for eBGP |
| **MRAI iBGP** | 5 sec | Minimum Route Advertisement Interval for iBGP |

### TTL Behavior
• **eBGP default TTL = 1** → Neighbors must be directly connected (unless using `ebgp-multihop`)  
• **iBGP default TTL = 255** → Can reach neighbors through multiple hops

---

## Multicast
• **BGP does not use multicast/broadcast** → Each session uses unicast (point-to-point) TCP between two peers

---

## Protocol/Port(s)
| Protocol | Port | Type |
|----------|------|------|
| **BGP** | TCP 179 | Unicast sessions between peers |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **Aggregate-Address** | Summarizes multiple prefixes into one |
| **Atomic Aggregate** | Used with route aggregation |
| **AGGREGATOR** | Information on who performed aggregation |
| **COMMUNITY** | Tags a route with certain properties (no-export, internet, custom numeric) |
| **Route Dampening** | Penalizes unstable routes (flaps) by increasing a "penalty" and potentially suppressing the route advertisement temporarily |
| **NLRI** | Network Layer Reachability Information - routing information carried in BGP updates |
| **Confederations** | Breaks larger ASs into smaller sub-ASs |

---

## BGP Flavors

### iBGP (Internal BGP)
• **Used within the same AS**  
• **More granular routing control** needed for large enterprises or service providers  
• **Default behavior:** iBGP neighbors do not re-advertise iBGP learned routes to other iBGP neighbors  
• **Topology requirement:** Full mesh topology required within AS, OR use of route reflectors or confederations

### eBGP (External BGP)
• **The routing protocol of the internet**  
• **Used between different ASs** for inter-domain routing  
• **Direct connectivity** typically required (TTL=1)

---

## Path Selection Process
*Cisco's 11-Step Process (in order)*

| Step | Attribute | Scope | Preference | Description |
|------|-----------|-------|------------|-------------|
| **1** | **Weight** | Local only (Cisco-specific) | Higher preferred (default: 0) | Applies only to local router; not exchanged |
| **2** | **LOCAL_PREF** | iBGP only | Higher preferred (default: 100) | Exit point out of AS; propagated throughout same AS |
| **3** | **Locally Originated** | Local | Local routes preferred | Routes originated by this router |
| **4** | **AS_PATH** | Global | Shorter preferred | Sequence of AS numbers; used for loop avoidance |
| **5** | **ORIGIN** | Global | IGP < EGP < Incomplete | How route was originally injected |
| **6** | **MED** | Inter-AS | Lower preferred | Multi-Exit Discriminator; suggests entry point |
| **7** | **Path Type** | Global | eBGP < iBGP | External paths preferred over internal |
| **8** | **IGP Metric** | Local | Lower preferred | Metric to NEXT_HOP |
| **9** | **Multipath** | Local | Load balance | If enabled, multiple equal paths |
| **10** | **Age** | Local | Oldest preferred | Route stability consideration |
| **11** | **Router ID** | Global | Lower preferred | Tiebreaker using BGP Router ID |

### Key Path Attributes Details

**NEXT_HOP**
• IP address to reach the advertised prefix  
• Uses lowest IGP metric when multiple paths exist

**MED (Multi-Exit Discriminator)**
• Suggests to external neighbors how to enter your AS (when multiple entrances exist)  
• Only compared when the neighboring AS is the same for both routes  
• Lower is preferred but not always honored by default

---

## BGP Features

| Feature | Purpose | Benefits |
|---------|---------|----------|
| **Route Reflectors (RR)** | Reflect routes between iBGP clients | Reduces need for full iBGP mesh |
| **Confederations** | Split AS into multiple sub-AS systems | Alternative solution to iBGP full mesh |
| **BGP Peer Groups** | Group neighbors with same outbound policy | Reduces CPU overhead by generating updates once |

---

## BGP Message Types

| Message Type | Purpose | Content |
|--------------|---------|---------|
| **Open** | Initiate BGP session | AS number, hold time, BGP version, Router ID |
| **Keepalive** | Maintain connection | Sent every 60 sec by default |
| **Update** | Route information | Conveys NLRI (new routes) or withdraws unfeasible routes |
| **Notification** | Error indication | Malformed packet, hold timer expired; closes session |

---

## Neighbor States

| State | Description |
|-------|-------------|
| **Idle** | Initial state or error state |
| **Connect** | Attempting or waiting to establish TCP connection |
| **Active** | TCP connection failed, trying alternate connection |
| **OpenSent** | Exchange of Open messages and waiting for Keepalives |
| **OpenConfirm** | Open message received, waiting for Keepalive or Notification |
| **Established** | Routing information (NLRI) can be exchanged |

### BGP Session Requirements
• **Manual configuration required** → BGP does not dynamically discover peers  
• **AS numbers must match** expected relationship (iBGP vs eBGP)  
• **TCP connectivity** → Peers must reach each other over TCP 179

---

## Best Practices/Security Considerations

### Configuration Management
• **Use consistent BGP configuration** across all routers (AS numbers, neighbor statements, route policies)  
• **Ensure route reflector consistency** in larger networks  
• **Maintain thorough documentation** of BGP configurations, policies and network design  
• **Use change management procedures** when modifying BGP settings

### Route Filtering and Control
• **Implement prefix lists, route maps** and distribute lists to filter routes  
• **Filter both inbound and outbound** updates to prevent invalid route propagation  
• **Use attributes appropriately** (Local Preference, MED) to influence path selection  
• **Apply consistent attributes** across network to enforce desired routing policies

### High Availability
• **Deploy multiple BGP sessions** with backup or alternative paths  
• **Consider BGP multipath configuration** for load-balancing across equal-cost paths  
• **Configure BGP Graceful Restart** to minimize disruptions during router reload  
• **Enable Route Refresh capability** for dynamic re-advertisement without session reset

### Security Implementation
• **Use BGP MD5 authentication** on all BGP sessions to prevent unauthorized establishment  
• **Consider TCP-AO** if supported (MD5 can be vulnerable to certain attacks)  
• **Apply strict inbound/outbound filtering** to prevent route hijacking or malicious injection  
• **Limit BGP session peers** to known IP addresses using ACLs or firewall rules  
• **Consider TTL security** to restrict session initiation to directly connected neighbors

### Network Isolation
• **Isolate BGP routers** from general user network using dedicated management networks  
• **Use VPN or OOB management** for BGP configurations where possible  
• **Enable detailed logging and SNMP traps** for BGP events (flaps, route changes)

---

## Troubleshooting

### Session Establishment Issues

| Problem | Likely Causes | Solutions |
|---------|---------------|-----------|
| **Neighbor stuck in Idle/Active** | Incorrect IP or no route to neighbor | Verify IP connectivity, routing, ACLs |
| **Session won't establish** | Authentication mismatch, firewall blocking | Check MD5 auth, ACLs, TCP 179 access |
| **Configuration mismatch** | Wrong AS numbers, parameter mismatch | Compare neighbor config on both ends |

### Common Troubleshooting Steps
1. **Verify IP connectivity** between BGP peers  
2. **Ensure ACLs/firewalls** allow TCP port 179  
3. **Confirm BGP MD5 authentication** matches on both sides  
4. **Check TTL security settings** that might cause issues  
5. **Compare neighbor configuration** and AS numbers on both ends

### Routing Problems
• **Check prefix lists and route maps** for correct filtering and modification  
• **Adjust attributes via route maps** if chosen path is not optimal  
• **Look for patterns** in route withdrawals and re-advertisements (unstable links)  
• **Validate inbound/outbound policies** for consistent prefix filtering  
• **Ensure adequate resources** for BGP update processing on high-traffic networks

---

## Insights

### Path Selection Manipulation
• **Network administrators often modify Weight** (locally) or LOCAL_PREF (AS-wide) to control outbound path selection  
• **AS_PATH prepending** influences inbound traffic by making paths appear longer  
• **Lower MED** indicates preferred entry point for external AS with multiple routes

### Path Selection Reality
• **Rarely reach oldest path, RID, or IP** in selection process, but they ensure single best path  
• **By default, BGP chooses only one best path** → Enable ECMP for equal-attribute routes  
• **Communities indirectly affect best path** by triggering route-map logic that modifies attributes

### Configuration Considerations
• **Incorrect AS_PATH** can cause suboptimal routing or blackholing  
• **Consistent policy application** crucial for predictable routing behavior  
• **Regular monitoring** essential for detecting and preventing routing anomalies

---

## Commands
*[To be completed during lab sessions]*
