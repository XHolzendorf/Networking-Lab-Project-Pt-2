Network Engineer Home Lab – Part 2 (Advanced Enterprise Features)

Goal of Part 2

In this lab, we expand the network to include:
•	Multiple switches
•	OSPF dynamic routing
•	Redundancy concepts
•	Port security
•	SSH remote management
•	Basic network monitoring
Now we’re simulating something closer to a real company network.
________________________________________
🏢 Step 1: Expand the Topology
Add:
•	1 additional Router
•	1 additional Switch
•	2 more PCs
Talking Points:
•	“Real networks don’t run on one router and one switch.”
•	“We’re now simulating multiple departments or branch offices.”
•	“Redundancy and scalability are what separate small networks from enterprise networks.”
________________________________________
🌍 Step 2: Configure OSPF (Dynamic Routing)
Instead of static routes, we use OSPF.
Router 1:
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
Router 2:
router ospf 1
 network 192.168.30.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
Talking Points:
•	“OSPF allows routers to automatically learn routes.”
•	“This is scalable. If we add another network, OSPF adapts.”
•	“Dynamic routing is used in almost every enterprise environment.”
Then show:
show ip route
Highlight the O routes (OSPF learned routes).
________________________________________
🔁 Step 3: Redundancy Concept (Basic Introduction)
Explain:
•	Why having 2 routers prevents downtime.
•	What happens if one router fails.
You can simulate:
•	Shut down one interface:
interface g0/0
shutdown
Then show how routing adjusts.
Talking Points:
•	“Downtime costs companies money.”
•	“Redundancy increases reliability.”
•	“High availability is a key network engineering concept.”
________________________________________
🔐 Step 4: Port Security on the Switch
Protect switch ports from unauthorized devices.
interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
Talking Points:
•	“Port security prevents someone from unplugging a PC and plugging in their own laptop.”
•	“This is basic physical-layer security.”
•	“Security isn’t just firewalls — it starts at the switch.”
________________________________________
🔑 Step 5: Configure SSH for Secure Remote Access
Instead of Telnet, use SSH.
hostname R1
ip domain-name lab.local
crypto key generate rsa
username admin secret Cisco123
line vty 0 4
 transport input ssh
 login local
Talking Points:
•	“Never use Telnet in production.”
•	“SSH encrypts login credentials.”
•	“Remote management is essential in real jobs.”
________________________________________
📊 Step 6: Basic Monitoring Commands
Show how to monitor:
show ip interface brief
show ip route
show running-config
show logging
show processes cpu
Talking Points:
•	“Monitoring is proactive troubleshooting.”
•	“Engineers don’t wait for problems — they look for warning signs.”
•	“These commands are daily-use tools in real environments.”
________________________________________
🧪 Step 7: Troubleshooting Scenario (Make It Engaging)
Create a problem:
•	Wrong OSPF network statement
•	Shutdown interface
•	Incorrect gateway
•	Port security violation
Then solve it live.
Talking Points:
•	“Interviews love troubleshooting stories.”
•	“Always check Layer 1 first.”
•	“Use a structured method — don’t guess.”

