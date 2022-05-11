# Komendy potrzebne do zadania

## Po wejściu na każdy router

```bash 
configure private
update
```

## Ustawiamy VLAN na każdym routerze

X  <- Nasz numer
YY <- YY numerek VLAN 
```bash
set interfaces ge-0/0/2 vlan-tagging
set-interfaces ge-0/0/2 unit XYY vlan-id XYY
set interfaces ge-0/0/2 unit XYY family inet address 192.168.X.INTERFEJS (np .9/30)

set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/2.XYY

run show route

set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X00
```