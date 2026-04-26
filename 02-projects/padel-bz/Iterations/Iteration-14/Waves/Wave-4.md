# Wave 4: Regression, Doku und Review

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-14.md`

## Legacy-Inhalt

## Wave 4 - Regression, Doku und Review
**Agenten**: Agent E, Agent Doc, Agent R, PO Lead
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 3
**Ziel**: Die Iteration ist testbar, dokumentiert und merge-ready.

### Task: 14.10 Automatisierte Tests ausbauen
**Priority**: P0
**Blocked by**: 14.4, 14.5, 14.8
**Agent**: Agent E

**Akzeptanzkriterien**:
- [ ] Model-Tests decken alle Pflichtfeldkombinationen ab.
- [ ] Tests decken Hansefit aus/an mit fehlenden und gueltigen Zusatzfeldern ab.
- [ ] Tests decken fehlende Datenschutz- und AGB-Zustimmung ab.
- [ ] Tests decken Normalisierung versteckter Hansefit-Felder ab.
- [ ] `go test ./...` ist gruen oder Abweichungen sind dokumentiert.

**Kontext**:
- `internal/model/contact_test.go`
- `internal/service/contact_service_test.go`
- optional neue Handler-Tests

**Output**:
- Aktualisierte Test-Suite.
- Testlauf-Protokoll.

### Task: 14.11 Manuelle QA-Matrix ausfuehren
**Priority**: P0
**Blocked by**: 14.8
**Agent**: Agent E

**Akzeptanzkriterien**:
- [ ] Normale Anfrage mit Nachname, Vorname, E-Mail, Datenschutz, AGB und Freitext optional funktioniert.
- [ ] Normale Anfrage ohne Freitext funktioniert.
- [ ] Normale Anfrage ohne Datenschutz wird blockiert.
- [ ] Normale Anfrage ohne AGB wird blockiert.
- [ ] Hansefit-Anfrage ohne Hansefit-ID wird blockiert.
- [ ] Hansefit-Anfrage ohne Playtomic-E-Mail wird blockiert.
- [ ] Hansefit-Anfrage mit allen Feldern funktioniert.
- [ ] Hansefit-Felder sind beim ersten Laden nicht sichtbar.
- [ ] Hansefit-Felder verschwinden beim Deaktivieren wieder.
- [ ] Mobile Formularbedienung funktioniert.

**Kontext**:
- Lokale App
- Browser-Validierung
- Server-Validierung

**Output**:
- QA-Ergebnis in dieser Iterationsdatei oder separatem Statuskommentar.

### Task: 14.12 Dokumentation aktualisieren
**Priority**: P1
**Blocked by**: 14.6, 14.7
**Agent**: Agent Doc

**Akzeptanzkriterien**:
- [ ] `docs/Content.md` beschreibt die neuen Formularlabels und Pflichtfelder.
- [ ] Datenschutzerklaerung/AGB-Texte oder Content-Blocker sind dokumentiert.
- [ ] `docs/Operations.md` beschreibt, welche Felder fuer Hansefit-Anfragen im Admin relevant sind.
- [ ] Iterationsstatus und Restrisiken sind aktualisiert.

**Kontext**:
- `docs/Content.md`
- `docs/Operations.md`
- `plan/iterations/iteration-14.md`

**Output**:
- Aktualisierte Doku.

### Task: 14.13 Finaler Review
**Priority**: P0
**Blocked by**: 14.10, 14.11, 14.12
**Agent**: Agent R

**Akzeptanzkriterien**:
- [ ] Keine Pflichtfeld-Regressionsluecke zwischen Frontend und Backend.
- [ ] AGB-/Datenschutz-Zustimmung ist nicht nur UI-seitig abgesichert.
- [ ] Hansefit-Felder werden nicht versehentlich fuer normale Kontaktanfragen gespeichert.
- [ ] CSRF, Rate-Limit und SMTP-Flows sind nicht regressiert.
- [ ] Migrationen sind rueckrollbar.
- [ ] Offene Content-/Legal-Risiken sind klar als Blocker oder Restrisiko markiert.

**Kontext**:
- Vollstaendiger Diff der Iteration.

**Output**:
- Review-Findings.
- Merge-Readiness-Entscheidung.

**Wave-Exit-Gate**:
- [ ] Automatisierte Tests sind gruen oder Abweichungen dokumentiert.
- [ ] Manuelle QA-Matrix ist bestanden.
- [ ] Doku und Restrisiken sind aktualisiert.
- [ ] PO Lead gibt Iteration 14 frei.
