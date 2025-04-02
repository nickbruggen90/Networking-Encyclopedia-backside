# MPLS - RFC 3031 (MPLS Architecture), RFC 3032 (MPLS Label Stack Encoding), RFC 3209 (RSVP-TE), RFC 5036 (LDP Version 1)

### Overview:
> * MPLS is a high performance, scalable technique used in modern networks to speed up and shape the flow of traffic.
> * MPLS works by attaching short, fixed-length labels to packers and using these labels to make forwarding decisions - bypassing the more complex lookups typically associated with IP routing; this label based forwarding mechanism improves efficiency, supports traffic engineering and enables the creation of VPNs
---
### Timers:
> * LDP – Hello Timer = 30 sec || Hold Timer = 90 sec
> * LSP/RSVP-TE  – Refresh Interval = 30 sec || State Lifetime (hold timer) = 90 sec
---
### Multicast:
> * LDP – IPv4: 224.0.0.2 (all routers group)
> * LDP - IPv6: FF02::2 (link-local all-routers)
---
### Port: 
> * TCP/UDP - 646
---
### Priority Levels:
> * TC Field in Header: 0 - 7
---


