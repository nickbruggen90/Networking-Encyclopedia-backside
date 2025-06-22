# DHCP - Dynamic Host Configuration Protocol
*RFC 2131 (IPv4), RFC 3315 (IPv6), RFC 2132 (options and message fields)*

## Overview
• Automatically assigns IP addresses and other network configuration parameters (subnet mask, default gateway, DNS server) to devices on a network  
• DHCPv6 is often used with SLAAC or can do stateful (like DHCPv4) or even stateless DHCPv6 which will provide DNS but let SLAAC handle IP addressing

---

## Timers

| Timer | Typical Value | Description |
|-------|---------------|-------------|
| **Lease Time** | Variable (hours/days) | The duration for which an IP address is assigned to a client |
| **Renewal Time (T1)** | 50% of lease time | DHCP client attempts to renew its lease with the DHCP server directly |
| **Rebinding Time (T2)** | 87.5% of lease time | If renewal fails, client tries to contact any available DHCP server to renew the lease |
| **Discovery/Offer Timeouts** | 4-8 seconds | DHCP clients wait for an offer after sending a Discover message before retrying |

---

## Multicast/Broadcast

| Message Type | Communication Method | Description |
|--------------|---------------------|-------------|
| **DHCP Discover** | Broadcast (MAC FF:FF:FF:FF:FF:FF, IP 255.255.255.255) | DHCP clients initially use broadcast to locate a DHCP server |
| **DHCP Offer, Request, Acknowledge** | Usually broadcast on local subnet | Messages are broadcast until client has an assigned IP address |
| **Multicast Considerations** | Limited use | Some advanced implementations may use multicast internally or in IPv6 DHCPv6 deployments |

---

## Protocols/Ports

| Component | Port | Protocol |
|-----------|------|----------|
| **Server** | UDP 67 | UDP |
| **Client** | UDP 68 | UDP |

---

## Priorities

### QoS Considerations
• **DHCP Message Prioritization** → Treated as high-priority broadcast traffic during initial configuration phase  
• **QoS** → DHCP traffic typically uses default DSCP values (often CS0, best effort) but may be marked with higher DSCP value in congested networks  
• **Operational Priority** → Critical for network access; some networks implement QoS policies to prioritize DHCP packets to avoid delays

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **DHCP** | A network management protocol used to dynamically assign IP addresses and other network configuration parameters to devices on a network |
| **DHCP Server** | The device (or service) that holds a pool of IP addresses and configuration settings. It leases these addresses to DHCP clients for a defined period |
| **DHCP Client** | Any network device (PC, printer, smartphone, etc) that requests network configuration information from a DHCP server |
| **DHCP Relay Agent** | Any network device (usually a router) that forwards DHCP messages between clients and servers when they are not on the same local subnet |
| **Lease** | A temporary allocation of an IP address to a DHCP client. The lease has a defined duration, after which the client must renew it |
| **DHCP Options** | Additional parameters provided by the DHCP server (domain name, NTP server, boot file names) using option fields |
| **DHCP Snooping** | A LAN switch feature that prevents untrusted ports from injecting DHCP offers. Builds a table of MAC-to-IP bindings from DHCP traffic on trusted interfaces |

---

## DHCP Process (DORA)

| Step | Message | Direction | Description |
|------|---------|-----------|-------------|
| **D** | **Discover** | Client → Broadcast | Client broadcasts a DHCPDISCOVER |
| **O** | **Offer** | Server → Client | One or more DHCP servers respond with DHCPOFFER; contains available IP address, subnet mask, lease duration and other requested options |
| **R** | **Request** | Client → Broadcast | Client chooses one offer and broadcasts a DHCPREQUEST stating which server's offer it is accepting |
| **A** | **Acknowledge** | Server → Client | Chosen server finalizes the process with DHCPACK; alternatively may respond with DHCPNAK if offered address is no longer valid |

---

## DHCP Message Types

| Message Type | Direction | Description |
|--------------|-----------|-------------|
| **DHCPDISCOVER** | Client → Server | Initial broadcast to locate DHCP servers |
| **DHCPOFFER** | Server → Client | Server responds with available IP address and configuration |
| **DHCPREQUEST** | Client → Broadcast | Client requests specific IP address from chosen server |
| **DHCPACK** | Server → Client | Server confirms lease and provides final configuration |
| **DHCPNAK** | Server → Client | Server denies request due to conflict or invalid request |
| **DHCPDECLINE** | Client → Broadcast | Client detects address conflict and declines offer |
| **DHCPRELEASE** | Client → Server | Client releases IP address back to server |
| **DHCPINFORM** | Client → Server | Client requests additional configuration data only |

---

## DHCP Options

### Common Options (1-30)

| Option | Description |
|--------|-------------|
| **1** | Subnet mask |
| **2** | Time offset |
| **3** | Default gateway (router) |
| **4** | Time server |
| **5** | Name server |
| **6** | DNS server(s) |
| **7** | Log server |
| **12** | Client's hostname |
| **15** | Domain name |
| **19** | IP forwarding |
| **23** | Default TTL for IP packet |
| **26** | Specifies the MTU of the interface |
| **28** | Broadcast address on the subnet |

### Critical Options (40-70)

| Option | Description |
|--------|-------------|
| **42** | NTP server IPs |
| **43** | Vendor-specific Information (Cisco WLC, Aruba, etc) |
| **44** | NetBIOS name server (WINS) |
| **50** | IP the client requests (used in DHCPREQUEST) |
| **51** | IP address lease time in seconds |
| **53** | DHCP message type |
| **54** | Server identifier |
| **58** | Renewal (T1) time value |
| **59** | Rebinding (T2) time value |
| **60** | Vendor Class Identifier (PXEClient, MSFT 5.0, etc) |
| **61** | Client identifier |
| **66** | TFTP server name (commonly used for IP phone or device provisioning) |
| **67** | Bootfile name (PXE boot or phone configuration) |

### Advanced Options (80+)

| Option | Description |
|--------|-------------|
| **81** | Client FQDN |
| **82** | Relay agent information |
| **119** | Domain search list |
| **120** | SIP servers |
| **121** | Classless static routes (RFC 3442) |
| **150** | TFTP server (Cisco) |

---

## TLV Structure

### Option 43 - Vendor-Specific Information
• **Every DHCP option** = TLV format  
• **Option 43** = TLV that contains sub-TLVs  

### Cisco WLC Use of Option 43
| Field | Value | Description |
|-------|-------|-------------|
| **Type** | F1 | WLC-specific identifier |
| **Length** | Variable | Total bytes for all controller IPs |
| **Value** | Hex format | Controller IP(s) in hexadecimal |

---

## Header Breakdown

### DHCP IPv4 Header Structure

| Field | Size (bytes) | Description |
|-------|--------------|-------------|
| **op** | 1 | Message type; 1 = BOOTREQUEST (client), 2 = BOOTREPLY (server) |
| **htype** | 1 | Hardware type; 1 = Ethernet |
| **hlen** | 1 | Hardware address length (6 for MAC) |
| **hops** | 1 | Used by relay agents to count relay hops |
| **xid** | 4 | Transaction ID; randomly generated by client |
| **secs** | 2 | Seconds elapsed since client began DHCP process |
| **flags** | 2 | Broadcast flags (bit 15 = 1 means broadcast reply) |
| **ciaddr** | 4 | Client IP address (if already assigned) |
| **yiaddr** | 4 | "Your" IP address; the address offered to the client |
| **siaddr** | 4 | IP of the next server (used for booting) |
| **giaddr** | 4 | Relay agent IP address |
| **chaddr** | 16 | Client hardware address (MAC) |
| **sname** | 64 | Optional server host name |
| **file** | 128 | Boot file name (used for PXE boot, etc) |
| **options** | Variable | DHCP options in TLV format |

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Simplified Network Management** | DHCP automates the assignment of IP addresses and configuration parameters, reducing manual configuration and minimizing errors |
| **Efficient Resource Utilization** | Dynamic allocation ensures IP addresses are reused efficiently, particularly important in large or dynamic networks |
| **Centralized Control** | Allows network administrators to maintain centralized control over IP address allocation, simplifying management and troubleshooting |
| **Scalability** | Scalable to support large networks and can be integrated with additional protocols (like DHCP Snooping) to enhance security |
| **Reduced Administrative Overhead** | Automation of IP configuration and renewal processes frees up administrative resources |

---

## Use Cases

| Environment | Application |
|-------------|-------------|
| **Enterprise Networks** | Automatically assign IP addresses to large numbers of devices, ensuring consistent configuration across the network |
| **Guest Networks** | Provide temporary IP addressing for guest users while enforcing restrictions using DHCP Snooping and VLAN segmentation |
| **Wireless Networks** | In conjunction with 802.1X and DHCP Snooping, secure wireless access and prevent unauthorized devices from obtaining network configurations |
| **Data Centers and Cloud Environments** | Manage IP address allocation for dynamic workloads and virtual machines, enabling efficient resource utilization |
| **Service Provider Networks** | Provision customer devices with tight controls to prevent unauthorized or rogue devices from joining networks |

---

## Best Practices/Security Considerations

### Planning and Configuration
• **Keep well-defined subnets** ensuring enough addresses. Plan your address space wisely  
• **Reserve your gateway IP** network devices and any static hosts; use exclusions  
• **Use Option 6** to provide correct DNS server addresses

### Security Implementation
• **Enable DHCP Snooping** to block rogue devices  
• **Ensure DHCP Snooping is enabled** on all access ports and VLANs to prevent unauthorized DHCP servers  
• **Configure trusted ports only** where legitimate DHCP servers are connected. All user-facing ports should be untrusted  
• **Use network segmentation and ACLs** to restrict DHCP traffic to authorized areas only

### Monitoring and Protection
• **Implement rate limiting** on DHCP traffic to minimize the impact of potential DoS attacks  
• **Regularly monitor DHCP logs** and the DHCP Snooping binding table to detect unusual patterns  
• **Integrate DHCP Snooping with IP Source Guard** to validate that devices use correct IP addresses associated with their MAC addresses

### Encryption and Authentication
• **DHCP over IPv4 is not encrypted by default** use DHCPv6 with authentication where available  
• **For IPv4, secure the management plane** to prevent interception or modification of DHCP messages

---

## Common Issues and Fixes

### No DHCP Response/IP Address Not Assigned

| Issue | Cause | Solution |
|-------|-------|----------|
| **Client connectivity** | Physical connection or interface problems | Verify client is physically connected and interface is enabled |
| **DHCP Snooping misconfiguration** | Valid DHCP offers being dropped | Ensure DHCP Snooping is correctly configured on the VLAN |
| **Server reachability** | Client cannot reach DHCP server | Confirm client can reach DHCP server; verify DHCP relay (helper address) configurations |
| **Pool exhaustion** | No available addresses in DHCP pool | Ensure there are enough available addresses in the DHCP pool |

### Authentication or Filtering Problems

| Issue | Cause | Solution |
|-------|-------|----------|
| **Trusted port misconfiguration** | Legitimate DHCP servers not marked as trusted | Verify ports connected to legitimate DHCP servers are marked as trusted |
| **Untrusted port configuration** | Client DHCP messages being blocked | Confirm untrusted ports are correctly configured to allow client DHCP messages |
| **Rate limiting** | DHCP traffic being throttled | Monitor DHCP traffic and ensure rate limiting is not too restrictive |

### Lease and Timing Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Frequent renewals** | Lease time too short | Check lease time settings on DHCP server |
| **Timing synchronization** | NTP issues affecting lease timing | Ensure NTP is set, especially if DHCP server provides option 42 (NTP server) |

---

## Insights

### Configuration Pitfalls
• **Excluded ranges can overlap** and inadvertently reduce your pool size to zero  
• **No IP helpers or wrong IP helper** leads to clients not receiving addresses  
• **Ensure NTP is set** especially if the DHCP server provides option 42 (NTP server)

### Monitoring and Troubleshooting
• **Check Syslog messages** for any DHCP-related errors or warnings  
• **Ensure SNMP traps are properly configured** to alert you to DHCP server issues  
• **Regular monitoring of DHCP statistics** helps identify potential issues before they impact users

### Integration Considerations
• **DHCP Snooping integration** with DAI provides comprehensive Layer 2 security  
• **Proper VLAN configuration** is critical for DHCP functionality across network segments  
• **Relay agent configuration** must be consistent across all network devices

---
