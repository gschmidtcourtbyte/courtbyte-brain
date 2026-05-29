# Wave 5 — Tests, Security-Review, Contract-Härtung

**Skill:** `/testing-eng` + `/review-eng` + `/security-eng`
**Status:** Done (2026-05-14) — Test- und Review-Pass abgeschlossen; kein P0/P1-Finding offen.
**Blocked by:** Waves 1–4

## Ziel

Die neue Karriere-Funktionalität gegen fachliche, technische und
sicherheitsrelevante Risiken absichern.

## Tasks

1. Unit- und Handler-Tests vervollständigen.
2. Contract-Checks für Upload, Download, Job CRUD, Rejection.
3. Security-Review:
   - File Upload Abuse
   - MIME Spoofing
   - Auth Download
   - PII Logging
4. Retention-Testfälle mit kontrollierter Zeitbasis.
5. Review-Findings nach Severity sortieren.

## Akzeptanzkriterien

- [x] Keine P0/P1 Review-Findings offen.
- [x] Kritische Karriere-Flows sind getestet.
- [x] Retention-Verhalten ist deterministisch validiert.

## Umsetzung 2026-05-14

- Tests in `backend/internal/submission/service_test.go` (16 Tests):
  Upload-Validation, Content-Type-Spoofing, Größenlimit, Rejection-Workflow
  (genau einmal), Mailer-Failure-Strategie, Retention-Löschung inkl.
  fehlendes Storage-Objekt, OriginGuard.
- Handler-Contract-Tests in `backend/internal/handler/admin_submissions_test.go`
  (Auth-Guards, 401, Attachment-404, Job-CRUD, Rejection mit Mailer-Ausfall)
  und `public_submissions_test.go` (Multipart, spoofed PDF).
- Retention-Tests nutzen injizierte `Now`-Funktion in `ServiceConfig` —
  deterministisch, ohne `time.Sleep`.

## Security-Review 2026-05-14

| Risiko | Mitigation | Status |
|---|---|---|
| File Upload Abuse (große Dateien) | `MaxBytesReader` 60 MiB pro Request, 10 MiB pro Datei, max 5 Anhänge | OK |
| MIME-Spoofing | Magic-Byte-Check (`%PDF-`, OLE-Header, ZIP-Header) zusätzlich zum Content-Type | OK, Test deckt es ab |
| Auth Download | `AdminSubmissionHandler.requireAdmin` vor `DownloadApplicationAttachment`, Streaming über Backend statt Bucket-URL | OK |
| PII Logging | Mailer loggt keine Mail-Body-Felder; Bewerbungsdaten erscheinen nicht in `slog.Info` | OK |
| CSRF Public-Form | `OriginGuard` gegen `FRONTEND_ORIGIN` für Multipart und JSON | OK |
| Rate-Limit Bypass | Redis-Token-Bucket pro IP für `/api/applications` und `/api/contact` | OK |
| Storage-Inkonsistenz | Reihenfolge Storage → DB → Rollback; Retention tolerant gegen `ErrStorageObjectGone` | OK |

Keine P0/P1-Findings. Offene Folge-Items (kein Iteration-Blocker):

- Echte Malware-Scanning-Pipeline (`scan_status` bleibt vorbereitet).
- Presigned Direct Uploads für höhere Volumen.

