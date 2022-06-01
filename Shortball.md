# Szort Ball - to jest slang jak coś
> Tymek, Paweł z klubu R i Majkel

## Router
```bash
set interfaces ge-0/0/'INTERFEJS' vlan-tagging
set interfaces ge-0/0/'INTERFEJS' vlan-id 'VLANS'
set interfaces ge-0/0/'INTERFEJS' unit 'VLANS' family inet address 192.168.X.'ADDRESS'/'MASKA' (np .9/30)

set routing-instance 'ROUTERX' instance-type virtual-router
set routing-instance 'ROUTERX' interface ge-0/0/'INTERFEJS'.'VLANS' (np. 1.1000)
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
<br>

## Konkuter
#### Wolny adres z sieci (pierwszy jest zajęty przez router)
```bash
set interfaces ge-0/0/'INTERFEJS' unit 0 family inet address 192.168.X.'ADDRESS'/'MASKA' (np .9/30)

set routing-instance 'KOMPUTERX' instance-type virtual-router
set routing-instance 'KOMPUTERX' interface ge-0/0/'INTERFEJS'.0 (np. 1.1000)
```