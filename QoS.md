# QoS - RFC 4598 (DSCP Config Guidelines), RFC 2474 (DS Field for IPv4), IEEE 802.1p (CoS)

### Overview:
> * QoS refers to a collections of techniques and mechanisms used to manage network resources and traffic to ensure that critical applications receive the performance they need
> * QoS prioritizes traffic based on its type (voice, video, data) and enforces policies such as traffic classification, making, queuing, scheduling, congestion avoidance, policing and shaping
> *  In converged networks, QoS is essential to reduce latency, packet loss and jitter; factors that are especially critical for real-time applications like VoIP and video conferencing
---
### Timers:
> * Policers – token bucket rate timers; determines how quickly tokens refill for packet forwarding eligibility
> * Shapers – traffic shaping timers; used to calculate when packets can be released to stay within a desired rate
> * WRED – queue monitoring timers; monitors queue depth over time to decide when to begin dropping
> * LLQ/CBWFQ – scheduler intervals; timing tied to interface scheduling logic for strict priority or fair queueing.
---
### Multicast:
> * Multicast routing protocol control messages (PIM, IGMP) often need QoS handling
> * Multicast voice/video must be prioritized with QoS to avoid jitter/delay
> * mDNS is often rate-limited or deprioritized in QoS for IoT heavy networks
---
### Priorities:
> * Layer 2 (CoS): Utilizes IEEE 802.1p, which marks frames with priority values (0 - 7) in the VLAN tag
> * Layer 3 (DSCP): Uses the DSCP field in the IP header (bits 0 - 5 of the TOS byte) to mark packets. The DSCP marking is then used to determine forwarding treatment as the packet traverses the network
---
### Terminology/Definitions:
> * QoS – a set of network techniques used to manage traffic priority, bandwidth allocation, delay and jitter
> * DSCP – a 6-bit field within the IP header’s TOS, used to classify packets into various service levels (0 - 63)
> * CoS – a 3-bit field in the VLAN tag defined in IEEE 8021.p that is used in L2 switching for traffic prioritization (priority ranges 0 - 8)
> * ECN –
> * Marking – the process of setting DSCP values in IP headers (or CoS values in 802.1p) to indicate the priority of the packet
> * Classification – the method by which traffic is identified and sorted into different classes based on defined criteria (source/destination IP, port numbers, protocols)
> * Queuing – the process of placing packets into different queues based on their classification. Common queuing algorithms include Priority Queuing (PQ), Weighted Fair Queuing (WFQ) and Low Latency Queuing (LLQ)
> * Policing – a QoS function that limits the rate of traffic entering the network by dropping or re-marking excess packets
> * Shaping – the process of buffering packets to smooth out bursts of traffic, delaying packets as necessary to adhere to a specified rate
> * Congestion Avoidance – techniques (i.e. WRED) used to preemptively drop packets to prevent buffer overflow
> * Weighted Random Early Detection (WRED) – a congestion avoidance mechanism used in routers and switches that selectively drop packets before a queue becomes full; this helps prevent global synchronization and minimizes bursty packet losses; especially for traffic classes with lower priority
> * Jitter – the variation in the delay of packet arrivals over a network. High jitter can disrupt real-time applications because variability in delay can lead to out-of-order delivery and reduced QoS
> * Expedited Forwarding (EF) – a DiffServ PHB defined for high-priority, low-latency traffic. EF is mapped to DSCP 46 and is used to ensure that real-time traffic receives strict priority
> * Assured Forwarding (AF) – a group of DiffServ PHB that provides different levels of delivery assurance under congestion
> * Drop Precedence – a parameter within AF classes that indicate the priority for packet dropping congestion. Packets with higher drop precedence (such as 3 in AF13) have higher probability of being dropped than those with a lower drop precedence. This mechanism allows for finer differentiation of traffic within the same AF class
> * DSCP Values – determines the PHB in the DiffServ architecture. Common PHBs include best effort, Expedited Forwarding, and Assured Forwarding
> * CS Values – often used as a baseline for QoS configurations and can be mapped to IEEE 802.1p CoS values at L2
---
### AF and EF DSCP Values:
> * AF11 - 1 - DSCP 10 – lower-priority, minor assurance in congestion scenarios
> * AF12 - 2 - DSCP 12 – slightly higher drop probability than AF11
> * AF13 - 3 - DSCP 14 – highest drop probability in AF1x, less reliable under congestion
> * AF21 - 1 - DSCP 18 – used for traffic that needs a moderate level of assurance
> * AF22 - 2 - DSCP 20 – increased drop precedence compared to AF21
> * AF23 - 3 - DSCP 22 – highest drop probability in AF2x
> * AF31 - 1 - DSCP 26 – higher-priority data traffic with moderate assurance
> * AF32 - 2 - DSCP 28 – sightly more aggressive than AF31
> * AF33 - 3 - DSCP 30 – highest drop probability in AF3x
> * AF41 - 1 - DSCP 34 – often used for critical data traffic that still tolerates some loss
> * AF42 - 2 - DSCP 36 – intermediate drop precedence within AF4x
> * AF43 - 3 - DSCP 38 – highest drop probability in AF4x
> * EF - DSCP 46 – strict priority to ensure minimal delay; used for real-time applications like VoIP
---
### 802.1p CoS Values: 0 - 7
> * 0 = lowest priority (best effort)
> * 7 = highest priority
*These values allow switches to prioritize traffic during forwarding and can be mapped to corresponding DSCP values when traffic transitions to L3*  
> * CS0 - DSCP 0 - best effort
> * CS1 - DSCP 8 - often used for low-priority background traffic
> * CS2 - DSCP 16 - typically used for lower priority traffic
> * CS3 - DSCP 24 - mid-level priority
> * CS4 - DSCP 32 - often used for signaling and moderately high level priority
> * CS5 - DSCP 40 - higher priority
> * CS6 - DSCP 48 - used for network control traffic in some cases
> * CS7 - DSCP 56 - highest priority; rarely used, sometimes reserved for network control
---
> For L2 QoS, in the IPv4 header, CoS 802.1p  
CoS field is part of the VLAN tag in IEEE 802.1Q (3 bits)  
Ranges from 0 - 7, with higher value indicating higher priority  
---
### QoS Process:
> * Incoming Traffic → Classification/Marking → Queuing → Scheduling/Forwarding → Policing/Shaping
> * Classification – traffic is inspected and classified into different classes (VoIP, video best-effort, etc) using class-maps
> * Marking – packets are marked with appropriate DSCP values, ensuring their priority is recognized across the network
> * Queuing and Scheduling – packets are placed in queues based on their classification. Scheduling algorithms then decide the order of packet transmission to meet performance requirements
> * Policing and Shaping – policing is applied to limit the traffic rate and enforce policy boundaries, while shaping smooths out bursts by buffering excess packets
---
### Process Overview:
> * Policy-Based QoS – configure policies that define how traffic is classified, marked and treated. This usually is done via class-maps and policy-maps in Cisco IOS
> * Edge vs Core – mark traffic at the network edge (ingress) and trust those markings across the core, applying traditional policies as needed at egress points
> * Congestion Management – utilize mechanisms like WRED to preemptively drop packets in a congested queue to avoid buffer overflow and maintain overall network performance
---
### Header Breakdown: 
*For L3 QoS,in the IPv4 header, DSCP is part of the TOS byte (bits 0 - 5)*  
> * 6 - DSCP - allowing for 64 possible values
> * 2 - ECN - Explicit Congestion Notification
---
### Key Benefits:
> * Enhanced Performance – guarantees bandwidth and minimizes latency for critical applications such as VoIP and video conferencing
> * Efficient Bandwidth Utilization – prioritizes traffic, ensuring high-priority flows get preferential treatment during congestion
> * Improved User Experience – reduces jitter and packet loss, resulting in smoother performance for real-time applications
> * Scalability – QoS policies can be deployed across large networks to manage diverse traffic types efficiently
---
### Use Cases:
> * Voice and Video – prioritizing real-time communications to endure low latency and minimal packet loss
> * Data Centers – managing bandwidth and preventing congestion in data center networks
> * Service Provider Networks – differentiating services for various customer traffic and ensuring SLAs are met
> * Converged Networks – handling mixed traffic environments where data, voice and video share the same infrastructure
---
### Best Practices/Security Considerations:
> * QoS markings should be applied and trusted only at the network edge. Untrusted devices could spoof DSCP/CoS values if proper access control measures aren’t in place
> * Ensuring that QoS configurations align with security policies (such as ACLs, secure management and authentication) is critical to prevent unauthorized changes or manipulation
> * Regularly monitor QoS statistics and logs to detect anomalies that could indicate misconfigurations or deliberate attacks, and adjust policies as network conditions change
> * Mark traffic at the network edge and trust markings only from authenticated sources
> * Use precise classification criteria in your class-maps to ensure correct traffic segmentation
> * Test QoS policies in a lab environment before deployment to prevent network disruptions
> * Maintain detailed documentation of QoS configurations and any changes to policies for future reference
---
### Troubleshooting:
> * Verify that your class-map matches the intended traffic (correct DSCP values)
> * Ensure that marking is done at the ingress and trusted through the network
> * Review queue statistics to identify if certain queues are constantly congested
> * Ensure that the policy-map applied to the interface aligns with your design goals (priority vs fair queuing)
---
### Common Issues and Fixes:
> * Packet not prioritized as expected?
>   * Cause?: DSCP or CoS markings are missing, incorrect or not trusted at intermediate devices
>   * Fix: Ensure classification and marking policies are applied at the edge and that core devices are configured to trust those markings
> * Voice or video traffic experiencing jitter or delay?
>   * Cause?: use LLQ or voice (priority in policy-map), verify bandwidth allocations, and check for queue starvations of other classes
>   * Fix:
> * Excessive packet drops under load?
>   * Cause?: misconfigured policers or insufficient bandwidth allocation
>   * Fix?: review policing rates, ensure shaping is used where needed, and increase buffer sizes or bandwidth guarantees for affected classes
> * Traffic being dropped unexpectedly?
>   * Cause?: WRED or shaping thresholds to aggressive
>   * Fix: tune WRED thresholds to allow more buffer before dropping, or implement shaping policies to smooth out bursts
> * QoS appears to be configured but has no effect?
>   * Cause?: mls qos not enabled globally on Catalyst switches
>   * Fix: use mls qos in global config mode, and verify that policies are attached to the correct interfaces
> * Class-map matches not triggered?
>   * Cause?: incorrect match criteria (DSCP mismatch, undefined ACL)
>   * Fix: double check class-map match statements, and verify they reflect actual traffic behavior with show policy-map interface
> * High CPU or delay on network devices?
>   * Cause?: too many policy-maps or too granular classification
>   * Fix: simplify class-maps, reduce number of classes, or offload classification where hardware support exists
> * QoS counters not increasing?
>   * Cause?: policy not applied at interface, or traffic not matching class-map
>   * Fix: use show policy-map interface to verify applied policies and match counters
---
### Insights:
> * Malicious users might intentionally mark their traffic with high-priority DSCP or CoS values to gain preferential treatment, potentially impacting the performance of critical applications
> * Inadvertent misconfigurations of QoS policies can lead to unintended prioritization or even traffic starvation, which can be exploited to degrade network performance
> * In networks supporting voice, video and data, QoS is essential for maintaining performance and reliability
> * Misconfigurations can lead to severe performance issues, so thorough planning and testing are vital
> * QoS remains a dynamic field with ongoing improvements in standards and best practices, especially as networks become more converged and virtualized
> * Modern network management tools (like Cisco DNA) can automate QoS policy deployment and monitoring, reducing manual errors
---
Commands:
> * 
---

