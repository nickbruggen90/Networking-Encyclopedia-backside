# SNMP - RFC 1157 (v1), RFC 1901 (v2), RFC 3411-3418 (v3)

### Overview:
> * SNMP is a standard protocol used to monitor and manage network devices. It allows network administrators to collect information about network performance, diagnose problems and configure network devices remotely.
> * SNMP operates over UDP and uses a client-server model, where SNMP manager (client) queries SNMP agents (servers) running on network devices
> * SNMP provides critical insights into network performance and health, making it indispensable for network management and troubleshooting
---
### Timers:
> * Polling Interval – how often the SNMP manager queries the agent (define by the NMS, not SNMP itself)
> * Trap Timeout – for SNMPv2c/v3 informs, how long the agent waits for an acknowledgment before retrying
> * Retry Interval/Retry Count – manager-side configuration that controls retransmission attempts on failed requests
---
### Multicast:
> * SNMP does not use multicast. All communication is unicast (manager-to-agent or agent-to-manager Traps/informs)
---
### Protocol/Port(s):
> * UDP: 161 – managers
> * UDP: 162 – agents to send traps/informs
> * *SNMPv3 optionally supports TCP transport, though UDP remains the most common*
---
### Priorities:
> * SNMP does not define internal priority. QoS/CoS markings can be applied at the network level (DSCP, IP Precedence) to prioritize SNMP messages
---
### Terminology/Definitions:
> * SNMP Manager – a system (often a dedicated network management station) that sends SNMP requests to network devices to retrieve or set information
> * SNMP Agent – software running on a network device that collects and stores management information and responds to SNMP queries
> * MIB – a database schema of all objects a device can report or modify
> * OID – a unique identifier assigned to each managed object in the MIB. OIDs are represented as a series of numbers
> * Community String – a password-like string used in SNMPv1 and SNMPv2c for read-only or read-write access. It acts as a form of authentication, though it is not encrypted
> * SNMP Trap – a message send by an SNMP agent to the SNMP manager to alert it of certain events or errors
> * Trap – unsolicited alert sent from agent to manager (one-way)
> * Inform – like a trap, but requires an acknowledgment 
---
### SNMP Versions:
> * SNMPv1 – the original version, defined in RFC 1157, with basic functionality and minimal security. Simple and widely supported, uses community strings for authentication, limited error handling and security
> * SNMPv2c – an updated version that includes improvements like the GETBULK operation but still relies on community strings for security. Adds GETBULK for efficient data retrieval. Retains community-based security.
> * SNMPv3 – adds significant security improvements with user-based authentication and encryption, defined in RFC 3411-3418. Includes user based access control providing granular permissions. Also provides robust security features: authentication protocols like HMAC-SHA or HMAC-MD5 to verify the sender; as well as encryption protocols DES and AES to encrypt SNMP messages. USM (user based security model) allows for granular permission of users. VACM (view-based access control model) defines read/write views for OIDs
---
### SNMP Process:
> * Polling – the SNMP manager sends GET or GETNEXT/GETBULK requests to an SNMP agent to collect data from the devices MIB
> * Configuration Changes – the SNMP manager can use SET requests to modify configuration on the network device
> * Trap/Inform Requests – agents send asynchronous notifications (traps or inform messages) to the manager when predefined events occur (link down, temperature threshold exceeded, etc)
---
### SNMP Message Types:
> * GET – retrieves the value of one or more MIB objects
> * GETNEXT – retrieves the value of the next object in the MIB hierarchy, useful for walking the MIB
> * GETBULK – available on v2c and v3; efficiently retrieves large blocks of data, minimizing the number of messages
> * SET – allows the SNMP manager to change the value of a writable MIB object
> * TRAP – unsolicited messages sent by the SNMP agent to report events
> * INFORM – available on v2c and v3; similar to traps, but include a mechanism for acknowledgement, ensuring reliable delivery
---
### Best Practices/Security Considerations:
> * For security, always prefer SNMPv3 over SNMPv1/v2c because of its robust authentication and encryption capabilities
> * Apply SNMP views or ACLs to restrict which IP addresses can query your network devices
> * Use strong, unique passwords for SNMPv3 users. Avoid using default community strings like “public” or “private”
> * Regularly review SNMP statistics and logs to monitor for anomalies and ensure the health of your SNMP infrastructure
> * Ensure your network devices are running updated firmware to mitigate known SNMP vulnerabilities
---
### Common Issues and Fixes:
> * Ensure that the SNMP community or SNMPv3 user credentials match between the SNMP manager and agent
> * Verify any ACLs or SNMP views configured on the device that might restrict SNMP access
> * Confirm that the SNMP manager can reach the SNMP agent and that no firewall or routing issues block SNMP traffic
> * Ensure that both the SNMP manager and the device supports the same SNMP version for proper communication
> * SNMP GET Fails?
>   * Fix: verify SNMP version, credentials, ACLs and if MIB is supports
> * No Traps Received?
>   * Fix: check UDP/162 firewall rules, destination reachability, and trap configuration on agent
> * Polling Errors or Timeouts?
>   * Fix: ensure device IP is reachable, SNMP process is running, and ACLs are not blocking
> * Mismatch in Version Support?
>   * Fix: ensure both manager and agent are using the same SNMP version
> * Access Denied to OID?
>   * Fix: SNMPv3 view restrictions or SNMPv1/v2c community string access level is too limited
---
### Insights:
> * Transitioning to SNMPv3 is highly recommended due to the lack of security features in v1/v2c
> * SNMP is often integrated with network management systems (NMS) and SIEM solutions to provide centralized monitoring and alerting
> * Leveraging SNMP traps and polling can help in proactively identifying network issues before they escalate
---
### Commands:
> * 
---
