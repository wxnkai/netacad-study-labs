# Lab: Configuring SOHO Wired & Wireless Connectivity

## Overview & Scenario
This lab focuses on the implementation of a Small Office/Home Office (SOHO) network. The scenario involves establishing a multi-service connection (Internet and Video) via a cable provider and configuring a centralized wireless router to manage local traffic and security for multiple end devices.

## Key Objectives
1. **Physical Layer Connectivity:** Correctly interfacing coaxial and copper media using splitters and modems.
2. **Router Management:** Accessing and securing a SOHO router via a Web Graphical User Interface (GUI).
3. **Network Services:** Implementing DHCP for automated IPv4 addressing with defined scope limits.
4. **WLAN Security:** Configuring SSID broadcasting and WPA2-Personal (AES) encryption.

## Technical Implementation Summary
### Physical Connections
- **Media Type:** Utilized Coaxial cables for the provider drop to the modem and TV.
- **Local Infrastructure:** Established a star topology using Copper Straight-Through cables from the router to wired hosts.

### Router Configuration Highlights
- **Credential Hardening:** Transitioned from factory default admin/admin to a complex administrative password to prevent unauthorized GUI access.
- **DHCP Optimization:** Restricted the Maximum Number of Users to 10. This minimizes the "attack surface" by limiting the number of available dynamic IP addresses.
- **Wireless Security:** Applied Wi-Fi Protected Access 2 (WPA2) Personal to offer a stronger encryption than WEP/WPA on a 2.4GHz band.

## Troubleshooting & Insights
- **Convergence Speed:** During the DHCP handshake, the Packet Tracer "Fast Forward Time" tool was essential to bypass STP (Spanning Tree Protocol) listening/learning states on the router's integrated switch ports.
- **Verification:** Connectivity was validated by successful HTTP GET requests from both wired and wireless clients to the skillsforall.srv domain, confirming DNS and routing were functional.
- **Security Best Practice:** A key takeaway was the immediate change of default credentials. In a real-world environment, keeping default logins is a primary vulnerability exploited by threat actors.