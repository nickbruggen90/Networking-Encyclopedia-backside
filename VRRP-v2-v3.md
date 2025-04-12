VRRPv2, v3 - RFC: 3768 (for v2), RFC: 5798 (for v3)

### Overview:
> * Open standard; provides high availability for default gateway functionality
> * Uses Master/Backup model (Master responds to ARP requests)
> * Only one router can be a Master a time; the rest are backups monitoring the Master
> * VRRPv3 relies on IP Sec if needed
> * VRRPv2 is enabled directly on the interface; while VRRPv3 is done globally first, then interface second
> * *You need to specify whether your enabling IPv4/IPv6 or both globally, doing so provides 	  clarity in dual-stack environments*
---
### Timers:
> * Advertisement Interval = 1 sec
> * Master Down Interval = 3 sec default (x3 Advertisement Interval) + Skew Time
> * Skew Time = (256 - Priority) / 256
> * *Skew Time is a small offset that ensures the Backup with the highest priority times out first to transition to Master state*
---
### Multicast:	
> * *VRRPv2 is mainly used for IPv4; VRRPv3 supports IPv4 and IPv6*
> * IPv4 Multicast = 224.0.0.18
> * IPv6 Multicast = ff02::12
---
### Virtual MAC:
> * IPv4 MAC = 00:00:5E:00:01:(XX) - XX represents VRID in hex 
> * IPv6 MAC = 00:00:5E:00:02:(XX) - XX represents VRID in hex 
---
### Protocol/Port(s):
> * IP Protocol: 112
---
### Priorities:
> * Priority Range = 1 - 254 (highest priority wins; incase of tie highest IP wins)
> * *The router that has the VIP as the physical IP automatically has a priority of 255*
> * *Preempt is enabled by default*
> * Priority 255 is automatically assigned to a router that owns the VIP
---
### Terminology/Definitions:
> * VRRP – protocol used to assign a virtual default gateway for redundancy
> * Master – router currently forwarding for the VIP responds to ARP
> * Backup – routers monitoring the Master; ready to take over if needed
> * VRID – identifies the group
> * Skew Time – delay offset to prevent tie-breakers during failover
> * Preemption – allows a higher-priority router to take over the Master role if it becomes available again
---
### VRRP States:
> * Init → Backup → Master
> * Init – VRRP process initialization; not yet participating
> * Backup – router is monitoring VRRP advertisements (monitoring Master); ready to transition to Master
> * Master – router actively forwarding packets and responding to ARP for the VIP
---
### Header Breakdown = 
> * 4  - Version (2 = IPv4, 3 = IPV4, IPv6)
> * 4 - Type (1 = Advertisement)
> * 8 - VRID
> * 8 - Priority (default = 100) (255 = VIP owner)
> * 8 - Count IP Addresses (usually 1)
> * 8 - Authentication Type (typically 0x00)
> * 8 - Advertisement Interval (default = 1 sec)
> * 16 - Checksum for integrity
> * variable - one or more VIPs
---
#### Header Definitions:
> * Type: only 1 type exists; Advertisement
> * Count IP Address: number of VIP’s included in a VRRP packet(typically 1)
> * Authentication Type: not used in modern VRRP
---
### Best Practices/Security Considerations:
> * v3 is backward compatible with IPv4; but you should run one version on a single interface.
> * For IPv6 ensure ICMPv6 is permitted. VRRP still uses IP Protocol 112 but link-local and ND are critical for hosts to reach the default IPv6 gateway
> * Enable interface tracking
> * Document VRID, priorities, and tracking policies for consistency
> * Use VRRPv3 for IPv6 and dual-stack environments
> * In dual-stack networks, define both IPv4 and IPv6 support during global configuration
> * Ensure all routers in the group use the same VRID and VIP
> * Enable interface tracking to dynamically reduce priority if a critical interface goes down
> * Secure the control plane; while VRRP lacks native authentication, place VRRP routers on secure segments
> * For IPv6, ensure ICMPv6 and NS are permitted to support default gateway resolution
---
### Common Issues and Fixes:
> * All routers must use the same VRID on the interface for the same VIP
> * Check if both routers assume IP address owner role
> * Ensure passwords and authentication is the same on all routers
> * All Routers Remain In Backup State?
>   * Cause?: no master elected or IP conflicts
>   * Fix: verify VRID, IP ownership and priorities
> * Multiple Masters?
>   * Cause?: misconfigured preempt, mismatched priorities, or multicast issues
>   * Fix: synchronize priorities and ensure multicast is not being blocked
> * No Response to ARP/ND?
>   * Cause?: multicast disabled, wrong VRID, or IPv6 ND issues
>   * Fix: verify IP protocol 112 reachability and ICMPv6 allowance
> * VRRPv3 Not Starting?
>   * Cause?: missing global vrrp version and IP family support
>   * Fix: configure vrrp version 3 globally and enable IPv4/IPv6 appropriately
---
### Insights:
> * Usually VRRP triggers a gratuitous ARP to update caches
> * VRRP is supported across multi-vendor environments since its open-standard unlike HSRP
> * VRRPs behavior is deterministic due to priority + IP tie-breakers
> * Most platforms support interface tracking, which is crucial for dynamic failover
> * VRRP is often used in multi-vendor or MPLS deployments due to open standardization
---
### Commands:
> *
---

