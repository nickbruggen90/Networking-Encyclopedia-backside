# NETCONF/RESTCONF - RFC 6241 (NETCONF), RFC 8040 (RESTCONF)

### Overview:
> * NETCONF and RESTCONF are protocols designed for network configuration and state management, allowing automation tools and software to programmatically read or modify network device settings using structured data models like YANG
> * NETCONF is stateful, XML-based protocol designed for full configuration lifecycles
> * RESTCONF is a RESTful, HTTP-based protocol designed to expose YANG data via standard web operations
---
### Protocol/Port(s):
> * NETCONF - TCP: 830 (secured via SSH)
> * RESTCONF - TCP: 443 (secured via HTTPS, optionally with token based auth)
---
### Terminology/Definitions:
> * YANG – data modeling language used to define schema for configuration/state data
> * Datastore – logical container for configuration/state (running, startup, candidate)
> * RPC (NETCONF) – remote procedure call, XML encoded command used for configuration and queries
> * REST URI (RESTCONF) – REST-style paths to YANG-defined resources
> * Capabilities – advertised features and module support presented at sessions establishment (NETCONF)
> * Content-Type Headers (RESTCONF) – define expected response format
> * Base Path (RESTCONF) – All data queries start at /restconf/data
> * Operations Path (RESTCONF) – invoking RPC-style operations via /restconf/operations
---
### Transport/Encoding:
> * NETCONF -
>   * Transport → SSH (TCP 830)
>   * Encoding → XML (only)
>   * Protocol → stateful, RPC based
>   * Data Model → YANG
---
> * RESTCONF -
>   * Transport → HTTP/HTTPS (default 443)
>   * Encoding → JSON and XML
>   * Protocol → stateless, REST-style (GET/POST,etc)
>   * Data Model → YANG
---
### Common NETCONF Workflow:
> * 1. Client opens SSH connection to port 830
> * 2. Server sends capabilities
> * 3. Client sends <rpc> with desired operation (<get-config>, <edit-config>)
> * 4. Server responds with <rpc-reply>
> * 5. Client closes session with <close-session>
---
### Common RESTCONF Workflow:
> * 1. Client sends HTTP GET to retrieve config or state
> * 2. Client sends POST/PUT/PATCH to modify config
> * 3. Data is accessed under structured URLs
---
### Supported Operations:
> * NETCONF - 
>   * <get> – read state (operational data)
>   * <get-config> – read from config datastore
>   * <edit-config> – modify config data
>   * <delete-config> – remove data
>   * <copy-config> – copy between datastores
>   * <commit> – commit candidate config
>   * <lock>/<unlock> – locking access to config
---
> * RESTCONF -
>   * GET – retrieve data
>   * POST – create resource
>   * PUT – replace resource
>   * PATCH – partial modification
>   * DELETE – delete resource
---
### Best Practices/Security Consideration:
> * Use YANG Explorer or Postman for testing and learning RESTCONF API structure
> * For NETCONF, use ncclient (Python library) or Cisco’s native tools (CSR/IOS-XE)
> * Always query capabilities to ensure supports features
> * Validate config against YANG schema before deploying in production
> * Prefer HTTPS and token-based auth for RESTCONF security
> * Use GET before PUT or PATCH to avoid unintended overwrites
> * Limit user access with RBAC to protect sensitive models
> * Use logging or telemetry to track changes made via NETCONF/RESTCONF
> * Treat both protocols as configuration interfaces, not telemetry (use gNMI/streaming for that)
> * NETCONF -
>   * Always use SSH transport (TCP 830)
>   * Username/password or SSH keys are required
>   * SSH ACLs or RBAC may control device access
>   * Validate all XML input; avoid injection attacks
>   * Use <lock>/<unlock> to avoid configuration race conditions
> * RESTCONF -
>   * Use HTTPS (TLS 1.2+); HTTP is discouraged
>   * Authentication via HTTP basic, OAuth2, or session tokens
>   * YANG data model exposure should be RBAC-scoped
>   * Use input validation and strong tokens to prevent API misuse
>   * Avoid using wildcare DELETE or PATH without bounds
---
### Common Issues and Fixes:
> * Unsupported YANG Models?
>   * Check device capability list or software version
> * RESTCONF Returns 404?
>   * URL path or module name might be incorrect
> * Permission Errors?
>   * RESTCONF may require elevated privileges or API tokens
> * NETCONF Session Fails?
>   * Cause?: port 830 not open
>   * Fix: enable SSH subsystem and NETCONF on device
> * RPC Returns <rpc-error>?
>   * Cause?: invalid XML or unsupported operation
>   * Fix: check schema and device capabilities
> * RESTCONF Returns 404?
>   * Cause?: invalid URI or module not supported
>   * Fix: double-check the exact YANG model and path
> * Auth Fails?
>   * Cause?: incorrect credentials or expired token
>   * Fix: re-authenticate and check RBAC rules
> * Config Not Applying?
>   * Cause?: using wrong datastore or uncommitted candidate
>   * Fix: use <commit> or apply to running config
---
### Use Cases:
> * Configuration Management (Ansible, Python, Terraform)
> * State Monitoring (interface status, BGP neighbors, etc)
> * Zero-Touch Provisioning (combined with DHCP/PnP and automation hooks)
> * Closed-Loop Automation (monitor-change-trigger-update workflows)
> * Third-Party API Integration (SDN, DNAC, NSO, NSX)
