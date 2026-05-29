# Wave 2 — Schema für Kontaktanfragen + Bewerbungen

**Skill:** `/migration-eng` + `/database-eng`
**Status:** Implemented; Railway migration smoke pending (2026-04-29)
**Blocked by:** Wave 1 nicht fachlich blockierend; Wave 1 bleibt P0 zuerst

## Ziel

PostgreSQL-Schema für öffentliche Kontaktanfragen und Karriere-Bewerbungen
einführen. Das Schema muss Admin-Inbox-Statusfelder enthalten, damit Iteration 5
ohne erneute Schema-Grundsatzänderung die UI an echte Daten binden kann.

## Tasks

### 1. Migration `002_public_submissions.up.sql`

Tabellen:

- `contact_inquiries`
  - `id bigserial primary key`
  - `name text not null`
  - `email text not null`
  - `phone text`
  - `subject text not null`
  - `message text not null`
  - `status text not null default 'new'`
  - `is_read boolean not null default false`
  - `ip_address text`
  - `user_agent text`
  - `created_at timestamptz not null default now()`
  - `updated_at timestamptz not null default now()`
- `career_applications`
  - `id bigserial primary key`
  - `first_name text not null`
  - `last_name text not null`
  - `email text not null`
  - `phone text`
  - `position text not null`
  - `message text`
  - `status text not null default 'new'`
  - `is_read boolean not null default false`
  - `ip_address text`
  - `user_agent text`
  - `created_at timestamptz not null default now()`
  - `updated_at timestamptz not null default now()`
- `career_application_attachments`
  - `id bigserial primary key`
  - `application_id bigint not null references career_applications(id) on delete cascade`
  - `attachment_type text not null default 'document'`
  - `original_filename text not null`
  - `content_type text not null`
  - `size_bytes bigint not null`
  - `storage_provider text not null default 'local'`
  - `storage_key text not null`
  - `checksum_sha256 text`
  - `scan_status text not null default 'pending'`
  - `created_at timestamptz not null default now()`
  - `updated_at timestamptz not null default now()`

Constraints:

- Not-blank Checks für Pflichtfelder.
- Status-Checks:
  - Contact: `new`, `in_progress`, `done`
  - Application: `new`, `interview`, `accepted`, `rejected`
- Attachment-Checks:
  - `attachment_type`: `resume`, `cover_letter`, `certificate`, `portfolio`,
    `document`, `other`
  - `scan_status`: `pending`, `clean`, `infected`, `failed`
  - Dateiname, MIME-Type, Storage Provider und Storage Key nicht leer
  - `size_bytes > 0`

Indexes:

- `(status, created_at DESC)`
- `(is_read, created_at DESC)`
- `created_at DESC`
- `lower(email)` optional für spätere Suche.
- Attachment:
  - unique `storage_key`
  - `(application_id, created_at DESC)`
  - `(scan_status, created_at DESC)`

### 2. Migration Runner prüfen

- Sicherstellen, dass `backend/internal/migrations/migrations.go` die neue
  Migration einbettet und sortiert ausführt.
- Keine Down-Migration nötig, da aktueller Runner nur `.up.sql` unterstützt.

### 3. DB-Contract dokumentieren

- Statuswerte mit deutschem UI-Mapping für spätere Iteration 5 festhalten.
- File-Upload-Binaries bleiben out of scope; Wave 2 legt nur
  Attachment-Metadaten und Storage-Referenzen für spätere Upload-Verarbeitung
  an.

## Akzeptanzkriterien

- [x] Migration ist idempotent über `CREATE TABLE IF NOT EXISTS` /
      `CREATE INDEX IF NOT EXISTS`.
- [x] Pflichtfelder und Statuswerte sind per Constraint abgesichert.
- [x] Inbox-relevante Indexe existieren.
- [x] `go test ./...` läuft gegen den Migration-Embed ohne Fehler.
- [ ] Railway-Staging kann Migration beim Backend-Start anwenden.

## Umsetzung 2026-04-29

- `backend/internal/migrations/sql/002_public_submissions.up.sql` erstellt:
  - `contact_inquiries`
  - `career_applications`
  - `career_application_attachments`
- `backend/internal/migrations/migrations_test.go` prüft, dass Migrationen
  eingebettet, sortiert und die erwarteten Tabellen in Wave 2 enthalten sind.
- Upload-Hinweis berücksichtigt: Die Attachment-Tabelle speichert nur Metadaten
  und Storage-Keys. Die eigentliche Dateiannahme, Validierung, Speicherung und
  Virenprüfung bleibt bei den späteren Upload-/Handler-Waves.

## DB-Contract

| Tabelle | Statuswerte | Deutsches UI-Mapping |
|---|---|---|
| `contact_inquiries` | `new`, `in_progress`, `done` | Neu, In Bearbeitung, Erledigt |
| `career_applications` | `new`, `interview`, `accepted`, `rejected` | Neu, Im Gespräch, Angenommen, Abgelehnt |
| `career_application_attachments` | `pending`, `clean`, `infected`, `failed` | Ausstehend, Freigegeben, Blockiert, Fehlgeschlagen |

## Verifikation 2026-04-29

- `go test ./...` — clean
- Lokale PostgreSQL-Prüfung in temporärer DB `wave2_migration_check`:
  - Migration `001_admin_auth.up.sql` angewendet
  - Migration `002_public_submissions.up.sql` angewendet
  - Tabellen `contact_inquiries`, `career_applications`,
    `career_application_attachments` vorhanden
  - 14 erwartete Tabellen-/Inbox-/Attachment-Indexe vorhanden
  - Positiver Insert für Kontakt, Bewerbung und Attachment erfolgreich
  - Negativer Insert mit ungültigem Bewerbungsstatus korrekt durch
    `career_applications_status_valid` abgelehnt
  - Temporäre Prüfdatenbank anschließend gelöscht

## Out of Scope

- Dateiannahme, Binary-Storage, Download-Handler und Virenscan-Ausführung.
- Mail-Queue / Notification-Tabellen.
- Volltextsuche.
