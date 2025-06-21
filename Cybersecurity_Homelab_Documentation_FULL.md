
Executive Summary

This document outlines the design, setup, and security hardening of a custom-built cybersecurity homelab. Built using affordable, second-hand enterprise-grade hardware, this lab simulates both attacker and defender scenarios with a focus on VLAN segmentation, administrative access control, and layered network defense. The entire process—from physical wiring to firmware troubleshooting and firewall configuration—was independently completed as part of a hands-on cybersecurity training project.

---

1. Hardware and Equipment

- Router: TP-Link ER605 V2 Omada Gigabit Multi-WAN VPN Router
- Switch: TP-Link TL-SG108E 8-Port Gigabit Easy Smart Managed Switch (Managed Mode)
- Attacker Machine: Raspberry Pi 5 (16GB RAM) running Kali Linux
- Admin Machine: Windows 11 PC with Intel i7-12700, 16GB RAM, NVIDIA RTX 3060
- Additional Software: VMware Workstation, Nmap, browser-based interfaces, and command-line utilities

Ethernet cabling was prioritized to improve reliability and control over traffic. All components were manually configured and tested without relying on automation.

---

2. Network Architecture

The network was segmented using VLANs to isolate environments:

- VLAN 10 – Admin Network  
  - Subnet: 192.xxx.xx.x/xx  
  - PC connected directly to Router Port 3

- VLAN 20 – Attacker Network  
  - Subnet: 192.xxx.xx.x/xx  
  - Kali Linux (Raspberry Pi 5) connected via Switch Port 2

- Interconnects  
  - The switch was plugged into Router Port 2  
  - WAN port on the router connected to home internet for external access and updates

- Router Management Interface:  
  - IP: 192.xxx.xx.x (VLAN 10)  
  - Web UI and SSH restricted to the Admin VLAN only

A simplified network diagram visually depicts this topology and VLAN separation.

---

3. Firmware & Setup Troubleshooting

The router presented a common issue: the hardware label indicated v2.6, while the interface showed v2.0 and firmware version 2.1.2. Standard upgrades failed with an error message.

Solution:
1. First upgraded to firmware v2.2.3 (2023-12-01)
2. Then upgraded to firmware v2.3.0 (2024-04-28)

This two-step sequence unlocked full access to VLAN, ACL, and modern interface features.

---

4. Security Hardening & ACL Configuration

Custom Access Control List (ACL) rules were created for layered defense:

- Block Router Access from VLAN 20
  - Denied HTTP/HTTPS access to router from Kali (simulating attacker isolation)

- Block VLAN 20 to Router Ping
  - Router became non-discoverable from attacker subnet

- Allow Admin PC Only
  - Source IP: 192.xxx.xx.xx explicitly allowed for web UI and SSH
  - Ensures only authorized local admin access

- Block Inter-VLAN Communication
  - VLAN 20 blocked from reaching VLAN 10

All firewall rules were placed in the correct order and tested via ping and Nmap. Results showed all ports as filtered from VLAN 20, confirming stealth mode.

---

5. Key Achievements

- Built a segmented enterprise-style lab from scratch
- Resolved critical firmware compatibility issues
- Implemented strong ACL-based perimeter defense
- Validated segmentation using professional tools (Nmap, ping)
- Achieved Zero Trust-style policy enforcement

---

6. Next Steps

With this secure base network in place, the following expansions are planned:

- Add Metasploitable 2 and Windows 10 VMs to VLAN 20 for offensive testing
- Use Kali to perform enumeration, scanning, and exploitation
- Install Suricata or Zeek on Raspberry Pi for detection
- Build local logging pipelines and alerting mechanisms

---

7. Conclusion

This project showcases the technical and analytical abilities required to build and secure a functional cybersecurity lab. All planning, configuration, and documentation was completed independently. The environment provides an ideal platform to explore both offensive and defensive concepts and is fully ready for advanced red/blue team simulations.

---

Appendix: Network Diagram

                +----------------------+
                |     Home Router      |
                |   (Internet Access)  |
                +----------+-----------+
                           |
                           | WAN
                     +-----+-----+
                     |  ER605     | (TP-Link VPN Router)
                     |            |
         +-----------+----+------+----------+
         | Port 2         | Port 3         |
         | (To Switch)    | (PC - VLAN 10) |
         |                |                |
     +---+---+      +-----+-----+          
     |Switch |------| Raspberry Pi (Kali - VLAN 20)
     +-------+      +---------------------------+

VLAN 10 – Admin network: 192.xxx.xx.x/xx  
VLAN 20 – Attacker/Lab network: 192.xxx.xx.x/xx
