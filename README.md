
#
# Automatyczne powiekszenie dysku serwera windows
#
 Jeszcze jest sporo todo
 Ale kończy mi się czas wiec pierwszy szkielet
 w skrypcie w kilku miejscach jest zaszyta nazwa serwera windows "wins" - klucz do parametrów

 generalnie powinna byc podawana jako opcja lub system powinien zapytać o nazwe serwera
 To pierwsze ToDo <BR>
 Drugie ToDo to wielkość powiekszenia dysku - tez powinna być opcja z linii  <BR>
 Trzecie ToDo - przbudowanie skryptu tak aby mógł być uruchamiany dla wielu serwerów<BR>
 Czwarte Todo - rozumiem że możemy mieć różnie serwery wirtualne - w zależności od konfiguracji można wybierać inne moduły dostępu do nich. Ja u siebie mam proxmox-a i na nim zrobiłem przykład<BR>
 Piąte Todo - brakuje obsługi błędów które mogły by zaistnieć z powodów które nie powinny mieć miejsca<BR>

# Założenia:
 Rozumiem iz załoenie 3 identycznych dyskow jest po to aby było wiadomo które powiekszać
 Ale to chyba nie jest dobra droga bo i tak musimy wiedziec na jakiej maszynie jest virtualny serwer
 z tego wynika potrzeba uzupelnienia danych inventories vars. <BR>
 Tam dla każdego serwera windows zbudowana jest konfiguracja i tam mogła by sie znaleść informacja o dyskach np. oracle <BR>
 Ja w swoim przypadku nazwałem wolumeny na windowsie w stylu oracle_data<BR>
 I w praktyce mozna by bylo wyszukac volumeny które maja w nazwie np oracle lub cokolwiek
 i bardziej wybiórczo ustawiać wielkości dysków

# To co może się nie podobać ... 
 1. wiekszość kodu to skrypty bashowe,
    są 3 uruchamianych z dysku z opcjami resztę wprowadziłem bezpośrednio do kodu 
    w tych 3 miałem problem z znakami " <BR>
 2. Do połączenia z serwerem windows używam ssh a nie PowerShell,
   dwa pierwsze dni strawilem na ustawienie środowiska cantos + WS 2012 + testowy Windows 7 i przejść między nimi<BR>
  PowerShell bronił sie mocno przed dostępem zdalnym i pierwsze co uruchomiłem to ssh z kluczami
  później uruchomiłem przebitkę przez PowerShell ale już nie chciałem tracić czasu na ustawianie autoryzacji
  więc nie używam komend PowerShell <BR>
 3. Nie korzystałem z importowanych role - pewnie było by ładniej ale obecnie i tak playbook jest krotki <BR>

# Teraz logika wykonywania:
 1. Sprawdzenie wolnego miejsca na dyskach windowsa, jeśli jest to prawdopodobnie coś się wcześniej nie powiodło. Bo możliwe że wystarczy powiększyć wolumeny.<BR>
 2. Pobranie wielkości dysków które mają być powiększone. Po pierwsze system zwraca tylko powtarzające się wielkości po drugie wielkość jest zapisywana pod WinDiskSize i później sprawdzana czy przypadkiem już nie powiększyliśmy dysku na virtualce a windows tego jeszcze nie widzi.<BR>
 3. Wyciągana jest nazwa dysków na virtualce zgodna wielkościowo z tym co jest na windowsie<BR>
 4. Powiększenie dysków na virtualce z dwoma warunkami 1. Dyski na windowsie nie mają wolnej przestrzeni co mogło by świadczyć iż nie zostały powiększone oraz jeśli aktualna wielkość dysków na virtualce oraz na windowsie jest identyczny - bo może już wcześniej ten krok został wykonany<BR>
 5. Zatrzymanie serwera od strony maszyny wirtualnej i poczekanie aż faktycznie się zatrzyma<BR>
 6. Uruchomienie serwera<BR> 
 7. Powiększenie przestrzeni na powiększonych dyskach<BR>
<BR>
 Generalnie krok 5 i 6 nie dla wszystkich wirtualnych serwerów będzie konieczny, bo przy wyższych kernelach już nie jest - warunek może być ustawiany w var.yml


