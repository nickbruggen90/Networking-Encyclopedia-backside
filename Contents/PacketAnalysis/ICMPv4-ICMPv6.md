# ICMP - Internet Control Message Protocol
*RFC 792 (ICMPv4), RFC 4443 (ICMPv6)*

## Overview
• ICMP is a Layer 3 protocol used by network devices to send error messages and operational information  
• Such as when a destination is unreachable or when a ping request is used to test reachability  
• **Not used for data delivery** but assists with network diagnostics, control and path MTU discovery  
• **Encapsulated directly inside IP** (protocol number 1 for ICMPv4, and 58 for ICMPv6)  
• **Connectionless and operates above IP** but below TCP/UDP

---

## Protocol Numbers

| Version | IP Protocol Number | Usage |
|---------|-------------------|-------|
| **ICMPv4** | 1 | IPv4 error and control messages |
| **ICMPv6** | 58 | IPv6 error and control messages |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **ICMP** | A supporting protocol carried by IP used for diagnostics, control and error reporting |
| **Type** | A numeric value (8 bits) indicating the general purpose of the ICMP message |
| **Code** | A sub-classification of the Type field, offering additional detail |
| **Checksum** | A 16-bit field used to verify the integrity of the ICMP header and data |
| **Payload** | The data carried in an ICMP message |
| **PMTUD** | Path MTU Discovery - a process that relies on ICMP Type 3, Code 4 messages to discover the largest packet size that can traverse a path without fragmentation |
| **Rate Limiting** | Routers may limit the number of ICMP messages sent per second to prevent misuse or DoS via ICMP |
| **Glean Adjacency** | Occurs when the device does not have a L2 address for the next hop, it uses ARP (or NS for IPv6) and may send ICMP errors if unresolved |
| **Punt** | A CEF action where a packet is handed off to the CPU for processing |

---

## Type vs Code

| Field | Purpose | Description |
|-------|---------|-------------|
| **Type** | Category definition | Defines the category of ICMP message |
| **Code** | Sub-classification | Provides further detail or context within that message type - essentially a sub-type |

---

## ICMPv4 Types

| Type | Name | Description |
|------|------|-------------|
| **0** | Echo Reply | Echo reply to Type 8 |
| **3** | Destination Unreachable | Various unreachable conditions |
| **5** | Redirect | Router redirect message |
| **8** | Echo Request | Standard ping request |
| **9** | Router Advertisement | Router advertising its presence |
| **10** | Router Solicitation | Host requesting router information |
| **11** | Time Exceeded | TTL or fragment reassembly timeout |
| **12** | Parameter Problem | IP header parameter issue |
| **13** | Timestamp Request | Request for timestamp |
| **14** | Timestamp Reply | Response to timestamp request |

---

## ICMPv4 Codes

### Type 3 Codes (Destination Unreachable)

| Code | Description | Usage |
|------|-------------|-------|
| **0** | Network unreachable | No route to destination network |
| **1** | Host unreachable | Host not responding |
| **2** | Protocol unreachable | Protocol not available |
| **3** | Port unreachable | Port not available |
| **4** | Fragmentation needed; DF set | **PMTUD** - packet too large, fragmentation needed |
| **5** | Source route failed | Source routing error |
| **9** | Network administratively prohibited | Network access denied by policy |
| **10** | Host administratively prohibited | Host access denied by policy |
| **13** | Communication administratively prohibited | Communication blocked by policy |
| **14** | Host precedence violation | Precedence issue |
| **15** | Precedence cutoff in effect | Precedence cutoff active |

### Type 5 Codes (Redirect)

| Code | Description |
|------|-------------|
| **0** | Redirect for datagram network |
| **1** | Redirect datagram for hosts |
| **2** | Redirect for TOS and network |
| **3** | Redirect for TOS and host |

### Type 11 Codes (Time Exceeded)

| Code | Description | Usage |
|------|-------------|-------|
| **0** | TTL exceeded in transit | **Used in traceroute** |
| **1** | Fragment reassembly time exceeded | Fragment timeout |

### Type 12 Codes (Parameter Problem)

| Code | Description |
|------|-------------|
| **0** | Pointer indicates the error in the header |
| **1** | Missing required option |
| **2** | Bad length |

---

## ICMPv6 Types

| Type | Name | Description |
|------|------|-------------|
| **1** | Destination Unreachable | Various unreachable conditions |
| **2** | Packet Too Big | Packet exceeds MTU |
| **3** | Time Exceeded | Hop limit or fragment timeout |
| **4** | Parameter Problem | IPv6 header parameter issue |
| **128** | Echo Request | Ping request |
| **129** | Echo Reply | Reply to type 128 |
| **133** | Router Solicitation (NDP) | Request for router information |
| **134** | Router Advertisement (NDP) | Router advertising its presence |
| **135** | Neighbor Solicitation (NDP) | Similar to ARP request |
| **136** | Neighbor Advertisement (NDP) | Similar to ARP reply |
| **137** | Redirect | Router redirect message |

---

## ICMPv6 Codes

### Type 1 Codes (Destination Unreachable)

| Code | Description |
|------|-------------|
| **0** | No route to destination |
| **1** | Communication with destination administratively prohibited |
| **2** | Beyond scope of source address |
| **3** | Address unreachable |
| **4** | Port unreachable |
| **5** | Source address failed ingress/egress policy |
| **6** | Reject route to destination |
| **7** | Error in source routing header |

### Type 3 Codes (Time Exceeded)

| Code | Description |
|------|-------------|
| **0** | Hop limit exceeded in transit (TTL equivalent) |
| **1** | Fragment reassembly time exceeded |

### Type 4 Codes (Parameter Problem)

| Code | Description |
|------|-------------|
| **0** | Erroneous header field encountered |
| **1** | Unrecognized next header type |
| **2** | Unrecognized IPv6 option encountered |

---

## Behavior and Considerations

### Operational Characteristics
• **ICMP is subject to rate-limiting or blocking** by firewalls  
• **Devices often ignore ICMP redirect messages** due to security concerns  
• **Routers send ICMP Time Exceeded** for TTL expiry (during traceroute)  
• **ICMP unreachable messages may be disabled** on edge firewalls for security  
• **Path MTU Discovery requires ICMP Fragmentation Needed** (Type 3 Code 4)

### Security Implications
• **Rate limiting** prevents ICMP-based DoS attacks  
• **Selective blocking** balances functionality with security  
• **Redirect message handling** often disabled to prevent routing manipulation

---

## Header Breakdown

### ICMPv4 Header Structure

| Field | Size (bits) | Description |
|-------|-------------|-------------|
| **Type** | 8 | ICMP message type |
| **Code** | 8 | Sub-type within the message type |
| **Checksum** | 16 | Header and data integrity check |
| **Rest of Header** | 32 | Type-specific data |
| **Payload** | Variable | Message-specific data |

### ICMPv6 Header Differences
• **Same basic structure** as ICMPv4  
• **Expanded to support:**
  - Multicast messaging
  - Neighbor Discovery (ND)
  - Mandatory use in IPv6 for control-plane tasks

---

## Use Cases

| Use Case | ICMP Types Used | Description |
|----------|-----------------|-------------|
| **Ping** | Type 8/0 (v4), Type 128/129 (v6) | Test device reachability |
| **Traceroute** | Type 11 Code 0 | Uses TTL expiration to map network path |
| **Path MTU Discovery** | Type 3 Code 4 (v4), Type 2 (v6) | Relies on "Fragmentation Needed" messages |
| **Routing Protocol Support** | Various | Some routing protocols use ICMP for adjacency |
| **Error Reporting** | Type 3, 11, 12 | Destination unreachable, redirect messages |
| **IPv6 Neighbor Discovery** | Types 133-137 | Essential for IPv6 operation |

---

## Best Practices/Security Considerations

### Selective ICMP Allowance
• **Allow ICMP selectively** for tools like ping, traceroute and PMTUD  
• **Use ICMP rate limiting** on routers to prevent DoS vectors  
• **Be cautious with ICMP redirects** - often disabled for security

### Monitoring and Filtering
• **Monitor unusual ICMP patterns** (large volume of unreachable or echo messages)  
• **Consider filtering unnecessary ICMP types** at borders (echo on WAN interfaces)  
• **Use ACLs or zone-based firewalls** to scope allowed ICMP traffic

### IPv6 Specific Considerations
• **For IPv6, do not block ICMPv6** or features like SLAAC and NDP will break  
• **ICMPv6 is mandatory** for proper IPv6 operation  
• **Types 133-137 are critical** for Neighbor Discovery Protocol

### Implementation Guidelines
• **Balance security with functionality** when filtering ICMP  
• **Document ICMP filtering policies** for troubleshooting purposes  
• **Test PMTUD functionality** after implementing ICMP filtering

---

## Common Issues and Fixes

| Issue | Symptoms | Potential Cause | Solution |
|-------|----------|-----------------|----------|
| **Ping Fails but Routing is Fine** | Connectivity tests fail | ICMP Echo blocked | Check ACLs or firewalls for ICMP filtering |
| **Traceroute Stops Mid-Path** | Incomplete path discovery | Time exceeded or unreachable blocked | Enable ICMP Type 11 at intermediate devices |
| **PMTUD Breaks Large File Transfer** | Large packets fail to transmit | ICMP Fragmentation Needed blocked | Allow Type 3 Code 4 (v4) or Type 2 (v6) |
| **IPv6 Hosts Don't Autoconfigure** | SLAAC not working | ICMPv6 Router Advertisement blocked | Allow ICMPv6 Types 133-137 |
| **Excessive ICMP Unreachable Messages** | High ICMP traffic volume | Routing loop or black hole | Verify routes and FIB entries |

### Detailed Troubleshooting Steps

#### Connectivity Testing Issues
1. **Verify ICMP filtering policies** on firewalls and ACLs  
2. **Check rate limiting configurations** that might drop ICMP  
3. **Test with different ICMP types** to isolate the issue

#### Path Discovery Problems
1. **Verify TTL handling** at intermediate devices  
2. **Check ICMP Time Exceeded generation** capability  
3. **Review hop-by-hop ICMP filtering**

#### MTU Discovery Failures
1. **Verify ICMP Fragmentation Needed** message generation  
2. **Check DF bit handling** in network devices  
3. **Test with different packet sizes** to confirm PMTUD operation

#### IPv6 Autoconfiguration Issues
1. **Verify ICMPv6 Router Advertisement** transmission  
2. **Check Neighbor Discovery** message flow  
3. **Review IPv6 addressing** and router configuration

#### ICMP Flooding
1. **Implement rate limiting** on ICMP message generation  
2. **Investigate routing loops** or misconfigurations  
3. **Review network topology** for potential issues

---
