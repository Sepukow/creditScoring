# 6. Łączymy wszystko w silnik i uruchamiamy symulację

**Termin roboczy: do 10 stycznia 2027 r.**  
**Efekt:** cztery nasze modele działają wspólnie i generują pełny wynik finansowy.

[← README](../../README.md) · [← Scoring](05_KALIBRACJA_I_SCORING.md) · [Dalej: optymalizacja →](07_OPTYMALIZACJA.md)

## Pojęcia

**Silnik decyzyjny (decision engine)** — kod, który otrzymuje wyniki modeli i według reguł podejmuje decyzję. Sam nie musi być modelem ML.

**`%scoring_engine(wej,wyj)`** — makro SAS w przykładowym repozytorium. Parametr `wej` wskazuje tabelę wejściową, `wyj` wyjściową; w środku liczymy scoring i decyzje.

**`pd`, `cross_pd`, `pr`** — finalne pola zwracane do procesu: odpowiednio PD dla ocenianego produktu, PD przyszłego produktu cross-sell i PR odpowiedzi.

**Reguła akceptacji** — np. jeśli `pd` przekracza uzasadniony próg, decyzja może być `D`. Granice podane w repozytorium są przykładowe, nie automatycznie najlepsze dla naszych modeli.

**`decline_reason`** — kod przyczyny decyzji. Pomaga wyjaśnić, skąd biorą się odrzucenia i różnice w P&L.

**`998 not active customer`** — istniejąca zasada niedostępności gotówkowego dla nieaktywnego klienta. **Nie wolno jej modyfikować**.

**Dynamiczna symulacja** — po podjęciu decyzji aktualizujemy historię zaakceptowanych produktów, a następne miesiące korzystają z tej historii. Dlatego zmiana progu może wpłynąć także na późniejsze wnioski.

**P&L** — Profit & Loss, wynik finansowy. W porównaniach używamy faktycznie symulowanego zysku zaakceptowanych kredytów; hipotetyczne zyski odrzuconych nie są przychodem banku.

## Jak łączymy kod

1. Kopiujemy zatwierdzone modele i kalibracje z [etapu 5](05_KALIBRACJA_I_SCORING.md) do odizolowanej wersji procesu. Każdy właściciel potwierdza, który plik jest aktualny.
2. Otwieramy [`process/codes/decision_engine.sas`](https://github.com/prozenba/PROCESS_SIMULATION/blob/main/process/codes/decision_engine.sas), sprawdzamy kontrakt `%scoring_engine(wej,wyj)` i kolejność czterech scoringów.
3. Testujemy na kilku kontrolnych wnioskach osobno: ratalnym, gotówkowym klienta aktywnego i gotówkowym klienta nieaktywnego. Zapisujemy `pd`, `cross_pd`, `pr`, `decision` i `decline_reason`.
4. Weryfikujemy, że reguła `998 not active customer` działa bez zmian, a wartości PD i PR trafiają do właściwych zmiennych.
5. Na start wykorzystujemy **referencyjną strukturę decyzji**; progi dopasujemy w następnym etapie. Nie zmieniamy naraz modelowania i wszystkich warunków, bo nie ustalimy przyczyny różnicy.
6. Uruchamiamy pełen proces przez [`codes/all_contents.sas`](https://github.com/prozenba/PROCESS_SIMULATION/blob/main/codes/all_contents.sas) w osobnym katalogu. Zapisujemy log i raport HTML.
7. Porównujemy wynik 1975–1987: `ins`, `css`, lata, liczbę akceptacji, odrzucenia i stopę zdarzeń. Wykonujemy drugi identyczny przebieg.

## Scenariusz: po wymianie modeli zysk gwałtownie spada

Nie poprawiamy od razu wszystkich progów. Najpierw sprawdzamy, czy dla INS użyliśmy PD Ins, dla CSS PD Css, czy PR nie został pomylony z PD, czy współczynniki kalibracji odpowiadają nowemu score, czy silnik nie przyjmuje wszystkich przez zakomentowane warunki i czy raport liczy wynik właściwej populacji.

Jeśli testy przechodzą, spadek może być rzeczywistym skutkiem słabszych modeli lub niedopasowanej strategii. Dopiero wtedy przechodzimy do [optymalizacji](07_OPTYMALIZACJA.md).

## Wynik końcowy

Działający `decision_engine.sas`, cztery scoringi, komplet raportów i logów z jednej wersji, poprawne liczniki oraz wynik, który da się odtworzyć. [Powiązane zadania](../../issues/6).
