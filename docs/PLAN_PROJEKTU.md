# Plan realizacji — Credit Scoring 2026/2027

## Cel

Budujemy automatyczny proces akceptacji kredytowej, w którym cztery modele oceniają ryzyko niewykonania zobowiązania i prawdopodobieństwo odpowiedzi na ofertę, a silnik decyzyjny zarządza akceptacją i sprzedażą krzyżową. Sprawdzamy wynik w symulacji dynamicznej lat 1975–1987. Strategia wzorcowa przynosi **663 327 PLN**; pracujemy nad uzasadnioną i odtwarzalną poprawą.

**Termin formalny:** 25 stycznia 2027 r. — wysyłka materiałów mailem; projekt wraz z obroną 20 punktów. Pozostałe szczegóły punktacji i organizacji obrony sprawdzamy z prowadzącą.

## Zakres wynikający z opisu projektu

1. Własne ulepszenie jednego z etapów modelowania lub dokumentowania — SAS 4GL albo Python.
2. **PD Ins:** `default12=1` dla kredytu ratalnego (`product='ins'`).
3. **PD Css:** `default12=1` dla kredytu gotówkowego (`product='css'`).
4. **PD Css Cross:** `default_cross12=1` dla przyszłego kredytu gotówkowego z perspektywy aplikacji o ratalny.
5. **PR Css Cross:** `cross_response=1` — prawdopodobieństwo odpowiedzi na propozycję gotówkową.
6. Silnik decyzyjny, wdrożenie wszystkich modeli, pełna symulacja i raporty zysku/straty za lata 1975–1987.

W ramach zadania 1 nie tworzymy osobnego, samodzielnego stanowiska. Łączymy je z rolą właściciela PR Css Cross, wspieranego przez pozostałe osoby. W starszym opisie dopuszczono jeden model AI zamiast karty skoringowej — rozważymy go dopiero po uruchomieniu kompletnej wersji bazowej. Ponadto możliwa jest segmentacja i budowa dodatkowych modeli.

## Architektura i zależności

```text
Dane źródłowe → miesięczna ABT → cztery modele → score i kalibracja PD/PR
          → scoring_engine → decyzja A/D/N → dynamiczna symulacja
          → aktualizacja historii → kolejne miesiące → P&L i dokumentacja
```

Przyszłe wnioski nie są niezależne od bieżących decyzji. Dlatego wynik pojedynczej tabeli scoringowej nie zastępuje symulacji procesu: akceptacja kredytu może zmienić późniejsze cechy klienta, a tym samym kolejne decyzje.

**Pierwsze spotkanie: 18 listopada 2026 r.** Ustalamy podział pracy, dostęp do Workbench i osobę od pierwszego uruchomienia symulatora. [Agenda i instrukcja spotkania](krok_po_kroku/01_START.md).

## Etapy i warunki przejścia

| Faza | Termin wewnętrzny | Odpowiadamy za | Warunek zakończenia |
|---|---|---|---|
| **F0 Pierwsze spotkanie i organizacja** | 18 XI (spotkanie); do 22 XI (przygotowanie) | zakres, role, repo, plan pracy | G0: pięć uzgodnionych odpowiedzialności i backlog |
| **F1 Benchmark** | 29 XI | Workbench, wersje repozytoriów, odtworzenie strategii wzorcowej | G1: log, HTML i uzgodniony wynik/liczniki |
| **F2 Kontrakty danych** | 6 XII | ABT, targety, dojrzałość zdarzeń, brak leakage, próbki | G2: cztery zaakceptowane kontrakty |
| **F3 Modele v1** | 20 XII | cztery modele, binning, selekcja, metryki | G3: cztery model cards + kandydaci scoringu |
| **F4 Kalibracja i SAS** | 3 I | score→PD/PR, poprawny kod SAS, testy zgodności | G4: cztery wdrażalne scoringi |
| **F5 Integracja** | 10 I | silnik decyzji i pierwszy pełny przebieg | G5: raport P&L nowych modeli i kontrola reguł |
| **F6 Optymalizacja** | 17 I | eksperymenty ze strategią i wybór wersji | G6: uzasadniony wariant z historią prób |
| **F7 Finalizacja** | 22 I | dokumentacja, audyt, próbna obrona | G7: zweryfikowany pakiet końcowy |
| **Oddanie** | 25 I | wysłanie finalnej wersji | potwierdzenie wysłania |

Terminy etapów to nasz wewnętrzny plan pracy. Krytyczną ścieżką pozostaje: **F1 → F2 → F3 → F4 → F5 → F6 → F7**. Możemy prowadzić dokumentację, analizę ryzyk i prace nad zadaniem 1 równolegle.

## Kryterium ukończenia dowolnego zadania

Zadanie uznajemy za zakończone, jeśli:
- istnieje konkretny rezultat z zapisanym miejscem w repozytorium lub lokalizacją raportu;
- da się wskazać dane źródłowe, wersje kodu i parametry;
- wykonano adekwatne testy oraz opisano ograniczenia;
- druga osoba przeprowadziła review;
- rezultat jest zgodny z kontraktem integracyjnym i gotowy do dalszego etapu.

Nie oznaczamy jako DONE pracy, dla której mamy jedynie deklarację lub screen bez źródła danych.

## Kryteria stop

Wstrzymujemy integrację, jeśli wykryto: przyszłą informację w modelu, błędną obserwowalność targetu, niezgodny scoring SAS, nieodtwarzalny rezultat, zmianę reguły `998 not active customer` albo rozbieżność definicji wyniku finansowego.

## Źródła i pierwszeństwo

- Harmonogram 2026/2027 — termin oddania i zasady zaliczenia.
- Opis projektu udostępniony w materiałach — zakres sześciu zadań, modele i silnik.
- Kod z wersjonowanych repozytoriów — faktyczna implementacja, którą musimy przetestować.
- Raporty przykładowe — oczekiwany sposób dokumentowania, nie gwarancja bezbłędnego kodu.

Rozbieżności zapisujemy jako otwarte kwestie, nie rozstrzygamy ich bez sprawdzenia. Szczegóły z wcześniejszego opisu wymagają potwierdzenia dla edycji 2026/2027.
