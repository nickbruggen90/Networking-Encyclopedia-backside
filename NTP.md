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
### Best Practices/Security Considerations:
> * Configure at least two servers for redundancy
> * Use authentication
> * Keep a good hierarchy. Avoid multiple devices referencing each other in a loop
> * Only use NTP Master if there is no external time source available
> * Make sure UDP 123 is open and NTP server is reachable; check ACLs blocking UDP 123
> * Only allow synchronization from trusted sources. Use ACLs or the restrict command to limit which systems can query or modify time.
> * Deploy one more internal stratum 1 or 2 servers. Do not solely rely on public NTP sources
> * Disable monitoring, control queries and mode 6/7 packets unless absolutely necessary
> * Avoid NTP over public networks when possible
> * Use symmetrical-routing and low-latency paths, especially in real-time environments (VoIP, finance), as asymmetry can cause time drift
> * Regularly monitor NTP logs and peer status
> * Manually set the initial clock for new devices
> * Isolate NTP traffic; optionally use QoS or CoPP to protect NTP packets from congestion or abuse
---
### Common Issues and Fixes:
> * Incorrect system time?
>   * Cause?: no reachable NTP server or invalid server configured
>   * Fix: verify NTP server IP is correct and reachable; test with ping or ntp peer commands
> * High offset or jitter?
>   * Cause?: unstable network path or high latency
>   * Fix: prefer local or geographically close NTP servers; review network path and QoS for stability
> * NTP association stuck in INIT or INSANE state?
>   * Cause?: authentication mismatch or large time difference
>   * Fix: ensure both sides share the correct key; manually set the clock close to real time before retry
> * Server not syncing?
>   * Cause?: ACL misconfiguration (restrict too tight)
>   * Fix: review and adjust ntp access-group or restrict statements to permit sync from trusted sources
> * Unexpected NTP peer chosen?
>   * Cause?: lower stratum peer is unreachable or unstable
>   * Fix: use ntp peer or ntp server with prefer keyword to influence server selection
> * NTP flap or bouncing peers?
>   * Cause?: inconsistent connectivity or bad time source
>   * Fix: remove unreliable NTP servers; verify interface stability and routing path
> * Time gradually drifting again after sync?
>   * Cause?: no regular updates or polling is blocked
>   * Fix?: ensure device maintains regular communication with NTP source; check firewall and polling intervals
> * Log messages showing “no association ID” or “No server specified”?
>   * Cause?: misconfigured or missing NTP peer setup
>   * Fix: double check ntp server, ntp peer, or ntp master configuration commands
> * Router/Device losing sync after reload?
>   * Cause?: NTP takes time to reestablish after boot
>   * Fix: consider setting a hardware clock (clock set) and synchronizing in post-NTP lock with ntp update-calander
> * NTP used in DoS/amplification attack?
>   * Cause?: NTP takes time to reestablish after boot
>   * Fix: disable or restrict monlist, control queries and consider upgrading to versions without this vulnerability
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
