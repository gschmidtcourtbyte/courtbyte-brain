# Iteration 9: Highlightfarbe und Hellgruen-Archiv

## Ziel
Die bisherige Hellgruen-Farbe wird in `logo/farbcodes.md` als Reserve dokumentiert. Alle aktiven Button- und Akzentstellen, die bisher Hellgruen genutzt haben, wechseln auf die definierte Highlightfarbe `#D8633D`.

## Ausgangslage
- `web/static/css/style.css` nutzt aktuell `--accent: #d9e1a6` und `--accent-deep: #c3cc86`.
- Mehrere UI-Stellen verwenden direkte Hellgruen-RGBA-Werte wie `rgba(217,225,166,...)`.
- `logo/farbcodes.md` dokumentiert das Hellgruen bisher als aktiven Akzenttoken.
- Neue Produktvorgabe:
  - Altes Hellgruen bleibt fuer spaetere Nutzung dokumentiert.
  - Aktive Highlight-/CTA-Farbe ist `#D8633D`.

## Scope
- Farbdokumentation: `logo/farbcodes.md`
- Design Tokens und UI-Farbverwendungen: `web/static/css/style.css`
- Iterationsplanung und QA-Status: `plan/iterations/iteration-9.md`
- Kein HTML-, JS-, Backend- oder Asset-Umbau.

## Produktentscheidung
Die alte Hellgruen-Farbe wird als expliziter Reservewert abgelegt, nicht als aktiver Token. Die aktive Highlightfarbe wird ueber CSS-Variablen zentral gesteuert, damit Buttons, Badges, Statuspunkte, Fokuszustaende und Alerts konsistent umgestellt sind.

## Critical Path
1. Alte Hellgruen-Werte und neue Highlightfarbe in `farbcodes.md` eindeutig dokumentieren.
2. CSS-Tokens zentral auf `#D8633D` umstellen.
3. Direkte Hellgruen-RGBA-Werte durch Highlight-RGBA-Tokens ersetzen.
4. Syntax-, Such- und Containerchecks ausfuehren.
5. Iterationsstatus und Restrisiken dokumentieren.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Wave-Steuerung, Akzeptanzkriterien |
| documentation-eng | Farbcodes-Doku als Single Source of Truth aktualisieren |
| frontend-eng | Design Tokens, Button- und Akzentfarben konsistent umstellen |
| testing-eng | Syntax-, Such- und HTTP-Pruefungen |
| review-eng | Kontrast-/Regression-Risiken und Restbefunde bewerten |

## Task: 9.1 Farbcodes-Doku aktualisieren
**Priority**: P0
**Blocked by**: none
**Skillset**: documentation-eng

**Akzeptanzkriterien**:
- [x] `#d9e1a6` ist als ehemalige Hellgruen-/Reservefarbe dokumentiert.
- [x] `#c3cc86` ist als ehemaliger tieferer Hellgruen-Akzent dokumentiert.
- [x] `#D8633D` ist als aktive Highlightfarbe dokumentiert.
- [x] Die Token-Tabelle weist `--accent` auf `#D8633D`.

**Output**:
- Aktualisierte `logo/farbcodes.md`.

## Task: 9.2 CSS-Tokens umstellen
**Priority**: P0
**Blocked by**: 9.1
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] `--accent` nutzt `#D8633D`.
- [x] Hover-/Interaktionsfarbe ist passend zur Highlightfarbe definiert.
- [x] Button-Text nutzt eine lesbare Ink-Farbe fuer den neuen Akzent.
- [x] Direkte Token-Nutzung bleibt zentral wartbar.

**Output**:
- Aktualisierte `:root`-Tokens in `web/static/css/style.css`.

## Task: 9.3 Hellgruen-Verwendungen ersetzen
**Priority**: P0
**Blocked by**: 9.2
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Keine aktiven Hellgruen-RGBA-Werte bleiben in `web/static/css/style.css`.
- [x] Buttons nutzen die neue Highlightfarbe.
- [x] Badges, Statuspunkte, Fokuszustaende, Kartenakzente und Alerts nutzen Highlight-RGBA.
- [x] Kontaktformular-Inline-Akzent bleibt ueber `var(--accent)` automatisch auf Highlightfarbe.

**Output**:
- CSS-Aenderungen in `web/static/css/style.css`.

## Task: 9.4 QA & Abschluss
**Priority**: P1
**Blocked by**: 9.3
**Skillset**: testing-eng, review-eng, documentation-eng

**Akzeptanzkriterien**:
- [x] CSS-Brace-Check besteht.
- [x] JS-Syntaxcheck besteht, auch wenn JS nicht geaendert wurde.
- [x] Suche bestaetigt: alte Hellgruen-Werte sind nur noch in der Farbdoku/Logo-Referenzen vorhanden, nicht in aktivem CSS.
- [x] Container liefert Startseite und CSS erfolgreich aus.
- [x] QA-Grenzen sind dokumentiert.

**Output**:
- Aktualisierter QA-Status in dieser Datei.

## Execution Waves

### Wave 1 — Farb-Dokumentation
**Skillsets**: product-own, documentation-eng
**Tasks**: 9.1
**Ziel**: Alte Hellgruen-Farbe sichern und neue Highlightfarbe als aktiven Markenakzent festhalten.

### Wave 2 — Token Foundation
**Skillsets**: frontend-eng, review-eng
**Tasks**: 9.2
**Ziel**: CSS-Variablen so umstellen, dass alle aktiven Akzente zentral auf `#D8633D` basieren.

### Wave 3 — UI-Anwendung
**Skillsets**: frontend-eng
**Tasks**: 9.3
**Ziel**: Direkte Hellgruen-Verwendungen durch Highlight-Tokens ersetzen.

### Wave 4 — QA & Abschluss
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 9.4
**Ziel**: Syntax, Auslieferung und Suchbefunde pruefen und dokumentieren.

## Abhaengigkeits-Graph

```text
9.1 Farbdoku
  -> 9.2 CSS Tokens
  -> 9.3 UI Anwendung
  -> 9.4 QA/Doku
```

## Umsetzungsstatus

### Wave 1 — Farb-Dokumentation: done
- `logo/farbcodes.md` dokumentiert `#d9e1a6` und `#c3cc86` als ehemalige Hellgruen-/Reservewerte.
- `#D8633D` ist als aktive Highlightfarbe eingetragen.
- `#E1734F` ist als Hover-/Interaktionsfarbe und `#1A1714` als Textfarbe auf Highlight-Flaechen dokumentiert.

### Wave 2 — Token Foundation: done
- `web/static/css/style.css` nutzt zentral:
  - `--accent: #D8633D`
  - `--accent-rgb: 216, 99, 61`
  - `--accent-hover: #E1734F`
  - `--accent-ink: #1a1714`
- `.btn-primary` nutzt jetzt `--accent` und `--accent-ink`.
- Kontrastcheck fuer Button-Text:
  - `#1a1714` auf `#D8633D`: 4.91:1
  - `#1a1714` auf `#E1734F`: 5.75:1

### Wave 3 — UI-Anwendung: done
- Direkte Hellgruen-RGBA-Werte im aktiven CSS wurden durch `rgba(var(--accent-rgb),...)` ersetzt.
- Betroffen sind Status-Badge, GPS-Badge, Map-Pin, Pulse-Animation, Formular-Fokus und Success-Alert.
- Link- und Formular-Hover nutzen `--accent-hover`.

### Wave 4 — QA & Abschluss: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- `rg` bestaetigt: keine alten Hellgruen-Werte und kein `accent-deep` mehr in `web/static/css/style.css`.
- `rg` bestaetigt: alte Hellgruen-Werte sind in `logo/farbcodes.md` als Reserve dokumentiert.
- Docker-Container laufen; die Startseite liefert im Container HTTP 200.
- Container-Auslieferung von `style.css` zeigt die neuen Highlight-Tokens.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.

## Definition of Done
- [x] Alte Hellgruen-Farbe ist in `logo/farbcodes.md` gesichert.
- [x] Aktive Highlightfarbe `#D8633D` ist in `logo/farbcodes.md` dokumentiert.
- [x] Buttons und aktive Akzentstellen nutzen `#D8633D` bzw. daraus abgeleitete Tokens.
- [x] Aktives CSS enthaelt keine alte Hellgruen-Implementierung mehr.
- [x] QA-Status und Rest-Risiken sind dokumentiert.
