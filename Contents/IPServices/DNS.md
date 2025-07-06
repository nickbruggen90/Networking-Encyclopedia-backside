# DNS - Domain Name System
*RFC 1034 (concept), RFC 1035 (implementation)*

## Overview
• DNS is a hierarchical distributed naming system that translates human-readable domains into IP addresses  
• It functions as the "phone book" of the internet, allowing users to access websites using memorable names instead of numerical IP addresses  
• Translates domain names (FQDN) to IP address  
• Critical for accessing websites, services and various protocols (email, VoIP, etc)

---

## Protocols/Ports

| Port | Protocol | Description |
|------|----------|-------------|
| **UDP 53** | UDP | Used for standard queries (fast, no handshake) |
| **TCP 53** | TCP | Used for zone transfers (AXFR/IXFR), responses>512 bytes (DNSSEC or EDNS) |
| **TCP 853** | TCP | DNS over TLS (DoT) |
| **TCP 443** | TCP | DNS over HTTPS (DoH) |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **FQDN** | Fully qualified domain name |
| **Zone** | Administrative portion of the DNS namespace |
| **Domain** | A subset of the DNS namespace |
| **Record** | Mapping data (A, AAAA, MX, etc) |
| **Resolver** | Client that queries DNS server |
| **Recursive Resolver** | Queries another DNS server until it gets an answer |
| **Authoritative Server** | Holds the actual DNS record for a zone |
| **Root Server** | 13 logical servers (A - M) that serve the DNS root zone |
| **DoH** | DNS over HTTPS; encrypted DNS queries via HTTPS |
| **DoT** | DNS over TLS; encrypted DNS queries via TLS |
| **DNS Filtering** | Blocking malicious/unwanted domains |
| **Anycast DNS** | Multiple servers sharing the same IP address for redundancy; used by Google DNS or Cloudflare for geo-distributed resolution |
| **Edge DNS** | DNS servers closer to end users for faster resolution |

---

## DNS Hierarchy

| Level | Description |
|-------|-------------|
| **Root (.)** | Top level of DNS hierarchy |
| **Top-Level Domains (TLD)** | .com, .net, .org |
| **Second-Level Domains** | example.com |
| **Subdomains** | www.example.com |

---

## Types of DNS Servers

| Type | Description |
|------|-------------|
| **Root** | Top level of DNS hierarchy, 13 root server clusters worldwide (A-M), directs queries to appropriate TLD servers |
| **TLD** | Top-Level Domain servers (.com, .net, .ord, etc), manage second level domain registrations, direct queries to authoritative name servers |
| **Authoritative Name Servers** | Holds the actual DNS records for the domain, runs on Primary/Secondary server model, provide definitive answers for their assigned domains |
| **Recursive Resolvers** | Often ISP or enterprise-provided (8.8.8.8, 1.1.1.1), handle client queries and handle full resolution, cache results and query other servers on behalf of clients |

---

## DNS Record Types

| Record Type | Description |
|-------------|-------------|
| **A** | Maps hostname to IPv4 |
| **AAAA** | Maps hostname to IPv6 |
| **CNAME** | Canonical name (alias) |
| **MX** | Mail exchange server |
| **NS** | Name server for a domain |
| **PTR** | Reverse DNS (IP to name) |
| **TXT** | Text record (SPF, DKIM, etc) |
| **SOA** | Start of authority, zone metadata |
| **SRV** | Location of services |
| **DNSKEY, RRSIG, DS** | DNSSEC-related |

---

## DNS Resolution Process

| Step | Description |
|------|-------------|
| **1** | Client sends query to local server |
| **2** | Resolver checks cache |
| **3** | If not cached: queries root server |
| **4** | Then TLD server |
| **5** | Then authoritative name server |
| **6** | Response sent back to client |
| **7** | Caching |

---

## DNS Security

| Threat | Mitigation |
|--------|------------|
| **Spoofing/Poisoning** | DNSSEC (digital signatures) |
| **MITM** | DNS over HTTPS (DoH), DNS over TLS (DoT) |
| **Amplification DDoS** | Rate limiting, RRL (response rate limiting) |
| **Cache Poisoning** | Randomized source ports, 0x20 encoding, DNSSEC |

---

## Advanced Topics

| Topic | Description |
|-------|-------------|
| **DNSSEC** | Authenticates DNS response and prevent tampering, digital signatures, chain of trust from root to domain, RRSIG, DNSKEY, DS, NSEC/NSEC3, prevents DNS spoofing and cache poisoning attacks |
| **EDNS(0)** | Extension for DNS, allows larger packets, flags, DNSSEC support |
| **Split-Horizon DNS** | Different responses based on query source |
| **Zone Transfers** | AXFR - full zone transfer, IXFR - incremental |

---

## Troubleshooting DNS

| Tool | Description |
|------|-------------|
| **nslookup** | Basic DNS lookup tool |
| **dig** | Advanced DNS lookup tool |
| **host** | Simple DNS lookup utility |
| **ipconfig /flushdns** | Windows DNS cache flush |
| **systemd-resolve --flush-caches** | Linux DNS cache flush |
| **tcpdump or wireshark** | Network packet analysis |

---

## Common Issues and Fixes

| Issue | Cause | Description |
|-------|-------|-------------|
| **Misconfigured Resolver** | Configuration errors | Incorrect resolver settings |
| **Stale Cache Entries** | TTL values | TTL values determine how long records are cached |
| **Split-brain DNS Inconsistencies** | Configuration mismatch | Different answers for internal vs external queries |
| **Missing PTR Records** | Reverse lookup configuration | Affecting reverse lookups |
| **DNS Propagation** | Global distribution delay | Changes take time to spread globally (24 - 48 hours) |
| **DNS Poisoning** | Security breach | Malicious injection of false DNS data |

---

## Best Practices

• Always configure redundant DNS servers  
• Use public resolvers for speed  
• Monitor for latency, cache hits, anomalies, and availability  
• Implement DNSSEC and DoT/DoH where supported  
• Use short TTLs for dynamic records; long TTLs for static, stable records  
• Regular DNS audits and cleanup

---

## Use Cases

| Use Case | Description |
|----------|-------------|
| **Web Browsing** | Domain name resolution for websites |
| **Email Routing** | Via MX records |
| **Load Balancing** | Via SRV or round-robin A/AAAA |
| **Internal Enterprise Name Resolution** | With split DNS |
| **Reverse Lookups** | Security logs, RBLs, SIEM correlation |

---

## Insights

• TTL impacts how long records are cached - critical for migration or failover planning  
• Recursive resolvers can serve stale data if authoritative DNS becomes unreachable
