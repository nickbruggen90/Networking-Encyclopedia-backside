# NTP - RFC 1305 (NTPv3), RFC 5905 (NTPv4)

### Overview
> * Synchronizes the clocks of network devices to a reference time source
> * Operates on a client-server or peer-to-peer model
> * NTP can be over IPv4 (224.0.1.1) and IPv6 (ff02::101)
---
### Timers: 
> * Poll Interval Timer – Common Ranges = 64 (minimum) - 1024 (maximum)  
	*NTP dynamically adjusts poll interval between a minimum and maximum*
---
### Multicast:
> * IPv4 Multicast: 244.0.1.1
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
> * Stratum 16 - error state 
>   * Lower stratum levels are a more authoritative source. Typically, a client device has a stratum one higher than the server from which it’s learning time.
---
### NTP Modes:
> * Client – a device querying an NTP server
> * Server – a higher stratum device that offers updates to the client
> * Peer (symmetric) – two NTP-enabled devices (peers) can both supply time to each other
> * Broadcast/Multicast – one device broadcasts time updates; listeners receive passively
>   * Client/Server model uses unicast
---
### Use Cases:
> * Logging/Timestamp – Syslog, SNMP traps, debug logs
> * Certificates – PKI, SSL
> * Authentication – Kerberos, 802.1X
---
### Best Practices:
> * Configure at least two servers for redundancy
> * Use authentication
> * Keep a good hierarchy. Avoid multiple devices referencing each other in a loop
> * Only use NTP Master if there is no external time source available
> * Make sure UDP 123 is open and NTP server is reachable; check ACLs blocking UDP 123
---
### Troubleshooting:
> *
---
### Insights:
> * On Linux/Unix NTP daemons, you can adjust the minpoll/maxpoll settings in ntp.conf
> * NTP maintains an 8-bit “reach” register showing the success/failure of recent polls
> * Each poll’s response is used to calculate offset (difference between local clock and server) and round-trip delay. Over time, NTP uses these measurements to adjust the local clock more finely
> * Dispersion or “jitter” values track how consistent the time source has been. Large dispersion can cause the client to switch to another server.
> * The first time a device boots, or if the clock is far off, it can take a few polls before it is considered synchronized. You may see “unsynced” or “stratum unknown” initially
> * If the server is stratum 1 but the device sees it as a stratum 16 (unsynced), then it’s possible the server is not truly synced to the reference clock
---
### Commands:
> *
---
