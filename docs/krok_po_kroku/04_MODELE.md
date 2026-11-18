# 4. Budujemy cztery modele — od zmiennych do karty skoringowej

**Termin roboczy: do 20 grudnia 2026 r.**  
**Efekt:** cztery działające modele v1 z kodem, metrykami, interpretacją i pierwszym scoringiem.

[← README](../../README.md) · [← Dane](03_DANE_I_TARGETY.md) · [Dalej: scoring i kalibracja →](05_KALIBRACJA_I_SCORING.md)

## Najpierw: co przewiduje każdy model?

| Model | Co oznacza wynik? | Co robi z nim bank? |
|---|---|---|
| **PD Ins** | ryzyko `default12=1` dla kredytu ratalnego | wybiera, czy zaakceptować ratalny |
| **PD Css** | ryzyko `default12=1` dla kredytu gotówkowego | ocenia ryzyko gotówkowego |
| **PD Css Cross** | ryzyko `default_cross12=1` związane z przyszłym kredytem gotówkowym | uwzględnia ryzyko przyszłej sprzedaży |
| **PR Css Cross** | szansa `cross_response=1` | ocenia możliwość skorzystania z dodatkowej oferty |

Obowiązują kontrakty danych z [etapu 3](03_DANE_I_TARGETY.md). Ten sam przepis modelowania nie oznacza jednej wspólnej próby dla wszystkich czterech problemów.

## Pojęcia

**EDA (Exploratory Data Analysis)** — oglądamy rozkłady cech, brakujące dane, podejrzane wartości i ich relację z targetem. Na przykład szukamy, czy bardzo wysokie zadłużenie lub opóźnienia występują częściej w grupie default.

**Binning** — zamieniamy liczby na przedziały, np. niskie, średnie i wysokie obciążenie. Granice powinny mieć sens i wystarczającą liczebność, a nie tylko poprawiać wynik na train.

**WoE (Weight of Evidence)** — sposób zakodowania przedziału na podstawie udziału dobrych i złych obserwacji. Pozwala budować interpretowalną kartę skoringową. Znak WoE zależy od przyjętej konwencji — kontrolujemy go w kodzie.

**IV (Information Value)** — podsumowanie tego, jak mocno cecha różnicuje grupy targetu. Wysokie IV nie oznacza automatycznie, że cecha jest bezpieczna, stabilna lub dostępna na czas.

**Regresja logistyczna** — podstawowy model przewidujący prawdopodobieństwo zdarzenia na podstawie cech. W tym projekcie daje łatwą do opisania bazę dla scorecardy.

**Scorecard / karta skoringowa** — tabela punktów: cecha + przedział → punkty. Suma punktów daje score klienta, który następnie kalibrujemy do PD lub PR.

**AUC i Gini** — mówią, jak model rozdziela obserwacje ze zdarzeniem i bez zdarzenia. `Gini = 2 × AUC – 1` dla standardowej definicji i poprawnego kierunku score. Nie mierzą same w sobie zysku ani dokładności PD.

**KS** — największa różnica między rozkładami score dwóch klas; kolejna miara separacji.

**PSI** — sygnał, czy rozkład score lub cechy zmienił się między okresami. Nie traktujemy go jako automatycznego testu poprawności modelu.

**VIF / współliniowość** — miara tego, jak silnie predyktory dublują informacje; bardzo podobne zmienne mogą utrudnić interpretację współczynników.

**Walidacja czasowa** — uczymy na wcześniejszych okresach i sprawdzamy na późniejszych, jeśli pozwala na to dojrzałość targetu. Chroni przed częścią pułapek losowego podziału historii.

## Pracujemy według jednego przepisu

1. Ładujemy **wyłącznie próbę zatwierdzoną w etapie 3**, nie cały plik na chybił trafił.
2. Pokazujemy liczność i event rate w train, validation i test; analizujemy missing.
3. Wybieramy sensowne cechy, przygotowujemy biny i WoE/IV; dla każdej cechy notujemy, dlaczego została lub nie została użyta.
4. Budujemy najpierw prostą regresję logistyczną, potem porównujemy warianty. Nie rozpoczynamy od bardziej skomplikowanego algorytmu bez punktu odniesienia.
5. Liczymy Gini/AUC, KS, diagnostykę stabilności, zachowanie w czasie i kalibrację. Sprawdzamy kierunki współczynników oraz współliniowość.
6. Eksportujemy kartę punktową i wstępny kod SAS. Przed wdrożeniem sprawdzamy, czy skoring ma te same przedziały i punkty co raport.
7. Spisujemy wnioski: co działa, co jest niestabilne, których zmiennych nie ufamy, jak zmiana polityki akceptacji może wpłynąć na model.

## Przykład: dobry Gini, zły model

Załóżmy ilustracyjnie, że model osiąga Gini 0,70, ale wykorzystuje cechę opartą na danych z okresu po udzieleniu ocenianego kredytu. Taki wynik jest nieważny: klient w chwili złożenia wniosku nie mógłby mieć tej informacji. Usuwamy przeciekającą cechę, trenujemy ponownie i dopiero wtedy podajemy wynik.

Drugi przypadek: model dobrze odróżnia ryzykownych klientów, ale jego szacowane PD są systematycznie zawyżone. Dlatego oprócz Gini będziemy oceniać kalibrację w [etapie 5](05_KALIBRACJA_I_SCORING.md).

## Z jakich plików korzystamy?

- [ASBSAS](https://github.com/prozenba/ASBSAS) — SAS i przykłady budowania scorecard.
- [ASBPYTHON](https://github.com/prozenba/ASBPYTHON) — notebook modelowania i raportowania.
- `materials_all/example_of_project_documentation/example_of_model_SAS/` i `example_of_model_PYTHON/` — przykłady raportów PD Ins.
- `materials_all/codes_tips.pdf` — podpowiedzi dotyczące targetów, binningu i selekcji.

## Co powinno działać na koniec?

Każdy z nas ma kod treningu, wybrany model, raport metryk, kartę skoringową, wstępną implementację SAS i opis ograniczeń. **Cztery modele to cztery osobne wyniki, ale jeden standard opisu.** [Powiązane zadania](https://github.com/Sepukow/creditScoring/issues/4).
