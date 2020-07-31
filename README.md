
@settings {
  font-size: 50;
}

#
# Automatyczne powiekszenie dysku serwera windows
#
 Jeszcze jest sporo todo
 Ale konczy mi się czas wiec pierwszy szkielet
 w skrypcie w kilku miejscach jest zaszyta nazwa serwera windows "wins" - klusz do parametrow

 generalnie powinna byc podawana jako opcja lub system powinien zapytac o nazwe serwera
 To pierwsze ToDo
 Drugie ToDo to wielkosc powiekszenia dysku - tez powinna byc opcja z linni  
 Trzecie ToDo - przbudowanie skryptu tak aby mógł być uruchamiany dla wielu serwerów
 Czwarte Todo - rozumiem że możemy mieć różnie serwery wirtualne - w zależności od konfiguracji można wybierać inne moduły dostępu do nich. Ja u siebie mam proxmox-a i na nim zrobiłem przykład
 Piąte Todo - brakuje obsługi błędów które mogły by zaistnieć z powodów które nie powinny mięc miejsca

# Założenia:
 Rozumiem iz załorzenie 3 identycznych dyskow jest po to aby bylo wiadomo ktore powiekszac
 Ale to chyba nie jest dobra droga bo i tak musimy wiedziec na jakiej maszynie jest serwer windows
 z tego wynika potrzeba uzupelnienia danych inventories vars.
 tam dla kazdego serwera windows zbudowana jest konfiguracja i tam mogla by sie znalesc informacja o dyskach np. oracle
 Ja w swoim przypadku nazwalem wolumeny na windowsie w stylu oracle_data
 I w praktyce mozna by bylo wyszukac volumeny ktore maja w nazwie np oracle lub cokolwiek
 i bardziej wybiórczo ustawiać wielkości dyskow
 

# To co może się nie podobac ... 
 1. wiekszosc kodu to skrypty bashowe,
    są 3 uruchamianych z dysku z opcjami reszte wprowadziłem bezpośrednio do kodu 
    w tych 3 miałem problem z znakami "
 2. Do połaczenia z serwerem windows używam ssh a nie PowerShell,
   dwa pierwsze dni strawilem na ustawienie srodowiska cantos + WS 2012 + testowy Windows 7
#  PowerShell bronił sie mocno przed dostępem zdalnym i pierwsze co uruchomiłem to ssh z kluczami
#  później uruchumiłem przebitkę przez PowerShell ale już nie chcialem tracić czasu na ustawianie autoryzacji
#  więc nie uzywam komend PowerShell 
# 3. Nie korzystałem z importowanych rolle - pewnie było by ładniej ale obecnie i tak playbook jest krotki
# 
# Teraz logika wykonywania:
# 1. Sprawdzenie wolnego miejsca na dyskach windowsa, jeśli jest to prawdopodobnie coś się wcześniej nie powiodło. Bo możliwe że wystarczy powiększyć wolumeny.
# 2. Pobranie wielkości dysków którem mają być powiększone. Po pierwsze system zwraca tylko powtarzające się wielkości po drugie wielkość jest zapisywana pod WinDiskSize i później sprawdzana czy przypadkiem już nie powiększyliśmy dysku na virtualce a windows tego jeszcze nie widzi.
# 3. Wyciągana jest nazwa dysków na virtualce zgodna wielkościowo z tym co jest na windowsie
# 4. Powiększenie dysków na wirtualce z dwoma warunkami 1. Dyski na windowsie nie mają wlnej przestrzeni co mogło by świadczyć iż nie zostały powiększone oraz jeśli aktualna wielkość dysków na wirtualce oraz na windowsie jest identyczny - bo może już wcześniej ten krok został wykonany
# 5. Zatrzymanie serwera od strony maszyny wirtualnej i poczekanie aż faktycznie się zatrzyma
# 6. Uruchomienie serwera 
# 7. Powiększenie przestrzeni na powiększonych dyskach

# Generalnie krok 5 i 6 nie dla wszystkich wirtualnych serwerów będzie konieczny, bo przy wyższych kernelach już nie jest - warunek może być ustawiany w var.yml


