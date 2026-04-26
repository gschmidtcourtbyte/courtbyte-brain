# Wave 1: Datenbasis und Kontaktvertrag

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-12.md`

## Legacy-Inhalt

## Wave 1 - Datenbasis und Kontaktvertrag
**Agenten**: Agent A, Agent C
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Datenbank, Modell und Service-Vertrag koennen neue Hansefit-Felder speichern und filtern.

### Task: 12.1 Contact-Migration erweitern
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A

**Akzeptanzkriterien**:
- [x] Neue Migration `002_hansefit_admin.up.sql` erweitert `contact_submissions`.
- [x] Felder vorhanden: `playtomic_email`, `hansefit_status`, `hansefit_processed_at`, `hansefit_confirmation_sent_at`.
- [x] `hansefit_status` erlaubt mindestens `unprocessed` und `processed`.
- [x] Bestehende Datensaetze bleiben gueltig.
- [x] Index fuer Admin-Liste vorhanden: Hansefit-Anfragen nach Status/Erstellzeit.
- [x] Down-Migration entfernt nur die neuen Felder/Indizes.

**Kontext**:
- Bestehende Tabelle: `migrations/001_contact_submissions.up.sql`.
- Admin-Liste soll nur `hansefit_registration = true` anzeigen.

**Output**:
- `migrations/002_hansefit_admin.up.sql`
- `migrations/002_hansefit_admin.down.sql`

### Task: 12.2 Contact-Model und Repository anpassen
**Priority**: P0
**Blocked by**: 12.1
**Agent**: Agent C

**Akzeptanzkriterien**:
- [x] `ContactSubmission` enthaelt `PlaytomicEmail`, `HansefitStatus`, `HansefitProcessedAt`, `HansefitConfirmationSentAt`.
- [x] `ContactSubmissionRequest` enthaelt `PlaytomicEmail`.
- [x] Create/Scan-Queries beruecksichtigen neue Felder.
- [x] Repository bietet Admin-Query `ListHansefitRegistrations`.
- [x] Repository bietet Status-Update mit Schutz gegen doppelte Bestaetigung.
- [x] Nicht-Hansefit-Anfragen bleiben speicherbar und werden nicht in der Hansefit-Liste ausgegeben.

**Kontext**:
- `internal/model/contact.go`
- `internal/repository/contact_repo.go`

**Output**:
- Aktualisiertes Contact-Model
- Aktualisiertes Contact-Repository

**Wave-Exit-Gate**:
- [x] Migrationen sind reviewbar.
- [x] Kontaktmodell kann neue Daten speichern.
- [x] Admin-Filter ist technisch vorbereitet.

---
