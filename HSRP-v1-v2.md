# HSRPv1, v2 – Cisco proprietary

### Overview:
> * Provides a redundant gateway by allowing multiple routers to share a single virtual IP and MAC
> * Ensures network availability by electing an Active and Standby router (highest priority wins)
> * HSRPv1 is the default
> * HSRP Tracking dynamically lowers priority if a certain interface or router fails
---
### Timers:
> * HSRPv1 Hello Timer = 3 sec || Hold = 10 sec
> * HSRPv2 works in milliseconds
---
### Multicast:
> * HSRPv1 only supports IPv4; HSRPv2 supports both IPv4 and IPv6
> * HSRPv1 IPV4 Multicast = 224.0.0.2
> * HSRPv2 IPV4 Multicast = 224.0.0.102
> * HSRPv2 IPv6 Multicast = FF02::66
*HSRPv1 supports only plaintext authentication; HSRPv2 supports MD5 hashing and plaintext*  
---
### Virtual MAC:
> * HSRPv1 MAC = 00:00:0C:07:AC(XX) – XX represents the group number in hex
>   * 	*group range = 0 - 255*
> * HSRPv2 MAC = 00:00:0C:9F:F(XXX) – XXX represent the group number in hex
> *   *group range = 0 - 4095*
*Group numbers are local to the interface; they can be reused on different interfaces
---
### Protocol/Port(s):
> * UDP: 1985
---
### Priorities:
> * Priority Range = 0 - 255 (highest priority wins; incase of tie highest IP wins)
>   * *Default priority is 100
>   * *Preemption must be enabled for high-priority router to take over as Active
>   * *Priority 0 is a special priority that causes router to immediately give up its role
---
### Terminology/Definitions:
> * HSRP – Cisco proprietary FHRP that allows multiple routers to present a single VIP and MAC address to hosts for default gateway redundancy
> * HSRP Group – a logical grouping of routers that share the same VIP and MAC addres and participate in the HSRP election
> * VIP – the shared IP address used by hosts as their default gateway. This IP is mapped to a virtual MAC address
> * Virtual MAC Address – a MAC address generated and shared by HSRP group members; it allows seamless MAC-level gateway failover
> * Active Router – the router currently responsible for forwarding traffic sent to the VIP. It responds to ARP requests for the VIP and handles all traffic
> * Standby Router – the backup router that monitors the Active router. It takes over the role if the Active router fails
> * Speak State – the router sending Hello messages and participating in the election process to become the Active or Standby
> * Learn State – the router has not yet learned the VIP through HSRP hello messages
> * Coup Message – sent by a router that wants to take over the Active role (when it has a higher priority and preemption is enabled)
> * Resign Message – sent by the current Active router to signal it is stepped down
> * Preemption – allows a higher-priority router to take over as the Active role from a lower-priority one
> * Tracking – a feature that dynamically adjusts HSRP priority based on the status of the tracked object (usually an interface). If the object failed, the routers priority is reduced
> * Authentication – optional feature to secure HSRP messages; plaintext in v1, MD5 hash in v2
> * Hold Timer – duration a router waits after not hearing a Hello message before assuming the active router is down
> * Hello Timer – interval at which Hello messages are sent by routers in speak, standby and active
> * Split Brain – a condition where two routers assume the Active role simultaneously due to timer or connectivity issues, potentially causing traffic disruptions
---
### HSRP States:
> Init → Learn → Listen → Speak → Standby → Active  
> * Init – router is starting HSRP, no configuration active
> * Learn – router has not yet learned the VIP through HSRP hello messages
> * Listen – knows the VIP; but not yet active or standby
> * Speak – sends hello messages; participates in election
> * Standby – ready to take over if active fails
> * Active – currently forwarding packets
>   **The active router responds to ARP for Virtual MAC; and forwards traffic for VIP
>   **The standby router monitors the active router
>   **Only routers in speak, standby or active send HSRP messages
>   **Only active router responds to ARP on the VIP
---
### HSRP Fields = 
> * 1 - Protocol Version 
> * 1 - Message Type (Op Code)(0 = Hello, 1 = Coup, 2 = Resign)
> * 1 - Group Number (0 - 255)
> * 1 - Reserved (set to 0x00)
> * 1 - State (0 = Init, 1 = Learn, 2 = Listen, 4 = Speak, 8 = Standby, 16 = Active)
> * 2 - Hello Timer
> * 2 - Hold Timer
> * 1 - Priority
> * 8 - Authentication Data
> * 4 - Virtual IP
---
### Best Practices/Security Considerations:
> * Enable preemption (off by default)
> * Use MD5 hashing
> * Adjust timers for faster convergence (standardize timers)
> * Use HSRPv2 in large environments or where IPv6 is needed (scalability)
> * Use different HSRP groups for different VLANS to enable per-VLAN redundancy or load balancing
> * Avoid using group number 0 in production
> * Use CoPP to rate limit HSRP messages on routers
---
### Common Issue and Fixes:
> * Priority Mismatch?
>   * Ensure the correct router has the highest priority
> * Timer Synchronization?
>   * Verify all routers in the same group have the same timers
> * Wrong Group ID/Overlapping?
>   * Make sure they are consistent on all routers within the subnet
> * Ensure Authentication matches on both sides
> * Both Routers in an Active State?
>   * Cause?: Misconfiguration on groups or authentication settings
>   * Fix: Verify group numbers or authentication keys
> * No Communication Between Routers?
>   * Cause?: VLAN mismatch or L2 connectivity issue
>   * Fix: Verify VLAN and trunk settings
> * High CPU Usage?
>   * Cause?: Too many HSRP groups or excessive failover events
>   * Fix: Optimize group configurations and timers
> * Router Not Learning VIP?
>   * Cause?: interface not receiving HSRP hellos or misconfigured HSRP group
>   * Fix: verify both routers are on the same subnet, HSRP group and VLAN
> * HSRP Flapping?
>   * Cause?: interface instability or inconsistent preemption/timers
>   * Fix: check physical link, increase hold timer, verify redundancy configuration
---
### Insights:
> * If an interface is configured with multiple subinterfaces, each VLAN can have its own HSRP group. This helps with LB as well.
> * Each HSRP group can have its own virtual IP representing specific services or traffic flows.
> * Multicast can be unreachable due to network filtering.
> * If timers are mismatched it can cause “split-brain”, where both active and standby routers both think they are active.
> * Use Preempt Delay to give the router time to populate tables
---
### Commands:
> * 
---
