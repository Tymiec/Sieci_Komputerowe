# Sieci Komputerowe
> Tymek Białkowski, Paweł z klubu R, inspirowane notatkami Karoska 

## Konwencja nazw

Zamiast **X** wszędzie w tym pliku należy wpisać swój numerek!

|   Zagadnienie	|   Konwencja nazw	|
|---	|---	|
|   Pliki z konfiguracją	|   LAN-gr5-`DD.MM.RRRR`	|
|   Wirtualne routery	|   ROUTER`X`	|
|   Grupy routerow	|   GRUPA`I`, dowolne	|
|   unit-id    |   vlan-id |
|    Polityki   |    FROM-`PROTOCOL`-`X`   |
|   Loopback    |   192.**X**.0.**\<NR ROUTERA\>**/32  |

<div style="page-break-after: always;"></div>

<center>
    <h1>Ustawianie routera</h1>
</center>

### Ustawiamy vlan i interfejs
```ps1
set interfaces ge-0/0/1 vlan-tagging
set interfaces ge-0/0/1 unit X00 vlan-id X00
set interfaces ge-0/0/1 unit X00 family inet address 192.168.X.INTERFEJS (np .9/30)
```
### Tworzymy router i dodajemy do niego interface'y
```
set routing-instance ROUTERX instance-type virtual-router
set routing-instance ROUTERX interface ge-0/0/1.X00 
```

### Tworzenie i modyfikacja staticów

```ps1
set routing-instances ROUTERX routing-options static route 192.168.X.4/30 next-hop 193.168.X.30

activate / deactivate routing-instances ROUTERX routing-options static
set routing-instances ROUTERX routing-options static route 0/0 next-hop 192.168.1.6
```

### Konfiguracja protokołu RIP
```ps1
set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X00
```

### Polityka from RIP
```ps1
set policy-options policy-statement FROM-RIP-X from protocol rip
set policy-options policy-statement FROM-RIP-X then accept
set routing-instances ROUTERX protocols rip group GRUPA1 export FROM-RIP-X

run show advertising-protocol rip 192.168.13.6
```

### Ustawianie polityki
> Trzeba pamiętać o exportowaniu!
```ps1
set policy-options policy-statement FROM-DIRECT-X from protocol direct
set policy-options policy-statement FROM-DIRECT-X then accept

run show configuration (sprawdzenie polityki)

set routing-instances ROUTERX protocols rip group GRUPA1 export FROM-DIRECT-X
```

## OSPF
#### Nikt nie wie po co ale robimy loopbacki (commitujemy dopiero po 2 komendach)
```ps1
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



<div style="page-break-after: always;"></div>

<center>
    <h1>Ustawianie switch'a</h1>
</center>

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

<div style="page-break-after: always;"></div>

<center>
    <h1>Ustawianie komputera</h1>
</center>

## Ustawianie komputera

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



<div style="page-break-after: always;"></div>

<center>
    <h1>Komendy pomocnicze</h1>
</center>

Wyświetlamy historię wpisanych komend w danym terminalu
```ps1
run show cli history
```

Wyświetlamy zmiane które chcemy zcommitować
```ps1
show | compare
```

Powrót na całym routerze do stanu z poprzedniego commita
```ps1
rollback
```
> Uwaga, komenda nie uzwględnia tylko naszych commitów tylko przywraca najnowszy commit na danym routerze

Wyświetlanie różnych ustawionych na routerze rzeczy
```ps1
run show routing instances

show interfaces terse (bez terse pokazuje szczegółową konfigurację)

show route table ROUTERX (bez table ROUTERX pokazuje całą tablice routingu)

run show configuration routing-instances ROUTERX (bez routing-instances ROUTERX pokazuje całą konfiguracje)
```

## Obsługa routera
>Po wejściu na router zawsze wpisujemy
```ps1
configure private
update
```
## Zapisywanie i ładowanie 
> Zapisywanie
```ps1
update
save LAN-gr3-dd.mm.rrrr
```

update
save LAN-gr3-01.06.2022

> Ładowanie
```ps1
load override *nazwa* 
commit
```
Zamiast ```*nazwa*``` wpisujemy nazwę zapisanej konfiguracji

LAN-gr3-dd.mm.rrrr

basic.cfg to podstwowa konfiguracja routera
> Po załadowaniu wszystkie osoby zalogowane na router powinny użyć komenduy update

#### Testowanie
```ps1
run show route
run ping 192.168.X.29 routing-instance ROUTERX
run show route table ROUTERX.
```

> Zaktualizowano 01.06.2022