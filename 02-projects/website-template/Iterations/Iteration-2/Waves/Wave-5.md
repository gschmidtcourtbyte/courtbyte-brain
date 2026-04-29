# Iteration 2 — Wave 5: Railway Verification

**Skill:** testing-eng, review-eng
**Status:** Done

## Tasks

- [x] Backend nach Railway `staging` deployen
- [x] Frontend nach Railway `staging` deployen
- [x] Railway Healthchecks prüfen
- [x] Login-Smoke-Test über Railway-Domain ausführen
- [x] Risiken und Restarbeiten dokumentieren

## Acceptance

- Keine lokale Testpflicht in dieser Iteration.
- Verifikation ist anhand von Railway-Deployment-Status und Railway-URLs nachvollziehbar.

## Ergebnis 2026-04-29

- Backend und Frontend wurden nach Railway `staging` deployed.
- Healthchecks: Backend `200`, Frontend `200`.
- `/admin` ist erreichbar.
- Login über `frontend/admin/api/login` erfolgreich.
- Session-Check über `frontend/admin/api/me` nach Login erfolgreich.
- Logout über `frontend/admin/api/logout` erfolgreich; anschließender Session-Check liefert `401`.
