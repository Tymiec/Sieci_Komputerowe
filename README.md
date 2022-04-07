# Sieci grupa 3

## Konfiguracja intefejsów

### Zaczynamy zawsze od:

```bash 
configure private
update
```
### Potem ustawiamy vlan i interfejs

```bash
set interfaces ge-0/0/1 vlan-tagging
set-interfaces ge-0/0/1 unit X00 vlan-id X00
set interfaces ge-0/0/1 unit X00 family inet address 192.168.X.INTERFEJS (np .9/30)
show | compare
commit
run show route
```

### Dodajemy interfejs do naszego routera
```bash
set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.X00
commit
```

#### Testowanie
```bash
run show route
run ping 192.168.X.29 routing-instance ROUTERX
run show route table ROUTERX.
```
#### Dodatkowe notatki do interfejsów
```bash
run show configuration set interface ge-0/0/2.0
set interface ge-0/0/2.0 family inet address 192.168.X.1/30
commit (az bedzie sukces)
run show configuration
run show route
run show interfaces terse
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

```bash
run telnet 172.30.33.XXX     (XXX to numer routera)
```

logujemy się
```
configure private
load override LAN-gr3-dd.mm.rrrr
commit
```
