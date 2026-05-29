# Wave 4 — Tests

**Status:** Done
**Skill:** testing-eng

## Ziel

Regressionen fuer Contact-Proxy, Admin-Inquiries und UI-Mapping absichern.

## Tasks

- Bestehende Backend-Tests laufen lassen.
- Frontend Lint/Typecheck/Build ausfuehren.
- Falls sinnvoll: Route-Handler-Tests oder fokussierte Mapping-Tests fuer
  Inquiry-Status/Read-State ergaenzen.
- Fehlerpfade fuer Kontaktformular mindestens manuell gegen gemockte Responses
  oder Staging pruefen.

## Acceptance Criteria

- [x] `go test ./...` gruen.
- [x] `npm run lint` gruen.
- [x] `npm run typecheck` gruen.
- [x] `npm run build` gruen.
