# Wave 2 — Admin-Proxy-Routen

**Status:** Done
**Skill:** handler-eng

## Ziel

Die vorhandenen Backend-Inquiry-Endpunkte ueber Next.js Admin-Routen verfuegbar
machen.

## Tasks

- `frontend/app/admin/api/inquiries/route.ts` fuer Listenabruf.
- `frontend/app/admin/api/inquiries/[id]/route.ts` fuer Detail und PATCH.
- Query-Parameter `status`, `read`, `limit`, `offset` durchreichen.
- Auth-Fehler und Backend-Fehler wie bestehende Admin-Proxies mappen.

## Acceptance Criteria

- [x] Unauthentifizierte Requests erhalten 401.
- [x] Admin-Session wird als Bearer Token ans Backend gegeben.
- [x] JSON-Antwort bleibt kompatibel mit Backend-Envelope.
