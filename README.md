# Credit Scoring — od danych do decyzji kredytowej

**Projekt na SGH, semestr zimowy 2026/2027 · pięć osób · termin oddania 25 stycznia 2027 r.**

Budujemy proces, który na podstawie danych o klientach ocenia ryzyko kredytu ratalnego i gotówkowego, przewiduje odpowiedź na dodatkową ofertę, podejmuje decyzje i symuluje wynik finansowy. Punktem odniesienia jest **663 327 PLN zysku** w symulacji obejmującej lata 1975–1987. Naszym zadaniem jest poprawić ten wynik i wykazać, skąd bierze się różnica.

Nie zaczynamy od trenowania czterech modeli na chybił trafił. Najpierw uruchamiamy symulator i rozumiemy dane. Dopiero później budujemy modele, integrujemy je i sprawdzamy, czy zmiany rzeczywiście poprawiają wynik.

> **Pierwsze spotkanie zespołu: środa, 18 listopada 2026 r.**  
> [Agenda, przygotowanie i decyzje na dziś](docs/krok_po_kroku/01_START.md). Godzinę ustalamy między sobą. Po spotkaniu każda osoba powinna znać swoją część oraz następne konkretne działanie.

## Jak wygląda nasz projekt?

```text
Dane historyczne / ABT
        │
        ├── PD Ins ────────── ryzyko kredytu ratalnego
        ├── PD Css ────────── ryzyko kredytu gotówkowego
        ├── PD Css Cross ──── ryzyko przyszłego gotówkowego
        └── PR Css Cross ──── szansa odpowiedzi na ofertę
        │
        ▼
Punkty modeli → kalibracja do PD / PR
        │
        ▼
Reguły akceptacji → decyzja A / D / N
        │
        ▼
Symulacja 1975–1987 → nowe historie klientów
        │
        ▼
Zysk, straty, stabilność i dokumentacja
```

To **proces dynamiczny**: decyzja z jednego miesiąca zmienia historię klienta i może wpłynąć na następne miesiące. Dlatego sam wysoki wynik Gini nie oznacza, że bank zarobi więcej.

## Instrukcja krok po kroku

**Każdy etap ma oddzielną instrukcję:** wyjaśnienia pojęć, przykłady z danych kredytowych, kolejność działań, typowe błędy i konkretny wynik, który mamy uzyskać. Czytamy je po kolei.

| Etap | Co konkretnie robimy? | Co mamy na końcu? | Termin |
|---|---|---|---|
| **[1. Start i pierwsze spotkanie](docs/krok_po_kroku/01_START.md)** | Rozumiemy cel, dzielimy pięć części, sprawdzamy dostęp | Każdy wie, co robi; ustalony właściciel symulatora | **18 XI — dziś** |
| **[2. Uruchomienie symulatora](docs/krok_po_kroku/02_SYMULATOR.md)** | Klonujemy kod, sprawdzamy środowisko i odtwarzamy wynik | Log, HTML i zweryfikowany benchmark 663 327 PLN | 29 XI |
| **[3. Dane i targety](docs/krok_po_kroku/03_DANE_I_TARGETY.md)** | Sprawdzamy ABT, produkty, klientów, missing, default i cross-sell | Cztery poprawnie opisane próby do trenowania | 6 XII |
| **[4. Cztery modele](docs/krok_po_kroku/04_MODELE.md)** | EDA, binning, WoE, regresja, Gini, stabilność, raport | Cztery pierwsze modele z kodem i wynikami | 20 XII |
| **[5. Scoring i kalibracja](docs/krok_po_kroku/05_KALIBRACJA_I_SCORING.md)** | Przeliczamy punkty na PD/PR, testujemy SAS | Cztery poprawnie działające scoringi | 3 I |
| **[6. Silnik i pełna symulacja](docs/krok_po_kroku/06_SILNIK_I_SYMULACJA.md)** | Łączymy modele, uruchamiamy reguły i liczymy P&L | Pierwszy wynik własnego procesu | 10 I |
| **[7. Poprawianie strategii](docs/krok_po_kroku/07_OPTYMALIZACJA.md)** | Porównujemy progi, cross-sell, zyski i ryzyko | Wybrana, odtwarzalna strategia | 17 I |
| **[8. Raporty i obrona](docs/krok_po_kroku/08_ODDANIE.md)** | Uzupełniamy dokumentację, testujemy paczkę i bronimy wybory | Komplet plików, próbna obrona, wysyłka | 25 I |

**Terminy powyższych etapów są naszym planem wewnętrznym.** Jedyny formalny termin oddania w aktualnym harmonogramie to **25 stycznia 2027 r.**

## Słownik na start — bez konieczności znania SAS

| Pojęcie | Proste wyjaśnienie |
|---|---|
| **ABT** | Tabela wniosków i cech klientów, z której korzystamy przy modelowaniu i decyzjach. |
| **Feature / cecha** | Informacja znana w chwili wniosku, np. wcześniejsza historia spłat. |
| **Target** | Zdarzenie, które przewidujemy, np. `default12=1`. |
| **PD** | Oszacowana szansa defaultu, czyli zdarzenia niewykonania zobowiązania zgodnie z definicją danych. |
| **PR** | Oszacowana szansa skorzystania z dodatkowej oferty kredytu. |
| **Scorecard** | Punkty przyznawane za cechy i ich przedziały; wynik przekładamy na PD/PR. |
| **Kalibracja** | Sprawdzenie, czy liczba punktów przekłada się na wiarygodne prawdopodobieństwo. |
| **Cut-off** | Granica prawdopodobieństwa wykorzystywana przy decyzji kredytowej. |
| **Gini** | Miara rozdzielania klientów ze zdarzeniem i bez niego. Nie mierzy zysku. |
| **Cross-sell** | Proponowanie klientowi dodatkowego kredytu gotówkowego. |
| **Reject inference** | Problem braku pełnej historii kredytowej klientów, którym nie przyznano kredytu. |
| **Leakage** | Błąd: model korzysta z informacji znanej dopiero po decyzji, którą miał przewidzieć. |
| **P&L** | Wynik finansowy: przychody i koszty/straty z kredytów w symulacji. |

Więcej wyjaśnień wraz z przykładami znajduje się w poszczególnych etapach.

## Kto za co odpowiada?

Na pierwszym spotkaniu przypiszemy pięć części do konkretnych osób:

| Część | Główne zadanie |
|---|---|
| **A — PD Ins** | model ryzyka kredytu ratalnego oraz pomoc przy uruchomieniu środowiska |
| **B — PD Css** | model ryzyka kredytu gotówkowego i wspólne testy jakości |
| **C — PD Css Cross** | model ryzyka cross-sell i kontrola poprawności targetów |
| **D — PR Css Cross** | model odpowiedzi na ofertę i dodatkowy kod raportowania |
| **E — Strategia** | połączenie modeli, silnik, eksperymenty i raport finansowy |

Każdy, kto przygotowuje model, odpowiada również za jego scoring, kalibrację, raport i testy. Wspólnie sprawdzamy, czy modele działają w silniku.

[Szczegóły podziału i harmonogramu](docs/ROLE_I_HARMONOGRAM.md).

## Trzy scenariusze, które musimy rozumieć

**1. Model ma świetne Gini, ale wykorzystuje przyszłość.** Przewiduje default za pomocą danych powstałych po udzieleniu kredytu. Wynik wygląda dobrze, lecz model jest bezużyteczny w chwili decyzji. Dlatego [najpierw sprawdzamy dane](docs/krok_po_kroku/03_DANE_I_TARGETY.md).

**2. Klient nie bierze oferty CSS.** Nie możemy automatycznie przypisać mu „braku defaultu” na kredycie, którego nie miał. Trzeba odróżnić zdarzenie niewystępujące od nieobserwowanego.

**3. Zmieniamy próg PD i rośnie zysk.** Nie ogłaszamy zwycięstwa po jednym przebiegu. Sprawdzamy, jak zmieniły się akceptacje, straty, wyniki w kolejnych latach i przyszła historia klientów.

## Materiały i środowisko

Pracujemy na materiałach z zajęć oraz repozytoriach:
- [ASBSAS](https://github.com/prozenba/ASBSAS) — modelowanie w SAS;
- [ASBPYTHON](https://github.com/prozenba/ASBPYTHON) — modelowanie i raportowanie w Pythonie;
- [PROCESS_SIMULATION](https://github.com/prozenba/PROCESS_SIMULATION) — dynamiczny silnik i raport zysku.

W instrukcjach etapów wskazujemy konkretne pliki, które trzeba otworzyć. Nie nadpisujemy wyników innych osób: główny skrypt SAS może czyścić katalogi robocze.

## Jak zapisujemy pracę?

Kod trzymamy w tym repozytorium. Gdy robimy większą zmianę, tworzymy osobną gałąź i Pull Request, żeby ktoś mógł sprawdzić wynik. Zadania techniczne i błędy są w [Issues](../../issues) — **to pomocnicza lista**, nie zastępuje instrukcji powyżej. [Krótkie zasady współpracy](docs/ZASADY_PRACY.md).

Nie zamykamy zadania tylko dlatego, że skrypt wykonał się bez komunikatu błędu. Musimy pokazać **co wyszło, dlaczego tak, gdzie jest wynik i jak to powtórzyć**.

## Dwie zasady, których nie łamiemy

1. **Nie zmieniamy reguły `998 not active customer`** — to ograniczenie z opisu projektu.
2. **Nie używamy przyszłych danych w decyzjach** — dzisiejszy model może znać wyłącznie to, co bank wiedział w chwili wniosku.

Szczegółowe wymagania z wcześniejszego opisu projektu potwierdzamy z prowadzącą dla edycji 2026/2027. Nie wstrzymuje to rozpoczęcia prac nad symulatorem i danymi.

---

**Zaczynamy od [pierwszego spotkania — 18.11.2026](docs/krok_po_kroku/01_START.md), a następnie od [uruchomienia referencyjnej symulacji](docs/krok_po_kroku/02_SYMULATOR.md).**
