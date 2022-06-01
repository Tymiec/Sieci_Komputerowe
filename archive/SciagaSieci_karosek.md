<center>
    <h2> Sieci komputerowe v4.0 by Karosek </h2>
</center>

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

Nazwy grup interfejsów są lokalne dla instacji routingu więc wszyscy możemy mieć takie same! Polityki routingu są globalne na routerze, czyli dostępne na poziomie całego routera a nie w instancji routingu. Dlatego w teorii wszyscy moglibyśmy użyć jednej polityki ale mamy wszyscy robić swoje własne do zadań.

## Ładowanie konfiguracji
Czynności, które wykonujemy tylko **raz** na każdym z używanych **routerów**

> configure private  
> load override LAN-gr5-DD.MM.RRRR  
> commit  

Osoby, które nie były zalogowane wcześniej na router, na którym była ładowana konfiguracja nie muszą nic więcej robić. Ale osoby, które zalogowały się przed załadowaniem konfiguracji powinny wykonać:
> update

## Zapisywanie konfiguracji
Konfiguracje można zapisywać wiele razy bo będzie nadpisywana. Pamiętajcie żeby wykonać `commit` przed zapisaniem konfiguracji!

> commit  
> update  
> save LAN-gr5-DD.MM.RRRR  

<div style="page-break-after: always;"></div>

<center>
    <h2>Wyświetlanie konfiguracji</h2>
</center>

### Wyświetlanie interfejsów

> show interfaces  
> show interfaces terse  

### Wyświetlanie tablicy routingu

> show route  
> show route table ROUTERX  
> run show configuration routing-instances ROUTERX  


### Sprawdzanie komunikacji, pingowanie, traceroute
Jeżeli jesteśmy w `configure private` to poniższe komendy trzeba poprzedzić słowem `run`!

> ping 192.168.X.1  
> ping 192.168.X.1 routing-instance ROUTERX  
> traceroute 192.168.X.1  

<center>
    <h2>Konfiguracja routerów</h2>
</center>

### Uruchomienie VLANu na interfejsie
> set interfaces ge-0/0/1 vlan-tagging

### Tworzenie wirtualnego routera
> set routing-instances ROUTERX instance-type virtual-router

### Ustawianie vLANu na interfejsie
> set interfaces ge-0/0/1 unit X00 vlan-id X00

### Przypisanie IP do interfejsu i konkretnego vLANu/unitu
> set interfaces ge-0/0/1 unit X00 family inet address 192.168.X.1/30

### Przypisanie vLANu i interfejsu do wirtualnego routera
> set routing-instances ROUTERX interface ge-0/0/1.X00

### Pokazywanie tablicy routingu tylko z danego protokolu
> run show route table ROUTERX protocol direct

<div style="page-break-after: always;"></div>

<center>
    <h2>Polityki, static routing i protokół RIP</h2>
</center>  

### Wpis statyczny do tablicy routingu
> set routing-instances ROUTERX routing-options static route 192.168.X.0/30 next-hop 192.168.X.2

### Wyłączanie/włączanie wpisów statycznych
> deactivate routing-instances ROUTERX routing-options static route 192.168.X.2/30  
> activate routing-instances ROUTERX routing-options static route 192.168.X.2/30  

### Włączenie protokołu RIP
> set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X00

### Ustawianie polityk
> set policy-options policy-statement FROM-DIRECT-X from protocol direct  
> set policy-options policy-statement FROM-DIRECT-X then accept  

### Włączenie polityk dla protokołu
`export` = wysyłanie, `import` = odbieranie
> set routing-instances ROUTERX protocols rip group GRUPA1 export FROM-DIRECT-X

### Sprawdzanie konfiguracji protokołu RIP
> run show rip neighbor instance ROUTERX  
> run show configuration policy-options  
> run show route advertising-protocol rip 192.168.X.1  
> run show route receive-protocol rip 192.168.X.2  

### Ustawianie polityki prefix-list
> set policy-options prefix-list LISTAX 192.168.X.8/30  
> set policy-options policy-statement POLITYKA-PROBNAX from prefix-list LISTAX  
> set policy-options policy-statement POLITYKA-PROBNAX then accept  
> set routing-instances ROUTERX protocols rip group GRUPA1 export POLITYKA-PROBNAX  

<div style="page-break-after: always;"></div>

<center>
    <h2>Polityki cd.</h2>
</center> 

### Zamiana polityk miejscami
Zamiast `before` można dać `after`
> insert routing-instances ROUTERX protocols rip group GRUPA1 export POLITYKA-A before POLITYKA-B

### Wyświetlanie polityk
> run show policy  
> run show policy FROM-DIRECT-X

### Polityka zabraniajaca rozglaszania tras z loopbackow
> set policy-options policy-statement REJECT-LOOPBACK-X from interface lo0.X  
> set policy-options policy-statement REJECT-LOOPBACK-X then reject

<center>
    <h2>Protokół OSPF</h2>
</center> 

### Konfiguracja
> set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X00

### Sprawdzanie działania
> run show ospf interface instance ROUTERX  
> run show ospf neighbor instance ROUTERX   

### Wyświetlanie bazy stanów łączy
> run show ospf database instance ROUTERX  
> run show ospf database lsa-id 192.168.X.2 instance ROUTERX detail  

### Czyszczenie bazy danych OSPF (Trzeba uruchomić pare razy żeby zadziałalo)
> run clear ospf database instance ROUTERX purge  

### Konfigurowanie loopback (Commitujemy dopiero po wpisaniu obu poleceń! Nie po jednym!)
> set interfaces lo0.X family inet address 192.X.0.\<NR ROUTERA\>/32  
> set routing-instances ROUTERX interface lo0.X

### Ustawianie interfejsu na pasywny ospf
> set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X00 passive

### Redystrybucja tras w ospf (wybieranie trasy z tablicy routingu i wysyłanie przez ospf)
> set routing-instances ROUTERX protocols ospf export FROM-DIRECT-X

<div style="page-break-after: always;"></div>

<center>
    <h2>Konfigurowanie switchy</h2>
    <small>
        VLANy przyjmujemy z zakresu X50 - X59.
        Literka Y wszędzie poniżej reprezentuje numer interfejsu. <br/>
        Gateway to zawsze pierwszy lub ostatni adres w puli. My przyjmujemy że będzie to pierwszy.
    </small>
</center>

### Wyświetlanie adresu mac
> run show interfaces ge-0/0/Y

### Konfigurowanie portu na switchu
Zamiast `access` można dać `trunk` zależy jak konfigurujemy port
> set interfaces ge-0/0/Y.0 family ethernet-switching port-mode access  
> set vlans NAZWA-X vlan-id X50  
> set interfaces ge-0/0/Y.0 family ethernet-switching vlan members NAZWA-X

Do grupy vlanów `NAZWA-X` możemy ich dodawać wiele i potem ustawić, aby interfejs obslugiwal tę grupę vlanow. Dlatego nie musimy manualnie przypisać np. 50 vlanów wszędzie tylko robimy jedną ich grupę i to właśnie tę grupę przypisujemny na interfejs.

### Pokazywanie informacji/sprawdzanie czy działa
> run show vlans  
> run show vlans detail  
> run show ethernet-switching table  
> run show interfaces  
> run show interfaces terse  