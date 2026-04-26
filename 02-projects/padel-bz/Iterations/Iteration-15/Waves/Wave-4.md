# Wave 4: Review und QA

**Status:** Done (Legacy-Migration)
**Skill:** review-eng
**Quelle:** `plan/iterations/iteration-15.md`

## Legacy-Inhalt

## Wave 4 — Review und QA
**Agenten**: Agent R, Agent E, PO Lead
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1-3
**Ziel**: Alle Aenderungen sind geprueft und merge-ready.

### Task 15.11: Finaler Review
**Priority**: P0
**Blocked by**: 15.1-15.10
**Agent**: Agent R (review-eng)

**Akzeptanzkriterien**:
- [ ] Social-Media-Links funktionieren (Instagram klickbar, WhatsApp verborgen).
- [ ] "Nachricht senden"-Button hat weisse Schrift.
- [ ] Kontaktformular zeigt Vorname|Nachname in korrekter Reihenfolge mit eindeutigen Labels.
- [ ] Hansefit-ID Validierung greift client- und serverseitig.
- [ ] Admin-Suche filtert korrekt und zeigt alle Ergebnisse bei leerer Suche.
- [ ] Keine Regressionen: CSRF, Rate-Limit, Hansefit-Processing, E-Mail-Versand.
- [ ] `go test ./...` ist gruen.
- [ ] Mobile Nav zeigt Social Icons korrekt.

**Kontext**:
- Vollstaendiger Diff der Iteration.

**Output**:
- Review-Findings.
- Merge-Readiness-Entscheidung.

**Wave-Exit-Gate**:
- [ ] Alle Tasks sind abgeschlossen.
- [ ] Review ist Blocker-frei.
- [ ] PO Lead gibt Iteration 15 frei.

---
