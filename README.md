Network Engineer Home Lab – Part 2 (Advanced Enterprise Features)

In this lab, we expand the network to include:

Multiple switches

OSPF dynamic routing

Redundancy concepts

Port security

SSH remote management

Basic network monitoring



Step 1: Expand the Topology
Add:

1 additional Router

1 additional Switch

2 more PCs

 Step 2: Configure OSPF (Dynamic Routing)

Instead of static routes, we use OSPF.

Router 1:
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0

Router 2:
router ospf 1
 network 192.168.30.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0

 Step 3: Redundancy Concept (Basic Introduction)

Explain:

Why having 2 routers prevents downtime.

What happens if one router fails?

You can simulate:

Shut down one interface:

interface g0/0
shutdown

 Step 4: Port Security on the Switch

Protect switch ports from unauthorized devices.

interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky

  Step 5: Configure SSH for Secure Remote Access

Instead of Telnet, use SSH.

hostname R1
ip domain-name lab.local
crypto key generate rsa
username admin secret Cisco123
line vty 0 4
 transport input ssh
 login local

📊 Step 6: Basic Monitoring Commands

Show how to monitor:

show ip interface brief
show ip route
show running-config
show logging
show processes cpu

“These commands are daily-use tools in real environments.”
