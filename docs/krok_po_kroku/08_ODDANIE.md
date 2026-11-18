# 8. Składamy dokumentację i przygotowujemy się do obrony

**Wersję zamrażamy do 22 stycznia 2027 r.**  
**Oddajemy projekt 25 stycznia 2027 r. (mail).**

[← README](../../README.md) · [← Optymalizacja](07_OPTYMALIZACJA.md)

## Pojęcia

**Model report** — dokumentacja pojedynczego modelu: próba, definicja targetu, zmienne, binning, estymacja, Gini, stabilność, kalibracja, scoring i ograniczenia. W materiałach są przykładowe raporty PD Ins w Excelu.

**Strategy documentation** — raport reguł akceptacji oraz efektów biznesowych: progi, przyczyny odrzuceń, odsetek akceptacji, P&L łącznie i per produkt.

**Reprodukowalność** — druga osoba potrafi uruchomić tę samą wersję kodu i otrzymać ten sam wynik na tych samych danych, bez ręcznej poprawy tabel.

**Review** — sprawdzenie wyniku przez drugą osobę, nie tylko przeczytanie jego opisu. Reviewer może zakwestionować target, kod scoringu lub sposób liczenia zysku.

**Ograniczenia** — czego wynik nie dowodzi. Przykładowo: wysoki zysk historyczny nie gwarantuje identycznego wyniku na nowych klientach; próba zaakceptowanych klientów ma selection bias.

## Co musi znaleźć się w paczce

- [ ] Raport modelu **PD Ins**, scoring SAS i kalibracja.
- [ ] Raport modelu **PD Css**, scoring SAS i kalibracja.
- [ ] Raport modelu **PD Css Cross**, scoring SAS i kalibracja.
- [ ] Raport modelu **PR Css Cross**, scoring SAS i kalibracja.
- [ ] Finalny `decision_engine.sas` i opis reguł.
- [ ] Wypełniona dokumentacja strategii na wzór `strategy_documentation.xlsx`.
- [ ] `profit_1975_1987.html`, log i jednoznaczna wersja wszystkich modeli.
- [ ] Własny kod zadania 1 z przykładem zastosowania i testami.
- [ ] Instrukcja uruchomienia oraz ograniczenia wyników.
- [ ] Potwierdzenie sprawdzenia przez pozostałe osoby.

## Co robimy przed wysyłką

1. Porównujemy liczby z raportów, kodu, Excela i prezentacji — muszą opisywać **ten sam** przebieg.
2. Testujemy paczkę po przeniesieniu do osobnego folderu, bez zależności od nieopisanych plików z prywatnego katalogu autora.
3. Sprawdzamy, czy deklarowane wyniki są odtwarzalne, a `998 not active customer` nadal działa.
4. Organizujemy próbne wystąpienie: każda osoba omawia swój model lub silnik, dwie trudne decyzje i jedno ograniczenie.
5. Po zamrożeniu 22 stycznia poprawiamy tylko błędy blokujące oddanie, nie rozpoczynamy nowych eksperymentów.
6. Wysyłamy komplet na właściwy adres prowadzącej przed terminem i zachowujemy potwierdzenie.

## Przykładowe pytania na obronie

**Dlaczego nie wystarczy wytrenować modelu z najlepszym Gini?** Bo o wyniku biznesowym decydują też kalibracja, progi, koszt błędnych decyzji i przyszła historia klientów.

**Dlaczego próbujemy rozróżnić zero od nieobserwowanego targetu?** Bo w analizie cross-sell brak przyszłego kredytu nie oznacza potwierdzonego braku jego defaultu.

**Jak udowadniamy działanie kodu SAS?** Pokazujemy testy punktowania, kod silnika, log pełnej symulacji, wersje modeli i końcowy raport.

**Co oznacza poprawa zysku?** Wynik tej samej definicji P&L z procesu na porównywalnych danych, przy zachowanych ograniczeniach; dodatkowo sprawdzamy stabilność w latach i produktach.

## Termin

Aktualny harmonogram 2026/2027 podaje **25 stycznia 2027 r.** jako termin mailowego oddania projektu. Szczegóły terminu obrony oraz starej punktacji należy potwierdzić z prowadzącą.

[Powiązane zadania](../../issues/8).
