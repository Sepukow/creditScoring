# 1. Start projektu — pierwsze spotkanie i podział pracy

**Pierwsze spotkanie: środa, 18 listopada 2026 r. (dziś). Godzinę ustalamy między sobą.**  
**Efekt spotkania:** każdy wie, nad czym pracuje, co musi uruchomić i co ma pokazać na kolejnym spotkaniu.

[← Wróć do README](../../README.md) · [Następny etap: uruchomienie symulatora →](02_SYMULATOR.md)

## Pojęcia, które trzeba rozumieć

**Repozytorium Git** — wspólny folder z historią zmian. Gdy ktoś poprawia kod, możemy sprawdzić, co zmienił i cofnąć błędną wersję. Samo wrzucenie plików do Git nie uruchamia modeli.

**Gałąź (branch)** — prywatna linia pracy w obrębie repozytorium. Każdy może rozwijać swoje rozwiązanie bez nadpisywania innych.

**Pull Request (PR)** — propozycja połączenia kodu z główną wersją. Przydaje się głównie do sprawdzenia, czy ktoś przypadkiem nie zmienił nazwy zmiennej albo struktury wyjścia używanej przez innych.

**Issue** — miejsce na zapisanie konkretnego problemu lub zadania. Nie jest celem samym w sobie. Jeśli coś wykonujemy, zostawiamy link do kodu lub wyniku, nie tylko zaznaczoną kratkę.

**SAS Viya Workbench** — środowisko, w którym uruchamiamy kod SAS i Python opisany w materiałach. **ASBSAS** i **ASBPYTHON** pomagają budować modele; **PROCESS_SIMULATION** symuluje decyzje kredytowe i liczy rezultat biznesowy.

**Punkt odniesienia (baseline)** — znany wynik, do którego porównujemy własną pracę. W materiałach: **663 327 PLN** za lata 1975–1987. To wynik przykładowej strategii, a nie rezultat naszej pracy.

## Co robimy dziś — agenda spotkania (około 45–60 min)

1. **10 min — rozumiemy zadanie.** Przechodzimy przez diagram z README: dane → cztery modele → decyzja → symulacja → zysk. Każdy ma umieć powiedzieć, co przewiduje jego model.
2. **10 min — przydzielamy pięć części.** Cztery osoby biorą po jednym modelu: PD Ins, PD Css, PD Css Cross, PR Css Cross. Piąta prowadzi silnik i symulację. Właściciel PR przygotowuje dodatkowo ulepszenie kodu, wspierany przez pozostałych. Ostateczny podział zapisujemy w [rolach](../ROLE_I_HARMONOGRAM.md).
3. **10 min — sprawdzamy dostęp.** Każdy otwiera repo, źródłowe materiały i potwierdza, czy posiada dostęp do Workbench. Brak dostępu zapisujemy od razu z nazwą konkretnej blokady.
4. **10 min — planujemy uruchomienie benchmarku.** Wskazujemy dwie osoby, które wspólnie przejdą [etap 2](02_SYMULATOR.md) i pokażą wynik oraz log.
5. **5–15 min — umawiamy sposób współpracy.** Ustalamy termin następnej wspólnej sesji technicznej, sposób zgłaszania błędów i miejsce trzymania dużych danych.

## Co każdy robi zaraz po spotkaniu

- Otwiera [Repozytorium ASBSAS](https://github.com/prozenba/ASBSAS), [ASBPYTHON](https://github.com/prozenba/ASBPYTHON) i [PROCESS_SIMULATION](https://github.com/prozenba/PROCESS_SIMULATION).
- Czyta fragment instrukcji Workbench z materiałów zajęć, potwierdza uruchomienie SAS i Pythona.
- Zagląda do dokumentacji własnego modelu w [etapie 4](04_MODELE.md), żeby zrozumieć jego target.
- Spisuje dwa pytania: czego nie rozumiem w danych i czego nie rozumiem w wyniku modelu.
- Osoby od symulatora przygotowują pierwsze odtworzenie wyniku referencyjnego — **nie modyfikują jeszcze progów ani modeli**.

## Przykład praktyczny

Ktoś bierze model PD Ins, ale jeszcze nie wie, jak policzyć ryzyko. Nie musi na pierwszym spotkaniu znać wszystkich procedur SAS. Musi rozumieć: „dla klienta składającego wniosek ratalny chcę oszacować ryzyko zdarzenia `default12=1`, a mój wynik będzie wejściem do decyzji banku”. Dopiero potem sprawdza definicję targetu i dane.

## Kiedy ten etap jest skończony?

Mamy ustalonych pięciu właścicieli, miejsce na kod, działający kontakt w zespole i jasno wskazaną osobę uruchamiającą benchmark. Do **22 listopada** zamykamy pozostałe sprawy organizacyjne.

[Zadania organizacyjne w GitHub](../../issues/1) są tylko listą kontrolną; najważniejsze są powyższe rezultaty.
