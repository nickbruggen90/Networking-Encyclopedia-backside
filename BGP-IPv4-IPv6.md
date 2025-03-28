BGP - RFC 4271 (defines BGP-4), RFC 4456 (route reflectors), RFC 4364 (BGP/MPLS IP VPNs), RFC 6793 (AS numbers)

### Overview:
> * Path-Vector protocol
> * Supports IPv4 and IPv6
> * Can handle hundreds of thousands (or millions) of routes; using the BGP table with all candidate paths. And injecting the best path into the router's RIB
---
### Timers:
> * Hold Timer = 180 sec || Keepalive Timer = 60 sec (⅓ of Hold) || Connect Retry = 120 sec 
> * MRAI eBGP = 30 sec || MRAI iBGP = 5 sec
     
> * eBGP default TTL is 1, meaning neighbors must be directly connected (unless using ebpg-multihop)
> * iBGP default TTL is 255
---
