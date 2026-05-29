# Wave 2 — Upload-, Mail- und Retention-Domainlogik

**Skill:** `/service-eng` + `/security-eng`
**Status:** Done (2026-05-14)
**Blocked by:** Wave 1

## Ziel

Die Domain-Fundamente für das produktive Karriereportal implementieren:

1. Bewerbungsdateien werden kontrolliert in den bereits vorhandenen
   S3-kompatiblen Bucket geschrieben.
2. Ablehnungen lösen fachlich korrekt Mail + 90-Tage-Retention aus.
3. Der spätere Cleanup-Job erhält eine wiederholbare Löschlogik für Datenbank
   und Storage.

## Architekturentscheidungen

- **Storage-Backend:** vorhandener S3-kompatibler Bucket.
- **Upload-Flow:** zunächst serverseitiger Upload in den Bucket, nicht
  browserseitiger Direct Upload per Presigned URL.
- **Download-Strategie:** Admin-Downloads bleiben auth-geschützt über das
  Backend; die konkrete Handler-Auslieferung folgt in Wave 3.
- **DB bleibt Source of Truth:** `career_application_attachments.storage_key`
  referenziert das Objekt im Bucket.
- **Keine Mail direkt aus Handlern:** Rejection-Mail wird aus der
  Service-Schicht ausgelöst.
- **Retention ist idempotent:** erneute Ausführung des Löschprozesses darf
  keine inkonsistenten Zustände erzeugen.

## Erwartete Konfiguration

Die Service-Schicht soll auf eine schlanke Storage-Konfiguration vorbereitet
werden:

- `S3_ENDPOINT`
- `S3_REGION`
- `S3_BUCKET`
- `S3_ACCESS_KEY_ID`
- `S3_SECRET_ACCESS_KEY`
- optional `S3_USE_PATH_STYLE`

Railway-Bucket-Credentials werden in der Deploy-Umgebung als Variablen
injiziert und auf dieses Config-Schema gemappt.

## Tasks

### 1. Storage-Abstraktion für Attachments

Neue Service-/Adapter-Grenze, z. B.:

- `PutObject(ctx, key, contentType, size, body)`
- `OpenObject(ctx, key)` oder Download-Read-Schnittstelle für Wave 3
- `DeleteObject(ctx, key)`

Konkreter Adapter:

- S3-kompatible Implementierung für den vorhandenen Bucket
- Keine Bucket-spezifischen Details in Handlern oder Domain-Services
- Fehler sauber typisieren:
  - transienter Storage-Fehler
  - Objekt nicht gefunden
  - Fehlkonfiguration

### 2. Attachment-Service

- Eingehende Datei-Streams validieren:
  - erlaubte MIME-Typen
  - maximale Dateigröße
  - originaler Dateiname normalisiert speichern
- SHA-256-Checksumme erzeugen.
- Storage-Key deterministisch generieren, z. B. pro Bewerbung und Attachment-ID
  bzw. UUID-basiert.
- Reihenfolge konsistent halten:
  1. Metadaten vorbereiten
  2. Objekt in Bucket schreiben
  3. Metadaten in `career_application_attachments` persistieren
  4. Bei DB-Fehler Objekt wieder entfernen oder Cleanup markieren
- `scan_status` bewusst weiterhin auf vorbereitetem Stand halten; echter
  Virenscan bleibt außerhalb dieser Wave.

### 3. Repository-Erweiterungen

Benötigte DB-Operationen:

- Attachment-Metadaten anlegen
- Attachments je Bewerbung listen
- Attachments für Cleanup laden
- Bewerbungen selektieren, deren `delete_after <= now()`
- Rejection-Felder atomar aktualisieren
- optional Bewerbungsdatensatz für Cleanup sperren/claimen, falls nötig

### 4. Rejection-Service

Beim Statuswechsel auf `rejected`:

- `rejected_at` setzen, falls noch nicht gesetzt
- `delete_after = rejected_at + 90 days`
- `deletion_marked_at` setzen
- Ablehnungs-Mail auslösen
- `rejection_email_sent_at` nur nach erfolgreichem Versand setzen
- Wiederholte Updates auf `rejected` dürfen:
  - keine zweite Mail erzeugen
  - `delete_after` nicht verschieben

Fehlerstrategie:

- Wenn der Mailversand fehlschlägt, muss der Zustand explizit sichtbar bleiben.
  Wave 2 soll klar entscheiden und testen, ob:
  - der Statuswechsel insgesamt fehlschlägt, oder
  - die Bewerbung rejected bleibt, aber `rejection_email_sent_at` leer bleibt
    und später retrybar ist.

Pragmatische Empfehlung für diese Iteration:

- **Status bleibt rejected, Mail-Fehler wird retrybar modelliert.**
  Sonst kann ein temporärer SMTP-Fehler fachliche Statusarbeit blockieren.

### 5. Mail-Service für Ablehnungen

- Neue Service-Grenze für E-Mail-Versand, z. B. `SendRejectionEmail`.
- Template-Daten:
  - Bewerbername
  - Position
  - Firmenname
  - klarer Ablehnungstext
- Keine PII oder Mail-Inhalte in Logs.
- Versand idempotent über `rejection_email_sent_at`.

### 6. Retention-Service

- Fällige Bewerbungen selektieren:
  - `status = 'rejected'`
  - `delete_after <= now()`
- Für jede Bewerbung:
  - Attachments laden
  - physische Objekte im Bucket löschen
  - Attachment-Metadaten löschen
  - Bewerbung löschen
- Der Service muss wiederholbar sein und Teilfehler robust behandeln:
  - fehlendes Objekt im Bucket darf den Cleanup nicht dauerhaft blockieren
  - echte Storage-Ausfälle werden sauber zurückgegeben

### 7. Tests

Unit-/Service-Tests für:

- erlaubte und verbotene Dateitypen
- Größenlimit
- Checksumme/Storage-Key-Verhalten
- erfolgreicher S3-Adapter-Aufruf über Fake/Mock-Schnittstelle
- Rejection-Workflow exakt einmal
- `delete_after` = `rejected_at + 90 days`
- Mail-Failure-Strategie
- Cleanup-Entscheidung und Wiederholbarkeit

## Akzeptanzkriterien

- [ ] Die Domain kennt einen abstrahierten Storage-Port und eine
      S3-kompatible Implementierung.
- [ ] Attachment-Service validiert Datei-Metadaten und persistiert Bucket-Key +
      DB-Metadaten konsistent.
- [ ] Storage-/DB-Fehler führen nicht zu stillen Inkonsistenzen.
- [ ] Ablehnung setzt Retention-Felder deterministisch und verschickt die
      Ablehnungs-Mail genau einmal.
- [ ] Mail-Fehler sind klar modelliert und retrybar.
- [ ] Cleanup-Service löscht fällige abgelehnte Bewerbungen inklusive
      Attachments und Bucket-Objekten wiederholbar.
- [ ] Keine sensitiven Dateiinhalte, Objekt-Keys mit PII oder Mail-Bodies in
      Logs.
- [ ] Service-Tests decken Upload, Rejection und Retention-Regeln ab.

## Out of Scope

- HTTP-Upload-/Download-Handler selbst — Wave 3.
- UI-Bindung der Datei-Uploads und Downloads — Wave 4.
- Presigned Direct Uploads und öffentliche Bucket-CORS-Konfiguration.
- Echte Malware-Scanning-Pipeline.

## Umsetzung 2026-05-14

- `submission/storage_s3.go` — S3-kompatibler `ObjectStorage`-Adapter mit
  `PutObject` / `OpenObject` / `DeleteObject` und getypten Storage-Fehlern
  (`ErrStorageObjectGone`, `ErrStorageUnavailable`).
- `submission/career_wave2.go` — Attachment-Service mit MIME-Validierung
  (PDF/DOC/DOCX), Magic-Byte-Check gegen Content-Type-Spoofing, Größenlimit,
  SHA-256, deterministischer Storage-Key und Rollback-Reihenfolge
  Storage → DB.
- `submission/career_wave2.go` — `RejectCareerApplication` setzt
  `rejected_at`, `delete_after = rejected_at + 90 days` und
  `deletion_marked_at` idempotent über `coalesce`; Mailversand idempotent
  über `rejection_email_sent_at` separat persistiert.
- `submission/mail_smtp.go` — SMTP-Mailer mit Template ohne PII-Logging.
- `submission/career_wave2.go` — `DeleteRejectedApplicationsDueForRetention`
  iteriert fällige Bewerbungen, löscht Storage-Objekte (tolerant gegenüber
  `ErrStorageObjectGone`), Attachment-Metadaten und Bewerbung. Re-Run sicher.
- `submission/repository.go` + `model.go` — neue DB-Operationen und
  Retention-Felder am `CareerApplication`-Modell.
- `cmd/api/main.go` — Cleanup-Loop (`runRetentionCleanupLoop`) mit
  konfigurierbarem Intervall + Limit.

## Verifikation 2026-05-14

- `go test ./internal/submission/...` deckt:
  - Upload-Validierung erlaubter/verbotener MIME-Typen
  - Content-Type-Mismatch (Magic-Byte)
  - Größenlimit
  - Storage-Persistenz über Fake-Adapter
  - Rejection-Workflow: setzt Retention deterministisch, sendet Mail genau
    einmal
  - Mail-Failure: Status bleibt `rejected`, `rejection_email_sent_at` leer,
    retrybar
  - Retention-Löschung über Storage und DB, tolerant gegenüber fehlendem
    Bucket-Objekt
