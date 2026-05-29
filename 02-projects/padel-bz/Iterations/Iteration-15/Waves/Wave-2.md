# Wave 2: Hansefit-ID Validierung (Backend + Frontend, parallel)

**Status:** Done (Legacy-Migration)
**Skill:** service-eng, frontend-eng, testing-eng
**Quelle:** `plan/iterations/iteration-15.md`

## Legacy-Inhalt

## Wave 2 — Hansefit-ID Validierung (Backend + Frontend, parallel)
**Agenten**: Agent A, Agent B, Agent E
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 (wegen Formular-Aenderungen)
**Ziel**: Die Hansefit-ID wird auf das vorgegebene Format validiert.

### Task 15.4: Hansefit-ID Format-Validierung im Model
**Priority**: P0
**Blocked by**: none
**Agent**: Agent B (service-eng)

**Akzeptanzkriterien**:
- [ ] Neues Regex oder Validierungsfunktion in `internal/model/contact.go`.
- [ ] Format: genau 8 Zeichen, beginnt mit 2 Ziffern, enthaelt mindestens 1 Buchstaben, Rest alphanumerisch.
- [ ] Validierung greift nur bei `HansefitRegistration == true`.
- [ ] Fehlermeldung ist benutzerfreundlich, z.B.: "Hansefit-ID muss 8 Zeichen lang sein, mit 2 Ziffern beginnen und mindestens einen Buchstaben enthalten."
- [ ] Bestehende Leerstellen-Pruefung bleibt erhalten (leere ID wird weiterhin separat abgefangen).
- [ ] Bereits gespeicherte IDs in der Datenbank werden nicht nachtraeglich invalidiert.

**Kontext**:
- `internal/model/contact.go` (Zeilen 81-94: aktuelle Validierung)
- Hansefit-ID Format: Start mit 2 Ziffern, min. 1 Buchstabe, genau 8 Zeichen, alphanumerisch.

**Output**:
- Aktualisierte Validate()-Funktion in contact.go.

### Task 15.5: Hansefit-ID Client-seitige Validierung
**Priority**: P1
**Blocked by**: 15.3 (Formular-Aenderungen)
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] HTML5 `pattern`-Attribut auf dem Hansefit-ID Input-Feld.
- [ ] `title`-Attribut mit Hinweis zum erwarteten Format.
- [ ] `minlength="8"` und `maxlength="8"` Attribute gesetzt.
- [ ] Placeholder wird angepasst (z.B. "12ABC345").
- [ ] Browser blockiert ungueltige Eingaben bei nativem Submit.
- [ ] Client-Validierung ist UX-Ergaenzung — Server bleibt Quelle der Wahrheit.

**Kontext**:
- `web/templates/pages/contact.html` (Zeilen 118-128)

**Output**:
- Aktualisiertes Hansefit-ID Input-Feld.

### Task 15.6: Tests fuer Hansefit-ID Validierung
**Priority**: P0
**Blocked by**: 15.4
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Gueltige IDs: "12AB3456", "99xABCDE", "01aB2C3D" passieren Validierung.
- [ ] Ungueltige IDs (zu kurz, zu lang, ohne Ziffern-Start, ohne Buchstabe, nur Ziffern) werden abgelehnt.
- [ ] Bestehende Validierungstests fuer leere ID bleiben gruen.
- [ ] `go test ./internal/model/...` ist gruen.

**Kontext**:
- `internal/model/contact_test.go`
- Erwartete Validierungsregeln aus Task 15.4.

**Output**:
- Erweiterte Test-Suite fuer Hansefit-ID Format.

**Wave-Exit-Gate**:
- [ ] Server lehnt ungueltige Hansefit-IDs ab.
- [ ] Client zeigt Format-Hinweis und blockiert ungueltige Eingaben.
- [ ] Tests decken gueltige und ungueltige Formate ab.

---
