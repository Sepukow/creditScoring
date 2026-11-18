# Zespół, role i terminy

## Odpowiedzialności

| Rola | Główna część | Przekrojowo | Recenzuje |
|---|---|---|---|
| **A** | PD Ins | Workbench, środowisko, odtwarzalność | PD Css |
| **B** | PD Css | metryki, standard walidacji i parity | PD Css Cross |
| **C** | PD Css Cross | pochodzenie etykiet, chronologia, leakage | PR Css Cross |
| **D** | PR Css Cross | zadanie 1: automatyczny monitoring i raportowanie | PD Ins |
| **E** | silnik decyzyjny i symulacja | integracja, eksperymenty, dokumentacja strategii i wysyłka | integracja z A/B |

W tej chwili używamy liter A–E, nie zgadujemy loginów uczestników. Właściciele modeli dostarczają również kod scoringowy, kalibrację, testy i dokumentację. Nie przerzucamy wszystkich prac wdrożeniowych na rolę E.

**Zasada wspólnej odpowiedzialności:** każda osoba zna również cele pozostałych modeli i potrafi objaśnić ciąg ABT → PD/PR → reguły → decyzja → P&L.

## Jak pracujemy

- Początek tygodnia: krótki przegląd zakończonych zadań, blokad i priorytetów.
- W środku tygodnia: sesja techniczna; pokazujemy testy i decyzje, nie tylko slajdy.
- Przed zamknięciem fazy: reviewer sprawdza kryterium G0–G7.
- Wszelkie istotne zmiany modeli lub strategii przechodzą przez Pull Request.
- Blokadę zgłaszamy w Issue od razu, gdy wymaga udziału innej osoby.

## Zasada przypisania

W każdym Issue podajemy:
- **Właściciel:** rola odpowiedzialna za doprowadzenie wyniku do odbioru;
- **Reviewer:** osoba przeprowadzająca niezależny przegląd;
- **Zależności:** zadania, które muszą być skończone wcześniej;
- **Rezultat:** nazwa raportu, kodu lub decyzji;
- **Kryteria odbioru:** testowalne warunki;
- **Termin wewnętrzny:** data kontrolna, nie nowy termin uczelni.

Po uzgodnieniu składu podmienimy role na użytkowników GitHub i przypiszemy im zadania.

## Plan terminów

| Do | Rezultat |
|---|---|
| **22.11.2026** | wspólne repo i podział pracy |
| **29.11.2026** | uruchomiona strategia referencyjna |
| **06.12.2026** | zatwierdzone definicje 4 populacji i targetów |
| **20.12.2026** | 4 działające modele v1 |
| **03.01.2027** | kalibracja i testy 4 scoringów |
| **10.01.2027** | pierwszy wynik pełnego silnika |
| **17.01.2027** | wybór wariantu końcowego |
| **22.01.2027** | finalny pakiet i próbna obrona |
| **25.01.2027** | formalne oddanie przez e-mail |

W okresie świątecznym planujemy bufor; dlatego wersje v1 modeli muszą być gotowe przed 21 grudnia.

## Gotowość do obrony

Każda osoba potrafi przedstawić: problem i dane, wybór cech oraz metod, metryki, kalibrację, sposób użycia wyniku w strategii i co najmniej jedno ograniczenie swojego komponentu. Podczas obrony nie ograniczamy się do stwierdzenia, że model osiągnął wysokie Gini.
