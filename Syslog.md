# SysLog - RFC 5424 (newest SysLog), RFC 3164 (legacy BSD SysLog) RFC 6587 (TCP), RFC 5425 (TLS)

### Overview: 
> * SysLog is the standardized method for sending log/event messages across an IP network
> *  Devices can report operational events to a SysLog server
---
### Timers:
> * Syslog itself is event-driven. Here are the supporting mechanisms and settings that introduce timers:
> * Log Message Rate-Limiting – some devices support rate-limiting to control how many messages are sent per second to prevent overloading the collector or the control plane
> * Buffered Logging Flush Timers – when logging to a buffer or file, messages may be flushed periodically based on system defined intervals
> * Time Stamps – syslog messages can include timestamps (either local system time or synchronized via NTP). Ensure NTP is enabled and accurate across the network for proper correlation
> * Heartbeat Messages – in certain implementations (like syslog-ng or rsyslog), you can configure periodic status updates or heartbeats to verify connection or status between cient and server
---
### Multicast:
> * Syslog is unicast-based by default. Syslog messages are always sent directly to the device.
---
### Protocol/Port(s):
> * UDP: 514
> * TCP: 514
> * TCP: 601 (newer implementation)
> * TCP over TLS: 6514
---
### Priorities:
> * Syslog uses facility and severity level
---
### Terminology/Definitions:
> * Facility – identifies the source subsystem or application (daemon, auth, local0-local7)
> * Messages might display %LINEPROTO-5-UPDOWN, where LINEPROTO is the subsystem, 5 is the severity level, and UPDOWN is the event
---
### SysLog Severity Levels:
> * (0) Emergency – system is unusable
> * (1) Alert – immediate action is needed
> * (2) Critical – critical condition
> * (3) Error – error condition
> * (4) Warning – warning condition
> * (5) Notice – significant but normal conditions
> * (6) Informational – informational messages
> * (7) Debug – debug-level messages
---
### Best Practices/Security Considerations WIP!!!:
> * Use NTP on all devices, so timestamps are correct
> * Limit who can send logs to your server (use ACLs or a management VLAN)
> * Choose severity wisely; to high (debug) can generate excessive noise, to low might miss important information
> * Use a loopback IP to ensure stable SysLog packet delivery
> * If you rely on local logs, increasing the buffer size can help retain more historical messagesUsing local0-local7 is common; helps separate logs from different device types or departments on the SysLog server
> * Implement rate-limiting to avoid flooding the syslog server
> * Protect Syslog servers with RBAC
---
### Common Issues and Fixes:
> * Logs Not Received on Syslog Server?
>   * Cause?: wrong IP or port; ACLs/firewall blocking
>   * Fix: verify server IP/port; check show logging and firewall rules
> * Incorrect Timestamp?
>   * Cause?: NTP misconfiguration or clock drift
>   * Fix: ensure NTP is enabled and synchronized
> * Duplicate Messages?
>   * Cause?: multiple logging destination enabled
>   * Fix: consolidate logging policies; avoid overlapping destinations
> * High CPU Usage on Router?
>   * Cause?: excessive logging or debug level
>   * Fix: lower severity level and/or enable logging rate limits
> * Syslog Buffer Too Small?
>   * Cause?: defaults may be too small to store valuable logs
>   * Fix: use logging buffered [size] to increase buffer size
> * No Logs Sent Over TLS?
>   * Cause?: missing trust relationship or unsupported version
>   * Fix: use valid X.509 certs, verify transport tcp port 6514 and TLC setup
> * Severity Filtering Misconfigured?
>   * Cause?: severity level on device us set too high or low
>   * Fix: use logging trap informational to adjust what’s sent
---
### Insights:
> * In traditional SysLog servers, you can configure the device to send logs using a specific facility so you can filter or categorize them more easily on the server side
> * Tools like Splunk, ELK Stack (Elasticsearch + Logstash + Kibana)or Graylog can parse and archive large volumes of logs with advanced searching mechanism
> * Syslog is often integrated into SIEM systems (Splunk / ELK Stack) to correlate logs across many sources
> * You can filter based on facility, severity or message pattern
---
### Commands:
> *
---

