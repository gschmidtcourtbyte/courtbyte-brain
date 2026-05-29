# Iteration 4 — Public Submissions + Rate Limits + Admin-Inbox-Grundlage

**Status:** Planned
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-04-29

## Ziel

Das Template nimmt erstmals echte öffentliche Eingaben entgegen:

1. **Kontaktformular** persistiert Anfragen in PostgreSQL.
2. **Karriereformular** persistiert Bewerbungen in PostgreSQL.
3. **Rate Limits + Browser-Schutz** verhindern offensichtlichen Missbrauch auf
   den öffentlichen POST-Endpunkten.
4. **Admin-Inbox-Grundlage** liefert backendseitige Admin-Endpunkte, damit
   Iteration 5 die Mock-Inboxen an echte Daten binden kann.
5. **P0 Responsive-Bugfix:** Das mobile Burger-Menü behält beim Scrollen immer
   einen Theme-Hintergrund; Menüeinträge dürfen nie transparent über dem
   Seiteninhalt stehen.

## Kontext

- Iteration 3 hat Admin-UI, echte Admin-Routen und Responsive-Grundlage gebaut.
- Bekannter Bug aus Wave 6 / manueller Prüfung: Auf der öffentlichen Site kann
  der Hintergrund des mobilen Burger-Menüs nach Scroll aus dem Headerbereich
  verschwinden. Das ist ein sichtbarer Responsive-Blocker und wird vor der
  Backend-Arbeit eingeplant.
- Backend hat aktuell nur Admin-Auth-Persistenz. `/api/contact` und
  `/api/applications` sind noch `501 Not Implemented`.
- File-Upload-Verarbeitung bleibt weiterhin out of scope; Wave 2 legt aber
  Attachment-Metadaten und Storage-Referenzen an, damit Lebensläufe/Dokumente
  später ohne erneute Schema-Grundsatzänderung angebunden werden können.

## Scope

**In:**
- P0 Bugfix in `frontend/components/Navbar.tsx`:
  - Mobile Drawer immer mit solid/Theme-konformer Surface rendern.
  - Overlay, z-index und Scroll-State regressionssicher prüfen.
  - Test auf gescrolltem Viewport bei 360 / 414 / 768 px.
- Migration `002_public_submissions.up.sql` für:
  - `contact_inquiries`
  - `career_applications`
  - `career_application_attachments` als Metadaten-/Storage-Referenz-Tabelle
  - Status-/Read-Felder für spätere Admin-Inbox-Bindung.
- Repository-/Service-Schicht für Contact + Applications.
- `POST /api/contact` und `POST /api/applications` mit JSON-Validierung,
  Max-Body-Size, Persistenz und sauberen Error-Codes.
- Redis-backed Rate Limiting für `/api/contact` und `/api/applications`.
- Origin-/CSRF-Basisschutz für Browser-POSTs aus `FRONTEND_ORIGIN`.
- Frontend-Formulare posten an Same-Origin Next.js API-Routes:
  - `frontend/app/api/contact/route.ts`
  - `frontend/app/api/applications/route.ts`
- Admin-API-Grundlage:
  - `GET /api/admin/inquiries`
  - `GET /api/admin/applications`
  - Status-/Read-Update-Endpunkte als Grundlage für Iteration 5.
- Railway `staging` Deploy + Smoke-Tests.

**Out:**
- Mail-Versand / Autoresponder (Iteration 7)
- Datei-Upload / CV-Download / Galerie-Upload (Iteration 6)
- Vollständige Admin-Frontend-Bindung Mock -> API (Iteration 5)
- Stellen-, Seiten-, Galerie- und Settings-CRUD (Iteration 6)
- A11y-/SEO-/Performance-Polish (Iteration 8)

## Entscheidungen

- **Bugfix zuerst:** Der Burger-Menü-Background-Bug ist P0, weil er Wave-6-
  Responsive-Review blockiert und direkt sichtbar ist.
- **Public Forms über Next Proxy:** Browser postet same-origin an das Frontend.
  Die Next Route proxyt serverseitig an das Go-Backend. Dadurch bleiben
  Backend-Domain-Wechsel und CORS-Komplexität aus den Komponenten raus.
- **Keine Upload-Verarbeitung in Iteration 4:** Karriere speichert
  Bewerbungstext und Submission-Metadaten. Wave 2 legt zusätzlich eine
  Attachment-Metadaten-Tabelle an; Dateiannahme, Storage-Writes, Downloads und
  Virenprüfung bleiben bei den späteren Upload-/Handler-Waves.
- **Admin-Inbox-API vor UI-Bindung:** Backend-Endpunkte und Statusfelder werden
  jetzt gebaut; die Admin-UI bleibt in Iteration 4 noch mock-orientiert und
  wird in Iteration 5 gezielt gebunden.
- **Rate Limit mit Redis:** Railway-Staging hat Redis bereits. Falls Redis
  fehlt, starten die Public POST-Endpunkte nicht stillschweigend ungeschützt,
  sondern liefern einen klaren Service-Unavailable-Fehler.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng / testing-eng | P0 Responsive-Bugfix Burger-Menü + Regression-Smoke | Implemented; Browser regression pending |
| Wave 2 | migration-eng / database-eng | PostgreSQL-Schema für Kontaktanfragen + Bewerbungen | Implemented; Railway migration smoke pending |
| Wave 3 | service-eng / security-eng | Services, Validierung, Rate Limits, Origin-/CSRF-Basisschutz | Planned |
| Wave 4 | handler-eng / frontend-eng | Public POST-Handler + Next.js API-Proxies + Form-Submit-UX | Planned |
| Wave 5 | handler-eng / service-eng | Admin-Inbox-API-Grundlage für Iteration 5 | Planned |
| Wave 6 | testing-eng / review-eng / infrastructure-eng | Tests, Railway Deploy, Smoke/Risiko-Review | Planned |

## Critical Path

1. Wave 1 muss zuerst laufen, damit Wave-6-Responsive-Review nicht weiter mit
   einem bekannten UI-Blocker startet.
2. Wave 2 muss vor Wave 3/4 fertig sein, weil Services und Handler gegen das
   finale Schema implementiert werden.
3. Wave 3 liefert Rate Limit + Validierung als Shared Foundation für Wave 4
   und Wave 5.
4. Wave 4 aktiviert öffentliche Eingaben.
5. Wave 5 baut die Admin-API-Grundlage für Iteration 5.
6. Wave 6 deployt und verifiziert alles auf Railway `staging`.

## Acceptance Criteria

- [ ] Burger-Menü-Drawer hat auf gescrollten Seiten immer sichtbaren
      Theme-Hintergrund; Links stehen nie direkt auf Seiteninhalt.
- [ ] `/api/contact` persistiert valide Kontaktanfragen in PostgreSQL und
      lehnt invalide Payloads mit `400` ab.
- [ ] `/api/applications` persistiert valide Bewerbungen in PostgreSQL und
      lehnt invalide Payloads mit `400` ab.
- [ ] Öffentliche POST-Endpunkte haben Redis-backed Rate Limits und liefern bei
      Überschreitung `429`.
- [ ] Browser-POSTs werden auf erlaubte Frontend-Origin geprüft.
- [ ] Kontakt- und Karriere-Frontend zeigen Loading-, Success- und Error-State.
- [ ] Admin-Endpunkte für Inquiries und Applications liefern persistierte Daten
      mit Auth-Schutz (`401` ohne Session).
- [ ] Status-/Read-Felder sind im Schema und über Admin-API aktualisierbar.
- [ ] `go test ./...`, `npm run lint`, `npm run typecheck`, `npm run build`
      sind grün.
- [ ] Railway `staging` Deploy erfolgreich; Public POST- und Admin-API-Smokes
      dokumentiert.

## Risiken

- **PII-Daten:** Kontakt/Bewerbungen enthalten personenbezogene Daten.
  Mitigation: keine Payloads in Logs; klare Validierung; späteres Lösch-/Export-
  Konzept in Polish/Compliance-Backlog.
- **Spam/Abuse:** Public POST-Endpunkte sind extern erreichbar. Mitigation:
  Redis Rate Limit ab dieser Iteration; Captcha bleibt optional/später.
- **CSRF-Scope:** Public Forms haben keine Auth-Cookies, aber Browser-POSTs
  bleiben state-changing. Mitigation: Origin-Prüfung gegen `FRONTEND_ORIGIN`;
  stärkeres Token-Konzept bei Bedarf später.
- **Mock/API-Drift:** Admin-UI nutzt noch Mock-Daten. Mitigation:
  Response-Typen aus Admin-Inbox-API in Iteration 5 als bindender Contract.

## Verification

- Wave 1 local gates 2026-04-29: `npm run lint`, `npm run typecheck`,
  `npm run build` clean.
- Wave 1 Browser-Viewport-Regression 360 / 414 / 768 px bleibt für Wave 6 bzw.
  eine lokale Browser-Prüfung offen.
- Wave 2 local gates 2026-04-29: `go test ./...` clean; Migration gegen
  temporäre lokale PostgreSQL-DB angewendet, Tabellen/Indexe/Constraint-Smoke
  erfolgreich.
