# Wave 4: QA & Abschluss

**Status:** Done (Legacy-Migration)
**Skill:** product-own, testing-eng, documentation-eng, review-eng
**Quelle:** `plan/iterations/iteration-9.md`

## Legacy-Inhalt

### Wave 4 — QA & Abschluss
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 9.4
**Ziel**: Syntax, Auslieferung und Suchbefunde pruefen und dokumentieren.

## Legacy-Status

### Wave 4 — QA & Abschluss: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- `rg` bestaetigt: keine alten Hellgruen-Werte und kein `accent-deep` mehr in `web/static/css/style.css`.
- `rg` bestaetigt: alte Hellgruen-Werte sind in `logo/farbcodes.md` als Reserve dokumentiert.
- Docker-Container laufen; die Startseite liefert im Container HTTP 200.
- Container-Auslieferung von `style.css` zeigt die neuen Highlight-Tokens.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
