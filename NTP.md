# NTP - RFC 1305 (NTPv3), RFC 5905 (NTPv4)

### Overview
> * Synchronizes the clocks of network devices to a reference time source
> * Operates on a client-server or peer-to-peer model
> * NTP can be over IPv4 (255.255.255.255 or 224.0.1.1) and IPv6 (ff02::101)
---
### Timers: 
> * Poll Interval Timer – Common Ranges = 64 (minimum) - 1024 (maximum)  
	*NTP dynamically adjusts poll interval between a minimum and maximum*
---
### Multicast:
> * IPv4 Multicast: 255.255.255.255 or 244.0.1.1
> * IPv6 Multicast: ff02::101
---
### Protocol/Port(s):
> * UDP: 123
---
### Terminology/Definitions:
> * Poll Interval Timer – determines how frequently a client queries its server for time updates. A device might initially poll every 64 seconds, then back off to a longer interval if the time remains stable. Once stable, NTP extends the interval to reduce network traffic and CPU usage.
> * Burst/iburst – a client sends a quick series of requests when first contacting a server to speed up initial synchronization
> * Reach Register – comparable to “dead timers” in other protocols; reflects recent success of polls
---
### Stratum Model:
> * Stratum 0 – reference clock (atomic clock, GPS receiver); non-networked references
> * Stratum 1 – NTP server directly connected to Stratum 0 device
> * Stratum 2 – syncs from a Stratum 1, and so on
> * Stratum 15 - “orphan stratum”; if all servers are inaccessible, devices can continue to sync each other using orphan stratum. Helps keep local time consistent in a complete outage  
>   * Lower stratum levels are a more authoritative source. Typically, a client device has a stratum one higher than the server from which it’s learning time.
---
