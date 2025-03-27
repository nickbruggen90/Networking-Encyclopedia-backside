# 3 Planes - Control, Data, Management

### Overview
* Each plane can be protected with separate policies (i.e., CoPP or Management Plane Protection)  
* In many SDN environments the the control plane is centralized on a controller  
* Is a routing protocol down? Control Plane  
* Is a device unreachable on SSH? Management Plane
* Are packets not being forwarded? Data Plane  
* Control Plane builds the path, Data Plane forwards the traffic, Management Plan interacts with the administrator.  
---
### Terminology/Defintions
> * ASICs – silicon chips designed to forward Data Plane tasks at line rate; hardware accelerated  
> * TCAM – accelerates lookup operations for ACL, QoS, VLAN filtering; stores rules in ternary format; allows parallel searches for matching rules; matches by prefix in routing table, ACLs and QoS  
---
### Real World Platforms
```
Catalyst Switches – Management Plane – accessed via console, SSH, web UI
		    Control Plane – typically handled by Supervisory Engine CPU
	            Data Plane – ASIC-drive switching fabric. Forwarding at line-rate. Only 			                            unusual or exceptional packets go to CPU
ISR/ASR routers – Management Plane – SSH, SNMP, etc to the routers CPU
		  Control Plane – typically handled by the Supervisor Engine CPU
		  Data Plane – CEF in hardware on ASR.
Nexus (Data Centers) – Management – NX-OS shell, API interfaces (NX-API, REST)
		       Control Plane – routing protocols, overlay protocols (VXLAN EVPN)
		       Data Plane – ASICs switching at line-rate. Micro-segmentation and 			                                  VXLAN encapsulation in hardware
```

#### *Control Plane* is responsible for building and maintaining network topology and decision making based on routing logic (builds and maintains routing and switching tables)
> * Is how devices talk to each other and build topology and routing knowledge. Makes decisions for traffic handling.  
> * Maintains RIB and Adjacency Tables. Maintains and updates routing, MAC and ARP tables.  
> * Typically runs in software on the Route Processor (RP) or Supervisor CPU  
> * Packets that requires inspection or are unrecognized are punted to the CPU (Control Plane)  
> * CoPP and CPPr are Cisco features that rate-limits or drops, as well as protocol filtering Control Plane traffic beyond certain thresholds  
> * Runs routing protocols (OSPF, EIGRP, BGP); including OSPF Hello packets and BGP updates  
> * Runs switching protocols (STP, VTP, DTP) including BPDU-based features (BPDUGuard/BPDU Filter); switches learn MAC addresses
> * Run neighbor discovery (CDP/LLDP, ARP/ND) to keep track of connected devices and next-hop information.
> * Signaling can include MPLS label distribution, IP Multicast (PIM), NAT control messages, IGMP or VLAN trunking protocols
> * Processes ICMP messages destined for the device itself
> * DHCP Snooping and DAI protect the Control Plane
> * Control Plane Technologies → CPU, specialized handling, CoPP, TCAM interaction


#### *Data Plane (aka Forwarding Plane)* is responsible for actual packet/frame forwarding and switching, including filtering, by moving user data from ingress to egress based on the FIB lookups.
> * Executes predetermined rules configured by Control Plane  
> * Is how traffic from end-hosts (or transit data) gets forwarded across the network. Forwards traffic built by the Control Plane  
> * Processed by ASICs on line cards (or sometimes NPUs) at lines speeds  
> * ASICs allow for high throughput with minimal CPU involvement (10+ gigs or more)  
> * Performs packet lookup against routing tables, applies ACL’s, QoS policies, MPLS, GRE, NAT translation, IPsec, VLAN tagging/removal and the final “push bits out the interface” logic  
> * Handles encapsulation/decapsulation of tunneling protocols (GRE, IPsec, VXLAN)  
> * Packets that requires inspection or are unrecognized are punted to the CPU (Control Plane)  
> * Data Plane Technologies → ASICs, TCAM, CEF  

```
Data Plane Traffic Flow:
Ingress → Policy Enforcement → Lookup → Egress
Ingress: devices checks FIB and Adjacency Tables to find next-hop
Policy Enforcement: ACL and/or QoS rule is applied (permit, deny, mark, etc)
Lookup: next-hop MAC or outgoing interface is determined in hardware
Egress: packet it placed in the correct interface queue and sent out
```

#### *Management Plane* is responsible for device management and administrative access.
> * The Management Plane is how humans talk to and configure devices. Used to configure, manage and monitor the network  
> * Relies on CPU for  configuration changes  
> * Includes protocols and services such as SSH, TelNet, SNMP, NetFlow, NETCONF/RESTCONF, HTTP/HTTPS, API interfaces (Cisco DNA or SDN controllers), REST APIs, local console/VTY, SysLog, gNMI/gRPC  
> * Relies on TFTP/FTP for transfer of IOS images, device reboots and backups  
> * Ties into AAA servers (TACACS+, RADIUS) for admin logins; use RBAC when possible  
> * Restrict access to only trusted hosts/subnets; often placed in a dedicated management VLAN  
> * Features like CPPr can also apply to management traffic  
> * Use out-of-band management if possible  
> * Not directly involved in CEF  

```
Control → Data = Control Plane programs the Data Plane with forwarding rules  
Management → Control = Management Plane configures and monitors Control Plane protocols  
Management → Data = Management Plane can indirectly influence Data Plane by applying ACLs/QoS Policy  
```
---
### Special Packet Scenarios:
* 1. Packets destined for same device:  
> * Where SRC/DST IP belong to the same device  
> * Loopback traffic (handled by software stack)  
> * Self-originating packets  
* 2. Expired TTL:  
> * When the TTL expires; an “ICMP Time Exceeded” message is sent back to the packets source  
* 3. Optional Headers: provides features/functions beyond basic packet forwarding  
> * Common Headers - RR (record route; captures IPs along path), timestamp, SSR (strict source routing; specifies exact route packet must follow), LSR (loose source routing; specifies intermediate routes)  
---
### Use Cases:
> * Enterprise Networks – separate management traffic (from controllers and management stations) from control traffic (routing protocols) and data traffic (user applications) to enhance performance and security
> * Data Centers – use dedicated control plane mechanisms to manage high-volume routing and dynamic workloads, while ensuring that management and data traffic are isolated
> * Service Providers – maintain strict separation of customer data (data plane) from control signaling (control plane) and administrative access (management plane) to provide secure, scalable services
> * SDN/SD-Access Deployments – leverage the three-plane model to integrate centralized policy management, dynamic routing and efficient data forwarding in a SDN
---
