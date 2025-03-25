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
