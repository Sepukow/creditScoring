# Strategia i symulator

Rozwijamy silnik decyzyjny, integrujemy cztery modele i uruchamiamy pełną symulację za lata 1975–1987. Wyniki liczymy zgodnie z [protokołem eksperymentów](../docs/EKSPERYMENTY_I_WALIDACJA.md).

Reguła `998 not active customer` pozostaje bez zmian. Nie korzystamy z przyszłych informacji przy podejmowaniu decyzji. Każdą konfigurację progów i modele identyfikujemy poprzez `run_id` oraz manifest wersji.

Nie uruchamiamy destrukcyjnych skryptów procesu na współdzielonych katalogach bez izolacji lub kopii.
