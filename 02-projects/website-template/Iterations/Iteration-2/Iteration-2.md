# Iteration 2 — Admin Login + Railway Runtime Foundations

**Status:** Railway Verified
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-04-29

## Ziel

Das Template bekommt einen funktionsfähigen Admin-Einstieg unter `/admin`.
Admin-Daten werden in PostgreSQL persistiert, Sessions werden über Redis gecached.
Die Iteration wird nicht lokal verifiziert; Build-, Deploy- und Smoke-Tests laufen
über Railway `staging`.

## Scope

**In:**
- Railway Redis-Service im Projekt `website-template` anlegen und mit Backend verbinden
- Backend-Konfiguration für `DATABASE_URL`, `REDIS_URL`, Admin-Bootstrap und Frontend-URL
- Versionierte Migrationen für Admin-User und Audit-Log
- Admin-Bootstrap ohne öffentliche Registrierung
- Passwortprüfung serverseitig, Session-Token als opaque Token, Redis-TTL
- API-Endpunkte: Login, Logout, Current Admin
- `/admin` Frontend-Seite mit Login-State und einfachem Dashboard-State
- Next.js API-Proxy, damit Admin-Cookie HttpOnly auf der Frontend-Domain bleibt
- Railway-only Verification dokumentieren und ausführen

**Out:**
- Öffentliche User-Registration
- Passwort-Reset
- Vollständiges CMS-Dashboard
- Lokale Test-/Smoke-Test-Verpflichtung
- Kontakt-/Bewerbungs-Persistenz

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | product-own / infrastructure-eng | Plan, Railway Redis, Variablen, Verification-Strategie | Done |
| Wave 2 | migration-eng / database-eng | Admin-Migrationen, Bootstrap-Datenmodell, Audit | Done |
| Wave 3 | service-eng / security-eng | Auth-Service, Redis-Sessions, Passwortprüfung | Done |
| Wave 4 | handler-eng / frontend-eng | API-Handler, `/admin` UI, Next.js Cookie-Proxy | Done |
| Wave 5 | testing-eng / review-eng | Railway Deploy, Health/Login Smoke-Test, Risiko-Review | Done |

## Acceptance Criteria

- [x] Railway-Projekt `website-template` hat einen Redis-Service in `staging`.
- [x] Backend-Service hat `DATABASE_URL`, `REDIS_URL`, `ADMIN_EMAIL`, `ADMIN_PASSWORD` und `FRONTEND_ORIGIN`.
- [x] Backend startet in Railway, führt Admin-Migrationen idempotent aus und bootstrapped genau einen Admin ohne Registration.
- [x] `POST /api/admin/login` validiert Admin-Credentials gegen PostgreSQL und legt eine Redis-Session mit TTL an.
- [x] `GET /api/admin/me` liefert mit gültiger Session Admin-Metadaten und ohne Session `401`.
- [x] `POST /api/admin/logout` invalidiert die Redis-Session.
- [x] `/admin` ist erreichbar und zeigt vor Login eine Login-Page, nach Login einen Admin-Bereich.
- [x] Browser-Session wird über HttpOnly-Cookie auf der Frontend-Domain gehalten; keine Tokens in LocalStorage.
- [x] Frontend und Backend sind erfolgreich nach Railway `staging` deployed.
- [x] Verifikation erfolgt über Railway-Domains, nicht lokal.

## Verification 2026-04-29

- Backend Railway deploy: successful
- Frontend Railway deploy: successful
- `GET https://backend-staging-97f2.up.railway.app/healthz` -> `200`
- `GET https://frontend-staging-a1ac.up.railway.app/healthz` -> `200`
- `GET https://frontend-staging-a1ac.up.railway.app/admin` -> `200`
- `GET https://frontend-staging-a1ac.up.railway.app/admin/api/me` without cookie -> `401`
- `POST https://frontend-staging-a1ac.up.railway.app/admin/api/login` -> `200`
- `GET https://frontend-staging-a1ac.up.railway.app/admin/api/me` with cookie -> `200`
- `POST https://frontend-staging-a1ac.up.railway.app/admin/api/logout` -> `200`
- `GET https://frontend-staging-a1ac.up.railway.app/admin/api/me` after logout -> `401`

## Risiken / Entscheidungen

- Für diese Iteration wird ein Bootstrap-Admin per Environment Variable erzeugt; Rotation/Reset folgt später.
- Redis ist Session-Cache, PostgreSQL bleibt Source of Truth für Admin-User und Audit.
- Die Admin-UI ist bewusst minimal, weil CMS-Funktionen erst in den Folgeiterationen kommen.
