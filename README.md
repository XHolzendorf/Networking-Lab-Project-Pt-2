Network Engineer Home Lab – Part 2
Advanced Enterprise Network Simulation (Cisco Packet Tracer)
Overview

This lab expands upon Part 1 by introducing enterprise-level networking concepts, including dynamic routing (OSPF), redundancy, port security, SSH remote management, and structured troubleshooting.

The objective is to simulate a small enterprise network with multiple routers and switches while implementing security and scalability best practices.

Topology Overview
Devices Used

2x Cisco 2911 Routers

2x Cisco 2960 Switches

4x PCs

1x Cloud (Internet Simulation)

Network Segments

VLAN 10 – Sales (192.168.10.0/24)

VLAN 20 – IT (192.168.20.0/24)

VLAN 30 – HR (192.168.30.0/24)

Inter-router link: 10.0.0.0/30

Configuration Steps
1️⃣ VLAN Configuration (Switch)
enable
configure terminal

vlan 10
 name SALES
vlan 20
 name IT
vlan 30
 name HR

interface fa0/1
 switchport mode access
 switchport access vlan 10

interface fa0/2
 switchport mode access
 switchport access vlan 20

interface fa0/3
 switchport mode access
 switchport access vlan 30

2️⃣ Router-on-a-Stick Configuration (R1)
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

interface g0/0
 no shutdown

3️⃣ Inter-Router Link Configuration
R1:
interface g0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown

R2:
interface g0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown

4️⃣ OSPF Dynamic Routing
R1:
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0

R2:
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0

Verification:
show ip route
show ip ospf neighbor

5️⃣ Port Security (Switch)
interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky


Verification:

show port-security interface fa0/1

6️⃣ SSH Remote Access Configuration (Router)
hostname R1
ip domain-name lab.local
crypto key generate rsa
username admin secret Cisco123

line vty 0 4
 transport input ssh
 login local


Verification:

show ip ssh

7️⃣ Redundancy Simulation

Test failover by shutting down an interface:

interface g0/1
 shutdown


Observe OSPF recalculation:

show ip route

8️⃣ Monitoring & Troubleshooting Commands
show ip interface brief
show ip route
show running-config
show vlan brief
show interfaces trunk
show access-lists
show logging
