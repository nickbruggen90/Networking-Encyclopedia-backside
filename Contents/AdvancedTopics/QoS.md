# QoS - Quality of Service
*RFC 4598 (DSCP Config Guidelines), RFC 2474 (DS Field for IPv4), IEEE 802.1p (CoS)*

## Overview
• QoS refers to a collection of techniques and mechanisms used to manage network resources and traffic to ensure that critical applications receive the performance they need  
• QoS prioritizes traffic based on its type (voice, video, data) and enforces policies such as traffic classification, marking, queuing, scheduling, congestion avoidance, policing and shaping  
• In converged networks, QoS is essential to reduce latency, packet loss and jitter; factors that are especially critical for real-time applications like VoIP and video conferencing

---

## Timers

| Timer Type | Purpose | Description |
|------------|---------|-------------|
| **Policers** | Token bucket rate timers | Determines how quickly tokens refill for packet forwarding eligibility |
| **Shapers** | Traffic shaping timers | Used to calculate when packets can be released to stay within a desired rate |
| **WRED** | Queue monitoring timers | Monitors queue depth over time to decide when to begin dropping |
| **LLQ/CBWFQ** | Scheduler intervals | Timing tied to interface scheduling logic for strict priority or fair queuing |

---

## Multicast
• **Multicast routing protocol control messages** (PIM, IGMP) often need QoS handling  
• **Multicast voice/video** must be prioritized with QoS to avoid jitter/delay  
• **mDNS** is often rate-limited or deprioritized in QoS for IoT heavy networks

---

## Priorities

| Layer | Standard | Field | Range | Usage |
|-------|----------|-------|-------|-------|
| **Layer 2 (CoS)** | IEEE 802.1p | VLAN tag priority | 0-7 | Marks frames with priority values in the VLAN tag |
| **Layer 3 (DSCP)** | RFC 2474 | IP header TOS byte | 0-63 | Uses bits 0-5 of the TOS byte to mark packets for forwarding treatment |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **QoS** | A set of network techniques used to manage traffic priority, bandwidth allocation, delay and jitter |
| **DSCP** | A 6-bit field within the IP header's TOS, used to classify packets into various service levels (0-63) |
| **CoS** | A 3-bit field in the VLAN tag defined in IEEE 802.1p that is used in L2 switching for traffic prioritization (priority ranges 0-7) |
| **ECN** | Explicit Congestion Notification - 2-bit field used for congestion signaling |
| **Marking** | The process of setting DSCP values in IP headers (or CoS values in 802.1p) to indicate the priority of the packet |
| **Classification** | The method by which traffic is identified and sorted into different classes based on defined criteria (source/destination IP, port numbers, protocols) |
| **Queuing** | The process of placing packets into different queues based on their classification. Common queuing algorithms include Priority Queuing (PQ), Weighted Fair Queuing (WFQ) and Low Latency Queuing (LLQ) |
| **Policing** | A QoS function that limits the rate of traffic entering the network by dropping or re-marking excess packets |
| **Shaping** | The process of buffering packets to smooth out bursts of traffic, delaying packets as necessary to adhere to a specified rate |
| **Congestion Avoidance** | Techniques (i.e. WRED) used to preemptively drop packets to prevent buffer overflow |
| **WRED** | Weighted Random Early Detection - a congestion avoidance mechanism that selectively drops packets before a queue becomes full; helps prevent global synchronization and minimizes bursty packet losses |
| **Jitter** | The variation in the delay of packet arrivals over a network. High jitter can disrupt real-time applications because variability in delay can lead to out-of-order delivery and reduced QoS |
| **Expedited Forwarding (EF)** | A DiffServ PHB defined for high-priority, low-latency traffic. EF is mapped to DSCP 46 and is used to ensure that real-time traffic receives strict priority |
| **Assured Forwarding (AF)** | A group of DiffServ PHB that provides different levels of delivery assurance under congestion |
| **Drop Precedence** | A parameter within AF classes that indicates the priority for packet dropping during congestion. Higher drop precedence means higher probability of being dropped |

---

## AF and EF DSCP Values

### Assured Forwarding (AF) Classes

| AF Class | Drop Precedence | DSCP Value | Description |
|----------|-----------------|------------|-------------|
| **AF11** | 1 | 10 | Lower-priority, minor assurance in congestion scenarios |
| **AF12** | 2 | 12 | Slightly higher drop probability than AF11 |
| **AF13** | 3 | 14 | Highest drop probability in AF1x, less reliable under congestion |
| **AF21** | 1 | 18 | Used for traffic that needs a moderate level of assurance |
| **AF22** | 2 | 20 | Increased drop precedence compared to AF21 |
| **AF23** | 3 | 22 | Highest drop probability in AF2x |
| **AF31** | 1 | 26 | Higher-priority data traffic with moderate assurance |
| **AF32** | 2 | 28 | Slightly more aggressive than AF31 |
| **AF33** | 3 | 30 | Highest drop probability in AF3x |
| **AF41** | 1 | 34 | Often used for critical data traffic that still tolerates some loss |
| **AF42** | 2 | 36 | Intermediate drop precedence within AF4x |
| **AF43** | 3 | 38 | Highest drop probability in AF4x |

### Expedited Forwarding (EF)
| Class | DSCP Value | Description |
|-------|------------|-------------|
| **EF** | 46 | Strict priority to ensure minimal delay; used for real-time applications like VoIP |

---

## CoS and CS Values

### 802.1p CoS Values (0-7)
• **0** = Lowest priority (best effort)  
• **7** = Highest priority  
*These values allow switches to prioritize traffic during forwarding and can be mapped to corresponding DSCP values when traffic transitions to L3*

### Class Selector (CS) Values

| CS Class | DSCP Value | Description |
|----------|------------|-------------|
| **CS0** | 0 | Best effort |
| **CS1** | 8 | Often used for low-priority background traffic |
| **CS2** | 16 | Typically used for lower priority traffic |
| **CS3** | 24 | Mid-level priority |
| **CS4** | 32 | Often used for signaling and moderately high level priority |
| **CS5** | 40 | Higher priority |
| **CS6** | 48 | Used for network control traffic in some cases |
| **CS7** | 56 | Highest priority; rarely used, sometimes reserved for network control |

---

## Header Breakdown

### Layer 2 QoS (CoS)
**CoS field is part of the VLAN tag in IEEE 802.1Q (3 bits)**  
• Ranges from 0-7, with higher value indicating higher priority

### Layer 3 QoS (DSCP)
**DSCP is part of the TOS byte in the IPv4 header (bits 0-5)**

| Field | Bits | Range | Purpose |
|-------|------|-------|---------|
| **DSCP** | 6 | 0-63 | Allowing for 64 possible values |
| **ECN** | 2 | 0-3 | Explicit Congestion Notification |

---

## QoS Process

### Processing Flow
```
Incoming Traffic → Classification/Marking → Queuing → Scheduling/Forwarding → Policing/Shaping
```

### Process Steps

| Step | Description | Implementation |
|------|-------------|----------------|
| **Classification** | Traffic is inspected and classified into different classes (VoIP, video, best-effort, etc) | Using class-maps |
| **Marking** | Packets are marked with appropriate DSCP values | Ensuring priority is recognized across the network |
| **Queuing and Scheduling** | Packets are placed in queues based on their classification | Scheduling algorithms decide transmission order |
| **Policing and Shaping** | Policing limits traffic rate and enforces policy boundaries | Shaping smooths out bursts by buffering excess packets |

---

## Process Overview

### Implementation Strategy
• **Policy-Based QoS** → Configure policies that define how traffic is classified, marked and treated. Usually done via class-maps and policy-maps in Cisco IOS  
• **Edge vs Core** → Mark traffic at the network edge (ingress) and trust those markings across the core, applying additional policies as needed at egress points  
• **Congestion Management** → Utilize mechanisms like WRED to preemptively drop packets in a congested queue to avoid buffer overflow and maintain overall network performance

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Enhanced Performance** | Guarantees bandwidth and minimizes latency for critical applications such as VoIP and video conferencing |
| **Efficient Bandwidth Utilization** | Prioritizes traffic, ensuring high-priority flows get preferential treatment during congestion |
| **Improved User Experience** | Reduces jitter and packet loss, resulting in smoother performance for real-time applications |
| **Scalability** | QoS policies can be deployed across large networks to manage diverse traffic types efficiently |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Voice and Video** | Prioritizing real-time communications to ensure low latency and minimal packet loss |
| **Data Centers** | Managing bandwidth and preventing congestion in data center networks |
| **Service Provider Networks** | Differentiating services for various customer traffic and ensuring SLAs are met |
| **Converged Networks** | Handling mixed traffic environments where data, voice and video share the same infrastructure |

---

## Best Practices/Security Considerations

### Security Implementation
• **QoS markings should be applied and trusted only at the network edge** → Untrusted devices could spoof DSCP/CoS values if proper access control measures aren't in place  
• **Ensure QoS configurations align with security policies** (ACLs, secure management and authentication) to prevent unauthorized changes or manipulation  
• **Regularly monitor QoS statistics and logs** to detect anomalies that could indicate misconfigurations or deliberate attacks

### Configuration Guidelines
• **Mark traffic at the network edge** and trust markings only from authenticated sources  
• **Use precise classification criteria** in your class-maps to ensure correct traffic segmentation  
• **Test QoS policies in a lab environment** before deployment to prevent network disruptions  
• **Maintain detailed documentation** of QoS configurations and any changes to policies for future reference

---

## Troubleshooting

### General Troubleshooting Steps
• **Verify that your class-map matches** the intended traffic (correct DSCP values)  
• **Ensure that marking is done at the ingress** and trusted through the network  
• **Review queue statistics** to identify if certain queues are constantly congested  
• **Ensure that the policy-map applied to the interface** aligns with your design goals (priority vs fair queuing)

---

## Common Issues and Fixes

| Problem | Cause | Solution |
|---------|-------|----------|
| **Packets not prioritized as expected** | DSCP or CoS markings are missing, incorrect or not trusted at intermediate devices | Ensure classification and marking policies are applied at the edge and that core devices are configured to trust those markings |
| **Voice or video traffic experiencing jitter or delay** | LLQ not configured or bandwidth allocation issues | Use LLQ or voice (priority in policy-map), verify bandwidth allocations, and check for queue starvation of other classes |
| **Excessive packet drops under load** | Misconfigured policers or insufficient bandwidth allocation | Review policing rates, ensure shaping is used where needed, and increase buffer sizes or bandwidth guarantees for affected classes |
| **Traffic being dropped unexpectedly** | WRED or shaping thresholds too aggressive | Tune WRED thresholds to allow more buffer before dropping, or implement shaping policies to smooth out bursts |
| **QoS appears configured but has no effect** | `mls qos` not enabled globally on Catalyst switches | Use `mls qos` in global config mode, and verify that policies are attached to the correct interfaces |
| **Class-map matches not triggered** | Incorrect match criteria (DSCP mismatch, undefined ACL) | Double check class-map match statements, and verify they reflect actual traffic behavior with `show policy-map interface` |
| **High CPU or delay on network devices** | Too many policy-maps or too granular classification | Simplify class-maps, reduce number of classes, or offload classification where hardware support exists |
| **QoS counters not increasing** | Policy not applied at interface, or traffic not matching class-map | Use `show policy-map interface` to verify applied policies and match counters |

---

## Insights

### Security Considerations
• **Malicious users might intentionally mark their traffic** with high-priority DSCP or CoS values to gain preferential treatment, potentially impacting the performance of critical applications  
• **Inadvertent misconfigurations of QoS policies** can lead to unintended prioritization or even traffic starvation, which can be exploited to degrade network performance

### Implementation Reality
• **In networks supporting voice, video and data** QoS is essential for maintaining performance and reliability  
• **Misconfigurations can lead to severe performance issues** so thorough planning and testing are vital  
• **QoS remains a dynamic field** with ongoing improvements in standards and best practices, especially as networks become more converged and virtualized

### Modern Management
• **Modern network management tools** (like Cisco DNA) can automate QoS policy deployment and monitoring, reducing manual errors  
• **Regular monitoring and adjustment** of QoS policies is essential as network conditions change

---

## Commands
*[To be completed during lab sessions]*
