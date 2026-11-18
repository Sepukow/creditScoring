# Zasady pracy w repozytorium

## Nasz sposób pracy

README i dokumentację utrzymujemy w repozytorium. Zadania i pytania zapisujemy w Issues. Zmiany w plikach nanosimy na osobnych gałęziach i łączymy przez Pull Request po review.

**Issue** opisuje pracę i warunki jej odbioru. **PR** przedstawia wykonanie: pliki, testy, wyniki i wpływ na pozostałe moduły. Komentarze pod zadaniem służą decyzjom, zależnościom i blokadom, a nie zastępują dokumentacji modelu.

## Nazewnictwo

- `feat/<numer-issue>-<opis>` — nowa funkcjonalność lub model;
- `fix/<numer-issue>-<opis>` — poprawka błędu;
- `docs/<numer-issue>-<opis>` — dokumentacja;
- `EXP-YYYYMMDD-NNN` — identyfikator pełnego eksperymentu;
- `PD_INS_v1`, `PD_CSS_v1`, `PD_CSS_CROSS_v1`, `PR_CSS_CROSS_v1` — robocze nazwy pierwszych wersji modeli.

## Statusy zadań

- **Backlog** — zadanie zdefiniowane, bez aktywnej pracy;
- **Ready** — poprzednie zależności i materiały są dostępne;
- **In progress** — właściciel wykonuje zadanie;
- **Review** — wynik do niezależnego sprawdzenia;
- **Blocked** — wymagane inne zadanie, środowisko lub decyzja;
- **Done** — spełnione kryteria odbioru i dostępny dowód.

Statusy będziemy prowadzić w widoku GitHub Projects. Do czasu jego konfiguracji sprawdzamy checklisty i komentarze w Issues.

## Pull Request — minimum

1. Krótki opis zmiany oraz powiązane Issue (`Closes #...` wyłącznie gdy zmiana faktycznie domyka zadanie).
2. Wersja i pochodzenie danych, użyte parametry, sposób uruchomienia.
3. Wyniki testów, porównanie z poprzednią wersją.
4. Efekty uboczne: zmiana kontraktu danych, scoringu, nazw zmiennych, progu lub formatu raportu.
5. Potwierdzenie, że nie naruszono reguły `998 not active customer` i nie zastosowano informacji przyszłych.

## Dane i środowiska

Nie commitujemy danych źródłowych, wielkich plików `.sas7bdat`, Parquet, tokenów, haseł ani chwilowych wyników z katalogów roboczych. W Git zapisujemy kod, małe artefakty, definicje danych, manifesty i raporty wymagane do recenzji.

Skrypt główny symulacji może czyścić biblioteki wynikowe (`PROC DATASETS ... KILL`). Dlatego nie uruchamiamy równolegle eksperymentów na tych samych folderach. Każdy przebieg ma osobny katalog lub kopię środowiska. Pełny raport zysku i log przypisujemy do konkretnego `run_id`.

## Decyzje i nierozstrzygnięte kwestie

Jeśli temat wpływa na model, populację, punktację lub architekturę, podejmujemy decyzję w Issue i dokumentujemy jej uzasadnienie. Nie poprawiamy w milczeniu rozbieżności między opisem projektu, przykładową dokumentacją i kodem.

Do wyjaśnienia z prowadzącą: aktualność szczegółowego zakresu ze starszej specyfikacji, interpretacja punktów „uchybienia wskaźników modeli/raportów”, organizacja i termin obrony.

## Standard komunikacji

W komentarzach podajemy konkret: wynik, link, blocker, oczekiwane działanie i termin. Pisząc „gotowe”, wskazujemy dokument, kod albo raport. W razie problemu opisujemy minimalny sposób odtworzenia błędu.
