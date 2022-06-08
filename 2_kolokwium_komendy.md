# Zadanie dodatkowe 2
> Tymek
## Router 136

```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X85 vlan-id X85
set interfaces ge-0/0/1 unit X80 vlan-id X80

set interfaces ge-0/0/1 unit X85 family inet address 192.168.X.139/29
set interfaces ge-0/0/1 unit X80 family inet address 192.168.X.193/30


set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.180
set routing-instance ROUTERX interface ge-0/0/1.185
```


## Router 135
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X80 vlan-id X80
set interfaces ge-0/0/1 unit X87 vlan-id X87
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X81 vlan-id X81

set interfaces ge-0/0/1 unit X80 family inet address 192.168.X.194/30
set interfaces ge-0/0/1 unit X87 family inet address 192.168.X.234/30
set interfaces ge-0/0/1 unit X88 family inet address 192.168.X.130/29
set interfaces ge-0/0/1 unit X81 family inet address 192.168.X.197/30



set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.180
set routing-instance ROUTERX interface ge-0/0/1.187
set routing-instance ROUTERX interface ge-0/0/1.188
set routing-instance ROUTERX interface ge-0/0/1.181
```
## Router 134
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X81 vlan-id X81
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X82 vlan-id X82

set interfaces ge-0/0/1 unit X81 family inet address 192.168.X.198/30
set interfaces ge-0/0/1 unit X88 family inet address 192.168.X.129/29
set interfaces ge-0/0/1 unit X82 family inet address 192.168.X.201/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.181
set routing-instance ROUTERX interface ge-0/0/1.182
set routing-instance ROUTERX interface ge-0/0/1.188
```
## Router 130
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X83 vlan-id X83
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X82 vlan-id X82

set interfaces ge-0/0/1 unit X83 family inet address 192.168.X.205/39
set interfaces ge-0/0/1 unit X88 family inet address 192.168.X.132/29
set interfaces ge-0/0/1 unit X82 family inet address 192.168.X.202/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.183
set routing-instance ROUTERX interface ge-0/0/1.182
set routing-instance ROUTERX interface ge-0/0/1.188
```
## Router 131
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X83 vlan-id X83
set interfaces ge-0/0/1 unit X88 vlan-id X88
set interfaces ge-0/0/1 unit X86 vlan-id X86
set interfaces ge-0/0/1 unit X84 vlan-id X84

set interfaces ge-0/0/1 unit X83 family inet address 192.168.X.206/30
set interfaces ge-0/0/1 unit X88 family inet address 192.168.X.131/29
set interfaces ge-0/0/1 unit X86 family inet address 192.168.X.229/30
set interfaces ge-0/0/1 unit X84 family inet address 192.168.X.225/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.183
set routing-instance ROUTERX interface ge-0/0/1.186
set routing-instance ROUTERX interface ge-0/0/1.188
set routing-instance ROUTERX interface ge-0/0/1.184
```
## Router 132
```powershell
set interfaces ge-0/0/1 vlan-tagging

set interfaces ge-0/0/1 unit X84 vlan-id X84
set interfaces ge-0/0/1 unit X85 vlan-id X85


set interfaces ge-0/0/1 unit X84 family inet address 192.168.X.226/30
set interfaces ge-0/0/1 unit X85 family inet address 192.168.X.137/29

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.184
set routing-instance ROUTERX interface ge-0/0/1.185
```
## Router 133
```powershell
set interfaces ge-0/0/1 vlan-tagging


set interfaces ge-0/0/1 unit X86 vlan-id X86
set interfaces ge-0/0/1 unit X85 vlan-id X85
set interfaces ge-0/0/1 unit X87 vlan-id X87

set interfaces ge-0/0/1 unit X86 family inet address 192.168.X.230/30
set interfaces ge-0/0/1 unit X85 family inet address 192.168.X.138/29
set interfaces ge-0/0/1 unit X87 family inet address 192.168.X.233/30

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.186
set routing-instance ROUTERX interface ge-0/0/1.185
set routing-instance ROUTERX interface ge-0/0/1.187
```