# 3. Rozumiemy dane: ABT, default i cross-sell

**Termin roboczy: do 6 grudnia 2026 r.**  
**Efekt:** każdy model ma jednoznacznie opisane wejście i prawidłową etykietę do uczenia.

[← README](../../README.md) · [← Symulator](02_SYMULATOR.md) · [Dalej: cztery modele →](04_MODELE.md)

## Pojęcia na przykładach

**ABT (Analytical Base Table)** — jeden wiersz z informacjami o wniosku i historii klienta na konkretny moment. Zawiera np. dane z formularza i agregaty wcześniejszych rat. Nie zakładamy, że każdy wiersz dotyczy innego klienta.

**`cid`, `aid`, `period`** — odpowiednio identyfikator klienta, aplikacji i okres przetwarzania (`YYYYMM`). Jeden `cid` może pojawiać się wielokrotnie. Gdy ten sam klient trafia do train i test, wyniki mogą być zbyt optymistyczne.

**Cecha / feature** — kolumna, którą model dostaje przed decyzją, np. wnioskowana kwota. Nie może być nią informacja o faktycznym default w przyszłym roku.

**Target / etykieta** — zdarzenie, które model ma przewidzieć. W przypadku PD Ins i PD Css jest to `default12`, przy cross-sell pojawiają się `default_cross12` i `cross_response`. Musimy przeanalizować, jak te pola powstają w skryptach.

**Default** — zdarzenie niewykonania zobowiązania według definicji przyjętej w danych i kodzie zajęć. Nie dopowiadamy własnej definicji opóźnienia czy wartości długu bez sprawdzenia materiałów.

**Cross-sell** — propozycja dodatkowego kredytu gotówkowego klientowi, który ma już relację z bankiem; stąd potrzebujemy zarówno modelu ryzyka gotówkowego, jak i modelu odpowiedzi na ofertę.

**Brak informacji ≠ zero.** Gdy klient nie zaciągnął przyszłego kredytu, nie zawsze wiadomo, czy „nie miałby defaultu”. Brak obserwowanego zdarzenia musimy rozróżnić od faktycznie zaobserwowanego wyniku zero.

**Censoring / niedojrzała etykieta** — np. wniosek z grudnia 1987 r. nie musi mieć pełnych 12 miesięcy historii po wniosku w dostępnych danych. Nie wrzucamy go bez sprawdzenia do „dobrych klientów”.

**Leakage / przeciek informacji** — sytuacja, w której model przy ocenianiu wcześniejszego wniosku korzysta z informacji dostępnej dopiero później, np. tego, czy klient skorzystał z przyszłej oferty.

**Reject inference** — problem, że zachowanie kredytowe znamy zwykle dla zaakceptowanych, a nie dla odrzuconych klientów. Nowa strategia może wprowadzać inne grupy klientów niż te, na których model się uczył.

## Co robimy

1. Otwieramy `materials_all/abt.xlsx` — słownik zmiennych. Następnie sprawdzamy rzeczywiste kolumny ABT w symulatorze, bo słownik nie musi zawierać wszystkich pól technicznych i targetów.
2. Liczymy aplikacje, unikalnych klientów, podział na miesiące, produkty INS/CSS, decyzje A/D/N oraz częstość zdarzeń. Zapisujemy nie tylko procenty, ale także liczniki.
3. Dla każdego targetu odnajdujemy fragment kodu, który go tworzy. Zapisujemy warunki, okno obserwacji, znaczenie wartości `0`, `1` i brakujących, w tym specjalnych missing SAS.
4. Ustalamy populację każdego modelu — produkt, decyzja, okres, dojrzałość etykiet. Nie kopiujemy bezmyślnie warunku `decision='A'` między modelami o różnych targetach.
5. Analizujemy rodziny zmiennych `app_`, `act_`, `agr_`, `ags_`. Przy każdej wybranej cesze sprawdzamy, **kiedy** powstaje.
6. Ustalamy podział train/validation/test tak, żeby ocenić model także w późniejszych okresach. Rozważamy klienta z wieloma aplikacjami i sposób uniknięcia przecieku między próbami.
7. Zapisujemy osobny kontrakt dla PD Ins, PD Css, PD Css Cross i PR Css Cross. Bez zaakceptowanej definicji danych nie rozpoczynamy porównywania Gini.

## Scenariusz: klient nie bierze oferty gotówkowej

Dla przykładowego klienta bank rozważa wysłanie oferty CSS, ale klient jej nie przyjmuje. W modelu odpowiedzi może to być `cross_response=0`, **jeśli** według kodu oferta była rzeczywiście obserwowalna i odpowiedź została ustalona. Dla modelu defaultu przyszłego kredytu gotówkowego nie wolno automatycznie przepisać `default_cross12=0`: kredytu nie było, więc ryzyko spłaty tego konkretnego kredytu mogło pozostać nieobserwowane.

## Co oddajemy dalej

Dla każdego modelu krótka tabela: `Kto?` (populacja), `Kiedy?` (moment scoringu), `Co?` (target), `Skąd?` (kod tworzący etykietę), `Które rekordy wykluczamy?`, `Jak walidujemy?`. W załączniku do danych podajemy liczebności.

**Przechodzimy dalej, gdy te cztery tabele są sprawdzone.** [Szczegółowe zadania](https://github.com/Sepukow/creditScoring/issues/3).
