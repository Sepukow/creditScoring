# Credit Scoring — automatyzacja procesu biznesowego

Projekt zespołowy na studiach magisterskich SGH (semestr zimowy 2026/2027).

## Cel

Budujemy kompletny proces akceptacji kredytowej dla dwóch produktów: kredytu ratalnego (`ins`) i gotówkowego (`css`). Cztery modele predykcyjne dostarczają prawdopodobieństwa ryzyka i odpowiedzi na ofertę, a silnik decyzyjny wykorzystuje je do podejmowania decyzji. Wyniki sprawdzamy w symulacji procesu za lata 1975–1987.

**Wynik strategii referencyjnej:** 663 327 PLN. Naszym celem jest poprawa tego wyniku i rzetelne uzasadnienie wybranej strategii.

**Termin oddania:** 25 stycznia 2027 r. (wysyłka mailowa). Projekt wraz z obroną: 20 pkt.

## Od czego zaczynamy

1. Czytamy [plan projektu](docs/PLAN_PROJEKTU.md) i [zasady pracy zespołu](docs/ZASADY_PRACY.md).
2. Sprawdzamy [podział odpowiedzialności](docs/ROLE_I_HARMONOGRAM.md).
3. Wybieramy zadanie w [Issues](../../issues), a następnie zgłaszamy w nim postępy i blokady.
4. Każdą zmianę kodu oddajemy przez Pull Request z opisem testów i wyników.
5. Wersje modeli, parametry strategii oraz pełne raporty symulacji dokumentujemy zgodnie z [protokołem eksperymentów](docs/EKSPERYMENTY_I_WALIDACJA.md).

## Jak składa się cały system

```text
Dane historyczne i ABT
       |
       v
+----------------+----------------+-------------------+-------------------+
| PD Ins         | PD Css         | PD Css Cross      | PR Css Cross      |
| default12      | default12      | default_cross12   | cross_response    |
+----------------+----------------+-------------------+-------------------+
       |
       v
Scoring + kalibracja PD/PR
       |
       v
Silnik decyzyjny (A / D / N)
       |
       v
Symulacja dynamiczna 1975–1987
       |
       v
Raport finansowy + analiza stabilności + dokumentacja
```

## Podział pracy

| Rola | Główna odpowiedzialność | Zadanie przekrojowe |
|---|---|---|
| A | PD Ins | Środowisko i reprodukowalność |
| B | PD Css | Standard walidacji |
| C | PD Css Cross | Kontrola targetów i chronologii |
| D | PR Css Cross | Własne ulepszenie kodu i raportowanie |
| E | Strategia i symulacja | Integracja, eksperymenty i oddanie |

Role A–E są oznaczeniami roboczymi. Przypiszemy je do konkretnych osób podczas kickoffu. Każdy odpowiada też za testy, scoring i dokumentację swojej części.

## Zasady, których przestrzegamy

- Nie zmieniamy reguły **`998 not active customer`**.
- Nie wykorzystujemy informacji z przyszłości do decyzji podejmowanych wcześniej.
- Nie traktujemy braku obserwowanego zdarzenia jako automatycznej etykiety „0”.
- Nie porównujemy wyników bez wskazania wersji kodu, danych, modeli i reguł.
- Nie nadpisujemy wzorcowych plików ani wyników innych osób.
- Samo wysokie Gini nie stanowi dowodu poprawy wyniku finansowego.

## Dokumentacja

- [Plan projektu i kryteria odbioru](docs/PLAN_PROJEKTU.md)
- [Role oraz harmonogram](docs/ROLE_I_HARMONOGRAM.md)
- [Zasady pracy zespołowej](docs/ZASADY_PRACY.md)
- [Dane, modele i kontrakty](docs/KONTRAKTY_DANYCH_I_MODELI.md)
- [Eksperymenty, testy i optymalizacja](docs/EKSPERYMENTY_I_WALIDACJA.md)
- [Finalne oddanie i obrona](docs/ODDANIE_I_OBRONA.md)

## Materiały źródłowe

Punktem wyjścia są materiały udostępnione na zajęciach oraz repozytoria [ASBSAS](https://github.com/prozenba/ASBSAS), [ASBPYTHON](https://github.com/prozenba/ASBPYTHON) i [PROCESS_SIMULATION](https://github.com/prozenba/PROCESS_SIMULATION). W czasie pierwszej konfiguracji zapisujemy konkretne wersje tych zależności. Materiały ze starszym opisem projektu traktujemy jako obowiązującą specyfikację roboczą do czasu potwierdzenia szczegółów dla semestru 2026/2027.

Repozytorium służy do koordynacji pracy i przechowywania naszych plików. Dane źródłowe i duże wyniki symulacji trzymamy w osobnej przestrzeni roboczej, nie w historii Git.
