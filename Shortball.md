# Szort Ball - to jest slang jak coś
#### Jak pingować
```bash
ping 'ADDRESS' routing-instance 'INSTANCJA-ROUTINGU' (KOMPX albo ROUTERX zależy jak kto nazywa)
```

## Router
#### Szybka konfiguracja
```ps1
set interfaces ge-0/0/'INTERFEJS' vlan-tagging
set interfaces ge-0/0/'INTERFEJS' vlan-id 'VLANS'
set interfaces ge-0/0/'INTERFEJS' unit 'VLANS' family inet address 192.168.X.'ADDRESS'/'MASKA' (np .9/30)

set routing-instances 'ROUTERX' instance-type virtual-router
set routing-instances 'ROUTERX' interface ge-0/0/'INTERFEJS'.'VLANS' (np. 1.1000)
```
#### Jeżeli są 2 routery to można zrobić statica (trzeba zrobić na obydwu routerach)
```ps1
set routing-instances 'ROUTERX' routing-options static route 'ADDRESS_SIECI'/'MASKA' next-hop 'ADDRESS_ROUTERA' (bez maski)
```

## OSPF :OOOO:
<<<<<<< HEAD
#### Nikt nie wie po co ale robimy loopbacki (commitujemy dopiero po 2 komendach)
```ps1
=======
#### Nikt nie wie po co ale robimy loopbacki (commitujemy dopiero po 2 komendach policji)
```bash
>>>>>>> 266fd92571cb241f0c770ef0236965aff94d68e3
set interfaces lo0.'NUMEREK' family inet address 192.'NUMEREK'.0.'KONIEC_IP_ROUTERA'/32
set routing-instances 'ROUTERX' interface lo0.'NUMEREK'
```
#### Na interfejsie między routerami (jak jest więcej vlanów to dla każdego tak robimy)
```ps1
set routing-instances 'ROUTERX' protocols ospf area 0 interface ge-0/0/'INTERFEJS'.'VLAN'
```
#### Na interfejsie routera idącego do switcha
```ps1
set routing-instances 'ROUTERX' protocols ospf area 0 interface ge-0/0/'INTERFEJS'.'VLAN' passive
```
#### Robienie polityk
```bash
set policy-options policy-statement FROM-DIRECT-'NUMEREK' from protocol direct
set policy-options policy-statement FROM-DIRECT-'NUMEREK' then accept
set routing-instances 'ROUTERX' protocols ospf export FROM-DIRECT-'NUMEREK'
```
#### Jeżeli nie działa to sprawdzamy arp liste czy jest tam mac routera / komputera
```bash
run show arp
```

<br>

## Switch

#### Tworzymy grupy dla vlanów i dodajemy je
```ps1
set vlans 'KOLORX' vlan-id 'VLANS'
```

#### Do każdego wyjścia ze switcha dodajemy grupe vlanów
```ps1
set interfaces ge-0/0/'INTERFEJS'.0 family ethernet-switching vlan members 'KOLORX'
```

#### Jeżeli port idzie w stronę routera / kolejnego switcha
```ps1
set interfaces ge-0/0/'INTERFEJS'.0 family ethernet-switching port-mode trunk
```

#### Jeżeli port idzie w stronę komputera
```ps1
set interfaces ge-0/0/'INTERFEJS'.0 family ethernet-switching port-mode access
```

#### Jeżeli nie działa to sprawdzamy tabele czy jest tam mac interfejsu komputera/switcha/routera
```ps1
run show ethernet-switching table
```
<br>

## Konkuter
#### Wolny adres z sieci (pierwszy jest zajęty przez router)
```ps1
set interfaces ge-0/0/'INTERFEJS' unit 0 family inet address 192.168.X.'ADDRESS'/'MASKA' (np .9/30)

set routing-instances 'KOMPUTERX' instance-type virtual-router
set routing-instances 'KOMPUTERX' interface ge-0/0/'INTERFEJS'.0 (np. 1.0)
set routing-instances 'KOMPUTERX' routing-options static route 0/0 next-hop 'ADDRESS_ROUTERA' (bez maski)
```
#### Jeżeli nie działa to sprawdzamy arp liste czy jest tam mac routera
```ps1
run show arp
```


