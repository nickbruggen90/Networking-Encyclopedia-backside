# HSRP - Hot Standby Router Protocol
*HSRPv1, v2 – Cisco proprietary*

## Overview
• Provides a redundant gateway by allowing multiple routers to share a single virtual IP and MAC  
• Ensures network availability by electing an Active and Standby router (highest priority wins)  
• **HSRPv1 is the default** version  
• **HSRP Tracking** dynamically lowers priority if a certain interface or router fails

---

## Timers

| Version | Hello Timer | Hold Timer | Precision |
|---------|-------------|------------|-----------|
| **HSRPv1** | 3 seconds | 10 seconds | Seconds |
| **HSRPv2** | Configurable | Configurable | **Milliseconds** |

---

## Multicast Addresses

### Version Support
• **HSRPv1** → Only supports IPv4  
• **HSRPv2** → Supports both IPv4 and IPv6

### Multicast Addresses

| Version | Protocol | Multicast Address |
|---------|----------|-------------------|
| **HSRPv1** | IPv4 | 224.0.0.2 |
| **HSRPv2** | IPv4 | 224.0.0.102 |
| **HSRPv2** | IPv6 | FF02::66 |

### Authentication Support
• **HSRPv1** → Supports only plaintext authentication  
• **HSRPv2** → Supports MD5 hashing and plaintext

---

## Virtual MAC Addresses

### HSRPv1 Virtual MAC
• **Format:** `00:00:0C:07:AC:XX`  
• **XX** represents the group number in hexadecimal  
• **Group range:** 0-255

### HSRPv2 Virtual MAC
• **Format:** `00:00:0C:9F:F:XXX`  
• **XXX** represents the group number in hexadecimal  
• **Group range:** 0-4095

### Group Number Scope
• **Group numbers are local to the interface** → Can be reused on different interfaces

---

## Protocol/Ports

| Transport | Port | Usage |
|-----------|------|-------|
| **UDP** | 1985 | HSRP communication between routers |

---

## Priorities

### Priority Configuration

| Parameter | Range/Value | Description |
|-----------|-------------|-------------|
| **Priority Range** | 0-255 | Highest priority wins; in case of tie, highest IP wins |
| **Default Priority** | 100 | Default value if not configured |
| **Special Priority** | 0 | Causes router to immediately give up its role |
| **Preemption** | Disabled by default | Must be enabled for high-priority router to take over as Active |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **HSRP** | Cisco proprietary FHRP that allows multiple routers to present a single VIP and MAC address to hosts for default gateway redundancy |
| **HSRP Group** | A logical grouping of routers that share the same VIP and MAC address and participate in the HSRP election |
| **VIP** | The shared IP address used by hosts as their default gateway. This IP is mapped to a virtual MAC address |
| **Virtual MAC Address** | A MAC address generated and shared by HSRP group members; allows seamless MAC-level gateway failover |
| **Active Router** | The router currently responsible for forwarding traffic sent to the VIP. It responds to ARP requests for the VIP and handles all traffic |
| **Standby Router** | The backup router that monitors the Active router. It takes over the role if the Active router fails |
| **Speak State** | The router sending Hello messages and participating in the election process to become the Active or Standby |
| **Learn State** | The router has not yet learned the VIP through HSRP hello messages |
| **Coup Message** | Sent by a router that wants to take over the Active role (when it has a higher priority and preemption is enabled) |
| **Resign Message** | Sent by the current Active router to signal it is stepping down |
| **Preemption** | Allows a higher-priority router to take over as the Active role from a lower-priority one |
| **Tracking** | A feature that dynamically adjusts HSRP priority based on the status of the tracked object (usually an interface) |
| **Authentication** | Optional feature to secure HSRP messages; plaintext in v1, MD5 hash in v2 |
| **Hold Timer** | Duration a router waits after not hearing a Hello message before assuming the active router is down |
| **Hello Timer** | Interval at which Hello messages are sent by routers in speak, standby and active states |
| **Split Brain** | A condition where two routers assume the Active role simultaneously due to timer or connectivity issues |

---

## HSRP States

### State Transition Flow
```
Init → Learn → Listen → Speak → Standby → Active
```

| State | Description | Behavior |
|-------|-------------|----------|
| **Init** | Router is starting HSRP | No configuration active |
| **Learn** | Router has not yet learned the VIP | Through HSRP hello messages |
| **Listen** | Router knows the VIP | But not yet active or standby |
| **Speak** | Router sends hello messages | Participates in election |
| **Standby** | Router is ready to take over | If active fails |
| **Active** | Router is currently forwarding packets | Handles all VIP traffic |

### State-Specific Behaviors
• **The active router responds to ARP for Virtual MAC** and forwards traffic for VIP  
• **The standby router monitors the active router**  
• **Only routers in speak, standby or active send HSRP messages**  
• **Only active router responds to ARP on the VIP**

---

## HSRP Header Fields

### Header Structure

| Field | Size (bytes) | Values | Description |
|-------|--------------|--------|-------------|
| **Protocol Version** | 1 | 0 (v1), 1 (v2) | HSRP version identifier |
| **Message Type (Op Code)** | 1 | 0=Hello, 1=Coup, 2=Resign | Type of HSRP message |
| **Group Number** | 1 | 0-255 | HSRP group identifier |
| **Reserved** | 1 | 0x00 | Reserved field |
| **State** | 1 | 0=Init, 1=Learn, 2=Listen, 4=Speak, 8=Standby, 16=Active | Current router state |
| **Hello Timer** | 2 | Configurable | Hello interval in seconds |
| **Hold Timer** | 2 | Configurable | Hold time in seconds |
| **Priority** | 1 | 0-255 | Router priority |
| **Authentication Data** | 8 | Variable | Authentication information |
| **Virtual IP** | 4 | IP address | Shared virtual IP address |

---

## Best Practices/Security Considerations

### Configuration Guidelines
• **Enable preemption** (off by default) for proper failover behavior  
• **Use HSRPv2 in large environments** or where IPv6 is needed (better scalability)  
• **Use different HSRP groups for different VLANs** to enable per-VLAN redundancy or load balancing  
• **Avoid using group number 0** in production environments

### Security Implementation
• **Use MD5 hashing** for authentication instead of plaintext  
• **Use CoPP to rate limit HSRP messages** on routers to prevent DoS  
• **Ensure authentication matches** on all routers in the same group

### Timer Optimization
• **Adjust timers for faster convergence** but consider stability vs. speed trade-offs  
• **Standardize timers** across all routers in the same group  
• **Use Preempt Delay** to give the router time to populate tables

---

## Common Issues and Fixes

| Issue | Potential Causes | Solution |
|-------|------------------|----------|
| **Priority Mismatch** | Incorrect priority configuration | Ensure the correct router has the highest priority |
| **Timer Synchronization** | Mismatched timer values | Verify all routers in the same group have the same timers |
| **Wrong Group ID/Overlapping** | Inconsistent group configuration | Make sure group IDs are consistent on all routers within the subnet |
| **Authentication Failure** | Mismatched authentication | Ensure authentication matches on both sides |
| **Both Routers in Active State** | Split-brain condition | Verify group numbers and authentication keys |
| **No Communication Between Routers** | L2 connectivity issues | Verify VLAN and trunk settings |
| **High CPU Usage** | Too many HSRP groups | Optimize group configurations and timers |
| **Router Not Learning VIP** | Missing HSRP hellos | Verify both routers are on the same subnet, HSRP group and VLAN |
| **HSRP Flapping** | Interface instability | Check physical link, increase hold timer, verify redundancy configuration |

### Detailed Troubleshooting

#### Split-Brain Scenarios
• **Cause:** Misconfiguration on groups or authentication settings  
• **Fix:** Verify group numbers and authentication keys match

#### Connectivity Issues
• **Cause:** VLAN mismatch or L2 connectivity problems  
• **Fix:** Verify VLAN assignments and trunk configurations

#### Performance Issues
• **Cause:** Too many HSRP groups or excessive failover events  
• **Fix:** Optimize group configurations and adjust timers

#### Learning Problems
• **Cause:** Interface not receiving HSRP hellos or misconfigured HSRP group  
• **Fix:** Verify subnet, group, and VLAN consistency

#### Flapping Behavior
• **Cause:** Interface instability or inconsistent preemption/timers  
• **Fix:** Check physical links, increase hold timer, verify configuration

---

## Insights

### Multi-VLAN Implementation
• **If an interface is configured with multiple subinterfaces** each VLAN can have its own HSRP group  
• **This helps with load balancing** as well as redundancy  
• **Each HSRP group can have its own virtual IP** representing specific services or traffic flows

### Common Operational Issues
• **Multicast can be unreachable** due to network filtering  
• **If timers are mismatched** it can cause "split-brain" where both active and standby routers think they are active  
• **Use Preempt Delay** to give the router time to populate routing and ARP tables

### Design Considerations
• **Group number reuse** → Same group numbers can be used on different interfaces  
• **Authentication importance** → Mismatched authentication can cause serious operational issues  
• **Timer tuning trade-offs** → Faster timers mean quicker failover but potential stability issues

### Load Balancing Strategy
• **Different VLANs can use different HSRP groups** with different active routers  
• **This provides both redundancy and load distribution** across multiple gateway routers

---
