set routing-instance ROUTER1 instance-type virtual-router

set interfaces ge-0/0/2 unit 178 vlan-id 178
set interfaces ge-0/0/2 unit 178 family inet address 192.168.1.122/30 
set routing-instance ROUTER1 interface ge-0/0/2.178

set interfaces ge-0/0/2 unit 171 vlan-id 171
set interfaces ge-0/0/2 unit 171 family inet address 192.168.1.85/28
set routing-instance ROUTER1 interface ge-0/0/2.171

set interfaces ge-0/0/2 unit 179 vlan-id 179
set interfaces ge-0/0/2 unit 179 family inet address 192.168.1.125/30
set routing-instance ROUTER1 interface ge-0/0/2.179

set interfaces ge-0/0/2 unit 176 vlan-id 176
set interfaces ge-0/0/2 unit 176 family inet address 192.168.1.109/29 
set routing-instance ROUTER1 interface ge-0/0/2.176

show | compare