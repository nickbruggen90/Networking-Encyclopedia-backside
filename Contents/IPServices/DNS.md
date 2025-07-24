# DNS - Domain Name System
*RFC 1034 (Concepts), RFC 1035 (Implementation)*

## Overview
• DNS is a hierarchical distributed naming system that translates human-readable domain names into IP addresses  
• Functions as the "phone book" of the internet, allowing users to access websites using memorable names instead of numerical IP addresses  
• Translates domain names (FQDN) to IP addresses  
• Critical for accessing websites, services and various protocols (email, VoIP, etc.)  
• Essential infrastructure that enables internet functionality through distributed resolution

---

## Protocol/Port(s)

| Protocol | Port | Usage |
|----------|------|-------|
| **UDP 53** | 53 | Standard DNS queries (fast, no handshake) |
| **TCP 53** | 53 | Zone transfers (AXFR/IXFR), responses >512 bytes (DNSSEC or EDNS) |
| **TCP 853** | 853 | DNS over TLS (DoT) |
| **TCP 443** | 443 | DNS over HTTPS (DoH) |

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **FQDN** | Fully Qualified Domain Name - complete domain name including all levels |
| **Zone** | Administrative portion of the DNS namespace |
| **Domain** | A subset of the DNS namespace |
| **Record** | Mapping data (A, AAAA, MX, etc.) |
| **Resolver** | Client that queries DNS server |
| **Recursive Resolver** | Queries other DNS servers until it gets an answer |
| **Authoritative Server** | Holds the actual DNS records for a zone |
| **Root Server** | 13 logical servers (A-M) that serve the DNS root zone |
| **DoH** | DNS over HTTPS; encrypted DNS queries via HTTPS |
| **DoT** | DNS over TLS; encrypted DNS queries via TLS |
| **DNS Filtering** | Blocking malicious/unwanted domains |
| **Anycast DNS** | Multiple servers sharing the same IP address for redundancy; used by Google DNS or Cloudflare for geo-distributed resolution |
| **Edge DNS** | DNS servers closer to end users for faster resolution |

---

## DNS Hierarchy

| Level | Description | Examples |
|-------|-------------|----------|
| **Root (.)** | Top level of DNS hierarchy | . |
| **Top-Level Domains (TLD)** | First level domains | .com, .net, .org |
| **Second-Level Domains** | Customer domains | example.com |
| **Subdomains** | Subdivisions of domains | www.example.com |

---

## Types of DNS Servers

| Server Type | Description |
|-------------|-------------|
| **Root Servers** | Top level of DNS hierarchy, 13 root server clusters worldwide (A-M), directs queries to appropriate TLD servers |
| **TLD Servers** | Top-Level Domain servers (.com, .net, .org, etc.), manage second level domain registrations, direct queries to authoritative name servers |
| **Authoritative Name Servers** | Holds the actual DNS records for the domain, runs on Primary/Secondary server model, provide definitive answers for their assigned domains |
| **Recursive Resolvers** | Often ISP or enterprise-provided (8.8.8.8, 1.1.1.1), handle client queries and handle full resolution, cache results and query other servers on behalf of clients |

---

## DNS Record Types

| Record Type | Description |
|-------------|-------------|
| **A** | Maps hostname to IPv4 address |
| **AAAA** | Maps hostname to IPv6 address |
| **CNAME** | Canonical name (alias) |
| **MX** | Mail exchange server |
| **NS** | Name server for a domain |
| **PTR** | Reverse DNS (IP to name) |
| **TXT** | Text record (SPF, DKIM, etc.) |
| **SOA** | Start of authority, zone metadata |
| **SRV** | Location of services |
| **SPF** | Sender Policy Framework (email security) |
| **DNSKEY, RRSIG, DS** | DNSSEC-related records |

---

## DNS Resolution Process

| Step | Description |
|------|-------------|
| **1. Client Query** | Client sends query to local DNS server |
| **2. Cache Check** | Resolver checks local cache for existing record |
| **3. Root Query** | If not cached, queries root server |
| **4. TLD Query** | Root server directs to appropriate TLD server |
| **5. Authoritative Query** | TLD server directs to authoritative name server |
| **6. Response** | Authoritative server provides answer |
| **7. Caching** | Response sent back to client and cached for future use |

---

## Security Considerations

### DNS Threats and Mitigations

| Threat | Description | Mitigation |
|--------|-------------|------------|
| **DNS Spoofing/Poisoning** | Injection of false DNS data | DNSSEC (digital signatures) |
| **Man-in-the-Middle** | Interception of DNS queries | DNS over HTTPS (DoH), DNS over TLS (DoT) |
| **Amplification DDoS** | Using DNS for traffic amplification attacks | Rate limiting, RRL (Response Rate Limiting) |
| **Cache Poisoning** | Corrupting resolver cache with false data | Randomized source ports, 0x20 encoding, DNSSEC |

---

## Advanced Topics

### DNSSEC
• Authenticates DNS responses and prevents tampering  
• Uses digital signatures and chain of trust from root to domain  
• Utilizes RRSIG, DNSKEY, DS, NSEC/NSEC3 records  
• Prevents DNS spoofing and cache poisoning attacks

### Other Advanced Features

| Feature | Description |
|---------|-------------|
| **EDNS(0)** | Extension for DNS, allows larger packets, flags, DNSSEC support |
| **Split-Horizon DNS** | Different responses based on query source |
| **Zone Transfers** | AXFR (full zone transfer), IXFR (incremental zone transfer) |

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **User-Friendly Navigation** | Enables use of memorable domain names instead of IP addresses |
| **Scalability** | Hierarchical structure supports billions of domain names globally |
| **Distributed Architecture** | No single point of failure, redundancy built into the system |
| **Caching Efficiency** | Reduces query load and improves response times |
| **Service Discovery** | Enables location of services through various record types |

---

## Use Cases

| Use Case | Description |
|----------|-------------|
| **Web Browsing** | Primary use for translating website names to IP addresses |
| **Email Routing** | MX records direct email to appropriate mail servers |
| **Load Balancing** | SRV records or round-robin A/AAAA records distribute traffic |
| **Internal Enterprise Resolution** | Split DNS for internal vs external name resolution |
| **Reverse Lookups** | PTR records for security logs, RBLs, SIEM correlation |

---

## Best Practices/Security Considerations

### Configuration Best Practices
• Always configure redundant DNS servers to ensure high availability  
• Use public resolvers for speed when appropriate (8.8.8.8, 1.1.1.1)  
• Implement DNSSEC and DoT/DoH where supported for enhanced security

### TTL Management
• Use short TTLs for dynamic records to enable quick changes  
• Use long TTLs for static, stable records to improve caching efficiency  
• Consider TTL impact during migrations or failover planning

### Monitoring and Maintenance
• **Monitor for latency, cache hits, anomalies, and availability**  
• **Perform regular DNS audits and cleanup** to maintain accuracy  
• **Review DNS logs** for security events and performance issues

---

## Common Issues and Fixes

| Issue | Cause | Solution |
|-------|-------|----------|
| **Slow DNS Resolution** | Misconfigured resolver or distant DNS servers | Configure closer/faster DNS servers, check network connectivity |
| **Stale Cache Entries** | TTL values causing outdated records to persist | Flush DNS cache, adjust TTL values appropriately |
| **Split-Brain DNS Inconsistencies** | Different answers for internal vs external queries | Review split-horizon configuration, ensure consistency |
| **Missing PTR Records** | Incomplete reverse DNS configuration | Configure appropriate PTR records for reverse lookups |
| **DNS Propagation Delays** | Changes take time to spread globally (24-48 hours) | Wait for propagation, use lower TTLs for faster updates |
| **DNS Poisoning** | Malicious injection of false DNS data | Implement DNSSEC, use secure resolvers, monitor for anomalies |

---

## Troubleshooting Tools

| Tool | Platform | Usage |
|------|----------|-------|
| **nslookup** | Windows/Unix | Basic DNS queries and troubleshooting |
| **dig** | Unix/Linux | Advanced DNS queries with detailed output |
| **host** | Unix/Linux | Simple hostname lookups |
| **ping** | All | Basic connectivity and name resolution testing |
| **traceroute/tracert** | Unix/Windows | Shows path and can reveal DNS resolution issues along the route |
| **ipconfig /flushdns** | Windows | Clear local DNS cache |
| **ipconfig /displaydns** | Windows | Display current DNS cache contents |
| **ipconfig /registerdns** | Windows | Refresh DHCP leases and re-register DNS names |
| **systemd-resolve --flush-caches** | Linux | Clear systemd DNS cache |
| **resolvectl** | Linux (systemd) | Query and manage DNS settings |
| **scutil** | macOS | System configuration utility for DNS settings |
| **netstat** | All | Show DNS-related network connections |
| **ss** | Linux | Modern replacement for netstat |
| **whois** | All | Domain registration and nameserver information |
| **drill** | Unix/Linux | Alternative to dig with additional features |
| **kdig** | Unix/Linux | Another dig alternative (part of Knot DNS) |
| **mtr** | Unix/Linux | Combines ping and traceroute functionality |
| **dnsrecon** | Unix/Linux | DNS reconnaissance tool for security testing |
| **fierce** | Unix/Linux | DNS scanner for finding subdomains |
| **nmap** | All | Can perform DNS enumeration and testing |
| **tcpdump/Wireshark** | Network | Packet capture and analysis |
| **tshark** | Network | Command-line version of Wireshark |

---

## Insights

### Operational Considerations
• TTL impacts how long records are cached → Critical for migration or failover planning  
• Recursive resolvers can serve stale data if authoritative DNS becomes unreachable  
• DNS is a critical single point of failure → Proper redundancy and monitoring are essential

### Performance Optimization
• Caching at multiple levels improves performance but can delay propagation of changes  
• Anycast DNS provides geographic distribution and improved response times  
• Edge DNS servers reduce latency by serving requests closer to end users

---
