# ARP - Address Resolution Protocol  
*RFC 826 (ARP), RFC 903 (RARP), RFC 1027 (Proxy ARP)*  

## Overview  
• Maps IP address to MAC address  
• Layer 2 protocol that resolves IPv4-to-MAC address within the local/broadcast domain; otherwise it will be MAC of default gateway  
• It is essential for Ethernet communication as it enables devices to discover the hardware address of the target device  
• ARP maps a 32-bit IPv4 to a 48-bit MAC address  
• Used only within the local subnet  
• Operates below IP, enabling IP packets to be encapsulated in Ethernet frames  
• Without ARP the host will not be able to resolve the hardware address of another device  

---

## Protocol Details

| Field | Value |
|-------|-------|
| **Protocol Number** | `0x0806` |
| **Broadcast MAC** | `FF:FF:FF:FF:FF:FF` |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **ARP Cache/Table** | Local storage of IP-to-MAC bindings |
| **Aging Timer** | ARP entries expire after a timeout (default 4 hours on Cisco) |
| **Static ARP** | Manually configured ARP entry that doesn't expire |
| **Dynamic ARP** | Learned via normal ARP request/reply interaction |

---

## Importance  
• Ethernet frames require destination MAC address  
• Devices need to communicate within the same subnet  
• Dynamic discovery/resolution through MAC address changes  

---

## Packet Types

| Type | Description |
|------|-------------|
| **ARP Request** | Broadcast asking for MAC address of an IP |
| **ARP Reply** | Unicast with the MAC address of that IP |
| **Gratuitous ARP** | Used for duplicate IP detection, failover/redundancy, MAC address change announcements, and updating ARP caches |

### Gratuitous ARP Use Cases
| Use Case | Description |
|----------|-------------|
| **Duplicate IP Detection** | A device sends a gratuitous ARP for its own IP; if another device responds, a conflict is present |
| **Failover/Redundancy** | Backup device takes over and announces it now owns the IP |
| **MAC Address Change** | NIC replaced or MAC changes, new info sent via gratuitous ARP |
| **Update ARP Cache** | Proactively updates peers' ARP tables in same network segment |

---

## Extended Variants

| Type | Description |
|------|-------------|
| **Proxy ARP** | (RFC 1027) Enabled by default; router responds to ARP requests for hosts in a different subnet. Useful when a host is misconfigured or unaware of subnetting |
| **Reverse ARP (RARP)** | (RFC 903) Requests IPv4 address when only MAC is known. Used by devices at bootup or config change. Replaced by DHCP |
| **Inverse ARP** | Maps IP address from known MAC address (opposite of normal ARP); used in Frame Relay and other NBMA networks |

---

## Operation Codes

| Opcode | Description |
|--------|-------------|
| **ARP Request** | Ask for MAC of specific IP |
| **ARP Reply** | Provide MAC for a specific IP |
| **RARP Request** | Reverse IP for known MAC |
| **RARP Reply** | Respond with IP address |

---

## Common Issues and Fixes

| Symptom | Cause | Fix |
|---------|-------|-----|
| **Host can’t reach local gateway** | ARP cache missing or incorrect | Clear/rebuild ARP cache |
| **Intermittent connectivity** | ARP cache expiration or conflict | Refresh ARP or investigate duplicate MACs |
| **Duplicate IP warning** | Gratuitous ARP conflict | Identify and correct IP overlap |
| **High CPU on switches** | Excessive ARP traffic (ARP flood/DoS) | Use DAI or rate-limit ARP traffic |
| **No ARP reply** | Host offline, firewalled, or L2 issue | Check host status and L2 connectivity |

---

## Insights  
• ARP only works for IPv4; IPv6 uses NDP (Neighbor Discovery Protocol)  
• ARP is trust-based and vulnerable to spoofing or poisoning attacks  
• Use Dynamic ARP Inspection (DAI) for Layer 2 ARP security  

---

## Security Considerations  
• Static entries prevent ARP spoofing but are not scalable  
• Gratuitous ARP detects IP conflicts or MITM attempts
