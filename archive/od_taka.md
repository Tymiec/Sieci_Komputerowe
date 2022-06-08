Rozpoczęcie konfiguracji:
Configure private – tryb konfiguracyjny
Update – pobranie konfiguracji (raz na samym początku).
Load override LAN-gr3-XX.XX.XXXX – tylko gdy wczytujemy konfiguracje.
Konfiguracje interfejsów (router):
	Zaczynamy od powyższych komend!!!
Set interfaces ge-0/0/Y unit 0 family inet address 192.168.X.URZĄDZENIE/maska (np .9/30).
Show | compare – pokazuje wprowadzone zmiany
Commit – zapisuje wprowadzone zmiany
Konfiguracja vlanow i interfejsów (1 przesłane ćwiczenie):
	Zaczynamy od załadowanie routera!
Set interfaces ge-0/0/1 vlan-tagging – włącza vlany na danym interfejsie.
Set interfaces ge-0/0/1 unit X00 vlan-id X00 – router rozumie vlana
Set interfaces ge-0/0/1 unit X00 family inet address 192.168.X.URZĄDZENIE/maska (np. .9/30)
Show | compare!
Commit
Tworzymy wirtualny router:
Set routing-instances ROUTERX instance-type virtual-router – tworzy wirtualny router.
Set routing-instances ROUTERX interface ge-0/0/1.X00 – podłącza dany vlan do danego interfejsu.
Commit
Komendy do sprawdzenia poprawności konfiguracji:
Run show interfaces terse – wyświetla skrótową informacje o interfejsach.
Run show configuration – pokazuje konfiguracje.
Run show route table ROUTERX – pokazuje moją tablice routingu lub bez „ROUTERX” pokaże całą tablice routingu.
Run ping 192.168.X.URZĄDZENIE routing-instance ROUTERX – pingujemy router o podanym adresie (jak jest dobrze skonfigurowane wszystko to powinna być odpowiedź.
Run traceroute 192.168.X.URZĄDZENIE routing-instance ROUTERX – pokazuje ścieżke jaką podążają pakiety.
Konfiguracja adresów statycznych:
Set routing-instances ROUTERX routing-options static route 192.168.X.ADRES-SIECI/maska (np. .8/30) next-hop 192.168.X.URZĄDZENIE – najpierw podajemy adres sieci, która jest miejscem docelowym, a następnie przekazujemy adres następnego interfejsu na tej drodzie.
Commit
(to jest static do sieci, gdy potrzebujemy statica na konkretny adres to zamiast adresu sieci podajemy interesujący nas adres ip danego interfejsu z maską 32)
Activate / deactivate routing-instances ROUTERX routing-options static – włącza/wyłącza adresy statyczne na routerze.
Set routing-instances ROUTERX routing-options static route 0/0 next-hop 192.168.1.6 -> static defaultowy (ważne dla gatewayów! Robimy go dla routerow granicznych, na których nie opłaca się robić ripa).
Konfiguracja protokołu dynamicznego RIP:
	Musi być aktywny na obu interfejsach!!!
Set routing-instances ROUTERX protocols rip group GRUPA1 neighbor ge-0/0/1.X00 – GRUPA1 to lokalna nazwa, uruchamia protokół rip na danym interfejsie (i vlanie).
Commit
Run show rip neighbor instance ROUTERX – pokazuje sąsiadów z aktywowanym ripem.
Static jest ważniejszy niż RIP!
Teraz piszemy polityke (są one globalne):
Set policy-options policy-statement FROM-DIRECT-X from protocol direct – (za DIRECT oraz direct podstawiamy adresy które chcemy rozgłosić)
Set policy-options policy-statement FROM-DIRECT-X then accept – (za DIRECT podstawiamy konkretna polityke!!! Chyba że chodzi nam o direct)
Commit
Set routing-instances ROUTERX protocols rip group GRUPA1 export FROM-DIRECT-X – (tak jak wyżej)
Commit
Polityki piszemy na routerach, które mają więcej niż 1 interfejs ripowy.
Komendy do sprawdzania:
Run show configuration routing-instances ROUTERX – pokazuje konfiguracje danego routera.
Run show policy – pokazuje polityki.
Run show route advertising-protocol rip [adres np. 192.168.12.X] – pokazuje co rozsyła router przez rip.
Deactivate routing-instances ROUTERX protocols rip (a potem commit) – wyłącza ripa na wszystkich interfejsach.
Run show route receive protocol rip (adres sąsiedniego interfejsu) – pokazuje co dostajemy z ripa od danego sąsiada.
Możemy stworzyć liste adresów, które chcemy rozgłosić przez ripa:
Set policy-options prefix-list LISTAX 192.168.X.8/30 (sieć albo adres ?)
Set policy-options policy-statement POLITYKA-PROBNA-12 then accept
Set routing-instances ROUTERX protocols rip group GRUPA1 export POLITYKA-PROBNA-X
Commit
Zamiana miejscami polityk:
Insert routing-instances ROUTERX protocols rip group GRUPA1 export POLITYKA-PROBNA-X before FROM-DIRECT-12 – na końcu nazwa polityki.
Skrócona instrukcja konfiguracji routerów/interfejsów:
Konfigurujemy interfejs (vlany!!!)
Tworzymy swój wirtualny router (nazywamy dużymi literami i dajemy swój numerek)
Przypisujemy interfejs (vlan), którego chcemy używać.
Konfigurujemy routing (statycznie lub dynamicznie).
Konfiguracja protokołu OSPF:
Set routing-instances ROUTERX protocols ospf area 0 interface ge-0/0/1.X00 – [DODAJEMY SŁÓWKO passive GDY CHCEMY ABY INTERFEJS BYŁ PASYWNY] uruchamia OSPF na danym interfejsie (w danym obszarze – area 0).
Commit
Komendy do sprawdzanie poprawności:
Run show ospf neighbor – sprawdzanie w ogólnej tablicy
Run show ospf neighbor instance ROUTERX – interfejsy na których działa OSPF (sąsiedzi).
Run show ospf interface instance ROUTERX – sprawdzanie jakie interfejsy działają aktualnie z OSPF, jak mamy BDR 0 to nie ma sąsiada z ospf.
Run show ospf database instance ROUTERX – pokazuje co zostało rozgłoszone.
Konfiguracja interfejsu loopback:
Set interfaces lo0 unit X family inet address 192.X.0.NR_ROUTERA/32 – NIE COMMITUJEMY JESZCZE !!!
Set routing-instances ROUTERX interface lo0.X
Commit
Po konfiguracji wszystkich interfejsów i loopbackow na jednym routerze z ospf wykonujemy komendę czyszczącą:
Run clear ospf database instance ROUTERX purge – działa dla całej sieci!
Activate / Deactivate routing-instances ROUTERX protocols ospf – deaktywuje ospf lub aktywuje ospf ( gdy chcemy wyłączyć/włączyć na jednym interfejsie to dopisujemy area 0 interface ge-0/0/1.X05)
Gdy chcemu użyć danej polityki z ospf’em:
Set routing-instances ROUTERX protocols ospf export [nazwa polityki]
Commit
Gdy chcemy zabronić rozsyłania loopbackow ospf’owi lub jakichkolwiek innych adresów piszemy polityke:
Set policy-options policy-statement FROM-LOOPBACK-X from interface lo0.X
Set policy-options policy-statement FROM-LOOPBACK-X then reject
Insert routing-instances ROUTERX protocols ospf export FROM-LOOPBACK-X
commit
Switchowanie:
	Jak wchodzimy przez port access to otrzymujey vlana jak wychodzimy to go tracimy, jak wchodzimy przez port trunk to mamy vlana i jak wychodzimy to tez mamy vlana.
	Komendy do wykonania na komputerze:
set interfaces ge-0/0/Y.0 family inet address 10.X.1.2/24
set routing-instances KOMPX-1 instance-type virtual-router – tworzy wirtualny komputer.
Set routing-instances KOMPX-1 interface ge-0/0/Y.0
Commit
Komendy do wykonania na switchu:
Set vlans NIEBIESKI-X vlan-id X50 – tworzy vlana
Set interfaces ge-0/0/Y.0 family Ethernet-switching port-mode access – ustawia tryb pracy interfejsu.
Set interfaces ge-0/0/Y.0 family Ethernet-switching vlan member NIEBIESKI-X
Commit
Aby wszystko działało jak należy musimy skonfigurować gatewaya! Takim gatewayem jest router. Konfigurujemy go tak jak zwykle router z tą różnicą że na odpowiednim interfejsie i jako adres podajemy adres gatewaya np. labach było 10.X.1.1. Następnie robimy adres statyczny deafultowy na komputerach i przekazujemy mu next-hopa na adres gatewaya czyli 10.X.1.1  JEST TO BARDZO WAŻNE, BEZ TEGO SIEĆ NIE BĘDZIE DZIAŁAĆ!
Komendy do sprawdzenia:
Run show Ethernet-switching table – pokazuje tablice słiczowania
Run show vlan detail – szczegółowe info o vlanach
Run show interfaces ge-0/0/Y – tu sprawdzamy adres mac [hardware address]
Run show arp – pokazuje tablice arpow.
