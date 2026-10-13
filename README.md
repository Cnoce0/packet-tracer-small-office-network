# packet-tracer-small-office-network
Created a small office network in cisco packet tracer. Evaluated 4 different problems on the network and solved them with write ups.
Small Office Network Simulation (Cisco Packet Tracer)

A simulated small-office network built in Cisco Packet Tracer to practice the networking tasks behind IT help desk and desktop support work: putting users on separate VLANs, routing between them, handing out addresses with DHCP, restricting guest access, and diagnosing faults. Built by Cameron Noce as a portfolio project. This is a simulation, not production hardware.

What this project demonstrates
VLAN segmentation separating Staff, IT, and Guest devices
Inter-VLAN routing using router-on-a-stick
DHCP serving addresses to each VLAN
Access control list (ACL) keeping Guest devices away from Staff and IT
Basic hardening: [port security, unused ports shut down, SSH instead of Telnet]
Structured troubleshooting: [4] deliberately introduced faults, each diagnosed and documented like a help desk ticket
Topology

Show Image

Device	Model	Role
[R1]	[Cisco 2911]	Router: inter-VLAN routing, DHCP, ACL
[SW1]	[Cisco 2960]	Access switch
[Server0]	Server-PT	[HTTP and DNS]
[PC1, PC2, PC3, PC4]	PC-PT	[2 Staff, 1 IT, 1 Guest]
Addressing plan

Edit this table to match what you actually built.

VLAN	Name	Subnet	Gateway	DHCP range
10	Staff	192.168.10.0/24	192.168.10.1	[192.168.10.50 to 192.168.10.200]
20	IT	192.168.20.0/24	192.168.20.1	[192.168.20.50 to 192.168.20.200]
30	Guest	192.168.30.0/24	192.168.30.1	[192.168.30.50 to 192.168.30.200]
Configuration and how I verified it
Feature	Configured on	Verified with
VLANs	Switches	show vlan brief
Trunk link	Switch to router	show interfaces trunk
Router-on-a-stick subinterfaces	Router	show ip interface brief
DHCP pools	Router	show ip dhcp binding
ACL for Guest isolation	Router	show access-lists
Port security	Access ports	show port-security
SSH	Router and switches	show ip ssh

Full device configurations are in configs/. All passwords in this lab are placeholders and are not used anywhere else.

Test results
Test	Expected	Result
Staff PC receives a DHCP address in 192.168.10.0/24	Address assigned	[Pass / Fail]
Staff PC pings IT PC	Reply	[Pass / Fail]
Guest PC pings Staff PC	Blocked	[Pass / Fail]
Guest PC reaches [the server]	[Allowed]	[Pass / Fail]
[IT PC connects to the router over SSH]	[Connects]	[Pass / Fail]

Screenshots for each test are in screenshots/.

Troubleshooting log

Each fault was introduced on purpose, then diagnosed and fixed the way a help desk ticket would be handled. Suggested scenarios (replace with the ones you actually did): a PC in the wrong VLAN, a wrong default gateway, an ACL blocking too much, and a trunk not allowing a VLAN.

TK-001: [Symptom as the user would report it, e.g. "Staff PC can't reach the server"]
Symptom: [What was observed]
Diagnosis: [Steps and commands, e.g. ping the gateway, show vlan brief, show running-config]
Root cause: [What was actually wrong]
Fix: [The exact change made]
Verified by: [How you confirmed it, e.g. a successful ping]
Screenshots: before | after
TK-002: [Symptom]
Symptom: [ ]
Diagnosis: [ ]
Root cause: [ ]
Fix: [ ]
Verified by: [ ]
Screenshots: before | after
TK-003: [Symptom]
Symptom: [ ]
Diagnosis: [ ]
Root cause: [ ]
Fix: [ ]
Verified by: [ ]
Screenshots: before | after
TK-004: [Symptom]
Symptom: [ ]
Diagnosis: [ ]
Root cause: [ ]
Fix: [ ]
Verified by: [ ]
Screenshots: before | after
Repository contents
.
|-- README.md
|-- small-office-network.pkt
|-- configs/
|   |-- [R1.txt]
|   `-- [SW1.txt]
`-- screenshots/
    |-- topology.png
    `-- [test and ticket screenshots]
Running it

Open small-office-network.pkt in Cisco Packet Tracer, which is free with a Cisco Networking Academy account.

Limitations and next steps
Packet Tracer approximates Cisco IOS, so some behavior can differ from real hardware.
Possible next steps: [ ] add NAT, [ ] add DNS records, [ ] add a wireless Guest network, [ ] add syslog.
About

Built by Cameron Noce, a Computer Science student at Ithaca College graduating December 2026. GitHub | LinkedIn
