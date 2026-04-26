# Wave 3: Admin-Suchfunktion (sequenziell: Repository → Handler → Template)

**Status:** Done (Legacy-Migration)
**Skill:** database-eng, handler-eng, frontend-eng, testing-eng
**Quelle:** `plan/iterations/iteration-15.md`

## Legacy-Inhalt

## Wave 3 — Admin-Suchfunktion (sequenziell: Repository → Handler → Template)
**Agenten**: Agent C, Agent D, Agent A, Agent E
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1
**Ziel**: Admins koennen in der Hansefit-Tabelle nach allen sichtbaren Feldern suchen.

### Task 15.7: Search-Query im Repository
**Priority**: P1
**Blocked by**: none
**Agent**: Agent C (database-eng)

**Akzeptanzkriterien**:
- [ ] Neue Methode `SearchHansefitRegistrations(ctx, query string)` im ContactRepository.
- [ ] SQL nutzt `ILIKE` mit `%query%` ueber: first_name, last_name, email, playtomic_email, hansefit_id, message.
- [ ] Leerer Suchbegriff liefert alle Ergebnisse (Fallback auf `ListHansefitRegistrations`).
- [ ] `playtomic_email`, `hansefit_id` und `message` sind nullable — `COALESCE` oder `IS NOT NULL`-Guard verwenden.
- [ ] Sortierung: offene Anfragen zuerst, dann nach Erstelldatum absteigend (wie ListHansefitRegistrations).
- [ ] SQL-Injection durch parametrisierte Queries ausgeschlossen.
- [ ] Performance: Fuer die erwartete Datenmenge (<1000 Zeilen) ist `ILIKE` ohne Index akzeptabel.

**Kontext**:
- `internal/repository/contact_repo.go` (Zeilen 104-136: ListHansefitRegistrations)
- DB-Schema: `migrations/001_contact_submissions.up.sql`, `migrations/002_hansefit_admin.up.sql`

**Output**:
- Neue Repository-Methode mit parametrisierter ILIKE-Suche.

### Task 15.8: Search-Handler und Route
**Priority**: P1
**Blocked by**: 15.7
**Agent**: Agent D (handler-eng)

**Akzeptanzkriterien**:
- [ ] Bestehende `List`-Methode in `admin_hansefit.go` liest `q` GET-Parameter.
- [ ] Bei nicht-leerem `q`: Repository-Search aufrufen statt List.
- [ ] `q` wird an das Template durchgereicht, damit das Suchfeld den Wert behaelt.
- [ ] Keine neue Route noetig — Suche laeuft ueber bestehende `/admin/hansefit?q=...`.
- [ ] Eingabe wird getrimmt und auf max. 200 Zeichen begrenzt.
- [ ] Leere Suche zeigt weiterhin alle Ergebnisse.

**Kontext**:
- `internal/handler/admin_hansefit.go` (Zeilen 33-54: List)
- `internal/service/contact_service.go` (Zeile 115: ListHansefitRegistrations)
- `cmd/server/main.go` (Router-Setup)

**Output**:
- Aktualisierter Hansefit-Handler mit Search-Logik.
- Service-Methode fuer Search (Durchreichung zum Repository).

### Task 15.9: Such-UI im Admin-Template
**Priority**: P1
**Blocked by**: 15.8
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Suchfeld oberhalb der Tabelle mit Label "Suche" und Placeholder "Name, E-Mail, Hansefit-ID...".
- [ ] Formular nutzt GET-Methode auf `/admin/hansefit`.
- [ ] Suchfeld behaelt den eingegebenen Wert nach Absenden (`value="{{.SearchQuery}}"`).
- [ ] Optional: "Zuruecksetzen"-Link der die Suche leert.
- [ ] Design passt zum bestehenden Admin-Stil (dunkles Theme, `.admin-panel` Kontext).
- [ ] Tabelle zeigt gefilterte Ergebnisse oder "Keine Treffer" Meldung.
- [ ] CSRF ist nicht noetig (GET-Request).

**Kontext**:
- `web/templates/admin/hansefit.html`
- `web/static/css/style.css`

**Output**:
- Aktualisiertes Admin-Hansefit-Template mit Suchfeld.
- CSS fuer Suchfeld-Styling.

### Task 15.10: Tests fuer Suchfunktion
**Priority**: P1
**Blocked by**: 15.7, 15.8
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Service-Layer-Test: Search mit Treffer und ohne Treffer.
- [ ] Leere Suche liefert vollstaendige Liste.
- [ ] Sonderzeichen im Suchbegriff fuehren nicht zu SQL-Fehler.
- [ ] `go test ./...` ist gruen.

**Kontext**:
- `internal/service/contact_service_test.go`
- `internal/model/contact_test.go`

**Output**:
- Erweiterte Test-Suite fuer Search.

**Wave-Exit-Gate**:
- [ ] Suchfeld ist im Admin sichtbar und funktional.
- [ ] Suche filtert ueber alle relevanten Spalten.
- [ ] Leere Suche zeigt alle Ergebnisse.
- [ ] Keine SQL-Injection moeglich.
- [ ] Tests sind gruen.

---
