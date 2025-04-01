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





