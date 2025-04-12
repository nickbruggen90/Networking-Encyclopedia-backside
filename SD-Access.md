# SD-Access - DNAC and TrustSec - Cisco proprietary (VXLAN - RFC 7348) (LISP - RFC 6830)

### Overview:
> * A solution for CAN
> * SD-Access can drastically simplify operations, improve security and unify wired/wireless administration
> * Built around DNAC as the controller - automating network design/provisioning (underlay + overlay), policy enforcement/segmentation and centralized management (DNA Assurance)                                                
> * Allows creation of multiple VNs within the campus fabric. Each VN maps to a separate VRF, providing macro segmentation. No extensive ACLs  per interface
> * Reduced configuration complexity; adding a new VLAN or site is simpler thanks to automation (minimal CLI based tasks)
> * Allows for centralized management (a single pane of glass)
> * Consistent policies using SGTs and VRFs across the campus (user-level segmentation)
> * VXLAN-based overlay with LISP control plane can handle large CAN deployments
---
### Terminology/Definitions:
> * Automation – centralized management of entire CAN network via a controller (devices, fabric overlay, policy)
> * Security & Segmentation – micro and macro segmentation via Cisco TrustSec and SGT to isolate users, devices or applications
> * Assurance & Analytics – real-time telemetry and analytics to detect performance or security issues
> * Simplified Operations – minimized CLI-based configurations, reduces deployment times and ensures consistent policy across network
> * Cisco DNA Center (DNAC) – a centralized management and automation platform. Provides GUI and APIs to define policy, manage devices, create VLANs and monitor network health
> * SGT (Security Group Tags) – is a 16-bit metadata tag assigned to users, devices or endpoints to define their security group within Cisco TrustSec-enabled networks
> * SGACL – defines what SGTs can communicate
> * RLOC – routing locator; the loopback IP of a fabric device used as the VXLAN tunnel endpoint
> * EID – endpoint ID; the IP address of the host being advertised or tracked in LISP
> * Map-Server/Resolver (MS/MR) – the LISP component on the Control Plane Node that handles endpoint mappings (EID to RLOC, and visa versa)
> * Fabric Devices – the actual physical devices that form the SD-Access Fabric
> * Overlay –  built using VXLAN (data plane) + LISP (control plane). A VXLAN-based tunnel network created amongst fabric nodes to carry user traffic with built-in segmentation (VN, VRFs, SGTs)
> * Underlay –  traditional routed infrastructure (IS-IS, OSPF, EIGRP) that provides IP connectivity among the physical network devices
> * Wireless Integration – wireless devices integrate into the  same SD-Access fabric; allowing policy
> * Design – defines sites, network settings (IP address pools, device roles) and global parameters in DNAC’s “Design” area
> * Policy – configure user groups, SGTs, and define who can communicate with whom (TrustSec). You can map these policies to VNs
> * Provision – allows you to assign devices to sites. As well as push configurations to devices automatically
> * Assurance – monitor network health, client performances, application analytics and get proactive alerts via DNAC’s AI/ML capabilities
> * Policy – SGT or other metadata can be carried into the VXLAN header for micro-segmentation enforcement
---
### Fabric Roles:
> * Fabric Edge – switches where endpoints connect; encapsulates traffic into VXLAN
> * Fabric Control Plane Node – runs LISP Map-Server/Resolver – tracks endpoint-to-RLOC mappings
> * Fabric Border – gateway between the SD-Access fabric and external networks
> * Intermediate Nodes - transit nodes that forward VXLAN traffic in the underlay
---
### Protocols and Technologies:
> * ISE (Identity Services Engine) – manages identity and SGT assignment; handles AAA along with policy definition. DNA Center applies these SGT-based policies throughout the fabric
> * Cisco TrustSec – microsegmentation mechanism via SGT; ISE issues the SGT assignment, the fabric edges apply the policy
> * LISP – controls endpoint location; separates EIDs (host IPs) from RLOC (fabric node loopbacks) within the Control Plane. When a host connects to a Fabric Edge, that edge registers the host with the Control Plane node. No need for every switch to carry all endpoint routes; only the CP node tracks them all
> * VXLAN – providing the overlay; encapsulates user traffic in a IP/UDP header within the Data Plane, allowing logical separation of different VNs. SGTs can be included in the VXLAN header extension.
---
### Protocol Recap Table:
> * Overlay Data Plane – VXLAN – encapsulates user traffic
> * Control Plane – LISP – tracks endpoints (EID to RLOC)
> * Identity/Auth – 802.1X / MAB + ISE – authenticates users and assigns SGTs
> * Policy – Cisco TrustSec – defines SGACLs and applies segmentation
> * Mgmt and Automation – DNAC – centralized management and monitoring
---
### SD-Access Architecture:
> * Fabric edge nodes are responsible for encapsulating traffic into VXLAN. They map endpoint IP-to-SGT or IP-to-mapping in the local database or query the control plane node
> * A fabric control plane node is typically a router or high end switch acting as a LISP Map-Server. It tracks endpoints (IP, MAC, SGT) to their location. Helps fabric edges locate where to forward traffic within the fabric.
> * A fabric border node is the exit/entry points between SD-Access fabric and external networks like data centers or the internet. They perform route or VRF-Handoff to preserve policy and segmentation.
> * A fabric intermediate node simply provides IP reachability for the VXLAN tunnels
> * VXLAN encapsulates L2 frames in UDP for L3 transport, enabling scalable segmentation
> * LISP is a CoPP to separate endpoint identity (EID) from location (RLOC). The control-plane node acts as a LISP map-server/map-resolver
> * The edge node identifies the control plane node (Map-Server) that “endpoint X is here”. Then the control plane node updates its mapping database EID → RLOC.
> * SGT is used for finer segmentation where endpoints can be assigned security groups via ISE. Traffic is tagged at the fabric edge and policies (SGACL) or VRF boundaries to allow or block traffic. This policy enforcement is consistent across the entire fabric; no need for scatter, per switch ACLs.
> * Macro-Segmentation can be achieved through VNs. Each VN is essentially a separate VRF.
> * Micro-Segmentation is within a single VN and can further divide or limit communication using SGTs
---
### Architecure Highlights:
> * Fabric Edge Node – connects wired/wireless endpoints, applies SGT, and encapsulates into VXLAN
> * Control Plane Node – runs LISP MS/MR to resolve endpoint locations (EID-to-RLOC mappings)
> * Border Node – bridges SD-Access fabric to external networks (VRF handoff, route leaking)
> * Intermediate Node – standard L3 switch/router providing IP reachability between nodes (no fabric awareness)
---
### Operational Workflow:
> * 1. Underlay Provisioning – ensure all fabric nodes have IP connectivity and routing amongst themselves. Done either manually or via DNAC. Routed IP fabric using OSPF/IS-IS/EIGRP with loopbacks
> * 2. Fabric Creation – selects which devices will be CP Node, Border Node and Edge Nodes. Done in DNAC, and deploys LISP/VXLAN/TrustSec configurations automatically
> * 3. VN and SGT Definition – define your VN (employee, guest, IoT, etc) and create security groups in ISE. DNAC uses these groups to define micro-segmentation rules.
> * 4. Policy Enforcement – when a host connects, the Edge Node consults ISE (via 802.1X or MAB) to determine the host's SGT. The Edge Node tags the traffic with that SGT inside the VXLAN. The Edge Node enforces policies if disallowed traffic attempts to cross SGT boundaries
> * 5. DNA Assurance – real time analytics about client onboarding, device health, path traces, etc
---
### Best Practices/Security Considerations:
> * Use Catalyst 9k (or certain Catalyst 3k/4k with newer software) for full hardware VXLAN support. Border and CP nodes are often high-end boxes for scale.
> * Make sure you have a stable underlay by having robust routing, enough IP addressing and stable links
> * Ensure AAA policies are well-defined. SGTs rely on correct identity assignment. Licensing and capacity on ISE matter in large deployments
> * Use multiple CP nodes for high-availability. Use multiple border nodes for external connectivity resilience.
> * Use Anycast Gateway so clients always see the same MAC/IP regardless of fabric edge location
> * Avoid excessive flooding by using LISP-based control plane for endpoint location resolution
> * Ensure consistent IP pools and SGT policies across fabric nodes to prevent policy mismatch
> * Monitor DNACs Assurance tab regularly for onboarding and authentication anomalies
---
### Common Issues and Fixes:
> * Host Not Onboarding?
>   * Fix: check ISE policy sets, SGT assignment, and switch radius config
> * No Traffic Between VN?
>   * Fix: confirm VRF route leaking is configured between VNs if inter-VN traffic is allowed
> * Control Plane Node Not Resolving?
>   * Fix: validate LISP reachability and registration from Edge → CP Node
> * SGACLs Not Working?
>   * Fix: check if correct SGTs are mapped and applied in ISE + DNAC
> * Wireless Device Not Honoring SGTs?
>   * Fix: ensure ELC is fabric-aware and integrated with DNAC
---
### Insights:
> * SD-Access is a major step toward intent-based networking; users and devices are segmented by identity - not IP
> * It reduces the need for complex manual configurations and legacy ACLs
> * Proper integration between DNAC, ISE and fabric roles is the backbone of successful deployment
---
### Commands:
> *
---

