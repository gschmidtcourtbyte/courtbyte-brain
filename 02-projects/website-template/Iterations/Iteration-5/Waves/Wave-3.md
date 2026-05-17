# Wave 3 — Karriere-API: Upload, Download, Jobs, Rejection

**Skill:** `/handler-eng`
**Status:** Done (2026-05-14)
**Blocked by:** Wave 2

## Ziel

Die Karriere-Domain über HTTP vollständig nutzbar machen.

## Tasks

1. Public Upload-Flow:
   - Bewerbungs-POST auf Multipart erweitern oder ergänzende Attachment-Route
     definieren.
2. Admin Download:
   - `GET /api/admin/applications/{id}/attachments/{attachmentId}/download`
3. Admin Job CRUD:
   - `GET /api/admin/jobs`
   - `POST /api/admin/jobs`
   - `PATCH /api/admin/jobs/{id}`
4. Rejection-/Status-API:
   - bestehendes PATCH für Applications so erweitern, dass
     `rejected` den Workflow aus Wave 2 auslöst.
5. Klare Error-Codes und Auth-Guards.
6. Handler-Tests für:
   - 401 Download
   - 404 Attachment
   - 400 Upload invalid
   - 200/201 Job CRUD
   - rejected-Workflow HTTP-seitig

## Akzeptanzkriterien

- [x] Downloads ohne Admin-Auth sind nicht möglich.
- [x] Job CRUD ist über stabile API nutzbar.
- [x] Rejection-Workflow ist über PATCH zuverlässig erreichbar.
- [x] Upload-/Download-Routen haben klare Statuscodes.

## Umsetzung 2026-05-14

- `handler/public_submissions.go` — `applicationMultipart` parst Multipart
  bis 60 MiB, max 5 Anhänge, validiert pro Datei und ruft
  `SubmitCareerApplicationWithAttachments`.
- `handler/admin_submissions.go` — `DownloadApplicationAttachment` streamt
  über `OpenObject` mit Content-Disposition; `ListJobs` / `CreateJob` /
  `PatchJob` wired; `PatchApplication` triggert Rejection-Workflow wenn
  `status=rejected`.
- `handler/admin_submissions.go` — `RunRetentionCleanup` als manueller
  Trigger für Staging-Smokes.
- `cmd/api/main.go` — Routes: `POST /api/applications` (multipart),
  `GET /api/admin/applications/{id}/attachments/{attachmentId}/download`,
  `GET|POST /api/admin/jobs`, `PATCH /api/admin/jobs/{id}`,
  `POST /api/admin/applications/retention/run`.
- Fehler-Mapping in `writeSubmissionError`: 400 `validation_failed`, 403
  `forbidden_origin`, 413 `payload_too_large`, 429 `rate_limited` mit
  Retry-After, 503 `dependency_unavailable` für Mail/Storage-Ausfälle.

## Verifikation 2026-05-14

- Handler-Tests in `admin_submissions_test.go` (Auth-Guards, 404 Attachment,
  Job-CRUD, Rejection mit Mailer-Ausfall) und `public_submissions_test.go`
  (spoofed PDF abgelehnt, Multipart-Erfolg).

## Bugfix 2026-05-14

- Leere `attachments`-File-Parts (Browser-Default für unbenutzten file-Input)
  werden im Multipart-Handler übersprungen. Vorher: Bewerbung ohne Datei
  schlug mit `validation_failed` (`originalFilename: required`,
  `sizeBytes: invalid`) fehl. Test:
  `TestPublicSubmissionHandlerAcceptsMultipartApplicationWithoutAttachments`.

