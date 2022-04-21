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
run show configuration
set interface ge-0/0/2.0
set interface ge-0/0/2.0 family inet address 192.168.X.1/30
commit (az bedzie sukces)
run show configuration
run show route
run show interfaces terse
set routing-instances ROUTERX routing-options static route 192.168.X.4/30 next-hop 193.168.X.30
commit
run show route
```
## Modyfikacja staticów

```bash
activate / deactivate routing-instances ROUTERX routing-options static

set routing-instances ROUTERX routing-options static route 0/0 next-hop 192.168.1.6
```

## Konfiguracja RIP
```bash
set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X00
commit
```

### Ustawianie polityki

#### From direct
```bash
set policy-options policy-statement FROM-DIRECT-X
from (warunek logiczny) 
then accept/reject
```

```bash
set policy-options policy-statement FROM-DIRECT-X from protocol direct
set policy-options policy-statement FROM-DIRECT-X then accept
run show configuration (sprawdzenie polityki)
set routing-instances ROUTERX protocols rip group GRUPA1 export FROM-DIRECT-X
```
#### From RIP
```bash
set policy-options policy-statement FROM-RIP-X from protocol rip
set policy-options policy-statement FROM-RIP-X then accept
set routing-instances ROUTERX protocols rip group GRUPA1 export FROM-RIP-X

run show advertising-protocol rip 192.168.13.6
```

### Komendy pomocniczne
```bash
run show rip ?
run show rip neighbor instance ROUTERX
run show route table ROUTERX.

```

## Ładowanie i zapisywanie

### Ładowanie
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
### Zapisywanie
```bash
update
save LAN-gr3-dd.mm.rrrr
```

### Zajęcia 3 nie wiem gdzie te komendy
```bash
deactivate routing-instances ROUTERX protocols rip
```

### Zajęcia 4
```bash
set policy-options prefix-list LISTAX 192.168.X.8/30
set policy-options policy-statement POLITYKA_PROBNA_X from prefix-list LISTA1
set policy-options policy-statement POLITYKA_PROBNA_X then accept
set routing-instances ROUTERX protocols rip group GRUPA1 export POLITYKA_PROBNA_X

insert routing-instances ROUTERX protocols rip group GRUPAX export FROM-RIP-1 before/after FROM-DIRECT-1 
```
(ustawiamy polityki tak jak chcemy w jakiej kolejnosc, sprawdzamy używając ```show | compare```)

## Protokół OSPF

```bash
set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X00 potem X01 potem X05
run show ospf neighbor instance ROUTERX
```