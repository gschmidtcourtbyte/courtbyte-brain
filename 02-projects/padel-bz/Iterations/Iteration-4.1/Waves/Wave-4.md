# Wave 4: Review & Abschluss

**Status:** Done (Legacy-Migration)
**Skill:** product-own, testing-eng, documentation-eng, review-eng
**Quelle:** `plan/iterations/iteration-4-1.md`

## Legacy-Inhalt

### Wave 4 — Review & Abschluss
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 4.1.5
**Ziel**: Visuelle Regression pruefen, Risiken dokumentieren, Iteration abschliessen.

## Legacy-Status

### Wave 4 — QA & Review: done
- CSS-Brace-Check: bestanden.
- Docker-Container: `padel-bz-app-1`, `postgres`, `redis` laufen.
- Docker-interner HTTP-Check: `/` und `logo_courts_transparent.svg` liefern HTTP 200.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
- Playwright-Screenshot wurde nicht ausgefuehrt; `npx --no-install playwright --version` wollte auf die npm Registry zugreifen und scheiterte an DNS/Netzwerkrestriktion.
