# REST APIs - RFC 8040 (RESTCONF)

### Overview:
> * REST APIs are the heart of modern network automation and SDN. They allow for centralized, consistent management of large-scale networks
> * REST API is a widely adopted architectural style that allows different systems to communicate over HTTP/HTTPS. In networking, REST APIs enable programmatic control of devices, allowing for configuration, monitoring and management tasks to be automated.
> * Cisco devices and management platforms (Cisco DNA Center, NX-OS) increasingly offer REST API interfaces to simplify network automation and integration with DevOps workflows
---
### Timers:
> * REST API transactions are generally stateless and rely on HTTP timeouts. Most REST clients have configurable timeout values (commonly 30-60 sec) for waiting on a response from the device
---
###Multicast:
>* REST API communications are inherently unicast (HTTP/HTTPS). There are no multicast considerations specifically for REST APIs
---
### Protocol/Port(s):
---
### Priorities:
---
### Terminology/Definitions:
> * REST – an architectural style for designing networked applications that use standard HTTP methods to operate on resources identified by URIs
> * API – a set of definitions and protocols that allow software components to communicate. REST APIs expose resources and operations over HTTP
> * URI (Uniform Resource Identifier) – a string used to identify a resource
> * JSON (Javascript Object Notation) – a lightweight data-interchange format commonly used for REST API responses
> * XML – a markup language used for data representation, also supported by some REST APIs
> * RESTCONF – a protocol that provides a RESTful interface to access NETCONF data stores, defined in RFC 8040
> * NETCONF – used to install, manipulate and delete the configuration of network devices
---
### Error Messages:
> * 200 OK – request succeeded
> * 201 Created – resource created
> * 400 Bad Request – invalid syntax
> * 401 Unauthorized – authentication failure
> * 404 Not Found – resource does not exist
> * 500 Internal Server Error – server-side issue
---
### HTTP Methods:
> * GET – retrieve a resource
> * POST – create a resource
> * PUT – update an existing resource
> * DELETE – remove a resource
---
### Architecture:
> * Stateless Communication – every REST API request from a client to a server must contain all information needed to understand and process the request. The server does not maintain any sessions state between requests
> * Resource-Based – resources (device configurations, operational data, etc) are identified using URIs. Operations are performed on these resources using standard HTTP methods
> * Uniform Interface – REST APIs use a uniform and predefined set of operations (HTTP methods), making it easier for clients to interact with the server
> * Representation – resources are typically represented in a structured format (most commonly JSON, but XML is also used) when transmitted between the client and server
> * Scalability – the statelessness and caching capabilities of REST make it highly scalable, which is essential for managing large networks
---
### Operational Overview:
> * Resources Identification – network devices expose configuration and operational data as resources accessible via URIs. For example, a Cisco router might expose its interface configuration at a URI like /restconf/data/Cisco-IOS-XE-native:native/interface
> * HTTP Methods –
>   * GET requests are used to retrieve device configurations or operational data
>   * POST requests create new configuration objects
>   * PUT requests update existing configurations
>   * DELETE requests remove configuration entries
> * Authentication – REST APIs typically require authentication, often via basic authentication, token, or certificates. Secure communication is ensured by using HTTPS
> * Integration with Automation – tools like python (using libraries like requests), Ansible or Postman can interact with REST APIs to automate configuration tasks, retrieve monitoring data or perform network changes programmatically
> * RESTCONF – Cisco’s RESTCONF protocol, defined in RFC 8040, provides a RESTful interface to the NETCONF data models, allowing standardized access to network configurations and state information
---
### Key Benefits:
> * Automation and Efficiency – REST APIs simplify network management by enabling automated configuration, monitoring and troubleshooting
> * Interoperability – REST APIs use standard HTTP methods, making them accessible from any platform or programming language
> * Scalability – they are inherently scalable due to their stateless design
> * Integration – easily integrate network devices with DevOps pipelines, ITSM systems, and centralized controllers (like Cisco DNA)
---
### Use Cases:
> * Configuration Management – automate the configuration of network devices, saving time and reducing manual errors
> * Monitoring and Analytics – retrieve operational data for real-time performance monitoring and historical analysis
> * Orchestration – integrate with orchestration tools (like Ansible) for multi-vendor and multi-device management
> * Rapid Troubleshooting – quickly fetch configuration and state information from device during troubleshooting, improving Mean Time to Resolution (MTTR)
---
### Best Practices/Security Considerations:
> * Always secure REST API communication with HTTPS to ensure data encryption in transit
> * Use robust authentication methods (basic auth over HTTPS, tokens or certificates). Avoid sending credentials in plaintext.
> * Limit REST API access using ACLs and RBAC. Only authorized users or systems should be allowed to interact with the API
> * Ensure that any data sent to the REST API is properly validated to prevent injection attacks
> * Monitor REST API usage and review logs for any unauthorized or suspicious activities
> * Always use HTTP with strong ciphers and proper certificate validation
> * Implement robust authentication (and possibly multi-factor) for REST API access. Use role-based access control to restrict permissions
> * Configure appropriate client-side and server-side timeouts to balance responsiveness and network reliability
> * Maintain clear documentation of the available endpoints, parameters and expected responses. Tools like Swagger and OpenAPI can help.
> * Use versioned APIs to maintain backward compatibility and facilitate upgrades without disrupting network operations
> * Implement proper error handling in your automation scripts to gracefully manage failed API calls
> * Continuously monitor API usage, log access attempts and review error logs to quickly detect and remediate any issues
> * Validate REST API calls in a controlled environment before deploying automation scripts to production devices
---
### Troubleshooting:
> * Verify that your credentials are correct and that HTTPS is properly configured
> * Ensure that the URI in your request exactly matches the resource path on the device (check for typos and correct data models)
> * Confirm that the device is reachable on the network and that firewalls are not blocking HTTP/HTTPS traffic
> * Make sure you specify the correct headers (application/yang-data+json or application/yang-data+xml) so that the device returns the expected data format
---
### Common Issues and Fixes:
> *
---
### Insights:
> * You can test REST API endpoints using tools like Postman to view responses and debug issues
> * Many network devices log REST API requests and errors, which can be reviewed via syslog or the devices management interface
> * REST API-driven automation enables rapid deployment and agile modifications to network configurations, which is critical in dynamic environments
> * Network engineers can integrate REST APIs with DevOps tools (like Ansible, Jenkins or Git) to enable continuous integration and continuous deployment (CI/CD) to network changes
> * Whole Cisco and other vendors may offer proprietary implementation, REST APIs are based on open standards (HTTP/HTTPS, JSON, XML) making them broadly accessible
> * As networks evolve to become more virtualized and software-driven, REST API skills will remain essential. Keeping up with RESTCONF and evolving API standards is crucial for modern network professionals

