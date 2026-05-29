# Wave 6 — Tests, Railway Deploy, Smoke/Risiko-Review

**Skill:** `/testing-eng` + `/review-eng` + `/infrastructure-eng`
**Status:** Planned
**Blocked by:** Waves 1–5

## Ziel

Iteration 4 wird lokal und auf Railway `staging` verifiziert. Fokus liegt auf
Regression des Burger-Menü-Fixes, Public POST Persistenz, Rate Limits und
Admin-Inbox-API-Grundlage.

## Tasks

### 1. Lokale Gates

- Frontend:
  - `npm run lint`
  - `npm run typecheck`
  - `npm run build`
- Backend:
  - `go test ./...`
  - `make build` falls relevant

### 2. Railway Deploy

- Backend nach `staging` deployen, weil Migration + API neu sind.
- Frontend nach `staging` deployen, weil Forms + Navbar-Bugfix neu sind.
- Deployment IDs in `Iteration-4.md` dokumentieren.
- Healthchecks:
  - Frontend `/healthz`
  - Backend `/healthz`

### 3. Public POST Smoke

Über Railway-Domain:

- `POST /api/contact` valide Payload -> `201`.
- `POST /api/contact` invalide Payload -> `400`.
- Rate-Limit-Szenario -> `429` oder dokumentierter manueller Check, falls
  nicht automatisiert.
- `POST /api/applications` valide Payload -> `201`.
- `POST /api/applications` invalide Payload -> `400`.
- Origin-Verstoß -> `403`.

### 4. Admin API Smoke

- Ohne Cookie/Bearer:
  - `GET /admin/api/me` -> `401`
  - Admin-Inbox-Endpunkte -> `401`
- Mit Admin-Session:
  - Login -> `200`
  - `GET /api/admin/inquiries` -> `200`
  - `GET /api/admin/applications` -> `200`
  - PATCH Status/Read -> `200`
  - Logout -> `200`

### 5. Responsive Regression Smoke

Auf Staging:

- Viewports: 360, 414, 768.
- Routen: `/`, `/ueber-uns`, `/kontakt`, `/ressourcen`.
- Nach unten scrollen, Burger öffnen.
- Prüfen:
  - Drawer-Hintergrund sichtbar.
  - Menüeinträge nicht transparent.
  - Overlay schließt.
  - Link-Click schließt.

### 6. Review

- Code-Review auf:
  - PII Logging
  - Rate-Limit-Bypass
  - Handler-SQL-Verstoß
  - Migration-Constraints
  - Response-Contract-Stabilität für Iteration 5

## Akzeptanzkriterien

- [ ] Lokale Frontend- und Backend-Gates grün.
- [ ] Railway Backend + Frontend Deploy erfolgreich.
- [ ] Public POST-Smokes dokumentiert.
- [ ] Admin API-Smokes dokumentiert.
- [ ] Burger-Menü-Regression auf Staging geprüft.
- [ ] Keine P0/P1 Review-Findings offen.
- [ ] `Iteration-4.md` Verification-Block ergänzt.

## Out of Scope

- Browser-Pixelreview der gesamten Iteration 3.
- Lighthouse/A11y/SEO.
- Mail-/Upload-Verification.
