# 7. Poprawiamy wynik: eksperymenty ze strategią

**Termin roboczy: do 17 stycznia 2027 r.**  
**Efekt:** wybieramy strategię na podstawie powtarzalnych porównań, a nie zgadywania progów.

[← README](../../README.md) · [← Silnik](06_SILNIK_I_SYMULACJA.md) · [Dalej: oddanie →](08_ODDANIE.md)

## Pojęcia

**Hipoteza** — konkretne przypuszczenie, które chcemy sprawdzić: „zaostrzenie progu PD Css obniży straty bardziej, niż zmniejszy przychody z akceptacji”.

**Wariant kontrolny** — konfiguracja, wobec której mierzymy zmianę. Jeśli naraz zmienimy modele i reguły, nie wiemy, co wpłynęło na wynik.

**Cut-off** — próg PD albo PR wykorzystywany w regule. Nie jest stałą własnością modelu; zależy od celu biznesowego i kalibracji.

**`run_id`** — unikatowy identyfikator eksperymentu, np. `EXP-20270112-001`. Pozwala połączyć kod, ustawienia, log i raport, zamiast mieć dziesięć plików nazwanych „final2”.

**Guardrails / miary kontrolne** — oprócz całkowitego zysku sprawdzamy wynik dla każdego produktu i roku, liczbę akceptacji, odsetek default i kalibrację, żeby nie zyskać jednorazowo kosztem niestabilności.

**Overfitting strategii** — wielokrotny dobór progów pod te same historyczne lata może dać świetny wynik wyłącznie w tej konkretnej symulacji. Nie przedstawiamy takiego wyniku jako niezależnej walidacji poza próbą.

**Reject inference** — gdy zmieniamy politykę, zaczynamy akceptować klientów innych niż w historii treningowej. Ich zachowanie nie musi odpowiadać założeniom modeli zbudowanych na poprzedniej populacji.

## Testy, które wykonujemy po kolei

| Test | Zmieniamy | Po co? |
|---|---|---|
| **E0** | nic: strategia wzorcowa | punkt odniesienia 663 327 PLN |
| **E1** | cztery nowe modele przy wzorcowych regułach | wpływ samych modeli |
| **E2** | progi PD Ins i PD Css | wpływ polityki ryzyka |
| **E3** | reguły wykorzystujące PD Cross i PR | wpływ sprzedaży krzyżowej |
| **E4** | uzasadnione reguły dla segmentów | czy odmienne grupy potrzebują innych progów |
| **E5** | opcjonalnie ponowne trenowanie na nowej ABT | czy zmiana strategii pogorszyła jakość modeli |

Przy każdej próbie zapisujemy, co zmieniliśmy, z której wersji modeli korzystamy, jaki był wynik łączny i produktowy oraz gdzie są log i raport. Nie zmieniamy reguły `998 not active customer`.

## Prosty przykład hipotezy

Wzorcowa strategia ma próg PD dla produktu CSS. Przetestujmy kilka wariantów **wokół** tej wartości, zachowując pozostałe reguły i wersje modeli. Każdy przebieg powinien mieć własny `run_id`. Jeśli próg bardziej liberalny podnosi zysk, ale zwiększa liczbę defaultów, sprawdzamy, czy zysk utrzymuje się również w innych latach.

**Nie ma sensu z góry obiecywać konkretnej poprawy**. Zysk jest wynikiem symulacji, a nie samych wartości Gini.

## Jak wybieramy wersję końcową?

Otwieramy zestawienie przebiegów i odrzucamy te, których nie możemy odtworzyć. Porównujemy profit 1975–1987, rozkład lat, INS/CSS, akceptacje, bad rate i stabilność. Osobno zapisujemy, które lata posłużyły do doboru progów, a które do sprawdzenia wyników, jeśli mechanika procesu na to pozwala.

Jeżeli żaden wariant nie pobije 663 327 PLN, oddajemy najlepsze poprawnie działające rozwiązanie i uczciwą analizę przyczyn. Nie poprawiamy ręcznie wyników, żeby osiągnąć wymaganą liczbę.

**Kończymy etap, mając jedną wybraną i uzasadnioną wersję strategii, kompletny rejestr eksperymentów i raport.** [Zadania pomocnicze](https://github.com/Sepukow/creditScoring/issues/7).
