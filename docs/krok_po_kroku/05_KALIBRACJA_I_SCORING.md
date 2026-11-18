# 5. Kalibracja i scoring — od punktów do PD/PR w SAS

**Termin roboczy: do 3 stycznia 2027 r.**  
**Efekt:** każdy z czterech modeli daje wynik, którego silnik może bezpiecznie użyć.

[← README](../../README.md) · [← Modele](04_MODELE.md) · [Dalej: silnik i symulacja →](06_SILNIK_I_SYMULACJA.md)

## Pojęcia i po co są

**Score** — suma punktów przyznanych klientowi przez kartę skoringową. Sam score nie mówi automatycznie „ryzyko 7%”, dopóki nie wiemy, jak przeliczyć skalę.

**PD (Probability of Default)** — oszacowane prawdopodobieństwo zdarzenia default w określonym horyzoncie. PD 0,08 oznacza oszacowane 8%, a nie gwarancję, że dokładnie 8 na 100 podobnych klientów nie spłaci kredytu.

**PR (Probability of Response)** — oszacowana szansa odpowiedzi na ofertę. Nie mylimy jej z PD: wysoka skłonność do odpowiedzi może wystąpić u klienta o wysokim ryzyku kredytowym.

**Kalibracja** — dopasowanie liczby punktów do prawdopodobieństwa zdarzenia. Przykładowo dwa modele mogą mieć podobne Gini, lecz jeden przewiduje średnio 4% ryzyka tam, gdzie obserwujemy 8%. W silniku ich progów nie można traktować identycznie.

**Funkcja logistyczna** — przykładowe przekształcenie: `p = 1 / (1 + exp(-(alpha + beta * score)))`. Współczynniki `alpha` i `beta` szacujemy lub uzasadniamy dla konkretnej próby; nie kopiujemy ich automatycznie z przykładów.

**Scoring code** — kod obliczający punkty dla nowego wniosku. W naszych materiałach może mieć postać `scoring_code.sas`, wywoływaną z silnika.

**Test zgodności / parity** — sprawdzenie, czy model źródłowy i kod wdrożony w SAS zwracają te same punkty oraz odpowiednio zbliżone PD/PR dla tych samych rekordów.

**Próg (cut-off)** — liczba, według której decydujemy o odrzuceniu/akceptacji, np. `PD > 0.08`. Wartość jest elementem strategii, a nie uniwersalnym progiem ryzyka.

## Co robimy z każdym modelem

1. Otwieramy zatwierdzoną kartę modelu z [etapu 4](04_MODELE.md). Ustalamy, czy większy score oznacza większe, czy mniejsze ryzyko (dla PR — większą czy mniejszą skłonność do odpowiedzi).
2. Dopasowujemy albo weryfikujemy funkcję score→PD/PR na prawidłowej próbie. Porównujemy średnią prognozę z faktyczną częstością zdarzenia w grupach score; zapisujemy współczynniki i ograniczenia.
3. Generujemy wersję `scoring_code.sas` oraz dokumentujemy, jak ją wywołać. Sprawdzamy zgodność nazw cech, typów i tego, czy model wymaga obsługi specjalnych braków SAS.
4. Testujemy każdą granicę binów: wartość dokładnie na granicy, minimalnie niżej i minimalnie wyżej, osobno missing i kategorię nieznaną.
5. Na wspólnych rekordach porównujemy score z narzędzia modelowania oraz score SAS. Punkty powinny być zgodne dokładnie; dla PD/PR ustalamy tolerancję liczbową i zapisujemy ją w teście.
6. Sprawdzamy, czy PD/PR należą do przedziału [0,1], czy nie ma nieplanowanych missing i czy wynik nie zmienia liczby wniosków.
7. Przekazujemy integratorowi nazwę pliku, nazwę pola wyjściowego, wersję współczynników i przykład testu.

## Scenariusz: scoring ma dobre metryki, ale błędnie obsługuje missing

Wyobraźmy sobie pole z kartą punktową, w której wartość poniżej 0,5 dostaje 63 punkty, a brak danych 81. W SAS brak liczb jest porządkowany poniżej zwykłych wartości liczbowych. Jeśli sprawdzimy `x < 0.5` **przed** `missing(x)`, brak może dostać 63 zamiast 81.

Przykład warunków w bezpieczniejszej kolejności:

```sas
select;
  when (missing(x)) score_x = 81;
  when (x < 0.5) score_x = 63;
  otherwise score_x = ...; /* odpowiednie pozostałe grupy z karty */
end;
```

To przykład testowania kolejności, nie gotowa definicja score dla naszej zmiennej. Punktacja i granice muszą zgadzać się z konkretnym raportem modelu.

## Co mamy na końcu?

Cztery osobne kody SAS, cztery zestawy współczynników kalibracji, testy granic i missing oraz krótki raport zgodności z modelami źródłowymi. **Dopóki wynik wdrożenia nie zgadza się z modelem, nie podłączamy go do silnika.** [Zadania pomocnicze](../../issues/5).
