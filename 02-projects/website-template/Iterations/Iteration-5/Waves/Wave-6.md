# Wave 6 — Railway-Smokes und Betriebsverifikation

**Skill:** `/infrastructure-eng` + `/testing-eng`
**Status:** Partially done (2026-05-14) — Cleanup-Loop produktiv und manueller Trigger dokumentiert; vollständiger Railway-Smoke-Lauf gegen Staging bleibt operative Aufgabe ohne Iteration-Blocker.
**Blocked by:** Waves 1–5

## Ziel

Die neue Karriere-Funktionalität in der Staging-Umgebung vollständig
verifizieren und betriebsreif dokumentieren.

## Tasks

1. Staging-Deploy der betroffenen Services.
2. Railway-Smokes:
   - Bewerbung mit Datei
   - Admin-Download
   - Rejection-Mail
   - `delete_after` korrekt gesetzt
   - Job erstellen/bearbeiten
3. Retention-Job-Betrieb:
   - Ausführungsmodus dokumentieren
   - manuelle Triggerbarkeit für Staging prüfen
4. Iteration-5 Verification-Block ergänzen.

## Akzeptanzkriterien

- [ ] Staging-Smokes erfolgreich dokumentiert (Playbook noch offen).
- [ ] Ablehnungs-Mail in Railway-Konfiguration getestet (offen — SMTP-Provider
      pro Kunden-Clone zu konfigurieren).
- [ ] Job CRUD auf Staging verifiziert (offen — manueller Smoke gegen
      Staging-Deploy).
- [x] Retention-Job betriebsseitig nachvollziehbar dokumentiert.

## Umsetzung 2026-05-14

- `cmd/api/main.go` — `runRetentionCleanupLoop` startet automatisch wenn
  `APPLICATION_RETENTION_CLEANUP_ENABLED=true`, Intervall via
  `APPLICATION_RETENTION_CLEANUP_INTERVAL`, Limit via
  `APPLICATION_RETENTION_CLEANUP_LIMIT`.
- Manueller Trigger: `POST /api/admin/applications/retention/run` (Admin-Auth).
- `backend/.env.example` und `README.md` führen die relevanten Variablen
  inkl. S3 + SMTP auf.

## Staging-Smoke-Playbook (zur Ausführung beim nächsten Staging-Deploy)

1. Bewerbungs-Upload: Multipart-POST `/api/applications` mit gültigem PDF.
   Erwartung: 201, `attachmentCount=1`.
2. Admin-Download: Login, dann
   `GET /api/admin/applications/{id}/attachments/{attachmentId}/download`
   gibt Datei zurück; ohne Session 401.
3. Rejection-Mail: PATCH `status=rejected`. Erwartung: Mail an Bewerber,
   `rejected_at`, `delete_after = rejected_at + 90d`,
   `rejection_email_sent_at` gesetzt.
4. Retention manuell: `POST /api/admin/applications/retention/run` nach
   `delete_after`-Manipulation in der DB. Erwartung: Bewerbung + Attachment
   + Bucket-Objekt verschwinden, Re-Run liefert `deleted: 0`.
5. Job-CRUD: POST/PATCH `/api/admin/jobs`, GET `/api/jobs` zeigt aktive
   Stelle öffentlich auf `/karriere`.
6. Auth-Guards: Admin-Routen ohne Session → 401, mit Session aber falscher
   Rolle → 401.

## Offene Operationsthemen (keine Iteration-Blocker)

- Staging-Bucket + SMTP-Credentials pro Kunden-Clone setzen.
- Smoke-Playbook beim nächsten Staging-Deploy abarbeiten und Ergebnisse
  hier nachtragen.
