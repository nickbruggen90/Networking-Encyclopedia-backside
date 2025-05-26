### Terminology/Definitions:
> * Frequency Bands –
>   * 2.4 GHz – longer range and better penetration but more prone to interference and limited non-overlapping channels
>   * 5 GHz - more channels, less interference, higher speeds but shorter range
>   * 6 GHz – newly available in WiFi 6E; even more channels and reduced interference
> * Modulation Techniques –
>   * DSSS (Direct-Sequence Spread Spectrum) – used in 802.11b
>   * OFDM (Orthogonal Frequency Division Multiplexing) – used in 802.11a/g/n/ac/ax for higher throughput and better resilience to multipath
> * MU-MIMO – enables simultaneous data streams to multiple clients, improving efficiency
> * Channel Bonding – combining multiple channels to increase bandwidth
---
### Components:
> * APs – devices that broadcast and receive radio signals, providing wireless connectivity
> * WLCs – centralized controllers that manage multiple APs, handling configuration, firmware updates and policy enforcement
> * Wireless Clients – devices such as laptops, smartphones, tablets and IoT gadgets that connect to the wireless network
> * Core/Distribution Network – the wired infrastructure that interconnects APs and controllers with the rest of the network
---
### Design Considerations:
> * Site Surveys – conduct physical and RF site surveys to determine optimal AP placement, ensuring full coverage and sufficient capacity
> * AP Density – determine the number of APs based on user density and expected traffic load. Overlapping coverage should be optimized to prevent co-channel interference
> * Cell Size and Overlap – balance between coverage area and interference. Smalls cells in high-density areas versus larger cells in open spaces
---
### Network Topology:
> * Controller-Based vs Autonomous APs –
>   * Controller-Based offers centralized management, easier firmware updates and consistent policy enforcement
>   * Autonomous APs are suitable for smaller deployments; however, they require individual configuration
> * Overlay/Underlay Integration – ensure that the wired underlay (typically using routing protocols like OSPF and EIGRP) supports the required bandwidth and low latency for wireless traffic
> * Redundancy and Scalability –
>   * Implement redundant controllers and APs to avoid single points of failure
>   * Design the wireless architecture with scalability in mind, planning for future growth and increased device density
> * QoS – plan for traffic prioritization to support real-time applications like VoIP and video, often using DSCP/802.1p mappings
> * Security and Segmentation – incorporate robust wireless security (WPA2/WPA3, 802.1X) and network segmentation (VLANs, guest networks) into the design
> * Interference Management – consider channel planning (using non-overlapping channels in 2.4 GHz, dynamic assignment in 5 GHz) and power level adjustments to reduce co-channel and adjacent-channel interference
---
### Wireless Fundamentals and Standards –
> * 802.11a – operates in the 5 GHz band OFDM modulation; up to 54 Mbps
> * 802.11b – operates in the 2.4 GHz band, DSSS modulation; up to 11 Mbps
> * 802.11g – operates in the 2.4 GHz band; uses OFDM; up to 54 Mbps; backward compatible with 802.11b
> * 802.11n (WiFi 4) – supports both 2.4 GHz and 5GHz bands; uses MIMO; up to 600 Mbps
> * 802.11ac (WiFi 5) – operates exclusively in 5 GHz; supports MU-MIMO; wider channels (80/160 MHz); Gbps speeds
> * 802.1ax (WiFi 6/6E) – operates in 2.4 GHz, 5 GHz and 6 GHz (WiFi 6E); introduces OFDMA and improved MU-MIMO for better performance in dense environments
---
## Wireless Network Management and Monitoring –
### Management Platforms:
>  * WLCs – centralized devices that manage and monitor APs, enforce policies and provide analytics
>  * NMS – tools like Cisco Prime Infrastructure, Cisco DNA Center or third-party solutions to monitor, configure and troubleshoot wireless networks
>  * SNMP and Telemetry – use SNMP, NetFlow and telemetry (including streaming telemetry) to gather operational data from APs and controllers
---
### Monitoring Tools and Techniques:
> * RF Spectrum Analysis – tools to detect interference, signal strength and channel utilization
> * Wireless Surveys – conduct periodic surveys to validate coverage and performance
> * Dashboard Analytics – centralized dashboards provide real-time visibility into AP status, client connections, throughput, error rates and security events
> * Event and Log Monitoring – use Syslog, SNMP traps and integrated analytics to track events like rogue AP detection, authentication failures or interference
---
## Wireless Propagation and RF Fundamentals –
### Key RF Concepts:
> * Frequency and Wavelength – frequency (in GHz) and wavelength (in meters) are inversely related. Lower frequencies (2.4 GHz) have longer wavelength, offering better penetration and coverage, whereas higher frequency (5 GHz, 6 GHz) provide more bandwidth but shorter range
> * Path Loss – signal attenuation as it travels through space, influenced by distance, obstacles and environmental factors
> * Fading and Multipath – reflections and diffractions cause multiple copies of the signal to arrive at the receiver at different times, leading to fading and potential signal degradation
> * Interference – can be caused by overlapping channels, non-WiFi devices (microwaves, cordless phones) and external RF sources. Proper channel planning and power management are crucial to minimize interference
> * Antenna Characteristics – gain, radiation patterns and polarization of antennas affect coverage and performance. Directional antennas can focus energy for longer-range coverage, while omnidirectional antennas provide coverage in all directions
---
### Channel Planning:
> * 2.4 GHz Band – typically channels 1, 6, 11 are used as they are non-overlapping
> * 5 GHz Band – provides many channels with less interference; dynamic channel selection can optimize performance
---
### RF Tools and Techniques:
> * Site Surveys – use tools like Ekahau and AirMagnet to map signal strength, identify dead zones and plan AP placement
> * Spectrum Analyzers – measures RF interference and channel utilization
> * Propagation Modeling – use software tools to predict coverage areas based on building materials, obstacles and AP power settings
---
## Wireless Security and Automation –
### Encryption Protocols:
> * WEP – obsolete and insecure
> * WPA/WPA2 – uses TKIP (for WPA) and AES (for WPA2) for encryption; WPA2 is the current standard for enterprise networks
> * WPA3 – the latest standard with enhanced security features, including SAE
---
### Authentication Methods:
> * 802.1X – provides port-based network access control using EAP and a RADIUS server
> * PSK – used in smaller networks, but less secure than 802.1X
> * Segmentation – use VLANs, guest networks and wireless profiles to isolate different type of traffic (employee, guest, IoT, etc)
---
### Additional Protections:
> * Rogue AP Detection – tools to detect unauthorized APs
> * WIPS – systems that actively block rogue devices and attacks
> * Encryption at the Radio Level – ensure robust encryption is configured on all APs
---
### Wireless Automation:
> * Use tools such as Cisco DNA Center, Cisco Prime Infrastructure or scripting (Python, Ansible) to automate AP configuration and policy updates
> * Integrate wireless telemetry to monitor real-time performance, track client behavior and adjust network parameters automatically
> * Leverage automation platforms to apply consistent security and QoS policies across all wireless devices
> * New APs can automatically download configuration from the controller, simplifying deployment and ensuring consistency
---
### Multicast:
> * Multicast traffic (IPTV, etc) is often transmitted at the basic rate to ensure all clients can receive the signal. Controllers may convert multicast to unicast for reliability
> * Ensure multicast streams are assigned appropriate VLANs and DSCP/CoS values so they can receive proper prioritization across the network
---
### Key Benefits:
> * Scalable Coverage – wireless architecture design enables seamless coverage across large campuses and multi-story buildings
> * Flexible Deployment – controller-based and autonomous AP options allow for tailored solutions based on organizational size and requirements
> * Real-Time Visibility – integrated management platforms provide continuous insights into performance and client behavior
> * Enhanced User Experience – QoS and RF optimization improve performance for latency-sensitive applications like VoIP and video conferencing
> * Improved Security – through 802.1X, rogue AP detection and wireless segmentation, enterprises can protect sensitive data and prevent unauthorized access
---
### Best Practices/Security Considerations:
> * Always configure WPA2/WPA3 Enterprise over PSK, especially in enterprise deployments
> * Use 802.1X for strong, identity-based access control
> * Separate guest traffic from corporate traffic using VLANs and dedicated SSIDs
> * Regularly scan for unauthorized access points and monitor for any unusual activity
> * Keep firmware up-to-date on APs and controllers to protect against vulnerabilities
> * Use secure protocols (HTTPS, SSH) for management access to APs and controllers, and implement RBAC
---
### Troubleshooting:
> * Use site survey tools or built-in reporting (signal strength, noise, interference) to assess coverage
> * Verify that 802.1X and encryption settings are active on APs. Check logs for authentication failures, rogue AP alerts and intrusion detection events
> * Coverage Gaps?
>   * Adjust AP placement or transmit power
>   * Re-run a site surrey and modify channel planning if needed
> * Interference?
>   * Change channels manually or enable dynamic channel assignment on controllers
> * Authentication Failures?
>   * Check 802.1X configurations on both APs and the RADIUS server
>   * Verify certificates (for WPA2/WPA3 Enterprise) and shared keys
> * Rogue APs?
>   *Use integrated rogue detection and WIPS functions to identify and isolate unauthorized APs
---




