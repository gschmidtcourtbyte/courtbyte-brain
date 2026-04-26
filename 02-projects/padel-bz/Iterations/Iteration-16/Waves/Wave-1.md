# Wave 1: Admin-Suche reproduzieren und Suchvertrag fixieren

**Status:** Done (Legacy-Migration)
**Skill:** database-eng, service-eng, handler-eng, frontend-eng, testing-eng
**Quelle:** `plan/iterations/iteration-16.md`

## Legacy-Inhalt

## Wave 1 - Admin-Suche reproduzieren und Suchvertrag fixieren
**Agenten**: Agent A, Agent B, Agent C, Agent D
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Der Fehler ist reproduzierbar, verstanden und mit einem klaren Suchvertrag korrigiert.

### Task 16.1: Defekte Admin-Suche reproduzieren
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (testing-eng), Agent D (handler-eng)

**Akzeptanzkriterien**:
- [ ] Lokales oder testbares Szenario mit mindestens 3 Hansefit-Anfragen existiert.
- [ ] Testdaten enthalten Treffer in Vorname, Nachname, E-Mail/Playtomic-E-Mail, Hansefit-ID und Freitext.
- [ ] Mindestens ein Suchbegriff mit erwartetem Treffer und ein Suchbegriff ohne Treffer sind dokumentiert.
- [ ] Beobachtetes Ist-Verhalten ist festgehalten: URL, Suchbegriff, erwartete Zeilen, tatsaechliche Zeilen.
- [ ] Der fehlerhafte Layer ist eingegrenzt: Form/Template, Handler, Service, Repository oder Daten.
- [ ] Vor dem Fix existiert ein fehlgeschlagener Test oder eine eindeutig reproduzierbare manuelle QA-Notiz.

**Kontext**:
- `web/templates/admin/hansefit.html`
- `internal/handler/admin_hansefit.go`
- `internal/service/contact_service.go`
- `internal/repository/contact_repo.go`

**Output**:
- Reproduktionsnotiz oder fehlgeschlagener Testfall.
- Entscheidung, welche Schicht korrigiert werden muss.

### Task 16.2: Repository-Suche korrigieren
**Priority**: P0
**Blocked by**: 16.1
**Agent**: Agent B (database-eng)

**Akzeptanzkriterien**:
- [ ] Nicht-leerer Suchbegriff liefert nur `hansefit_registration = TRUE` Zeilen mit Treffer.
- [ ] Leerer oder nur aus Leerzeichen bestehender Suchbegriff liefert alle Hansefit-Anfragen.
- [ ] Suche matcht case-insensitive ueber Vorname, Nachname, E-Mail, Playtomic-E-Mail, Hansefit-ID und Freitext.
- [ ] NULL-Felder verhindern keine Treffer in anderen Feldern.
- [ ] `%` und `_` im Suchbegriff werden nicht als unkontrollierte Wildcards behandelt.
- [ ] SQL bleibt parametrisiert.
- [ ] Sortierung bleibt: offene Anfragen zuerst, danach `created_at DESC`.
- [ ] Kein Treffer liefert eine leere Slice, keinen Fehler.

**Kontext**:
- `internal/repository/contact_repo.go`
- `migrations/001_contact_submissions.up.sql`
- `migrations/002_hansefit_admin.up.sql`

**Output**:
- Korrigierte Repository-Suchmethode.
- Testbare Query-Semantik.

### Task 16.3: Service- und Handler-Flow absichern
**Priority**: P0
**Blocked by**: 16.1
**Agent**: Agent C (service-eng), Agent D (handler-eng)

**Akzeptanzkriterien**:
- [ ] Handler liest `q` aus der Query, trimmt den Wert und begrenzt ihn auf 200 Zeichen.
- [ ] Bei nicht-leerem `q` wird die Search-Methode genutzt.
- [ ] Bei leerem `q` wird die List-Methode genutzt.
- [ ] `SearchQuery` wird an das Template weitergegeben.
- [ ] Fehler aus Repository/Service fuehren zu HTTP 500 und werden geloggt.
- [ ] CSRF bleibt unveraendert: GET-Suche benoetigt keinen CSRF-Token; POST-Aktion bleibt geschuetzt.

**Kontext**:
- `internal/handler/admin_hansefit.go`
- `internal/service/contact_service.go`
- `web/templates/admin/hansefit.html`

**Output**:
- Stabiler Admin-GET-Flow fuer Suche.

### Task 16.4: Admin-Template zeigt nur gefilterte Rows
**Priority**: P0
**Blocked by**: 16.2, 16.3
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Bei Suchbegriff rendert die Tabelle ausschliesslich `.Submissions` aus dem Search-Ergebnis.
- [ ] Es gibt keine clientseitig versteckten Alt-Zeilen und keine zweite ungefilterte Datenquelle.
- [ ] Suchfeld behaelt den eingegebenen Wert nach Absenden.
- [ ] "Zuruecksetzen" entfernt die Suche und zeigt wieder die volle Liste.
- [ ] Bei leerem Ergebnis wird "Keine Treffer fuer ..." angezeigt.
- [ ] Bei leerer Suche und leerer Datenbank wird "Noch keine Hansefit-Anfragen eingegangen." angezeigt.
- [ ] Tabellenlayout bricht bei kurzen, langen und fehlenden optionalen Feldern nicht.

**Kontext**:
- `web/templates/admin/hansefit.html`
- `web/static/css/style.css`

**Output**:
- Korrigiertes Admin-Template und ggf. CSS-Nachjustierung.

**Wave-Exit-Gate**:
- [ ] Die Fehlerursache ist bekannt.
- [ ] Suche mit Treffer zeigt nur passende Zeilen.
- [ ] Suche ohne Treffer zeigt den Keine-Treffer-Zustand.
- [ ] Leere Suche zeigt alle Hansefit-Anfragen.
- [ ] Sonderzeichen im Suchbegriff erzeugen keine falschen Massentreffer.

---
