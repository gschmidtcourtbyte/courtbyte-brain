# Wave 3: Tests, QA und Dokumentation

**Status:** Done (Legacy-Migration)
**Skill:** database-eng, frontend-eng, testing-eng, documentation-eng, review-eng
**Quelle:** `plan/iterations/iteration-16.md`

## Legacy-Inhalt

## Wave 3 - Tests, QA und Dokumentation
**Agenten**: Agent A, Agent E, Agent F, Agent R
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 und Wave 2
**Ziel**: Die Iteration ist regressionsarm, reviewfaehig und dokumentiert.

### Task 16.8: Automatisierte Tests fuer Admin-Suche
**Priority**: P0
**Blocked by**: 16.2, 16.3, 16.4
**Agent**: Agent A (testing-eng), Agent B (database-eng)

**Akzeptanzkriterien**:
- [ ] Test deckt Treffer ueber Vorname ab.
- [ ] Test deckt Treffer ueber Nachname ab.
- [ ] Test deckt Treffer ueber E-Mail oder Playtomic-E-Mail ab.
- [ ] Test deckt Treffer ueber Hansefit-ID ab.
- [ ] Test deckt Treffer ueber Freitext ab.
- [ ] Test deckt Nicht-Treffer ab.
- [ ] Test deckt leeren Suchbegriff ab.
- [ ] Test deckt Sonderzeichen wie `%` und `_` ab.
- [ ] Test stellt sicher, dass Nicht-Hansefit-Kontaktanfragen nicht in Suchergebnissen erscheinen.
- [ ] `go test ./...` ist gruen.

**Kontext**:
- `internal/repository/contact_repo.go`
- `internal/service/contact_service.go`
- `internal/handler/admin_hansefit_test.go`
- vorhandene Teststruktur im Repo

**Output**:
- Neue oder erweiterte Tests fuer Suchverhalten.
- Gruener Testlauf.

### Task 16.9: Manuelle Admin- und Frontend-QA
**Priority**: P1
**Blocked by**: 16.4, 16.5, 16.7
**Agent**: Agent A (testing-eng), Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Admin-Suche mit Vorname zeigt nur passende Row(s).
- [ ] Admin-Suche mit Hansefit-ID zeigt nur passende Row(s).
- [ ] Admin-Suche mit Freitext zeigt nur passende Row(s).
- [ ] Admin-Suche ohne Treffer zeigt Keine-Treffer-Zustand.
- [ ] Reset-Link zeigt wieder alle Hansefit-Anfragen.
- [ ] Homepage Desktop: Team ist nicht sichtbar, Sponsoren ist letzte Section.
- [ ] Homepage Mobile: Team ist nicht sichtbar, Sponsoren-Grid bricht sauber um.
- [ ] Navigation und Footer enthalten keinen Team-Link.
- [ ] AXA-Logo laedt im Browser.
- [ ] Platzhalter wirken als Logo-Platzhalter und nicht als Textkarten.

**Kontext**:
- Lokaler Dev-Server
- Browser-Viewport Mobile und Desktop
- `/admin/hansefit`
- `/`

**Output**:
- QA-Notiz mit getesteten Szenarien und offenen Abweichungen.

### Task 16.10: Dokumentation aktualisieren
**Priority**: P2
**Blocked by**: 16.5, 16.7
**Agent**: Agent F (documentation-eng)

**Akzeptanzkriterien**:
- [ ] `docs/Content.md` markiert die Team-Section als vorerst ausgeblendet oder entfernt sie aus der aktiven Seitenstruktur.
- [ ] `docs/Content.md` enthaelt die neue Sponsoren-Section und den AXA-Logo-Platzhalterstand.
- [ ] `docs/Operations.md` beschreibt bei Bedarf die Admin-Suche kurz: GET-Parameter `q`, gefilterte Hansefit-Zeilen, Reset-Verhalten.
- [ ] Keine veralteten sichtbaren `/#team` Referenzen bleiben in Content-Doku bestehen.

**Kontext**:
- `docs/Content.md`
- `docs/Operations.md`
- `plan/iterations/iteration-16.md`

**Output**:
- Aktualisierte Dokumentation.

### Task 16.11: Finaler Review
**Priority**: P0
**Blocked by**: 16.1-16.10
**Agent**: Agent R (review-eng), PO Lead

**Akzeptanzkriterien**:
- [ ] Admin-Suche erfuellt den Suchvertrag.
- [ ] Keine SQL-Injection- oder Wildcard-Regressionsrisiken offen.
- [ ] Team-Ausblendung erzeugt keine Dead Links.
- [ ] Sponsoren-Section ist responsive und visuell konsistent.
- [ ] AXA-Logo wird aus webfaehigem Pfad geladen.
- [ ] Tests sind gruen oder bekannte Ausnahmen sind explizit dokumentiert.
- [ ] Dokumentation ist konsistent mit sichtbarer Website.
- [ ] Keine ungewollten Aenderungen ausserhalb des Iterationsscopes.

**Kontext**:
- Vollstaendiger Diff der Iteration.
- Test- und QA-Ergebnisse.

**Output**:
- Review-Findings.
- Merge-Readiness-Entscheidung.

**Wave-Exit-Gate**:
- [ ] Alle P0-Tasks abgeschlossen.
- [ ] P1-Tasks abgeschlossen oder bewusst deferred.
- [ ] Review ist blockerfrei.
- [ ] PO Lead gibt Iteration 16 frei.

---
