# Automation - Network Automation
*RFC 6241 (NETCONF), RFC 8040 (RESTCONF)*

## Overview
• Automation in networking involves the use of software and scripts to manage, configure, monitor and troubleshoot network devices with minimal manual intervention  
• It reduces human error, speeds up repetitive tasks and ensures consistent configuration across devices

---

## Terminology/Definitions

| Term | Definition |
|------|------------|
| **Automation** | The process of using scripts, software or controllers to perform network configuration, monitoring and management tasks automatically |
| **Embedded Event Manager (EEM)** | A Cisco IOS feature that allows you to create event-triggered scripts to automate responses (logging, configuration changes, notifications) based on specific network events |
| **NETCONF** | A protocol defined in RFC 6241 used for installing, manipulating and deleting the configuration of network devices. It uses XML for data encoding |
| **RESTCONF** | A RESTful protocol defined in RFC 8040 that provides a simplified interface to access NETCONF data stores via HTTP(s), typically using JSON or XML |
| **API** | A set of protocols and tools that allows software applications to communicate with each other, essential for automation and integration |
| **DevOps** | A cultural and technical movement emphasizing collaboration, automation and continuous improvement, which has influenced network automation strategies |
| **YAML/JSON** | Data serialization languages often used for configuration files and API data exchanges in automation |

---

## Architecture

### Automation Components

| Component | Description | Implementation |
|-----------|-------------|----------------|
| **Centralized Control** | Modern network automation leverages centralized controllers and APIs | Cisco DNA Center, SDN controllers |
| **Event Driven Automation** | Tools monitor network events and trigger predefined actions | EEM scripts, automated responses |
| **API-Driven Management** | Programmatic access to device configuration and operational data | NETCONF, RESTCONF protocols |
| **IT Systems Integration** | Bridges network operations with broader IT systems | ITSM systems, DevOps pipelines |

### Architecture Flow
```
Centralized Controller/Scripts
        ↓
    API Calls (NETCONF/RESTCONF)
        ↓
Network Devices ← Event Triggers → EEM Scripts
        ↓
Automated Actions/Responses
```

---

## Operational Overview

### Implementation Methods

**EEM (Embedded Event Manager)**
• EEM scripts are triggered by network events and can execute commands locally on a device  
• They run in the device's IOS environment  
• Used for tasks such as automatic interface shutdown, configuration adjustments or alerts

**NETCONF/RESTCONF**
• These protocols allow for the retrieval and modification of device configuration using standardized, structured data  
• XML for NETCONF and JSON/XML for RESTCONF  
• Typically used in conjunction with automation platforms and scripts (often written in Python)

**APIs and Scripting**
• Automation scripts interact with network devices via APIs  
• Example: Python script using RESTCONF calls to back up device configurations or enforce policy changes across multiple devices

### Protocol Comparison

| Protocol | Data Format | Transport | Use Case |
|----------|-------------|-----------|----------|
| **NETCONF** | XML | SSH/TLS | Comprehensive device configuration management |
| **RESTCONF** | JSON/XML | HTTP/HTTPS | RESTful interface for simpler integration |
| **EEM** | Cisco IOS | Local device | Event-driven local automation |

---

## Key Benefits

| Benefit | Description | Impact |
|---------|-------------|---------|
| **Efficiency and Consistency** | Reduces manual intervention, ensuring consistent configuration and rapid deployment | Faster operations, fewer errors |
| **Proactive Management** | Automated monitoring can trigger immediate remedial actions to mitigate issues | Improved uptime, faster response |
| **Rapid Troubleshooting** | Automated data collection streamlines troubleshooting and configuration backups | Reduced MTTR |
| **Scalability** | Easily manage large networks by automating routine tasks and integrating with centralized controllers | Enterprise-scale management |

---

## Use Cases

### Common Automation Scenarios

**Automated Configuration Backups**
• Use RESTCONF/NETCONF scripts to regularly back up device configurations  
• Schedule automated exports to version control systems  
• Compare configurations for drift detection

**Real-Time Event Handling**
• Implement EEM scripts to automatically respond to critical events  
• Examples: interface failures, high CPU usage, security events  
• Trigger notifications, remediation, or escalation procedures

**Policy Compliance and Remediation**
• Automate policy enforcement across devices  
• Detect and remediate configuration drifts  
• Ensure consistent security postures

**Integration with DevOps**
• Leverage automation for CI/CD pipelines in network changes and updates  
• Infrastructure as Code (IaC) implementations  
• Automated testing and validation

---

## Best Practices/Security Considerations

### Security Implementation
• **Use HTTPS for RESTCONF** and secure channels for NETCONF  
• **Employ strong authentication methods** and regularly update credentials  
• **Ensure EEM scripts protection** with proper access controls on devices  
• **Limit automation script access** via RBAC on network management systems

### Development and Deployment
• **Begin with simple automation scripts** (basic EEM applets) before moving to complex tasks  
• **Validate all scripts** in a controlled environment prior to production deployment  
• **Use version control** (e.g., Git) to track changes to automation scripts and configuration files  
• **Document all automation procedures** including purposes, configuration and expected outcomes

### Operational Security
• **Regularly audit automation activities** and maintain logs to track changes and detect anomalies  
• **Secure your automation tools** and enforce RBAC  
• **Regularly audit automation activities** for compliance and security  
• **Utilize Cisco DevNet** and official documentation for latest best practices and sample code

### Change Management
• **Implement proper testing procedures** for all automation scripts  
• **Maintain rollback procedures** for automated changes  
• **Monitor automation execution** and maintain detailed logging  
• **Regular review and updates** of automation scripts as network needs evolve

---

## Insights

### Transformation Impact
• **Automation is transforming network management** by reducing manual workloads and enabling rapid, consistent changes  
• **Combining automation with centralized controllers** (Cisco DNA Center) and modern APIs paves the way for more agile, responsive networks

### Continuous Improvement
• **Automation scripts should evolve** as network needs change; regularly review and update scripts for optimal performance  
• **Embracing automation requires cultural shift** within IT teams towards collaboration, continuous learning and proactive management

### Strategic Considerations
• **Start small and scale gradually** to build expertise and confidence  
• **Focus on high-impact, repetitive tasks** for initial automation efforts  
• **Invest in team training** for automation tools and methodologies
