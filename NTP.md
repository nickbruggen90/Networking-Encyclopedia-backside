# NTP - RFC 1305 (NTPv3), RFC 5905 (NTPv4)

### Overview
> * Synchronizes the clocks of network devices to a reference time source
> * Operates on a client-server or peer-to-peer model
> * NTP can be over IPv4 (255.255.255.255 or 224.0.1.1) and IPv6 (ff02::101)
---
### Timers 
> * Poll Interval Timer – Common Ranges = 64 (minimum) - 1024 (maximum)
	*NTP dynamically adjusts poll interval between a minimum and maximum*
