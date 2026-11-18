# Eksperymenty, walidacja i wynik finansowy

## Punkt odniesienia

Przykładowa strategia za lata 1975–1987 ma 49 224 aplikacje, 10 369 zaakceptowanych i **663 327 PLN** zysku (642 637 PLN z `css`, 20 690 PLN z `ins`). Potwierdzamy te wartości przez wykonanie kodu i uzgodnienie definicji pól raportu; sam arkusz wzorcowy nie jest wystarczającym dowodem wykonania.

W aktualnej wersji repozytorium źródłowego odróżniamy wariant `decision_engine_st1_high.sas` od `decision_engine.sas`, w którym mogą być wyłączone reguły odrzutu. Zapisujemy dokładną wersję uruchomionego kodu.

## Protokół pełnego przebiegu

Do każdego `run_id` zapisujemy:
- datę, właściciela, SHA naszych plików i trzech repozytoriów;
- pochodzenie ABT, wersje czterech modeli i kalibracji;
- wersję silnika oraz komplet progów;
- log SAS, raport `profit_1975_1987.html` i wyniki liczbowe;
- P&L łącznie, dla `ins` i `css`, rok po roku, liczbę akceptacji, bad rate;
- hipotezę, porównanie z kontrolą, ograniczenia i decyzję o przyjęciu/odrzuceniu wariantu.

Nie sumujemy hipotetycznych zysków wniosków odrzuconych z rzeczywistym zyskiem portfela zaakceptowanego.

## Kolejność prób

| Próba | Zmiana | Pytanie |
|---|---|---|
| E0 | referencyjne modele i reguły | Czy potrafimy odtworzyć benchmark? |
| E1 | cztery nowe modele przy regułach referencyjnych | Co zmienia sama jakość modeli? |
| E2 | progi PD Ins / PD Css | Jak polityka ryzyka wpływa na P&L? |
| E3 | PD Cross i PR w cross-sell | Jak optymalizować oba produkty łącznie? |
| E4 | uzasadniona segmentacja | Czy różne grupy wymagają odrębnych reguł? |
| E5 | aktualizacja modeli na nowej ABT, opcjonalnie | Czy zmieniona polityka wymaga ponownego treningu? |

Nie eksperymentujemy na niezapisanych konfiguracjach. Jedna hipoteza powinna mieć czytelny wariant kontrolny.

## Walidacja danych i scoringu

Sprawdzamy unikalność kluczy, poprawność miesięcy, pochodzenie etykiet, dostępność zmiennych w chwili wniosku, obsługę missing, zgodność punktów SAS/Python i kompletność PD/PR. Każdy model ma niezależną próbę walidacyjną i stabilność w czasie.

## Walidacja silnika

Sprawdzamy:
- zachowanie reguły `998 not active customer`;
- prawidłowość przypisania `pd`, `cross_pd` i `pr`;
- zgodność decyzji A/D/N z regułami i powodami odrzutu;
- spójność liczników wniosków, zaakceptowanych kredytów i P&L;
- pełne przetworzenie wszystkich wymaganych miesięcy i poprawne zakończenie logu.

Wyniki dobierane przez wielokrotne optymalizacje w tych samych latach mogą być nadmiernie dopasowane. Oddzielamy próbę do strojenia od końcowej oceny na ile pozwala mechanika symulatora; ograniczenia oceny raportujemy wprost.

## Zadanie 1: monitoring modeli i strategii

Tworzymy wspólny raport porównawczy co najmniej dwóch, docelowo czterech modeli: rozkład punktów, brakujące wartości, Gini w czasie, PSI, kalibracja i zestawienie wariantów. Własny kod, przykład wywołania i testy powinny być wyodrębnione od biblioteki źródłowej. Raport ma służyć zarówno do ewaluacji, jak i do obrony.

## Warunki przyjęcia finalnego wariantu

- powtarzalna pełna symulacja z jednoznaczną wersją kodu;
- zachowane ograniczenia formalne i brak przecieku informacji z przyszłości;
- zrozumiały związek między zmianą reguł, akceptacjami, ryzykiem i zyskiem;
- raport zysku rok po roku i osobno dla produktów;
- udokumentowane ograniczenia stabilności i generalizacji.

Gdy żadna wersja nie pobije benchmarku, pokazujemy najlepszą poprawnie zweryfikowaną strategię i przyczyny różnicy; nie zmieniamy raportów ręcznie.
