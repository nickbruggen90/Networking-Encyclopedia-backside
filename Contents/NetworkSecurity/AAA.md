# AAA - Authentication, Authorization, Accounting
*RFC 2865/2866 (RADIUS)*

## Overview
• AAA provides a structured way to manage who can access the network (Authentication), what they can do (Authorization) and track their actions (Accounting)  
• RADIUS and TACACS+ are the two main protocols, each with different use cases  
• Proper AAA is critical for security, compliance and consistent user/device management across the enterprise

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **AAA** | A framework for controlling and tracking user access to network resources. It is divided into three core functions |
| **Authentication** | Verifies users' identities before granting access. Examples include username/password, tokens, certificates |
| **Authorization** | Defines what a user can do after they are authenticated. Examples include limiting commands (privileged levels), specifying allowed networks and resources |
| **Accounting** | Logs the users actions, resource usage and/or session statistics. Often used for auditing, billing and compliance |
| **RADIUS** | A widely used AAA protocol that provides centralized AAA for network access. RADIUS is typically used for 802.1X authentication, VPN access and wireless networks |
| **TACACS+** | Another AAA protocol developed by Cisco. TACACS+ separates the functions of AAA into distinct processes, providing more granular control compared to RADIUS. It is typically used for device management and administrative access |
| **802.1X** | An IEEE standard for port-based network access control that uses AAA (typically RADIUS) to authenticate devices before granting them network access |
| **EAP** | A framework used by 802.1X and many AAA systems that supports multiple authentication methods (EAP-TLS, EAP-PEAP) |

---

## Protocol/Port(s)

### RADIUS
| Protocol | Ports | Usage |
|----------|-------|-------|
| **RADIUS** | UDP 1812, UDP 1813 | Usually for 802.1X or remote user VPN in Cisco environments |

**RADIUS Characteristics:**
• Encrypts only the password field, not the entire payload  
• Combines authentication and authorization in one step  
• Typically used for network access (802.1X, VPN, Wi-Fi)  
• Also has vendor specific attributes for advanced features

### TACACS+
| Protocol | Port | Usage |
|----------|------|-------|
| **TACACS+** | TCP 49 | Usually used for privileged router/switch admin access in Cisco environments |

**TACACS+ Characteristics:**
• Encrypts the entire payload making it more secure for device administrators  
• Separates authentication, authorization and accounting more distinctly  
• Typically used for device administration (router/switch CLI access)

---

## Switches vs Routers
• Configuration is similar, but on Catalyst switches you may tie AAA to 802.1X for port-based authentication

---

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Improved Security** | AAA provides robust security by ensuring that only authenticated and authorized users can access network resources. It minimizes the risk of unauthorized access |
| **Centralized Management** | With a centralized AAA solution, network administrators can manage user access and permissions across all network devices from one location |
| **Granular Control** | AAA enables fine-grained access control by defining specific roles and permissions for different users or groups, ensuring that each user only has access to the resources they need |
| **Accountability and Auditing** | Through accounting, AAA tracks user activity and network usage. This is crucial for auditing, troubleshooting and compliance with regulatory requirements |
| **Scalability** | AAA solutions scale well with large networks, making them suitable for enterprise environments, SPs and multi-tenant architectures |

---

## Use Cases

### Protocol Selection by Use Case
| Use Case | Recommended Protocol | Reason |
|----------|---------------------|---------|
| **Device Administration** | TACACS+ | Fine-grained command authorization (certain commands allowed, others restricted) |
| **Network Access (802.1X, VPN, Wi-Fi)** | RADIUS | Standard protocol that includes necessary AV pairs for dynamic VLAN assignment, session termination, etc |

### Common Implementation Scenarios

**Enterprise Network Access**
• Use AAA to authenticate and authorize employees for access to wired and wireless networks  
• 802.1X, integrated with RADIUS, is commonly used in such environments

**Device Management**
• TACACS+ is widely used for managing administrative access to routers, switches and firewalls  
• Allows for detailed logging of configuration changes and command usage

**VPN Access**
• AAA systems secure remote access by authenticating users connecting via VPNs  
• Ensures that only authorized users can establish secure VPN sessions

**Guest and Public WiFi**
• Implement AAA to differentiate between employee and guest access  
• Apply appropriate policies for network segmentation and usage restrictions

**Service Providers**
• ISPs use AAA to manage access to network services, authenticate subscribers and bill based on usage  
• Ensures that only paying customers receive network services

**MFA Integration**
• Integrates RADIUS or TACACS+ with an external MFA server (RSA tokens, Duo, etc.)

**Privileged Levels**
• Cisco devices can assign different privilege levels or shell exec modes  
• TACACS+ can specify which level a user gets upon login

**Regulatory Compliance**
• Many industries (finance, healthcare) require detailed logging and auditing of user accounts  
• AAA provides the necessary accounting functions to meet these requirements

---

## Best Practices/Security Considerations

### Configuration Best Practices
• **Enable AAA new-model** → Gains flexible method-lists. Start with a test environment or console fallback to avoid locking yourself out  
• **Use OOB method for fallback** → Always maintain emergency access  
• **Test changes carefully** → Mistakes in AAA can lock out administrators if fallback is not properly configured  
• **Always keep a fallback** → Like local user or console to avoid lockouts

### Security Hardening
• **Always set encryption keys** for RADIUS/TACACS+ servers. Use strong encryption  
• **On the server side** → Store hashed or encrypted passwords  
• **Log high-level commands** or restrict certain commands to specific admin roles  
• **Separate RADIUS and TACACS+ servers** → Configure separate redundancy

### Access Control
• **Define user roles** and grant only the minimum permissions required for each role  
• **Use centralized RADIUS or TACACS+ servers** to manage AAA for all network devices  
• **Avoid transmitting passwords in clear text** → Use protocols that support encryption and challenge-response mechanisms

### Infrastructure Security
• **Ensure that RADIUS or TACACS+ servers are secured** and regularly updated to patch vulnerabilities  
• **Use firewalls and VLAN segmentation** to isolate AAA servers from general network traffic  
• **Deploy multiple AAA servers** in a redundant configuration to ensure continuous operation

### Monitoring and Maintenance
• **Maintain comprehensive logging** of authentication attempts, authorization decisions and accounting data  
• **Regular audits** help detect anomalies and ensure compliance with policies  
• **Review and update AAA policies periodically** to ensure they meet current security standards  
• **Use accounting data** to detect suspicious patterns and configure alerts for potential intrusions

---

## Troubleshooting

### Common Issues and Solutions

**Authentication Server Not Responding**
• **Cause:** Mismatched keys or server addresses commonly cause this error  
• **Solution:** Verify server reachability, keys, and configuration

**Debug and Monitoring**
• Use real-time debugging to see if the device sends requests and receives responses  
• Be mindful in production environments as debug can be verbose  
• Check the server's reachability, last response time and statistics

**Log Correlation**
• Check logs on both Cisco devices and AAA servers (Cisco ISE, ACS, open-source FreeRADIUS) to correlate attempts  
• Compare timestamps and transaction IDs between client and server logs

---

## Commands
*[To be completed during lab sessions]*
