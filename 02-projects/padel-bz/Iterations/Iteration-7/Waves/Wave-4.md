# Wave 4: QA & Dokumentation

**Status:** Done (Legacy-Migration)
**Skill:** product-own, testing-eng, documentation-eng, review-eng
**Quelle:** `plan/iterations/iteration-7.md`

## Legacy-Inhalt

### Wave 4 — QA & Dokumentation
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 7.5
**Ziel**: Ergebnis pruefen, Risiken dokumentieren und Iteration abschliessen.

## Legacy-Status

### Wave 4 — QA & Dokumentation: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- Docker-Container laufen; `/`, `logo_courts_transparent.svg`, `style.css` und `main.js` liefern im Container HTTP 200.
- Ausgelieferte CSS-Datei enthaelt `.nav-scroll-logo`, `clamp(276px,28.8vw,432px)` und `clamp(206px,57.6vw,276px)`.
- Kontrollwerte:
  - Desktop 1440x900: Hero-Logo 415px, Content-Y 541px, Navbar-Logo 130px.
  - Narrow 960x900: Hero-Logo 276px, Content-Y 402px, Navbar-Logo 96px.
  - Tablet 820x1180: Hero-Logo 276px, Content-Y 394px, Navbar-Logo 116px.
  - Mobile 390x844: Hero-Logo 225px, Content-Y 326px, Navbar-Logo 94px.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
- Playwright-Screenshot wurde nicht ausgefuehrt; `npx --no-install playwright --version` wollte auf die npm Registry zugreifen und scheiterte an DNS/Netzwerkrestriktion.
