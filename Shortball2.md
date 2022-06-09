# Szort Ball - to jest slang jak coś
#### Jak pingować
```bash
ping 'ADDRESS' routing-instance 'INSTANCJA-ROUTINGU' (ROUTERX)
```

## Router
#### Szybka konfiguracja
```bash
set interfaces ge-0/0/'INTERFEJS' vlan-tagging
set interfaces ge-0/0/'INTERFEJS' unit 'VLANS' vlan-id 'VLANS'
set interfaces ge-0/0/'INTERFEJS' unit 'VLANS' family inet address 192.168.X.'ADDRESS'/'MASKA' (np .9/30)

set routing-instances 'ROUTERX' instance-type virtual-router
set routing-instances 'ROUTERX' interface ge-0/0/'INTERFEJS'.'VLANS' (np. 1.1000)
```
#### Jeżeli są 2 routery to można zrobić statica (trzeba zrobić na obydwu routerach)
```bash
set routing-instances 'ROUTERX' routing-options static route 'ADDRESS_SIECI'/'MASKA' next-hop 'ADDRESS_ROUTERA' (bez maski)
```
#### Jeżeli mamy zrobić staticka tylko da danego adresu a nie dla sieci to zamiast adresu sieci dajemy adres urządzenia z maską 32
```bash
set routing-instances 'ROUTERX' routing-options static route 'ADDRESS_URZADZENIA'/32 next-hop 'ADDRESS_ROUTERA' (bez maski)
```

## OSPF :OOOO:
#### Nikt nie wie po co ale robimy loopbacki (commitujemy dopiero po 2 komendach policji)
```bash
set interfaces lo0.'NUMEREK' family inet address 192.'NUMEREK'.0.'KONIEC_IP_ROUTERA'/32
set routing-instances 'ROUTERX' interface lo0.'NUMEREK'
```
#### Na interfejsie między routerami (jak jest więcej vlanów to dla każdego tak robimy)
```bash
set routing-instances 'ROUTERX' protocols ospf area 0 interface ge-0/0/'INTERFEJS'.'VLAN'
```
#### Na interfejsie routera idącego do switcha
```bash
set routing-instances 'ROUTERX' protocols ospf area 0 interface ge-0/0/'INTERFEJS'.'VLAN' passive
```
#### Reject z loobbacka bo ospf rozgłasza wszystko
```bash
set policy-options policy-statement REJECT-LOOPBACK-'NUMEREK' from interface lo0.'NUMEREK'
set policy-options policy-statement REJECT-LOOPBACK-'NUMEREK' then reject
set routing-instances 'ROUTERX' protocols ospf export REJECT-LOOPBACK-'NUMEREK'
```

## RIP :StinkyCheese:
#### Dla każdego interfejsu na którym odpalamy ripa
```bash
set routing-instances ROUTERX protocols rip group GRUPA'NUMEREK' neighbor ge-0/0/'INTERFEJS'.'VLANS'
```
#### Polityczka
```bash
set policy-options policy-statement FROM-DIRECT-'NUMEREK' from protocol direct
set policy-options policy-statement FROM-DIRECT-'NUMEREK' then accept
set routing-instances 'ROUTERX' protocols rip group GRUPA'NUMEREK' export FROM-DIRECT-'NUMEREK'
```
#### Jeżeli mamy jeszcze ospf-a do rozgłoszenia przez rip
```bash
set policy-options policy-statement FROM-OSPF-'NUMEREK' from protocol ospf
set policy-options policy-statement FROM-OSPF-'NUMEREK' then accept
set routing-instances 'ROUTERX' protocols rip group GRUPA'NUMEREK' export FROM-OSPF-'NUMEREK'
```

#### Jak coś nie działa to albo ktoś zrobił pętle albo się pomyliliśmy sprawdzamy tablice routingu
```bash
run show route table 'ROUTERX'
```

<br>

## Switch

#### Tworzymy grupy dla vlanów i dodajemy je
```bash
set vlans 'KOLORX' vlan-id 'VLANS'
```

#### Do każdego wyjścia ze switcha dodajemy grupe vlanów
```bash
set interfaces ge-0/0/'INTERFEJS'.0 family ethernet-switching vlan members 'KOLORX'
```

#### Jeżeli port idzie w stronę routera / kolejnego switcha
```bash
set interfaces ge-0/0/'INTERFEJS'.0 family ethernet-switching port-mode trunk
```

#### Jeżeli port idzie w stronę komputera
```bash
set interfaces ge-0/0/'INTERFEJS'.0 family ethernet-switching port-mode access
```

#### Jeżeli nie działa to sprawdzamy tabele czy jest tam mac interfejsu komputera/switcha/routera
```bash
run show ethernet-switching table
```
<br>
