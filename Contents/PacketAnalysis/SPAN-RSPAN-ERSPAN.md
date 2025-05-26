# SPAN/RSPAN/ERSPAN - Cisco proprietary

### Overview: 
> * Monitors and analyzes network traffic; essential for troubleshooting, security and performance tuning
---
### Timers: 
> * No timers by default; session configurations and timeouts are platform specific or configurable. Standard VLAN and MAC table aging apply.
---
### Multicast MAC and VLAN Tagging:
> * VLAN Tag (4 bytes) – consists of a 2-byte TPID (0x8100) and a 2-byte TCI (3 bits for priority, 1 bit for CFI and 12 bits for VLAN ID)
> * Multicast MAC – especially with RSPAN, multicast MAC addresses may be used within the switching fabric; however SPAN/RSPAN sessions usually rely on dedicated VLANs rather than multicast addressing to carry mirror traffic. When a SPAN sessions carries multicast traffic, the original multicast MAC address remains unchanged
---
### Protocol/Port(s):
> * SPAN/RSPAN operates at L2 only and do not require any protocol/port
> * ERSPAN uses GRE (IP Protocol 47) to encapsulate mirrored traffic – important for ACL/firewall and routing configurations to allow GRE
---
### Priorities:
> * You can optionally configure CoS or DSCP markings on ERSPAN traffic to prioritize mirrored packets across a routed network
> * Destination ports used for SPAN should be treated as high-priority by monitoring systems, but they are not inherently QoS-prioritized unless configured
---
### Terminology/Definitions:
> * Source SPAN – the interface(s), VLAN(s) or direction (ingress/egress) from which traffic is mirrored
> * Destination SPAN – the monitoring port or tunnel interface that receives mirrored packets
> * Truncation – an optional SPAN/ERSPAN feature that limits the number of bytes copied per packet
> * ERSPAN ID – a value used to distinguish between multiple ERSPAN sessions when multiple sessions exist between the same source/destination pair
> * SPAN (switched port analyzer) – mirrors traffic from one or more source ports or VLANs to a destination (monitor) port on the same switch. Works only within the same switch (or stack). SPAN does not alter the original frame headers, it copies the frame
> * RSPAN (remote SPAN) – extends SPAN to allow mirroring of traffic from source ports on one switch to a destination port on a remote switch by using a dedicated RSPAN VLAN (across multiple switches). 
> * ERSPAN (encapsulated RSPAN) – further extends RSPAN by encapsulating mirrored traffic (local or remote) in GRE, allowing it to traverse routed (L3) networks; allows monitoring sessions to span L3 boundaries and even traverse WAN links
---
### ERSPAN Header Fields = 
> * 4 - Version - (typically 0 for Type II)
> * 12 - VLAN - (original VLAN of mirrored traffic)
> * 3 - COS - (priority bits from the original VLAN tag)
> * 1 - Truncated - (indicated if mirrored frame was truncated)
> * 10 - Index/Session ID - (id’s ERSPAN sessions – matches erspan-id)
> * 1 - Direction - (indicates ingress or egress)
> * 6 - Reserved
---
### Best Practices/Security Considerations:
> * SPAN –
>   * Only mirror necessary ports/VLANs to avoid overloading the destination port
>   * Ensure the destination port is dedicated to monitoring and not used for regular data traffic
>   * Verify that the mirrored traffic does not exceed the capacity of the destination interface
> * RSPAN –
>   * Use a dedicated RSPAN VLAN that is not used by production traffic
>   * Ensure that the RSPAN VLAN is allowed on all trunk links between source and destination switches
>   * Apply proper security measures (ACLs) to protect the RSPAN VLAN from unauthorized access
> * ERSPAN – 
>   * Ensure proper GRE tunnel configuration and IP reachability between source and collector
>   * Account for the additional overhead introduction by GRE/ERSPAN headers; apply QoS policies if needed
>   * Use clear and unique ERSPAN IDs to differentiate between multiple mirror sessions
---
### Common Issues and Fixes:
> * SPAN –
>   * Verify that the source and destination ports are correctly configured and not administratively down
>   * Check if the destination port is saturated by ensuring that only necessary traffic is mirrored
> * RSPAN –
>   * Ensure the RSPAN VLAN is consistently configured and allowed on all trunk links between the source and destination
>   * Confirm that all switches in the RSPAN domain support the RSPAN feature and are running compatible IOS versions
> * ERSPAN –
>   * Verify that the GRE tunnel is established and that there is IP connectivity between the source and collector
>   * Use debug commands to ensure that the ERSPAN header is formed correctly
>   * Monitor for dropped packets due to insufficient bandwidth or misconfigured QoS on the tunnel
> * Destination Port Not Receiving Traffic?
>   * Cause?: incorrect source interface/direction or misconfigured session ID
>   * Fix: verify the direction (ingress/egress/both) and match source to destination correctly
> * RSPAN Traffic Not Reaching Destination?
>   * Cause?: RSPAN VLAN not allowed across trunk or not configured to intermediate switches
>   * Fix: ensure RSPAN VLAN is allowed and present across the trunk path
> * ERSPAN Not Working?
>   * Cause?: GRE protocol not permitted or routed incorrectly
>   * Fix: check IP reachability and ensure GRE (IP Protocol 47) is allowed
> * Monitoring Tools Missing Packets or Dropping Frames?
>   * Cause?: destination port oversubscribed or mirrored more traffic than it can handle
>   * Fix: use filtering, packet truncation or increase monitoring interface capability
---
### Insights:
> * SPAN is ideal for local monitoring when the analyzer is on the same switch
> * RSPAN allows centralized monitoring across a campus LAN by using a dedicated VLAN for mirror traffic
> * ERSPAN extends the reach further by encapsulating mirror traffic for transport over routed networks, making it suitable for geographically dispersed networks or remote troubleshooting
> * Mirror sessions can introduce extra load on the network; it is important to plan mirror sessions during off-peak hours or use filtering to limit unnecessary traffic
> * Mirrored traffic often contains sensitive data; ensure that access to SPAN/RSPAN/ERSPAN sessions is tightly controlled and monitored
> * Use filters and specific sessions definitions to capture only relevant traffic
---
### Commands:
> * 
---
