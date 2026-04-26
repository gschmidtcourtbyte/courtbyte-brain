# Wave 3: QA & Abschluss

**Status:** Done (Legacy-Migration)
**Skill:** product-own, testing-eng
**Quelle:** `plan/iterations/iteration-11.md`

## Legacy-Inhalt

### Wave 3 — QA & Abschluss
**Skillsets**: testing-eng, product-own
**Tasks**: 11.4 (benötigt fertige Integration)
**Ziel**: Alle Routing-Szenarien durchprüfen, Container-Test, HTML-Validierung.

## Legacy-Status

### Wave 3 — QA & Abschluss: done
- Go-Build: Kompiliert fehlerfrei (Docker builder stage).
- Container startet, Maintenance-Mode-Log bestätigt.
- Test 1: `/` → HTTP 503, Coming-Soon-Seite mit Logo-SVG.
- Test 2: `/staging` → HTTP 200, echte Startseite (Baufortschritt, Hero).
- Test 3: `/staging/kontakt` → HTTP 200, Kontaktformular.
- Test 4: `/impressum` → HTTP 200, durchgelassen.
- Test 5: `/datenschutz` → HTTP 200, durchgelassen.
- Test 6: `/static/css/style.css` → HTTP 200, durchgelassen.
- Test 7: `Retry-After: 86400` Header gesetzt.
- Test 8: HTML geschlossen, Logo-SVG mit korrektem viewBox eingebettet.
