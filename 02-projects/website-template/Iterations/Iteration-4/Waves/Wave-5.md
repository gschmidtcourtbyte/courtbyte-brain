# Wave 5 — Admin-Inbox-API-Grundlage

**Skill:** `/handler-eng` + `/service-eng`
**Status:** Planned
**Blocked by:** Wave 3

## Ziel

Backendseitige Admin-Endpunkte für persistierte Kontaktanfragen und Bewerbungen
bereitstellen. Diese Wave liefert den Contract für Iteration 5, bindet aber die
Admin-Frontend-Pages noch nicht vollständig um.

## Tasks

### 1. Auth-geschützte Admin-Endpoints

Unter `/api/admin`:

- `GET /inquiries`
- `GET /inquiries/{id}`
- `PATCH /inquiries/{id}`
- `GET /applications`
- `GET /applications/{id}`
- `PATCH /applications/{id}`

Auth:

- Bearer-Token wie bestehende Admin-Endpunkte.
- Ohne gültige Session: `401`.

### 2. Query + Response Contract

Listen:

- Sortierung: `created_at DESC`.
- Pagination: `limit` + `offset`, sichere Defaults.
- Counts für `unread` und `status` optional als Response-Meta.

Details:

- Vollständige Felder für Admin-Inbox.
- Statuswerte bleiben englische Backend-Enums; UI mappt in Iteration 5 auf
  deutsche Labels.

PATCH:

- Contact: `status`, `read`
- Application: `status`, `read`
- Teilupdates erlaubt; invalide Statuswerte `400`.

### 3. Frontend Contract vorbereiten

- TypeScript-Typen für spätere Admin-Bindung entweder in `frontend/lib/admin`
  oder klar aus Response-Beispielen in der Wave-Doku ableiten.
- Keine großflächige UI-Bindung in dieser Wave.

## Akzeptanzkriterien

- [ ] Alle Admin-Inbox-Endpunkte sind auth-geschützt.
- [ ] Listen liefern persistierte DB-Daten sortiert nach `created_at DESC`.
- [ ] Detail-Endpunkte liefern `404` für unbekannte IDs.
- [ ] PATCH aktualisiert Status/Read-Feld idempotent.
- [ ] Response-Shape ist dokumentiert und stabil genug für Iteration 5.
- [ ] Handler-Tests decken `401`, `400`, `404`, `200` ab.

## Out of Scope

- Admin-Frontend von Mock auf API umstellen.
- Bulk Actions.
- Suche/Filter jenseits von `limit`/`offset` und optional Status.
