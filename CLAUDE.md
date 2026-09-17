# CLAUDE.md

Instrukcja dla Claude Code w tym repo (`zakupyolimp-art/cennik-www`).

## Co to jest

Publiczna, statyczna strona **https://zakupyolimp-art.github.io/cennik-www/** (GitHub Pages,
"Deploy from branch/root") — publiczny odpowiednik apki PWA `gastromall-cenniki-app`, ten sam
silnik JS (wyszukiwarka, koszyki, EmailJS, GA4), ale wszystkie dane zaszyte w jednym
samodzielnym `index.html` (bez backendu). To repo zawiera **tylko wynik buildu** — cały ten
plik jest generowany, nie edytuj go ręcznie w tym repo.

## Skąd się bierze `index.html`

Budowany w repo `cenniki-automatyzacja` (folder `Cenniki dla AI` lokalnie), skryptami w
`cennik-www-build/` (`import_dane.py` → `eksport_statyczny.py` → `zbuduj_statyczna.py`, źródło
danych: aktualny `Analiza_28_07_2026_aktualizacja_N.xlsm`). Automatycznie budowany i publikowany
przez `.github/workflows/aktualizacja-apki.yml` w repo `cenniki-automatyzacja`, w tym samym
momencie co regeneracja apki PWA (PN 15:00/WT 14:00 Warszawa, po komplecie cenników tygodnia).
Do ręcznej regeneracji: patrz `cenniki-automatyzacja/CLAUDE.md`, sekcja `cennik-www-build/`.

**Nigdy nie edytuj `index.html` w tym repo bezpośrednio** — zmiana zniknie przy najbliższej
automatycznej regeneracji. Zmiany w logice/wyglądzie strony robi się w
`cenniki-automatyzacja/cennik-www-build/static.html` (szablon z placeholderem
`__DANE_JSON__`), potem rebuduje i publikuje tutaj.

## GA4 + EmailJS

Ta sama właściwość GA4 (`G-4G62VNT284`) i to samo konto EmailJS (`service_gqdifwj`) co apka PWA
`gastromall-cenniki-app` — zdarzenia z obu narzędzi widoczne razem w jednym panelu GA,
rozróżnialne po `page_location`. Identyczne szablony EmailJS: `template_2tli8yz` ("Zgłoś
problem"), `template_vu215cg` (wysyłka zamówień Kuchni Centralnej, temat "ZAMÓWIENIE
RESTAURACJA ..."). Szczegóły pełnego mechanizmu (identity capture, auto-wypełnianie, koszyk
czyszczony po Wyślij) — patrz `gastromall-cenniki-app/CLAUDE.md`, sekcja "Analytics (GA4) +
EmailJS", ten sam kod/te same zasady, tylko osobny plik.

## Trigger: `LAST` — synchronizacja między sesjami/urządzeniami

Patrz `cenniki-automatyzacja/CLAUDE.md` i `gastromall-cenniki-app/CLAUDE.md` — ten sam
mechanizm: `git fetch origin` + `git log --oneline HEAD..origin/main` + `git status -sb`
w tym repo (i, jeśli dostępne, w obu pozostałych) przy każdym `LAST`.

## Powiązane repo

- `cenniki-automatyzacja` (prywatne) — źródłowe cenniki dostawców, skoroszyt Excel, skrypty
  budujące tę stronę (`cennik-www-build/`).
- `gastromall-cenniki-app` — apka PWA, siostrzany projekt z tym samym silnikiem JS.
