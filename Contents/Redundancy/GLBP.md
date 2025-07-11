# GLBP - Gateway Load Balancing Protocol
*Cisco proprietary*

## Overview
• GLBP can actively use multiple gateways in a single group (up to 4 Active routers)  
• Multiple routers can simultaneously forward traffic (with LB) for the same VIP  
• GLBP automatically distributes traffic across all gateways within the group without needing multiple gateway IPs or multiple FHRP groups  
• Use VRRP in mixed environments, and HSRP/GLBP in Cisco environments  
• The VIP is shared amongst the entire GLBP group  
• Each GLBP group has its own range of virtual MACs

---

## Timers

| Timer Type | Default Value | Purpose |
|------------|---------------|---------|
| **Hello Timer** | 3 sec | Interval for sending hello messages |
| **Dead Timer** | 10 sec (or x3 the Hello) | Time to wait before declaring neighbor dead |
| **Preempt** | 30 sec | Delay before preemption occurs |

*Preempt is disabled by default

---

## Multicast

| IP Version | Address | Usage |
|------------|---------|-------|
| **IPv4** | 224.0.0.102 | GLBP hello messages |
| **IPv6** | ff02::12 | GLBP hello messages |
| **MAC** | 01:00:5E:00:00:66 | Multicast MAC address |

---

## Protocols/Ports

| Protocol | Port | Usage |
|----------|------|-------|
| **UDP** | 3222 | GLBP communication |

---

## Priorities

| Parameter | Range/Value | Description |
|-----------|-------------|-------------|
| **Group Range** | 0 - 255 | GLBP group identifier range |
| **Priority Selection** | Higher priority value | Increases the router's chance of being elected the AVG |
| **Tie Breaker** | Highest IP | Breaks priority ties |

---

## Virtual MAC

| Component | Format | Description |
|-----------|--------|-------------|
| **Virtual MAC** | 00:07:b4:(XX:XX):(YY) | XX represents group number, YY represents AVF number |
| **Cisco OUI** | 00:07:b4 | Cisco Organizationally Unique Identifier |

### GLBP Group Limitations
• **1024 VIP (GLBP groups) per interface**  
• **GLBP groups are on a per VLAN basis**  
• **Group Range = 0 - 255**  
• **4 active AVFs per group, 1 AVG per group**

### AVF and AVG Roles
• **AVFs are the actual virtual router (virtual gateway) for each group**. AVFs forward packets to virtual MAC. Each AVF is assigned a unique virtual MAC. 4 active AVFs per group, remaining will be in listen(standby)  
• **AVG acts as a manager for each group**. AVGs assign virtual MACs to AVFs. AVGs respond to ARP requests for the VIP, using LB. AVG selection is based on highest priority (default is 100). Also monitors health and status of AVFs

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **AVG** | The router elected to manage and assign virtual MAC addresses to participating AVFs within the GLBP group. The AVG responds to ARP requests from clients with different virtual MAC addresses for load balancing |
| **AVF** | A router assigned a specific virtual MAC address by the AVG. It forwards traffic for the clients that receives its MAC address |
| **Virtual MAC Address** | A dynamically assigned MAC address for each AVF, allowing multiple routers to share the gateway role without needing manual client configuration |
| **GLBP Group** | A collection of routers configured to share the same virtual IP address and work together for load balancing and redundancy |
| **GLBP Priority** | A numerical value (default is 100) used to determine which router should become the AVG. The router with the highest priority is elected as the AVG |
| **GLBP Weight** | A value assigned to AVFs to determine their traffic-handling capacity. Higher weight value indicates a higher share of traffic |

---

## GLBP States

| State | Description |
|-------|-------------|
| **Disabled** | Not participating in GLBP |
| **Init** | Router has started GLBP |
| **Listen** | Listen for Hello packets from AVG; might forward if its assigned as an AVF |
| **Speak** | Negotiates to become AVG if there is no current AVG, or priority is higher |
| **Standby** | In line to become next AVG if current AVG fails |
| **Active** | Acting as the AVG (and possibly AVF) |

**State Progression:** Disabled → Init → Listen → Speak → Standby → Active

---

## Load Balancing Mechanisms

| Method | Description | Behavior |
|--------|-------------|----------|
| **Round-Robin (default)** | Cycles through AVFs, distributes ARP replies in turn | Each new ARP request is replied with a different virtual MAC; each host in the subnet might get a different MAC |
| **Weighted** | Based on router's assigned weight. Default weight is 100 | The router's weight influences how frequently the virtual MAC is given out. If the weight drops below the lower threshold, it no longer serves as AVF; if it rises above the higher threshold it can resume forwarding |
| **Host-Dependent** | Ensures a single host doesn't change its MAC if it ARPs multiple times | Once a host learns its AVF MAC it always uses the same MAC. Each host always uses the same AVF |

---

## Header Breakdown

### GLBP L7 Header Fields

| Field | Size (bits) | Description |
|-------|-------------|-------------|
| **Version** | 4 | GLBP version |
| **Op-Code** | 4 | Message operation code |
| **Message Length** | 16 | Length of GLBP message |
| **Router ID** | 32 | Unique router identifier |
| **Auth Type** | 8 | Authentication type |
| **Auth Data** | 64 | Authentication data |
| **GLBP Group** | 8 | Group number |
| **Priority** | 8 | Router priority |
| **Weight** | 8 | Router weight |
| **Hello Timer** | 16 | Hello interval |
| **Hold Timer** | 16 | Hold time |
| **Virtual MAC Count** | 8 | Number of virtual MACs |
| **Virtual MAC Address** | Variable | Virtual MAC addresses |
| **Load Balancing Mode** | 8 | LB mechanism in use |

### Op-Codes

| Code | Name | Purpose |
|------|------|---------|
| **0x01** | Hello | Neighbor discovery and maintenance |
| **0x02** | Coup | AVG takeover |
| **0x03** | Resign | AVG resignation |
| **0x04** | Redirect | Client redirection |
| **0x05** | Weight Change | Weight modification |

---

## Header Definitions

### Message Types
• **Hello Message** is used to announce the presence of a GLBP-capable router in a group. It helps maintain neighbor relationships and keep the GLBP group updated  
• **Redirection Message** is sent by the AVG to instruct a client to use a different AVF. This message helps optimize load balancing by directing client traffic to the most appropriate AVF  
• **Advertisement Message** is used by the AVFs to advertise their virtual MAC addresses and current load information. This allows the AVG to assign clients to the AVG that is best suited to handle their traffic

### Field Definitions
• **Authentication Type** – 0 = None, 1 = Plaintext, 2 = MD5  
• **Weight** – used in load balancing to determine how much traffic an AVF can handle  
• **Load Balancing Mode** – 0 = round-robin, 1 = weighted, 2 = host-dependent

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Load Balancing** | GLBP allows multiple routers to share a single virtual gateway IP address. Each router in the GLBP group (AVFs) is assigned a unique virtual MAC address, enabling the load balancing of client traffic among several physical routers |
| **Redundancy and HA** | If one router fails, GLBP automatically redistributes the load among the remaining routers, ensuring continuous network connectivity without manual intervention |
| **Automatic Virtual Gateway Assignment** | One router is elected as the AVG to assign virtual MAC addresses to the other routers (AVFs) in the group. This automatic mechanism simplifies gateway management |
| **Simplified Configuration** | GLBP reduces the administrative overhead by using a single virtual gateway IP for multiple physical routers, allowing for seamless failover and load balancing without the need for complex configurations |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Enterprise Networks** | In campus or branch office environments, GLBP provides both load balancing and redundancy for default gateway services, ensuring that user traffic is distributed evenly and that a backup gateway is available if one fails |
| **High-Availability Sites** | In environments requiring continuous network access (such as data centers or critical corporate networks), GLBP ensures that a failure of one gateway does not disrupt connectivity |
| **Multi-Router Deployments** | When multiple routers are available at a location, GLBP simplifies configuration by allowing them to work together as a single virtual gateway rather than requiring manual redistribution of traffic |
| **Cost Savings** | By balancing traffic over multiple inexpensive links (such as broadband connections) while maintaining redundancy, GLBP can reduce reliance on costlier MPLS circuits |

---

## Best Practices/Security Considerations

### Configuration Management
• Ensure that all routers in the GLBP group are configured with the same GLBP group number, hello intervals and hold times  
• Configure similar priorities on routers if you want equal load balancing, or adjust priorities to influence which router becomes the AVG  
• Set hello intervals and hold times appropriately to ensure timely detection of router failures without causing excessive traffic  
• Use GLBP in conjunction with dynamic routing protocols to ensure that both gateway load balancing and optimal routing are maintained  
• Document GLBP configurations, including group numbers, priorities and timing parameters, to ensure consistency and facilitate troubleshooting

### Security Hardening
• Protect your routing and management plane with strong access controls, ACLs and secure management protocols  
• Ensure only authorized routers participate in the GLBP group by securing the physical access to network devices and applying proper authentication  
• Regularly audit GLBP advertisements and group membership to detect any unauthorized changes or potential spoofing attempts  
• Use VLAN segmentation and other access control measures to limit GLBP domain exposure, ensuring that only trusted devices can participate  
• Keep device firmware up-to-date to mitigate vulnerabilities that could be exploited to tamper with GLBP operations

---

## Troubleshooting

### Common Issues and Solutions
• **Verify that GLBP hello messages are being transmitted and received**. If no hello messages are observed, check physical connectivity, interface configurations and ACLs that might be blocking multicast or broadcast traffic  
• **If load balancing isn't working as expected**, confirm that the AVG is correctly assigning virtual MAC addresses and that each AVF is using its assigned MAC  
• **If changes in topology or router failures are not being detected quickly**, review and adjust hello interval and hold time settings  
• **Compare configurations on all routers in the GLBP group** to ensure consistency (same GLBP group number, hello intervals and priorities)

---

## Insights

*[To be completed during lab sessions]*

---

## Commands

*[To be completed during lab sessions]*
