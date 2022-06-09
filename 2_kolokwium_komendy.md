# Zadanie dodatkowe 2
> Tymek
## Router 136

```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X85 vlan-id X85
set interfaces ge-0/0/1 unit X80 vlan-id X80

set interfaces ge-0/0/1 unit X85 family inet address 145.168.X.139/29
set interfaces ge-0/0/1 unit X80 family inet address 145.168.X.193/30


set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.180
set routing-instance ROUTERX interface ge-0/0/1.185
```
OSPF
```ps1
set interfaces lo0.X family inet address 250.0.X.136/32
set routing-instances ROUTERX interface lo0.X

set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X80



set policy-options policy-statement REJECT-LOOPBACK-X from interface lo0.X
set policy-options policy-statement REJECT-LOOPBACK-X then reject
set routing-instances ROUTERX protocols ospf export REJECT-LOOPBACK-X

set policy-options policy-statement FROM-DIRECT-X from protocol direct
set policy-options policy-statement FROM-DIRECT-X then accept
set routing-instances ROUTERX protocols ospf export FROM-DIRECT-X

insert routing-instances ROUTERX protocols ospf export REJECT-LOOPBACK-X before FROM-DIRECT-X
```
DONE

# Router 135
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X80 vlan-id X80
set interfaces ge-0/0/1 unit X87 vlan-id X87
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X81 vlan-id X81

set interfaces ge-0/0/1 unit X80 family inet address 145.168.X.194/30
set interfaces ge-0/0/1 unit X87 family inet address 145.168.X.234/30
set interfaces ge-0/0/1 unit X88 family inet address 145.168.X.130/29
set interfaces ge-0/0/1 unit X81 family inet address 145.168.X.197/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.180
set routing-instance ROUTERX interface ge-0/0/1.187
set routing-instance ROUTERX interface ge-0/0/1.188
set routing-instance ROUTERX interface ge-0/0/1.181
```

OSPF
```ps1
set interfaces lo0.X family inet address 250.0.X.135/32
set routing-instances ROUTERX interface lo0.X

set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X80
set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X88 passive

set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X81
set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X87

insert routing-instances ROUTER1 protocols ospf export REJECT-LOOPBACK-1 before FROM-DIRECT-1

set policy-options policy-statement FROM-DIRECT-1 from protocol direct
set policy-options policy-statement FROM-DIRECT-1 then accept
set routing-instances ROUTER1 protocols rip group GRUPA-1 export FROM-DIRECT-1

set policy-options policy-statement FROM-OSPF-1 from protocol ospf
set policy-options policy-statement FROM-OSPF-1 then accept
set routing-instances ROUTER1 protocols rip group GRUPA-1 export FROM-OSPF-1

set policy-options policy-statement FROM-RIP-1 from protocol rip
set policy-options policy-statement FROM-RIP-1 then accept 
set routing-instances ROUTER1 protocols rip group GRUPA-1 export FROM-RIP-1
set routing-instances ROUTER1 protocols rip group GRUPA-1 neighbor ge-0/0/1.187
set routing-instances ROUTER1 protocols rip group GRUPA-1 neighbor ge-0/0/1.181


set policy-options policy-statement REJECT-LOOPBACK-1 from interface lo0.1
set policy-options policy-statement REJECT-LOOPBACK-1 then reject
set routing-instances ROUTER1 protocols ospf export REJECT-LOOPBACK-1
```

# Router 134
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X81 vlan-id X81
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X82 vlan-id X82

set interfaces ge-0/0/1 unit X81 family inet address 145.168.X.198/30
set interfaces ge-0/0/1 unit X88 family inet address 145.168.X.129/29
set interfaces ge-0/0/1 unit X82 family inet address 145.168.X.201/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.181
set routing-instance ROUTERX interface ge-0/0/1.182
set routing-instance ROUTERX interface ge-0/0/1.188
```

OSPF
```ps1
set interfaces lo0.X family inet address 250.0.X.134/32
set routing-instances ROUTERX interface lo0.X
```

# Router 130
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X83 vlan-id X83
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X82 vlan-id X82

set interfaces ge-0/0/1 unit X83 family inet address 145.168.X.205/39
set interfaces ge-0/0/1 unit X88 family inet address 145.168.X.132/29
set interfaces ge-0/0/1 unit X82 family inet address 145.168.X.202/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.183
set routing-instance ROUTERX interface ge-0/0/1.182
set routing-instance ROUTERX interface ge-0/0/1.188
```

OSPF
```ps1
set interfaces lo0.X family inet address 250.0.X.130/32
set routing-instances ROUTERX interface lo0.X
```

# Router 131
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X83 vlan-id X83
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X86 vlan-id X86
set interfaces ge-0/0/1 unit X84 vlan-id X84

set interfaces ge-0/0/1 unit X83 family inet address 145.168.X.206/30
set interfaces ge-0/0/1 unit X88 family inet address 145.168.X.131/29
set interfaces ge-0/0/1 unit X86 family inet address 145.168.X.229/30
set interfaces ge-0/0/1 unit X84 family inet address 145.168.X.225/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.183
set routing-instance ROUTERX interface ge-0/0/1.186
set routing-instance ROUTERX interface ge-0/0/1.188
set routing-instance ROUTERX interface ge-0/0/1.184
```
OSPF
```ps1
set interfaces lo0.X family inet address 250.0.X.131/32
set routing-instances ROUTERX interface lo0.X
```

# Router 132
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X84 vlan-id X84
set interfaces ge-0/0/1 unit X85 vlan-id X85


set interfaces ge-0/0/1 unit X84 family inet address 145.168.X.226/30
set interfaces ge-0/0/1 unit X85 family inet address 145.168.X.137/29

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.184
set routing-instance ROUTERX interface ge-0/0/1.185
```

# Router 133
```powershell
set interfaces ge-0/0/1 vlan-tagging


set interfaces ge-0/0/1 unit X86 vlan-id X86
set interfaces ge-0/0/1 unit X85 vlan-id X85
set interfaces ge-0/0/1 unit X87 vlan-id X87

set interfaces ge-0/0/1 unit X86 family inet address 145.168.X.230/30
set interfaces ge-0/0/1 unit X85 family inet address 145.168.X.138/29
set interfaces ge-0/0/1 unit X87 family inet address 145.168.X.233/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.186
set routing-instance ROUTERX interface ge-0/0/1.185
set routing-instance ROUTERX interface ge-0/0/1.187
```