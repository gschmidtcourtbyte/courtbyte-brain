# Wave 5: Tests, QA und Review

**Status:** Done (Legacy-Migration)
**Skill:** product-own
**Quelle:** `plan/iterations/iteration-13.md`

## Legacy-Inhalt

## Wave 5 - Tests, QA und Review
**Agenten**: Agent E, Agent R, PO Lead
**Kann einzeln ausgefuehrt werden**: ja, nach Waves 1-4
**Ziel**: Feinschliff ist regression-sicher und abnahmefaehig.

### Task: 13.10 Regressionstests und manuelle QA
**Priority**: P1
**Blocked by**: 13.3, 13.5, 13.7, 13.8
**Agent**: Agent E

**Akzeptanzkriterien**:
- [ ] `go test ./...` laeuft erfolgreich.
- [ ] Hansefit-ID-Pflicht bei aktiver Checkbox ist getestet.
- [ ] Formular ohne Hansefit-Checkbox funktioniert ohne Hansefit-ID.
- [ ] Hansefit-Anfrage erscheint mit Hansefit-ID in Admin-Tabelle.
- [ ] Statuswechsel `unbearbeitet` -> `bearbeitet` funktioniert weiter.
- [ ] Doppelte Bestaetigung wird weiterhin verhindert.
- [ ] E-Mail-Template rendert mit Vorname, Nachname, Playtomic-E-Mail und Hansefit-ID.
- [ ] Login ist zentral und responsive geprueft.
- [ ] Dashboard-Kennzahlen und Tabelle sind plausibel.
- [ ] Bautagebuch-Auswahl funktioniert auf Desktop und Mobile.
- [ ] No-JS-Fallback zeigt `Aktueller Fortschritt`.
- [ ] Zusaetzliches Baufortschritt-Bild ist entfernt.

**Kontext**:
- Bestehende Tests: `internal/model/contact_test.go`, `internal/config/config_test.go`.
- Manuelle QA per lokalem Docker-Stack `http://localhost:8081`.

**Output**:
- Neue/aktualisierte Tests
- QA-Status in dieser Datei

### Task: 13.11 Security- und Merge-Review
**Priority**: P1
**Blocked by**: 13.10
**Agent**: Agent R

**Akzeptanzkriterien**:
- [ ] Admin-Routen bleiben ohne Session nicht erreichbar.
- [ ] CSRF-Schutz fuer Login, Logout und Statuswechsel bleibt aktiv.
- [ ] Hansefit-ID wird nicht unnoetig in Logs ausgegeben.
- [ ] E-Mail-Template-Fehler fuehren nicht zu falschem Status `bearbeitet`.
- [ ] Migration ist rueckrollbar.
- [ ] Test-Admin-Risiko bleibt dokumentiert.
- [ ] Offene Risiken sind dokumentiert.

**Output**:
- Review-Notizen
- Finaler Iterationsstatus

**Wave-Exit-Gate**:
- [ ] Tests und Review abgeschlossen.
- [ ] Abnahmekriterien sind nachvollziehbar erfuellt oder Rest-Risiken dokumentiert.

---
