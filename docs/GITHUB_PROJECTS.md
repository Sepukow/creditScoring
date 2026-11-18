# Konfiguracja GitHub Projects

## Cel

Używamy GitHub Projects jako wspólnej tablicy pracy, a Issues jako źródła szczegółowych zadań. Na start mamy osiem zadań nadrzędnych F0–F7 oraz 35 zadań operacyjnych. Wszystkie pozostają powiązane z repozytorium.

## 1. Tworzymy projekt

1. Otwieramy [Projects na profilu Sepukow](https://github.com/users/Sepukow/projects).
2. Wybieramy **New project** → **Board** (lub Table z późniejszym widokiem Board).
3. Nazwa: **Credit Scoring · SGH 2026/27**.
4. Wybieramy **Import items from repository** i wskazujemy `Sepukow/creditScoring`. Importujemy istniejące Issues, w tym zadania F0–F7. Jeśli import nie jest dostępny podczas tworzenia, w projekcie wybieramy **Add item / Add from repository** i zaznaczamy zadania.
5. Dodajemy projekt do zakładki Projects repozytorium, aby każdy uczestnik mógł do niego łatwo przejść.

**Ważne:** automatyczne dodawanie nowo tworzonych Issues nie importuje wcześniej istniejących zadań. Obecny backlog trzeba zaimportować osobno.

## 2. Pola projektu

| Pole | Typ | Wartości / sposób użycia |
|---|---|---|
| **Status** | Single select | Backlog, Ready, In progress, Review, Blocked, Done |
| **Faza** | Single select | F0, F1, F2, F3, F4, F5, F6, F7 |
| **Rola** | Single select | A, B, C, D, E; na czas kickoffu |
| **Reviewer** | Text lub użytkownik w opisie Issue | A, B, C, D, E |
| **Target date** | Date | terminy z Issues |
| **Priority** | Single select | Critical, High, Normal, Optional |

Po ustaleniu składu zespołu używamy natywnego pola **Assignees** i przypisujemy rzeczywiste konta GitHub. Litery A–E pozostają w dokumentacji ról.

## 3. Widoki

**Board / Kanban** — grupowanie według Status, przenoszenie zadań między Backlog, Ready, In progress, Review, Blocked i Done.

**Table / Backlog** — wszystkie Issues, filtrowanie po fazie, roli i terminie. Zadania nadrzędne F0–F7 mogą stanowić osobny widok.

**Roadmap / Harmonogram** — porównanie wewnętrznych terminów poszczególnych faz i zadań. Korzystamy z pola daty, nie wyłącznie z dat zapisanych w treści Issue.

**My work** — filtr Assignee = @me po przypisaniu kont uczestników.

## 4. Automatyzacje

W menu projektu otwieramy **Workflows**:
- przy zamknięciu Issue zmieniamy Status na Done;
- dla nowych Issues w repo konfigurujemy **Auto-add to project** (filtr `is:issue`), jeśli limit planu na to pozwala;
- dla nowych elementów ustawiamy Backlog lub Todo;
- nie zamykamy Issues automatycznie tylko dlatego, że ktoś ręcznie przesunął kartę — kryteria odbioru pozostają rozstrzygające.

## 5. Jak prowadzimy dzień pracy

1. Otwieramy Board i wybieramy zadanie w statusie Ready.
2. Sprawdzamy zależności, właściciela, reviewer i warunki odbioru.
3. Rozpoczynając pracę, zmieniamy status na In progress i przypisujemy konto.
4. Postępy, wyniki, decyzje i blokady zapisujemy w odpowiednim Issue.
5. Po wykonaniu zadania otwieramy PR z odniesieniem do Issue.
6. Po review i potwierdzeniu kryteriów odbioru zamykamy Issue; projekt oznacza je jako Done.

Nie tworzymy osobnych kopii tych samych zadań w innym systemie. Źródłem prawdy dla postępu są Issues i powiązany Projects.

## Materiały

- [GitHub — tworzenie projektu](https://docs.github.com/en/issues/planning-and-tracking-with-projects/creating-projects/creating-a-project)
- [GitHub — dodawanie istniejących Issues](https://docs.github.com/en/issues/planning-and-tracking-with-projects/managing-items-in-your-project/adding-items-to-your-project)
- [GitHub — automatyzacje](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-built-in-automations)
- [GitHub — auto-add nowych Issues](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/adding-items-automatically)
