# 2. Uruchamiamy symulator i odtwarzamy punkt odniesienia

**Termin roboczy: do 29 listopada 2026 r.**  
**Efekt:** możemy uruchomić cały proces w SAS i wiemy, co oznacza wynik 663 327 PLN.

[← README](../../README.md) · [← Start](01_START.md) · [Dalej: dane i targety →](03_DANE_I_TARGETY.md)

## Pojęcia

**Symulator** — kod, który przetwarza wnioski po kolei, podejmuje decyzje i aktualizuje późniejszą historię klientów. To nie pojedynczy model.

**Proces dynamiczny** — decyzja z jednego miesiąca wpływa na dane z następnych. Przykład: zaakceptowany kredyt ratalny może stworzyć historię spłat, której nie byłoby przy odrzuceniu.

**Strategia akceptacji** — reguły mówiące, kiedy kredyt zaakceptować lub odrzucić, np. na podstawie oszacowanego PD. Te reguły są w `decision_engine.sas`.

**Scoring** — obliczenie wyniku dla konkretnego wniosku na podstawie cech klienta; może być zamienione na prawdopodobieństwo zdarzenia.

**A/D/N** — `A`: zaakceptowany, `D`: odrzucony, `N`: produkt niedostępny (dla gotówkowego w szczególności ograniczenie aktywności klienta). To oznaczenia używane w kodzie procesu.

**Profit** — zysk wyniku symulacji, nie to samo co liczba udzielonych kredytów. W raporcie rozróżniamy wynik rzeczywiście przyjętych wniosków od potencjalnego wyniku wniosków odrzuconych.

**Log SAS** — zapis kolejnych wykonanych operacji, ostrzeżeń i błędów. „Powstał HTML” nie wystarcza, jeśli log zawiera istotny błąd.

## Gdzie dokładnie zaglądamy

- [PROCESS_SIMULATION/codes/all_contents.sas](https://github.com/prozenba/PROCESS_SIMULATION/blob/main/codes/all_contents.sas) — główny przebieg.
- [process/codes/decision_engine.sas](https://github.com/prozenba/PROCESS_SIMULATION/blob/main/process/codes/decision_engine.sas) — aktywny silnik (w obecnym przykładzie część reguł jest zakomentowana).
- [process/codes/decision_engine_st1_high.sas](https://github.com/prozenba/PROCESS_SIMULATION/blob/main/process/codes/decision_engine_st1_high.sas) — wersja z referencyjnymi progami.
- `materials_all/example_of_project_documentation/example_of_strategy/strategy_documentation.xlsx` — wzorcowe wyniki i ich struktura.

## Zrób to po kolei

1. **Otwórz Workbench i sklonuj trzy repozytoria** zgodnie z instrukcją z zajęć. Zapisz aktualne SHA: `git rev-parse HEAD` w każdym repo (to identyfikator wersji kodu).
2. **Sprawdź ścieżkę `WORKSPACE_PATH`.** Główny program korzysta z `&WORKSPACE_PATH./PROCESS_SIMULATION/`. W Linuxie wielkość liter w nazwie katalogu ma znaczenie.
3. **Przed uruchomieniem zrób kopię katalogu roboczego.** W `all_contents.sas` występują `proc datasets lib=data nolist kill;` oraz `proc datasets lib=abt nolist kill;`. Takie operacje czyszczą poprzednie wyniki — nie pracujemy kilkuosobowo na jednym katalogu wyjściowym.
4. **Zidentyfikuj aktywny wariant silnika.** Główny skrypt dołącza `process/codes/decision_engine.sas`. Jeśli chcemy odtworzyć przykład z progami, kontrolujemy, czy w kopii procesu rzeczywiście załadowano odpowiednią strategię (nie zmieniamy po cichu źródeł).
5. **Uruchom `all_contents.sas` w SAS Workbench.** Sprawdź, czy przebieg zakończył się bez istotnych błędów. Zachowaj log, HTML i dane liczbowe.
6. **Porównaj z przykładem:** 663 327 PLN, 49 224 aplikacje, 10 369 zaakceptowanych, zysk 642 637 PLN dla CSS i 20 690 PLN dla INS. Jeśli dane nie pasują, zapisz przyczynę lub hipotezę oraz dokładną wersję kodu.
7. **Powtórz bez zmian.** Wynik powinien być odtwarzalny. Jeśli nie jest, ustal, czy zależy od przypadkowości, wersji kodu, pozostałych danych lub konfiguracji.

## Scenariusz: wynik różni się od benchmarku

Uruchomiliśmy proces i dostaliśmy inny zysk. Nie modyfikujemy danych do momentu „trafienia” w 663 327 PLN. Najpierw sprawdzamy: czy aktywne są cut-offy, czy zmieniły się źródła danych, czy po drodze powstał error SAS i czy porównujemy właściwy wiersz raportu (wynik akceptacji, a nie hipotetyczne wyniki odrzuceń).

## Co zachowujemy

`run_id`, trzy SHA repozytoriów, opis wybranej strategii, datę i środowisko, log SAS, HTML zysków, liczniki wniosków i akceptacji. Bez tego porównania z dalszymi eksperymentami są bezużyteczne.

**Przechodzimy dalej, kiedy mamy powtarzalny punkt odniesienia albo konkretnie zdiagnozowaną blokadę.** [Zadania techniczne do tego etapu](../../issues/2).
