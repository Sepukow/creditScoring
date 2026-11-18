# Dane i modele — wspólny kontrakt

## Dlaczego zaczynamy od ABT

ABT (Analytical Base Table) zawiera cechy klienta i wniosku dostępne w danym momencie procesu. Symulator buduje historię klienta na podstawie zaakceptowanych kredytów. Gdy zmieniamy strategię, zmienia się również przyszła historia, a więc potencjalnie rozkład danych modelowych.

Nie traktujemy tabeli modelowej jako niezależnej od uruchomionej strategii. Każdy snapshot danych powinien mieć identyfikator procesu, okres i wersję kodu.

## Cztery kontrakty modeli

| Model | Etykieta | Moment predykcji | Najważniejsze kontrole |
|---|---|---|---|
| PD Ins | `default12` dla `product='ins'` | wniosek o kredyt ratalny | etykieta po 12 miesiącach, akceptowane wnioski i brak leakage |
| PD Css | `default12` dla `product='css'` | wniosek o kredyt gotówkowy | etykieta po 12 miesiącach, różnice populacji |
| PD Css Cross | `default_cross12` | wniosek o kredyt ratalny | obserwowalność defaultu przyszłego kredytu gotówkowego |
| PR Css Cross | `cross_response` | moment oferty cross-sell | poprawna definicja response, horyzont obserwacji, brak pomylenia targetów |

Filtry, warunki `decision='A'` oraz dokładna konstrukcja etykiet muszą wynikać z realnego kodu generującego ABT; nie przenosimy jednego filtra na wszystkie modele automatycznie. Kontrakt dla PR uwzględnia dopuszczoną w opisie perspektywę wniosku ratalnego i/lub gotówkowego; ostateczną populację ustalimy na podstawie działania symulatora.

## Minimalny kontrakt każdej próby

- jednostka obserwacji: `aid`, `cid`, `period` oraz sposób ich łączenia;
- zakres miesięcy, produkt, decyzja historyczna i warunki wykluczenia;
- definicja wartości 1, 0 i nieznanej/brakującej w targetcie;
- okno obserwacji (szczególnie końcowe miesiące 1987 r.);
- wpływ selekcji zaakceptowanych wniosków;
- lista zmiennych dopuszczonych na moment decyzji;
- rozkład braków, w tym specjalne wartości SAS `.`, `.d`, `.i`;
- liczności rekordów, klientów, zdarzeń, niezdarzeń, braków oraz podziałów train/validation/test;
- `source_run_id` i kod generujący dane.

Przy cross-sell brak produktu lub brak jego wyniku nie może bez zbadania oznaczać targetu 0. Wartości targetu powstające po złożeniu wniosku mogą służyć jako etykieta treningowa, ale nie jako cecha decyzyjna przy wcześniejszej aplikacji.

## Jednolity proces modelowania

1. Profilowanie danych i wykluczenie pól z przyszłości.
2. Podział train/validation/test z kontrolą chronologii oraz wielokrotnych wniosków tego samego klienta.
3. EDA, binning, WoE/IV, kontrola braków i stabilności.
4. Regresja logistyczna i selekcja zmiennych z uzasadnieniem efektów.
5. Gini/AUC, KS, PSI, Gini w czasie, kalibracja, analiza współliniowości i ograniczeń.
6. Generowanie karty skoringowej i implementacji SAS.
7. Testy wartości brakujących, granic przedziałów, score i PD/PR.
8. Raport modelu oraz review.

Nie ustalamy sztucznego progu Gini. Ocenę uzasadniamy jakością próby, stabilnością, kalibracją, interpretowalnością oraz wpływem na strategię.

## Kontrakt wdrożenia

Każdy model dostarcza:
- wersję kodu i raportu, opis próby oraz wersję źródłowej ABT;
- `scoring_code.sas` oraz współczynniki score→PD/PR;
- jednoznaczne pola wyjściowe dla silnika;
- wyniki zgodności scoringu ze źródłowym modelem;
- testy brzegów binów oraz braków SAS.

Zwracane prawdopodobieństwa muszą należeć do [0,1], a scoring nie może zmieniać liczby i identyfikatorów aplikacji. Przykładowe kody z materiałów poddajemy testom: warunek liczbowy w SAS może przechwycić missing przed osobną gałęzią `IS NULL`.

## Przykłady materiałów

Materiały zawierają `abt.xlsx`, `codes_tips.pdf`, gotowe raporty PD Ins w SAS/Python i cztery przykładowe scoringi w PROCESS_SIMULATION. Korzystamy z nich jako wzorców struktury raportu i implementacji, a nie jako nieweryfikowalnej prawdy o przyszłych danych.
