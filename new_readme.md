# Sieci Komputerowe notatki
> Tymek Białkowski

## Obsługa routera
Po wejściu na router zawsze wpisujemy
```bash
configure private
update
```
### Zapisywanie i ładowanie 
Zapisywanie
```bash
update
save LAN-gr3-dd.mm.rrrr
```

Ładowanie
```bash
load override *nazwa* 
commit
```
Zamiast ```*nazwa*``` wpisujemy nazwę zapisanej konfiguracji

LAN-gr3-dd.mm.rrrr

basic.cfg to podstwowa konfiguracja routera
> Po załadowaniu wszystkie osoby zalogowane na router powinny użyć komenduy update

## Komendy pomocniczne

Wyświetlamy historię wpisanych komend w danym terminalu
```bash
run show cli history
```

Wyświetlamy zmiane które chcemy zcommitować
```bash
show | compare
```

Powrót na całym routerze do stanu z poprzedniego commita
```bash
rollback
```
> Uwaga, komenda nie uzwględnia tylko naszych commitów tylko przywraca najnowszy commit na danym routerze

Wyświetlanie różnych ustawionych na routerze rzeczy
```bash
run show routing instances

show interfaces terse (bez terse pokazuje szczegółową konfigurację)

show route table ROUTERX (bez table ROUTERX pokazuje całą tablice routingu)

run show configuration routing-instances ROUTERX (bez routing-instances ROUTERX pokazuje całą konfiguracje)
```

> Zaktualizowano 04.05.2022