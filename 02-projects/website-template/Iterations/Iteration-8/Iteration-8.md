# Iteration 8 — Kontaktformular und echte Admin-Anfragen

**Status:** Implemented
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-05-14
**Bearbeitung:** Codex MCP Server

## Ziel

Das Kontaktformular soll Ende-zu-Ende ueber das echte Backend laufen und im
Adminbereich als echte Anfrage sichtbar sein. Die Anzeige neuer Anfragen soll
analog zum Karriereportal aus Backend-Daten entstehen: keine Mock-Liste, keine
statische Badge, sondern ungelesene Kontaktanfragen aus `contact_inquiries`.

## Kontext

- Das Backend hat bereits `POST /api/contact`, `GET /api/admin/inquiries`,
  `GET /api/admin/inquiries/{id}` und `PATCH /api/admin/inquiries/{id}`.
- Die oeffentliche Next.js-Route `frontend/app/api/contact/route.ts` proxied
  bereits auf das Backend. Der Kontaktseiten-Flow braucht aber noch robuste
  UX-/Fehlerbehandlung und einen Smoke gegen Staging.
- Die Admin-Seite `frontend/app/admin/(authed)/anfragen/page.tsx` nutzt aktuell
  noch `INQUIRIES` aus `admin-mock`.
- Seit Iteration 7 existiert `GET /api/admin/summary`; die Sidebar kann
  `unreadInquiries` anzeigen, wenn die Daten im Adminbereich sauber aktualisiert
  werden.

## Scope

**In:**

- Kontaktformular gegen echtes Backend verifizieren und bei Bedarf haerten:
  Submit-Guard, klare Erfolgsmeldung, differenzierte Fehlertexte, Reset nach
  Erfolg.
- Admin-Proxy-Routen fuer Anfragen unter `/admin/api/inquiries` und
  `/admin/api/inquiries/[id]`.
- Admin-Anfragen-Seite auf Backend-Daten umstellen:
  Liste, Detail, Nachrichtentext, Status, Read-State.
- Auswahl einer Anfrage markiert diese im Backend als gelesen.
- Statuswechsel `Neu`, `In Bearbeitung`, `Erledigt` schreibt per PATCH ins
  Backend.
- Neue-Anfragen-Zahl in Sidebar und Anfragen-Seite basiert auf echten
  ungelesenen Anfragen.
- Summary nach Read-/Status-Aenderungen aktualisieren oder invalidieren, damit
  die Badge nicht veraltet.
- Tests fuer Contact-Proxy, Admin-Inquiry-Proxy und UI-Datenmapping.
- Railway-Staging-Deploy und Smoke: Kontakt absenden, Admin sieht Anfrage,
  Badge sinkt nach Lesen.

**Out:**

- Kontakt-Antwort per SMTP aus dem Adminbereich.
- Kontakt-Eingangsmail an interne Adresse.
- Suche, Pagination-UI und Bulk-Aktionen fuer Anfragen.
- Dashboard-Statistiken ausserhalb der Sidebar/Anfragen-Seite.

## Entscheidungen

- Backend bleibt Source of Truth: Frontend-Mocks werden auf der Anfragen-Seite
  nicht mehr als Fallback fuer echte Daten genutzt.
- Read-State wird beim Oeffnen des Details geschrieben. Dadurch entspricht die
  Badge dem natuerlichen Admin-Workflow.
- Statuslabels bleiben deutsch im Frontend, werden aber auf Backendwerte
  gemappt:
  - `new` <-> `Neu`
  - `in_progress` <-> `In Bearbeitung`
  - `done` <-> `Erledigt`
- Die Antwort-Funktion bleibt sichtbar nur dann, wenn sie entweder entfernt oder
  klar als nicht aktiv behandelt wird. In dieser Iteration wird kein SMTP-Reply
  gebaut.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | product-own | Scope, Akzeptanzkriterien, Datenfluss | Done |
| Wave 2 | handler-eng | Next.js Admin-Proxy-Routen fuer Inquiries | Done |
| Wave 3 | frontend-eng | Kontaktformular-UX und Admin-Anfragen aus Backend | Done |
| Wave 4 | testing-eng | Unit-/Integrationstests, Typecheck, Build | Done |
| Wave 5 | infrastructure-eng/testing-eng | Railway-Staging-Deploy und Smoke | Done |
| Wave 6 | documentation-eng | README/Runbook und Iterationsabschluss | Done |

## Critical Path

1. Admin-Proxy-Routen bereitstellen; ohne sie kann die Admin-Seite keine echten
   Daten konsumieren.
2. Datenmapping fuer Kontaktanfragen definieren und Admin-Seite umstellen.
3. Read-/Status-PATCH implementieren, damit Badge und Liste nicht nur anzeigen,
   sondern echte Zustandsaenderungen schreiben.
4. Kontaktformular gegen Backend-Smoke verifizieren.
5. Tests und Staging-Deploy.

## Acceptance Criteria

- [x] Kontaktformular sendet erfolgreich an das echte Backend und erzeugt einen
      Datensatz in `contact_inquiries`.
- [x] Bewerber/Nutzer sieht nach Absenden eine klare, dauerhafte
      Erfolgsmeldung und kann nicht mehrfach versehentlich senden.
- [x] Backend-Validierungs-, Rate-Limit- und Servicefehler werden im
      Kontaktformular verstaendlich angezeigt.
- [x] Admin-Anfragen-Seite laedt echte Anfragen via `/admin/api/inquiries`.
- [x] Detailansicht zeigt echten Namen, E-Mail, Telefon, Betreff, Nachricht und
      Datum aus dem Backend.
- [x] Oeffnen einer neuen Anfrage markiert sie im Backend als gelesen.
- [x] Statuswechsel auf `In Bearbeitung` oder `Erledigt` persistiert im Backend.
- [x] Sidebar-Badge fuer `Anfragen` und Seitenzaehler zeigen echte ungelesene
      Anfragen.
- [x] Nach Lesen einer Anfrage aktualisiert sich die neue-Anfragen-Anzeige.
- [x] Lokale Tests, Lint, Typecheck und Build sind gruen.
- [x] Railway-Staging-Smoke ist dokumentiert.

## Arbeitspakete

## Task: Admin-Inquiry-Proxy
**Priority:** P0
**Blocked by:** none
**Akzeptanzkriterien:**
- [ ] `GET /admin/api/inquiries` proxied Query-Parameter an Backend.
- [ ] `GET /admin/api/inquiries/[id]` proxied Detailabruf.
- [ ] `PATCH /admin/api/inquiries/[id]` proxied Status/Read-Updates.
**Kontext:** Bestehendes Proxy-Muster in `frontend/app/admin/api/applications`.
**Output:** Next.js Route Handler.

## Task: Admin-Anfragen auf Backenddaten
**Priority:** P0
**Blocked by:** Admin-Inquiry-Proxy
**Akzeptanzkriterien:**
- [ ] Keine Nutzung von `INQUIRIES` als Datenquelle.
- [ ] Loading-/Empty-/Error-State vorhanden.
- [ ] Status- und Read-Mapping korrekt.
**Kontext:** `frontend/app/admin/(authed)/anfragen/page.tsx`.
**Output:** Umgestellte Admin-Seite.

## Task: Read-/Status-Synchronisierung
**Priority:** P0
**Blocked by:** Admin-Anfragen auf Backenddaten
**Akzeptanzkriterien:**
- [ ] Detailauswahl setzt `read=true`.
- [ ] Statusbuttons patchen Backend und aktualisieren lokalen State.
- [ ] Neue-Anfragen-Badge wird nach Aenderungen aktualisiert.
**Kontext:** `AdminSidebar` nutzt `GET /admin/api/summary`.
**Output:** Aktualisierte UI-State-Strategie.

## Task: Kontaktformular-UX haerten
**Priority:** P1
**Blocked by:** none
**Akzeptanzkriterien:**
- [ ] Doppel-Submit technisch verhindert.
- [ ] Erfolgsmeldung bleibt sichtbar.
- [ ] Fehlertexte unterscheiden 400, 429, 403, 503 und Netzwerkfehler.
**Kontext:** `frontend/app/(marketing)/kontakt/page.tsx`.
**Output:** Robuster Public-Flow.

## Task: Tests und Staging-Smoke
**Priority:** P1
**Blocked by:** alle P0 Tasks
**Akzeptanzkriterien:**
- [ ] Backend/Handler-Tests bleiben gruen.
- [ ] Frontend Lint, Typecheck und Build gruen.
- [ ] Staging: Anfrage absenden, Admin sieht Anfrage, Badge sinkt nach Lesen.
**Kontext:** Railway Staging aus Iteration 7.
**Output:** Verifikationsnotizen in Iteration 8.

## Risiken

- **Sidebar-Badge veraltet:** Wenn `AdminSidebar` nur beim Mount laedt, sinkt
  die Badge nach Lesen nicht automatisch. Mitigation: gemeinsamer Refresh-Event,
  kleiner Summary-Hook oder Revalidate nach PATCH.
- **Antwortformular suggeriert Funktion:** Aktuell ist Reply ein Stub. Wenn es
  stehen bleibt, muss UI klar sein oder der Block wird bis zur SMTP-Reply-
  Iteration entfernt/deaktiviert.
- **Rate-Limit-Smoke:** Kontaktformular hat 5 Requests pro 10 Minuten und IP.
  Staging-Smokes muessen sparsam laufen.
- **Dirty Worktree:** Iteration 7 ist noch uncommitted. Umsetzung von Iteration
  8 muss darauf aufsetzen und darf keine bestehenden Iteration-7-Aenderungen
  zurueckdrehen.

## Verification Plan

- Lokal:
  - `go test ./...`
  - `npm run lint`
  - `npm run typecheck`
  - `npm run build`
- Staging:
  - `POST /api/contact` ueber Frontend.
  - Admin-Login, `/admin/anfragen` zeigt neue Anfrage.
  - Auswahl setzt Anfrage auf gelesen.
  - Sidebar-Badge `Anfragen` sinkt entsprechend.
  - Statuswechsel bleibt nach Reload erhalten.

## Verification Ergebnis

- Lokal:
  - `go test ./...`: gruen.
  - `npm run lint`: gruen.
  - `npm run build`: gruen.
  - `npm run typecheck`: gruen nach separater Ausfuehrung. Der erste parallele
    Lauf kollidierte mit dem gleichzeitig erzeugten `.next/types`-Verzeichnis.
- Railway Staging:
  - Frontend Deploy: erfolgreich, Healthcheck gruen.
  - `POST https://frontend-staging-a1ac.up.railway.app/api/contact`: `201`.
  - `GET /admin/api/inquiries?limit=20`: neue Anfrage `Iteration 8 Kontakt Smoke`
    vorhanden.
  - `GET /admin/api/summary`: `unreadInquiries` vor Lesen `5`.
  - `PATCH /admin/api/inquiries/5 {"read": true}`: Anfrage gelesen.
  - `GET /admin/api/summary`: `unreadInquiries` danach `4`.
  - `PATCH /admin/api/inquiries/5 {"status": "in_progress"}`: Status persistiert.
  - `GET /admin/api/inquiries/5`: Status weiterhin `in_progress`, `isRead=true`.
