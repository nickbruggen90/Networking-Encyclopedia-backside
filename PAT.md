# PAT - RFC 1631, RFC 3022, RFC 4787
*REFER TO NAT NOTES*

### Overview:
> * Also known as NAT Overload; PAT is a variation of NAT that allows multiple hosts to share a single public IP address by using unique port numbers for each session
> * PAT changes the source IP address to the router's public IP address and assigns a unique source port; PAT translation table maintains this mapping
---
### Protocol/Port(s);
> * PAT operates on TCP, UDP, and ICP (using identifiers instead of protocols)
> * Dynamic port selection range: 1024 - 65535
---
### Terminology/Definitions:
> * Inside Local – private IP address assigned to a host inside the network
> * Inside Global – public IP address representing the internal host to the outside world
> * Translation Table – a temporary mapping of inside local IP:port to inside global IP:port created for each unique session
> * Port Exhaustion – a state where all available port numbers are in use, preventing new connections
> * PAT (NAT Overload) – a form of NAT where many internal IP addresses are translated to a single public IP address by differentiating sessions via a unique port number
---
### Operations:
> 1. Internal host sends packet to the Internet
> 2. The router replaces the source IP and source port with its own public IP and a unique port
> 3. Entry is created in the NAT translation table, mapping the Inside Local IP:Port to Inside Global IP:Port
> 4. When return traffic comes in, the router references the table to forward packets to the correct internal host and port
---
### Key Benefits:
> * Allows many-to-one IP address translation, perfect for IPv4
> * Transparent to internal hosts
> * Enables full outbound connectivity for private hosts with minimal public IP usage
---
### Use Cases:
> * Home/Business Internet access using a single public IP
> * ISP edge routers serving customers with overlapping internal IP space
> * Remote offices accessing centralized services using a single NATed IP
---
### Best Practices:
> * PAT can lead to a high number of simultaneous translations; monitor resource utilization to avoid port exhaustion
> * Ensure that appropriate firewall and ACL rules are in place
> * Document PAT configurations and associated ACLs to simplify troubleshooting and future audits
> * Monitor router CPU and NAT translation table utilization to prevent overload
> * Use ACLs to control what internal devices or subnets are allowed to use PAT
> * Document PAT pool IPs and their use cases for auditability
> * Set connection timeouts conservatively to recycle idle sessions and free ports
> * Avoid PAT for services that require inbound connections (unless port forwarding is configured)
---
### Common Issues and Fixes:
> * Look for excessive translations entries which might indicate a potential overload or misconfiguration
> * Issue: PAT Overload or Port Exhaustion?
>   * Cause?: too many simultaneous sessions from internal hosts
>   * Fix: use additional public IPs in the PAT pool; reduce session timeout; analyze for misbehaving apps
> * Inconsistent or Failed Connectivity?
>   * Cause?: NAT table full; no port available
>   * Fix: use show ip nat translations and show ip nat statistics to assess load
> * Issues With Applications Using Embedded IPs (SIP, FTP, etc)?
>   * Fix: use NAT ALG (application layer gateway) or NAT-aware versions of these protocols
> * Inbound Connections Fail?
>   * Cause?: PAT blocks unsolicited inbound traffic by default
>   * Fix: configure static NAT or port forwarding for specific services
---
### Insights:
> * PAT is a critical tool for IPv4 conservation but introduces complexity for some protocols, especially those embedding IPs or using dynamic ports.
> * As networks grow, reliance on PAT can introduce scale bottlenecks; IPv6 eliminates the need for NAT but many enterprises still rely on PAT in mixed environments.
> * Use ip nat pool and overload options on Cisco IOS to control how PAT operates and scales.
> * Debug tools like debug ip nat and logging timeouts are helpful when diagnosing overload or misbehaving traffic flows.
---
Commands:
> *
---


