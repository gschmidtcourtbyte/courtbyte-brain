# Wave 4 — Karriere-Frontend und Admin-UI an echte API binden

**Skill:** `/frontend-eng`
**Status:** Done (2026-05-14)
**Blocked by:** Wave 3

## Ziel

Die Karriere-Funktionen werden im UI vollständig echt nutzbar.

## Tasks

1. Öffentliche Bewerbung:
   - Datei-Input statt visual-only Upload-Zone
   - Upload-Fortschritt oder klarer Submit-State
   - Fehleranzeigen für Dateityp/Größe
2. Admin-Bewerbungsdetail:
   - echte Daten laden
   - Attachments anzeigen
   - Download-Buttons nutzen echte API
3. Statuswechsel:
   - `rejected` löst echten API-Call aus
   - UI zeigt klaren Persistenzstatus
4. Stellenverwaltung:
   - `Neue Stelle anlegen` öffnet echte Form
   - `Bearbeiten` lädt und speichert echte Daten
   - Aktiv/Inaktiv Toggle bleibt persistent
5. Loading-, Empty- und Error-States.

## Akzeptanzkriterien

- [x] Bewerber können Dateien im echten Formular hochladen.
- [x] Admins können Dateien sichtbar herunterladen.
- [x] Rejected-Status persistiert und UI reflektiert den Zustand.
- [x] Jobs können über Admin-UI angelegt und bearbeitet werden.

## Umsetzung 2026-05-14

- `app/(marketing)/karriere/page.tsx` — Bewerbungs-Formular mit echtem
  Datei-Input (`multiple`, `accept=".pdf,.doc,.docx,..."`), Client-Validation
  für Typ + 10 MB, Multipart-POST gegen `/api/applications`, klare Fehler-
  und Submit-States.
- `app/api/applications/route.ts` + `public-submission-proxy.ts` — Next.js
  Proxy reicht Multipart inklusive Origin/Referer/X-Forwarded-For an das
  Backend weiter.
- `app/admin/(authed)/karriere/page.tsx` — Bewerbungen + Stellen aus echter
  API geladen, Attachments mit Download-Link (`/admin/api/applications/.../
  attachments/.../download`), Status-Buttons rufen echtes PATCH inkl.
  `dependency_unavailable`-Recovery, Job-Dialog ruft POST/PATCH, Aktiv-
  Toggle persistiert.
- `app/admin/api/jobs/route.ts` + `[id]/route.ts` und `applications/...`
  Routen — auth-gefilterter Proxy in das Backend.
- Loading-, Empty- und Error-States vorhanden; Auth-Failure leitet zurück
  auf `/admin/login`.

## Verifikation 2026-05-14

- `npm run lint` clean, `npx tsc --noEmit` clean.
- Karriere-Admin manuell gegen lokales Backend bestätigt: Bewerbungen laden,
  Statuswechsel persistiert, Job-Anlage über echte API.

## Follow-Up 2026-05-14

- Öffentliche Karriere-Seite las bis dahin Stubs aus `lib/data.ts`. Da das
  in Iteration 5 explizit Out of Scope war, in einem Follow-up Bugfix
  separat behoben (siehe Iteration-Verification).

