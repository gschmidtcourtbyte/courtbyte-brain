# Wave 1: Datenvertrag Hansefit-ID

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-13.md`

## Legacy-Inhalt

## Wave 1 - Datenvertrag Hansefit-ID
**Agenten**: Agent A, Agent C
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Backend kann die Hansefit-ID speichern, lesen und validieren, ohne bestehende Anfragen zu brechen.

### Task: 13.1 Migration fuer Hansefit-ID
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A

**Akzeptanzkriterien**:
- [ ] Neue Migration `003_hansefit_id.up.sql` fuegt `hansefit_id` zu `contact_submissions` hinzu.
- [ ] Down-Migration entfernt nur `hansefit_id`.
- [ ] Bestehende Datensaetze bleiben gueltig.
- [ ] Normale Kontaktanfragen duerfen `hansefit_id = NULL` behalten.
- [ ] Migration ist mit lokaler Docker-Postgres-DB ausfuehrbar.

**Kontext**:
- Bestehende Migrationen: `migrations/001_contact_submissions.up.sql`, `migrations/002_hansefit_admin.up.sql`.
- Repository: `internal/repository/contact_repo.go`.

**Output**:
- `migrations/003_hansefit_id.up.sql`
- `migrations/003_hansefit_id.down.sql`

### Task: 13.2 Model, Repository und Service-Vertrag erweitern
**Priority**: P0
**Blocked by**: 13.1
**Agent**: Agent C

**Akzeptanzkriterien**:
- [ ] `ContactSubmission` enthaelt `HansefitID`.
- [ ] `ContactSubmissionRequest` enthaelt `HansefitID`.
- [ ] Create-/Scan-Queries beruecksichtigen `hansefit_id`.
- [ ] Hansefit-ID wird getrimmt und leere Werte werden als `nil` behandelt.
- [ ] Validierung: Wenn `Hansefitregistrierung` aktiv ist, ist Hansefit-ID Pflicht.
- [ ] Validierung: Wenn Hansefit-ID ohne Checkbox angegeben wird, bleibt normale Anfrage erlaubt.
- [ ] Existing Tests fuer Playtomic bleiben gruene Regression.

**Kontext**:
- `internal/model/contact.go`
- `internal/model/contact_test.go`
- `internal/service/contact_service.go`
- `internal/repository/contact_repo.go`

**Output**:
- Aktualisiertes Contact-Model
- Aktualisiertes Contact-Repository
- Erweiterte Validierungstests

**Wave-Exit-Gate**:
- [ ] Migrationen sind reviewbar.
- [ ] Domain-/Service-Vertrag ist konsistent.
- [ ] `go test ./internal/model ./internal/repository ./internal/service` laeuft oder dokumentiert bekannte Luecken.

---
