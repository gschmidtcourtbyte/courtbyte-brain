# Iteration 5 — Karriereportal produktiv machen

**Status:** Done (2026-05-14) — Code vollständig umgesetzt, Tests + Lint + Typecheck grün; vollständiger Staging-Smoke bleibt operatives Follow-up.
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-05-13

## Ziel

Das Karriereportal wird von einer Mock-/Grundlagenfunktion zu einem praktisch
nutzbaren Bewerbungs-Workflow ausgebaut:

1. Bewerber können zu ihrer Bewerbung Dateien hochladen.
2. Admins können diese Dateien in der Admin-UI sicher herunterladen.
3. Wird eine Bewerbung auf **Abgelehnt** gesetzt, erhält der Bewerber eine
   entsprechende E-Mail.
4. Abgelehnte Bewerbungen werden zur Löschung markiert und nach **90 Tagen**
   automatisch gelöscht.
5. Die Admin-Funktionen **Neue Stelle anlegen** und **Bearbeiten** werden an
   echte Backend-Endpunkte gebunden.
6. Die Karriere-Admin-UI arbeitet mit echten API-Daten statt Mock-State.

## Kontext

- Iteration 4 liefert bereits:
  - persistierte Bewerbungen in PostgreSQL
  - Attachment-Metadaten-Tabelle als Grundlage
  - Admin-API-Grundlage für Bewerbungen
  - öffentliche Bewerbungs-POSTs
- Railway Deployment läuft bereits; diese Iteration fokussiert sich auf die
  fachliche Vervollständigung des Karriereportals, nicht auf
  Infrastruktur-Grundaufbau.
- Kontaktanfragen bleiben in dieser Iteration unverändert. Der Fokus liegt
  bewusst exklusiv auf Karriere/Bewerbungen/Stellen.

## Scope

**In:**
- Upload-Verarbeitung für Bewerbungsdateien:
  - Lebenslauf
  - Anschreiben / weitere Nachweise
  - valide Dateitypen und Größenlimits
  - Persistenz von Metadaten + Storage-Key
- Download-Endpunkte für Admins mit Auth-Schutz.
- Anzeige und Download von Attachments in `/admin/karriere`.
- Ablehnungs-Workflow:
  - Statuswechsel auf `rejected`
  - Rejection-Mail an Bewerber
  - Markierung der Bewerbung zur Löschung
  - `delete_after = rejected_at + 90 days`
- Automatische Löschung nach 90 Tagen:
  - Bewerbung
  - zugehörige Attachments
  - physische Datei im Storage
- Stellenverwaltung:
  - `GET /api/admin/jobs`
  - `POST /api/admin/jobs`
  - `PATCH /api/admin/jobs/{id}`
  - optional `PATCH /api/admin/jobs/{id}/active`
- Admin-Frontend:
  - Bewerbungen an echte API binden
  - Attachments anzeigen und herunterladen
  - Stellen anlegen und bearbeiten
  - Statuswechsel gegen echte API
- E-Mail-Versand für Ablehnungen über SMTP-Konfiguration.
- Relevante Tests und Railway-Smokes.

**Out:**
- Vollständige Mail-Strecke für Eingangsbestätigungen und allgemeine
  Admin-Notifications außerhalb der Ablehnungs-Mail.
- Galerie-Upload.
- Seiten-/Settings-CRUD.
- Vollständiger generischer Job-Publishing-Workflow für die öffentliche
  Karriere-Seite, sofern die öffentliche Anzeige weiterhin aus vorhandenen
  Datenstubs gespeist wird. Diese Iteration konzentriert sich auf Admin CRUD
  und Bewerbungsprozess.
- Komplexe CV-Vorschau im Browser. Download genügt.

## Entscheidungen

- **Rejection ist ein fachlicher Workflow, nicht nur ein Statusfeld.**
  `rejected` löst Mail + Retention-Marker aus.
- **90 Tage gelten ab Ablehnungszeitpunkt.**
  `delete_after = rejected_at + 90 days`.
- **Löschen ist zweistufig.**
  Erst markieren, dann per Retention-Job physisch entfernen.
- **Dateien werden serverseitig autorisiert ausgeliefert.**
  Keine direkten öffentlichen Storage-URLs in der Admin-UI.
- **Der vorhandene S3-kompatible Bucket wird direkt angebunden.**
  Attachment-Metadaten bleiben in PostgreSQL; die Binärdateien liegen im
  bestehenden Bucket. Lokale Entwicklung darf über denselben Storage-Adapter
  oder einen kompatiblen Dev-Fallback laufen.
- **Uploads laufen initial serverseitig kontrolliert in den Bucket.**
  Das hält Validierung, Checksummenbildung und Metadatenpersistenz in einem
  transaktional verständlichen Backend-Flow. Presigned Direct Uploads bleiben
  eine spätere Optimierung, falls Dateivolumen oder Traffic steigen.
- **Admin-Downloads bleiben autorisiert über das Backend.**
  Keine öffentlichen Storage-URLs in der Admin-UI; das Backend prüft die
  Session und streamt bzw. liefert die Datei kontrolliert aus.
- **Job CRUD wird in derselben Iteration umgesetzt,**
  weil die Karriere-Admin-UI sonst weiterhin halb mock-basiert bleibt.

## Datenmodell-Erweiterungen

### `career_applications`

Zusätzliche Felder:
- `rejected_at timestamptz`
- `delete_after timestamptz`
- `deletion_marked_at timestamptz`
- optional `rejection_email_sent_at timestamptz`

### `career_jobs`

Neue Tabelle für echte Stellenverwaltung:
- `id bigserial primary key`
- `title text not null`
- `department text not null`
- `employment_type text not null`
- `location text not null`
- `description text not null`
- `is_active boolean not null default true`
- `created_at timestamptz not null default now()`
- `updated_at timestamptz not null default now()`

### `career_application_attachments`

Bestehende Tabelle wird funktional genutzt:
- Upload schreibt Metadaten + Storage-Key.
- Download liest über Attachment-ID.
- Löschjob entfernt DB-Row und physische Datei.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | migration-eng / database-eng | Schema für Jobs, Rejection-Retention und Upload-/Deletion-Support | Planned |
| Wave 2 | service-eng / security-eng | Storage-, Attachment-, Mail- und Retention-Domainlogik | Planned |
| Wave 3 | handler-eng | Upload-/Download-Handler, Job CRUD, Rejection-Status-Endpunkte | Planned |
| Wave 4 | frontend-eng | Öffentliche Bewerbung mit File-Upload + Admin-Karriere an echte API binden | Planned |
| Wave 5 | testing-eng / review-eng | Job-/Upload-/Mail-/Deletion-Tests, Security-Review, Contract-Checks | Planned |
| Wave 6 | infrastructure-eng / testing-eng | Railway-Smokes, Retention-Job-Betrieb, Iteration Verification | Planned |

## Critical Path

1. Wave 1 definiert Schema und Retention-Felder.
2. Wave 2 liefert Business-Regeln für Upload, Ablehnung und Löschfristen.
3. Wave 3 öffnet die API-Oberfläche.
4. Wave 4 bindet Frontend und Admin-UI.
5. Wave 5/6 härten und verifizieren.

## Acceptance Criteria

- [x] Bewerber können bei einer Karrierebewerbung mindestens eine Datei
      hochladen.
- [x] Ungültige Dateitypen oder zu große Uploads werden kontrolliert
      abgelehnt.
- [x] Admins sehen Attachments in der Bewerbungsdetailansicht und können sie
      herunterladen.
- [x] Attachment-Downloads sind ohne Admin-Session nicht möglich.
- [x] Statuswechsel auf `rejected`:
  - [x] setzt `rejected_at`
  - [x] setzt `delete_after = rejected_at + 90 days`
  - [x] markiert die Bewerbung zur Löschung
  - [x] versendet eine Ablehnungs-Mail an den Bewerber
- [x] Der Löschprozess entfernt fällige abgelehnte Bewerbungen nach 90 Tagen
      inklusive Attachments und physischer Dateien.
- [x] `Neue Stelle anlegen` schreibt echte Datensätze.
- [x] `Bearbeiten` aktualisiert echte Datensätze.
- [x] Admin-Karriere lädt Bewerbungen und Stellen aus echter API.
- [x] Statuswechsel in der Admin-UI persistieren gegen echte API.
- [x] `go test ./...`, `npm run lint`, `npm run typecheck` sind grün
      (`npm run build` lokal nicht ausgeführt, kein Iteration-Blocker).
- [~] Railway-Smokes für Upload, Download, Rejection-Mail, Job CRUD und
      Auth-Guards sind dokumentiert. Playbook in `Waves/Wave-6.md` vorhanden;
      Ausführung gegen Staging bleibt operatives Follow-up.

## Risiken

- **PII + Dokumente:** Bewerbungen enthalten sensible Daten und Dateien.
  Mitigation: Auth-geschützte Downloads, keine öffentlichen Objekt-URLs,
  minimale Logging-Daten, Security-Review.
- **Mail-Zuverlässigkeit:** Statusänderung darf fachlich nicht halb
  abgeschlossen sein. Mitigation: idempotenter Rejection-Workflow, klare
  Retry-/Fehlerstrategie dokumentieren.
- **Retention-Korrektheit:** Die 90-Tage-Regel muss deterministisch sein.
  Mitigation: explizites `delete_after`, getesteter Cleanup-Job, Clock-basierte
  Tests.
- **Storage-Lifecycle:** DB und physischer Storage können auseinanderlaufen.
  Mitigation: Lösch-Workflow zentralisieren, Fehlerfälle im Job sauber loggen
  und wiederholbar machen.
- **Scope-Größe:** Upload, Mail, Retention und Job CRUD sind zusammen substanziell.
  Mitigation: klare Waves, Upload/Mail/Retention sequenziell vor UI-Polish.

## Offene Detailentscheidungen

- Konkrete erlaubte MIME-Typen und Maximalgröße pro Datei.
  → Entschieden: PDF/DOC/DOCX, 10 MiB pro Datei, max 5 Anhänge, 60 MiB
  pro Request.
- Ob mehrere Dateien pro Bewerbung erlaubt sind oder ein kleines Limit gilt.
  → Entschieden: bis zu 5 Anhänge pro Bewerbung.
- SMTP-Provider und Absenderadresse je Kunden-Clone.
  → Bleibt operativ pro Clone in `.env`.
- Soll die öffentliche Karriere-Seite in derselben Iteration schon vollständig
  aus `career_jobs` gespeist werden oder erst in einer Folgeiteration?
  → Entschieden: in dieser Iteration Out of Scope. Nachträglich am
  2026-05-14 als kleiner Bugfix nachgezogen (siehe Verification).

## Verification

### Wave-Status

| Wave | Status |
|---|---|
| Wave 1 — Schema | Done 2026-05-14 |
| Wave 2 — Domain (Storage/Mail/Retention) | Done 2026-05-14 |
| Wave 3 — Handler-API | Done 2026-05-14 |
| Wave 4 — Frontend/Admin-UI | Done 2026-05-14 |
| Wave 5 — Tests + Security-Review | Done 2026-05-14, keine P0/P1-Findings |
| Wave 6 — Betrieb | Partially done 2026-05-14, Staging-Playbook bereit, Ausführung operatives Follow-up |

### Local Gates 2026-05-14

- `go test ./...` — clean (Submission-Service + Handler + Migrations).
- `npm run lint` — clean.
- `npx tsc --noEmit` — clean.
- Manuelle Karriere-Admin-Smokes lokal gegen `go run ./cmd/api` + Next.js
  Dev: Login → Bewerbung anzeigen → Status setzen → Stelle anlegen.

### Follow-Ups innerhalb Iteration 5 (2026-05-14)

- **Bugfix:** öffentliche `/karriere`-Seite las Stub-`JOBS` statt `career_jobs`.
  Behoben durch neuen Public-Endpoint `GET /api/jobs` (filtert
  `is_active=true`), Next.js-Proxy `app/api/jobs/route.ts` und Anpassung
  der Seite auf API-Fetch mit Loading/Empty-State. Vorher in Iteration 5
  bewusst Out of Scope, jetzt nachgezogen weil Job-Erstellung im Admin
  sonst keinen sichtbaren Effekt hatte.
- **Bugfix:** öffentliches Bewerbungs-Formular schlug fehl wenn keine Datei
  ausgewählt war (Browser-Default für unbenutzte file-Inputs sendet leeren
  Multipart-Part). `applicationMultipart` filtert jetzt leere File-Parts
  (Filename leer + Size 0). Regressionstest:
  `TestPublicSubmissionHandlerAcceptsMultipartApplicationWithoutAttachments`.

### Operative Follow-Ups (kein Iteration-Blocker)

- Staging-Smoke-Playbook aus `Waves/Wave-6.md` gegen Railway ausführen
  und Ergebnis dort nachtragen.
- S3- und SMTP-Credentials pro Kunden-Clone setzen.
- Echte Malware-Scanning-Pipeline (Felder + Workflow vorbereitet).
