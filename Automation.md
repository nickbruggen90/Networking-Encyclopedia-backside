# Automation - RFC 6241 (NETCONF), RFC 8040 (RESTCONF)

### Overview:
> * Automation in networking involves the use of software and scripts to manage, configure, monitor and troubleshoot network devices with minimal manual intervention. It reduces human error, speeds up repetitive tasks and ensures consistent configuration across devices.
---
### Terminology/Definitions:
> * Automation – the process of using scripts, software or controllers to perform network configuration, monitoring and management tasks automatically
> * Embedded Event Manager (EEM) – a Cisco IOS feature that allows you to create event-triggered scripts to automate responses (logging, configuration changes, notifications) based on specific network events
> * NETCONF – a protocol defined in RFC 6241 used for installing, manipulating and deleting the configuration of network devices. It uses XML for data encoding
> * RESTCONF – a RESTful protocol defined in RFC 8040 that provides a simplified interface to access NETCONF data stores via HTTP(s), typically using JSON or XML
> * API – a set of protocols and tools that allows software applications to communicate with each other, essential for automation and integration
> * DevOps – a cultural and technical movement emphasizing collaboration, automation and continuous improvement, which has influenced network automation strategies
> * YAML/JSON – data serialization languages often used for configuration files and API data exchanges in automation
---
### Architecture:
> * Centralized Control – modern network automation leverages centralized controllers (Cisco DNA Center) and APIs to distribute configuration changes and policies across multiple devices
> * Event Driven Automation – tools like EEM monitor network events and trigger predefined actions (logging, configuration changes, notification, etc) automatically
> * API-Driven Management – protocols such as NETCONF and RESTCONF enable programmatic access to device configuration and operational data, allowing automation scripts to retrieve, modify and update configurations dynamically
> * Integration with IT Systems – automation bridges network operations with IT service management (ITSM) systems and DevOps pipelines, enabling continuous integration and continuous deployment (CI/CD) of network configurations
---
### Operational Overview:
> * EEM – EEM scripts are triggered by network events and can execute commands locally on a device. They run in the device’s IOS environment and can be used for tasks such as automatic interface shutdown, configuration adjustments or alerts
> * NETCONF/RESTCONF – these protocols allow for the retrieval and modification of device configuration using standardized, structured data (XML for NETCONF and JSON/XML for RESTCONF). They are typically used in conjunction with automation platforms and scripts (often written in Python)
> * APIs and Scripting – automation scripts interact with network devices via APIs. For example, a Python script may use RESTCONF calls to back up device configurations or enforce policy changes across multiple devices
---
### Key Benefits:
> * Efficiency and Consistency – automation reduces manual intervention, ensuring consistent configuration and rapid deployment
> * Proactive Management – automated monitoring (via EEM) can trigger immediate remedial actions to mitigate issues
> * Rapid Troubleshooting – automated data collection through NETCONF/RESTCONF can streamline troubleshooting and configuration backups
> * Scalability – easily manage large networks by automating routine tasks and integrating with centralized controllers
---
### Use Cases:
> * Automated Configuration Backups – use RESTCONF/NETCONF scripts to regularly back up device configurations
> * Real-Time Event Handling – implement EEM scripts to automatically respond to critical events (interface failures, high CPU usage)
> * Policy Compliance and Remediation – automate policy enforcement across devices and remediate configuration drifts
> * Integration with DevOps – leverage automation for CI/CD pipelines in network changes and updates
---
### Best Practices/Security Considerations:
> * Use HTTP for RESTCONF and secure channels for NETCONF. Employ strong authentication methods and regularly update credentials
> * Ensure that EEM scripts do not expose sensitive information and are protected by proper access controls on the devices
> * Limit who can create, modify and execute automation scripts via RBAC on your network management systems
> * Regularly audit automation activities and maintain logs to track changes and detect anomalies
> * Begin with simple automation scripts (basic EEM applets) before moving to more complex tasks
> * Validate all automation scripts in a controlled environment prior to production deployment
> * Use version control (e.g., Git) to track changes to your automation scripts and configuration files
> * Document all automation procedures, including purposes, configuration and expected outcome
> * Secure your automation tools, enforce RBAC and regularly audit automation activities
> * Utilize Cisco DevNet and official Cisco documentation for the latest best practices and sample code
---
### Insights:
> * Automation is transforming network management by reducing manual workloads and enabling rapid, consistent changes
> * Combining automation with centralized controllers (Cisco DNA Center) and modern APIs paves the way for more agile, responsive networks
> * Automation scripts should evolve as network needs change, regularly review and update scripts for optimal performance
> * Embracing automation also requires a cultural shift within IT teams towards collaboration, continuous learning and proactive management
---





