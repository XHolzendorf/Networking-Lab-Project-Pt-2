🧪 Network Engineer Home Lab – Part 2 (Enterprise Expansion)
 Part 2 Goal

Upgrade your lab to include:

✅ Two routers (WAN connection)

✅ OSPF dynamic routing

✅ Redundant paths

✅ Layer 3 switching

✅ Port Security

✅ SSH remote management

✅ Syslog monitoring

✅ Basic WAN simulation

This is now mid-level network engineer territory.

🏗️ Step 1: New Topology Design
Add These Devices

2x Cisco 2911 Routers

1x Cisco 3560 Layer 3 Switch

1x Cisco 2960 Access Switch

3–4 PCs

1 Server (for Syslog)

🗺️ Logical Layout
           [Router1] -------- [Router2]
               |                    |
           (Core L3 Switch)     WAN Network
               |
          (Access Switch)
        |       |       |
      PC1     PC2     PC3

 Step 2: Configure the Core (Layer 3 Switch)

We are moving routing OFF the router and onto the Layer 3 switch.

Enable Routing
enable
configure terminal
ip routing

Create VLANs
vlan 10
 name SALES
vlan 20
 name IT
vlan 30
 name HR

Create SVI Interfaces (Gateway Interfaces)
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown


Now your switch is doing inter-VLAN routing.

 Step 3: Configure Trunk to Access Switch

On Core Switch:

interface g0/1
 switchport mode trunk


On Access Switch:

interface g0/1
 switchport mode trunk


Assign PCs to VLANs on the access switch.

Step 4: Connect Router1 to Core Switch

Router1 will provide WAN/internet access.

Assign IP Between Core and Router1

Core Switch:

interface g0/2
 no switchport
 ip address 10.0.0.2 255.255.255.252


Router1:

interface g0/0
 ip address 10.0.0.1 255.255.255.252
 no shutdown

 Step 5: Configure OSPF (Dynamic Routing)

We’ll run OSPF between:

Router1

Router2

Core Switch

On Core Switch
router ospf 1
 network 192.168.0.0 0.0.255.255 area 0
 network 10.0.0.0 0.0.0.3 area 0

On Router1
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
 network 172.16.0.0 0.0.0.3 area 0

On Router2
router ospf 1
 network 172.16.0.0 0.0.0.3 area 0


Verify:

show ip ospf neighbor
show ip route


You should see O routes in the routing table.

 Step 6: Configure SSH Remote Access

On Core Switch:

hostname CORE-SW
ip domain-name lab. local
crypto key generate rsa


Create User:

username admin secret Cisco123


Enable SSH:

line vty 0 4
 transport input ssh
 login local


Now you can SSH from a PC:

ssh -l admin 192.168.10.1


This is real-world admin access.

 Step 7: Configure Port Security

On Access Switch:

interface fa0/2
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky


Test by plugging another PC into the same port.

Check:

show port-security interface fa0/2

📡 Step 8: Configure Syslog Server

On Server:

Enable Syslog service

Assign IP (example: 192.168.20.50)

On Core Switch:

logging 192.168.20.50
logging trap informational


Now shut an interface and watch logs populate 👀

That’s real monitoring.

 Step 9: Simulate WAN Link Between Routers

Connect Router1 ↔ Router2:

Example IP scheme:

Router1: 172.16.0.1 /30
Router2: 172.16.0.2 /30


This simulates:

Branch office

Data center

ISP handoff

Commands to master:

show ip route
show ip ospf neighbor
show interfaces trunk
show vlan brief
show port-security
show logging
