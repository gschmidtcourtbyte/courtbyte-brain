# Wave 1: Vertrag und Datenbasis

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-14.md`

## Legacy-Inhalt

## Wave 1 - Vertrag und Datenbasis
**Agenten**: PO Lead, Agent A, Agent B, Agent C
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Der fachliche Formularvertrag und die persistente AGB-Zustimmung sind eindeutig definiert.

### Task: 14.1 Formularvertrag finalisieren
**Priority**: P0
**Blocked by**: none
**Agent**: PO Lead

**Akzeptanzkriterien**:
- [ ] Pflichtfelder sind dokumentiert: Nachname, Vorname, E-Mail, Datenschutz, AGB.
- [ ] Konditionale Pflichtfelder sind dokumentiert: Hansefit-ID und Playtomic-E-Mail nur bei Hansefit.
- [ ] Optionales Freitextfeld bleibt immer sichtbar.
- [ ] Telefon bleibt optional oder wird separat als P2-Entscheidung markiert.
- [ ] AGB-Content-Blocker ist explizit dokumentiert, falls kein finaler Text vorliegt.

**Kontext**:
- `web/templates/pages/contact.html`
- `internal/model/contact.go`
- `plan/iterations/iteration-13.md`

**Output**:
- Finaler Formularvertrag in dieser Iterationsdatei.

### Task: 14.2 AGB-Zustimmung im Datenmodell vorbereiten
**Priority**: P0
**Blocked by**: 14.1
**Agent**: Agent A

**Akzeptanzkriterien**:
- [ ] Neue Migration fuegt `terms_accepted BOOLEAN NOT NULL DEFAULT FALSE` zu `contact_submissions` hinzu.
- [ ] Down-Migration entfernt nur `terms_accepted`.
- [ ] Bestehende Datensaetze bleiben gueltig.
- [ ] Repository-Insert und Scan-Queries koennen das neue Feld verarbeiten.
- [ ] Keine neue Indexierung, da das Feld nicht fuer Listen/Filter genutzt wird.

**Kontext**:
- `migrations/001_contact_submissions.up.sql`
- `migrations/003_hansefit_id.up.sql`
- `internal/repository/contact_repo.go`

**Output**:
- `migrations/004_terms_accepted.up.sql`
- `migrations/004_terms_accepted.down.sql`
- Repository-Vertrag fuer `terms_accepted`

### Task: 14.3 Request- und Domain-Modell erweitern
**Priority**: P0
**Blocked by**: 14.1
**Agent**: Agent B

**Akzeptanzkriterien**:
- [ ] `ContactSubmission` enthaelt `TermsAccepted`.
- [ ] `ContactSubmissionRequest` enthaelt `TermsAccepted`.
- [ ] Validierung verlangt AGB-Zustimmung immer.
- [ ] Bestehende Datenschutz-Validierung bleibt unveraendert verpflichtend.
- [ ] Hansefit-Felder werden bei nicht aktivierter Registrierung normalisiert/ignoriert.

**Kontext**:
- `internal/model/contact.go`
- `internal/service/contact_service.go`
- `internal/model/contact_test.go`

**Output**:
- Aktualisiertes Model und Service-Normalisierung.
- Unit-Tests fuer AGB-Zustimmung und Hansefit-Normalisierung.

**Wave-Exit-Gate**:
- [ ] Der Formularvertrag ist eindeutig.
- [ ] AGB-Zustimmung ist technisch modelliert.
- [ ] Backend kann neue Consent-Daten speichern, ohne bestehende Daten zu brechen.

---
