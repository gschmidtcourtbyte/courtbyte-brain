# Wave 6 — Railway Deploy, Smoke-Tests Desktop+Mobile, Visual-Review, Polish-Backlog

**Skill:** `/testing-eng` + `/review-eng`
**Status:** Ready for Browser Review (Railway deploy done 2026-04-29)
**Blocked by:** Wave 5 (implemented; browser visual review pending)

## Deployment Baseline 2026-04-29

- Frontend Railway `staging` deploy: **SUCCESS**
  - Deployment ID: `1764f9bf-f4e5-49f8-8889-18ddd6901e95`
  - Domain: `https://frontend-staging-a1ac.up.railway.app`
  - Build: Dockerfile path from `frontend/railway.json`; `npm run build`
    successful; `/healthz` Railway healthcheck successful.
- Backend Railway `staging` remains unchanged and healthy:
  - Domain: `https://backend-staging-97f2.up.railway.app`
  - `GET /healthz` -> `{"status":"ok"}`
- HTTP smoke without browser:
  - `GET /` -> `200`
  - `GET /ressourcen` -> `200`
  - `GET /admin/login` -> `200`
  - `GET /admin/anfragen` -> `200`
  - `GET /admin/karriere` -> `200`
  - `GET /admin/seiten` -> `200`
  - `GET /admin/galerie` -> `200`
  - `GET /admin/einstellungen` -> `200`
- Note: one prior deploy attempt `6c171609-9d16-48a7-a5ab-be9192a78b73`
  failed because Railway analyzed the repo root as Railpack. Final deploy used
  `railway up -c --service frontend --environment staging --path-as-root frontend`.

## Ziel

Iteration 3 auf Railway `staging` deployen, alle Admin- und Frontend-Routen
auf Desktop **und** Mobile-Viewports smoke-testen, einen Visual-Review gegen
das Design-Original und einen Responsive-Review gegen die Breakpoint-Spec
durchführen und einen Polish-Backlog für minor Drifts erstellen.

## Tasks

### 1. Railway Deploy

- Frontend-Service nach `staging` deployen.
- Deploy-Logs prüfen (kein TS-Error, kein Build-Fehler).
- Frontend-Domain notieren (z. B. `frontend-staging-a1ac.up.railway.app`).

### 2. Smoke-Tests aller Admin-Routen

Für jede der folgenden Routen muss verifiziert werden:

| Route | Ohne Cookie | Mit Cookie |
|---|---|---|
| `/admin/login` | `200` | `200` |
| `/admin` | `→ /admin/login` | `200` |
| `/admin/anfragen` | `→ /admin/login` | `200` |
| `/admin/karriere` | `→ /admin/login` | `200` |
| `/admin/seiten` | `→ /admin/login` | `200` |
| `/admin/galerie` | `→ /admin/login` | `200` |
| `/admin/einstellungen` | `→ /admin/login` | `200` |

API-Routes weiterhin geprüft (Regression-Check):
- `GET /admin/api/me` ohne Cookie → `401`
- `POST /admin/api/login` mit Bootstrap-Credentials → `200`
- `GET /admin/api/me` mit Cookie → `200`
- `POST /admin/api/logout` → `200`
- `GET /admin/api/me` nach Logout → `401`

### 3. Visual-Review (Desktop)

- Browser-Test (Login + jede Section) gegen `Backend CMS.html`-Original
  bei 1440 px Breite.
- Diff-Liste erstellen für visuelle Abweichungen
  (Spacings, Schriften, Farben, Hover-States, Animationen).
- Findings nach Severity sortieren:
  - **Blocker:** Falsche Farben/Tokens, fehlende Komponenten, Auth-Bugs.
  - **Major:** Off-Spacings, fehlende Hover-Animations.
  - **Minor:** Mikro-Drifts (1–2 px Abweichungen, leicht andere Border-Radii).
- Blocker → zurück an `frontend-eng`-Agent (resume Wave 3 oder 4).
- Major + Minor → Polish-Backlog für eine spätere Iteration.

### 4. Responsive-Review (Mobile + Tablet)

Browser-Devtools im Responsive-Mode bei den fünf Viewport-Breiten
**360, 414, 768, 1024, 1440 px**. Für jede Breite und jede Route
(Frontend: `/`, `/ueber-uns`, `/karriere`, `/kontakt`, `/ressourcen`;
Admin: `/admin/login`, `/admin`, `/admin/anfragen`, `/admin/karriere`,
`/admin/seiten`, `/admin/galerie`, `/admin/einstellungen`) prüfen:

- Kein horizontaler Scroll.
- Keine überlappenden / abgeschnittenen Elemente.
- Burger-Menü (Frontend < 768 px) öffnet/schließt sauber.
- Admin-Sidebar-Drawer (< 1024 px) öffnet/schließt; Overlay-Tap schließt.
- Master-Detail (Anfragen + Bewerbungen) zeigt auf < 768 px nur
  eine Pane; Detail hat Back-Button.
- Seiten-Tabelle wird auf < 768 px zur Card-Liste.
- Tap-Targets für Buttons/Links mindestens 40×40 px.
- Findings in dieselbe Severity-Tabelle einsortieren wie der
  Visual-Review; Blocker-Fixes gehen zurück an Wave 5.

### 5. Iteration-3.md Verification-Block ergänzen

Format wie Iteration-2.md:

```markdown
## Verification YYYY-MM-DD

- Frontend Railway deploy: successful
- GET /admin/login → 200
- GET /admin without cookie → redirect /admin/login
- GET /admin with cookie → 200
- ... (alle obigen Smoke-Tests)
- Visual-Review: <Status> (siehe Polish-Backlog)
```

## Akzeptanzkriterien

- [x] Railway `staging` Deploy erfolgreich (kein TS-/Build-Error).
- [ ] Alle 7 Admin-Routen + 4 API-Routes smoke-getestet, Ergebnisse
      in Iteration-3.md dokumentiert.
- [ ] Visual-Review-Diff-Liste (Desktop) in Wave-6.md erstellt.
- [ ] Responsive-Review-Diff-Liste für 360/414/768/1024/1440 px in Wave-6.md
      erstellt — pro Route und Viewport ein Status (OK / Major / Blocker).
- [ ] Keine Blocker offen (alle nachbearbeitet via Wave-3/4/5-Resume).
- [ ] Iteration-3.md Status auf "Railway Verified" gesetzt.

## Out of Scope (in Wave 5)

- Polish-Findings selbst beheben (nur Blocker; Rest geht in Backlog).
- Performance-Tests / Lighthouse / A11y-Audit.

## Kontext

- **Iteration-2.md** als Verification-Format-Referenz
- **Existing Bootstrap-Admin** aus Iteration 2 (Email + Passwort aus Railway-Env)
- **Railway-Domain** muss aus dem Frontend-Service ermittelt werden
