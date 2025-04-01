# AAA - Authentication, Authorization, Accounting - RFC 2865/2866 (RADIUS)

### Overview:
> * AAA provides a structured way to manage who can access the network (Authentication), what they can do (Authorization) and track their actions (Accounting).
> * RADIUS and TACACS+ are the two main protocols, each with different use cases.
> * Proper AAA is critical for security, compliance and consistent user/device management across the enterprise
---
### Terminology/Definitions:
> * AAA – is a framework for controlling and tracking user access to network resources. It is divided into three core functions
> * Authentication – Verifies users' identities before granting access. Examples include username/password, tokens, certifications????
> * Authorization – Defines what a user can do after they are authenticated. Examples include limiting commands (privileged levels), specifying allowed networks and resources.
> * Accounting – Logs the users actions, resource usage and/or sessions statistics. Often used for auditing, billing and compliance.
> * RADIUS – a widely used AAA protocol that provides centralized AAA for network access. RADIUS is typically used for 802.1X authentication, VPN access and wireless networks
> * TACACS+ – another AAA protocol developed by Cisco. TACACS+ separates the functions of AAA into distinct processes, providing more granular control compared to RADIUS. It is typically used for device management and administrative access
> * 802.1X – an IEEE standard for port-based network access control that uses AAA (typically RADIUS) to authenticate devices before granting them network access
> * EAP – a framework used by 802.1X and many AAA systems that supports multiple authentication methods (EAP-TLS, EAP-PEAP)
---
### Protocol/Port(s):
> * RADIUS – UDP 1812, UDP 1813
>   * Usually for 802.1X or remote user VPN in Cisco environments. Encrypts only the password field, not the entire payload. Combines authentication and authorization in one step. Typically used for network access (802.1X, VPN, Wi-Fi). Also has vendor specific attributes for advanced features. 
  
> * TACACS+ – UDP 49
>   * Usually used for privileged router/switch admin access in Cisco environments. Encrypts the entire payload making it more secure for device administrators. Separates authentication, authorization and accounting more distinctly. Typically used for device administration (router/switch CLI access).
---




