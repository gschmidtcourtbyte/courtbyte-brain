# Wave 3: QA & Regression

**Status:** Done (Legacy-Migration)
**Skill:** testing-eng, review-eng
**Quelle:** `plan/iterations/iteration-6.md`

## Legacy-Inhalt

### Wave 3 — QA & Regression
**Skillsets**: testing-eng, review-eng
**Tasks**: 6.4
**Ziel**: Syntax, Auslieferung und Interaktionsrisiken pruefen.

## Legacy-Status

### Wave 3 — QA & Regression: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- App-Container laeuft; `/` liefert containerintern HTTP 200.
- Ausgelieferte CSS-Datei enthaelt `body.has-hero-nav:not(.nav-visible) > nav`.
- Ausgelieferte JS-Datei enthaelt `getScrollTop`, `requestAnimationFrame(updateHeroNav)` und `resize`-Listener.
- Erwartete Kontrollwerte:
  - Desktop 1440x900: Logo-Top 90px, Logo-Breite 346px, Content-Y 472px.
  - Tablet 820x1180: Logo-Top 84px, Logo-Breite 230px, Content-Y 348px.
  - Mobile 390x844: Logo-Top 68px, Logo-Breite 187px, Content-Y 288px.
