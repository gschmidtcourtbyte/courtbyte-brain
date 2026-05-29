# Wave 4: QA & Abschluss

**Status:** Done (Legacy-Migration)
**Skill:** product-own, testing-eng, documentation-eng, review-eng
**Quelle:** `plan/iterations/iteration-8.md`

## Legacy-Inhalt

### Wave 4 — QA & Abschluss
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 8.5
**Ziel**: Auslieferung, Syntax und Rest-Risiken dokumentieren.

## Legacy-Status

### Wave 4 — QA & Abschluss: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- Docker-Container laufen; `/`, `logo_courts_transparent_mit_slogan.svg`, `style.css` und `main.js` liefern im Container HTTP 200.
- `rg` bestaetigt: keine `.nav-scroll-logo`-Referenz mehr in Navbar/CSS.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
