# OSPF - RFC 2328 (OSPFv2, IPv4), RFC 5340 (OSPFv3, IPv6)

### Overview
* A link-state IGP protocol used within an AS  
* Builds a complete map of the network (LSDB) and calculates shortest path to each destination  
* OSPF scales well in large enterprise networks thanks to its use of areas, reducing the size of LSDBs  
* Priority and RID (loopback IP takes precedence or highest active IP) are used to elect DR/BDR  
---
