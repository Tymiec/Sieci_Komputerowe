# Sieci grupa 3

## Konfiguracja intefejsów
Zaczynamy zawsze od:

```bash configure private```

```bash update```
```bash ```

```bash ```

```bash ```
<!-- run show configuration set interface ge-0/0/2.0
set interface ge-0/0/2.0 family inet address 192.168.X.1/30
commit (az bedzie sukces)
run show configuration
run show route
run show interfaces terse -->
```bash
set interfaces ge-0/0/1 vlan-tagging

commit

set-interfaces ge-0/0/1 unit X00 vlan-id X00

set interfaces ge-0/0/1 unit X00 family inet address 192.168.X.INTERFEJS

show|compare

commit

run show route
```

```bash
set routing-instance ROUTER12 instance-type virtual-router
set routing-instance ROUTER12 interface ge-0/0/1.X00
commit
run show route
run ping 192.168.X.29 routing-instance ROUTERX
run show route table ROUTERX.

set routing-instances ROUTERX routing-options static route 192.168.X.4/30 next-hop 193.168.X.30
commit
run show route
```
## Usuwanie staticów

```bash
delete 
```

## Konfiguracja RIP

## Ładowanie i zapisywanie

Łączymy się z routerem 135 lub 138

wpisujemy:

```run telnet 172.30.33.XXX``` (XXX to numer routera)

logujemy się
```
configure private
load override LAN-gr3-dd.mm.rrrr
commit
```
