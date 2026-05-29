# Iteration 15: Social Media Links, UX-Fixes und Admin-Suchfunktion

## Ziel
Iteration 15 bringt dezente Social-Media-Verlinkungen in die Navigation, behebt UX-Defizite im Kontaktformular (Button-Farbe, Vorname/Nachname-Verwechslung, Hansefit-ID-Format) und fuehrt eine Volltextsuche im Admin-Backend ein.

## Ausgangslage
- Iteration 14 ist abgeschlossen: Kontaktformular mit Hansefit-Gating, AGB-Zustimmung und Pflichtfeldmatrix.
- Navigation in `web/templates/partials/nav.html` hat keine Social-Media-Links.
- `.btn-primary` nutzt `color:var(--accent-ink)` (#1a1714, dunkel) — der "Nachricht senden"-Button hat keine weisse Schrift.
- Hansefit-ID wird nur auf leeren String geprueft (`internal/model/contact.go:92-93`), kein Formatcheck.
- Kontaktformular zeigt "Name" (last_name) vor "Vorname" (first_name) — ambiges Label fuehrt zu vertauschter Dateneingabe im Admin.
- Admin-Hansefit-Tabelle hat keine Suchfunktion (`internal/handler/admin_hansefit.go`, `internal/repository/contact_repo.go`).

## Scope
- Instagram-Icon-Link in Desktop- und Mobile-Nav einbauen.
- WhatsApp-Community-Platzhalter vorbereiten (Link kommt spaeter).
- "Nachricht senden"-Button im Kontaktformular mit weisser Schriftfarbe.
- Kontaktformular: Feld-Reihenfolge auf Vorname|Nachname aendern, Label "Name" zu "Nachname" aendern.
- Hansefit-ID serverseitig und clientseitig validieren: Start mit 2 Ziffern, min. 1 Buchstabe, genau 8 Zeichen.
- Admin-Suchfunktion: Freitext-Suche ueber alle sichtbaren Spalten (Vorname, Nachname, E-Mail, Playtomic-E-Mail, Hansefit-ID, Nachricht).
- Tests fuer neue Validierung und Suchfunktion.

## Nicht im Scope
- WhatsApp-Community-Link final einbauen (Link wird spaeter geliefert).
- Redesign der Admin-Oberflaeche.
- Pagination der Admin-Tabelle.
- Aenderungen an der E-Mail-Versandlogik.

## Produktentscheidungen
- Social-Media-Icons werden rechts in der Nav platziert, vor dem Hansefit-CTA-Button, als schlichte SVG-Icons ohne Label.
- Im Mobile-Menu werden die Icons am Ende der Link-Liste angezeigt.
- WhatsApp-Link wird als `#` Platzhalter vorbereitet und per `hidden` Klasse oder Kommentar verborgen, bis der Link geliefert wird.
- Der "Nachricht senden"-Button erhaelt eine eigene CSS-Klasse oder einen Inline-Override fuer `color: white`, ohne die globale `.btn-primary` zu aendern.
- Formularfeld-Reihenfolge wird auf Vorname|Nachname (first_name|last_name) geaendert — deutsche Konvention. Das Label "Name" wird zu "Nachname" geaendert.
- Hansefit-ID Format: `^\d{2}[A-Za-z0-9]*$` mit genau 8 Zeichen und mindestens 1 Buchstabe. Validierung im Model und als HTML5-pattern.
- Admin-Suche: GET-Parameter `?q=suchbegriff` auf der Hansefit-Admin-Seite. Suche per SQL `ILIKE` ueber relevante Spalten.

## Critical Path
1. Frontend-Quick-Fixes (Social Links, Button-Farbe, Formular-Reihenfolge) sind unabhaengig.
2. Hansefit-ID-Validierung erfordert Model- und Frontend-Aenderung parallel.
3. Admin-Suche erfordert Repository → Handler → Template sequenziell.
4. Tests und Review nach allen Aenderungen.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe |
| Agent A | frontend-eng | Social Links, Button-Fix, Formular-Reihenfolge, Hansefit-ID pattern, Admin-Such-UI |
| Agent B | service-eng | Hansefit-ID Format-Validierung im Model |
| Agent C | database-eng | Search-Query im Repository |
| Agent D | handler-eng | Such-Handler, Route |
| Agent E | testing-eng | Unit-Tests fuer Validierung und Suche |
| Agent R | review-eng | Finaler Review |

---

## Wave 1 — Frontend Quick-Fixes (parallel, keine Abhaengigkeiten)
**Agenten**: Agent A
**Kann einzeln ausgefuehrt werden**: ja
**Ziel**: Visuelle UX-Korrekturen ohne Backend-Aenderungen.

### Task 15.1: Social-Media-Links in der Navigation
**Priority**: P1
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Instagram-Icon-Link in Desktop-Nav, rechts vor dem Hansefit-Button.
- [ ] Instagram-Link zeigt auf `https://www.instagram.com/courts.padelclub/`.
- [ ] Instagram-Icon oeffnet in neuem Tab (`target="_blank"`, `rel="noopener"`).
- [ ] WhatsApp-Icon vorbereitet aber unsichtbar (hidden oder auskommentiert), mit Platzhalter-Link `#`.
- [ ] Beide Icons im Mobile-Menu vorhanden (Instagram sichtbar, WhatsApp verborgen).
- [ ] Icons sind schlicht, passend zum dunklen Design, `width/height` ca. 20px.
- [ ] Keine neuen externen Abhaengigkeiten (Inline-SVG verwenden).

**Kontext**:
- `web/templates/partials/nav.html`
- `web/static/css/style.css`

**Output**:
- Aktualisiertes Nav-Template mit Social-Icons.
- CSS-Anpassungen fuer Icon-Spacing und Hover-Effekte.

### Task 15.2: Button-Farbe "Nachricht senden" weiss
**Priority**: P1
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Der Submit-Button im Kontaktformular hat weisse Schriftfarbe (`color: #fff` oder `color: var(--fg)`).
- [ ] Die globale `.btn-primary` Klasse bleibt unveraendert (Accent-Buttons in Nav etc. behalten dunkle Schrift).
- [ ] Loesung: Inline-Style oder eigene Klasse wie `.btn-primary-light` auf dem Kontakt-Submit.
- [ ] SVG-Icon im Button erbt die weisse Farbe.

**Kontext**:
- `web/templates/pages/contact.html` (Zeile 209)
- `web/static/css/style.css` (Zeile 152-155: `.btn-primary`)

**Output**:
- Aktualisierter Submit-Button im Kontaktformular.

### Task 15.3: Vorname/Nachname-Reihenfolge und Label-Fix
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Kontaktformular zeigt Felder in Reihenfolge: Vorname (first_name) | Nachname (last_name).
- [ ] Das Label des bisherigen "Name"-Felds wird zu "Nachname" geaendert.
- [ ] Placeholder bleibt: Vorname → "Max", Nachname → "Mustermann".
- [ ] Autocomplete-Attribute bleiben korrekt: `given-name` fuer Vorname, `family-name` fuer Nachname.
- [ ] `name`-Attribute bleiben unveraendert: `first_name`, `last_name`.
- [ ] Admin-Tabelle (`hansefit.html`) zeigt weiterhin Vorname|Nachname in korrekter Reihenfolge (keine Aenderung noetig, da Mapping korrekt ist).
- [ ] Server-seitige Validierung und Handler bleiben unveraendert.

**Kontext**:
- `web/templates/pages/contact.html` (Zeilen 43-70)
- `web/templates/admin/hansefit.html` (Zeilen 40-53)

**Output**:
- Aktualisiertes Kontaktformular-Template.

**Wave-Exit-Gate**:
- [ ] Instagram-Link ist in Desktop- und Mobile-Nav sichtbar und klickbar.
- [ ] WhatsApp-Platzhalter ist vorbereitet aber nicht sichtbar.
- [ ] "Nachricht senden"-Button hat weisse Schrift.
- [ ] Kontaktformular zeigt Vorname vor Nachname mit eindeutigen Labels.

---

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

## Parallelisierung

```
Wave 1 (parallel, keine Abhaengigkeiten):
  Agent A → 15.1 Social Links
  Agent A → 15.2 Button-Farbe
  Agent A → 15.3 Vorname/Nachname Fix

Wave 2 (nach Wave 1 fuer 15.5, parallel fuer 15.4):
  Agent B → 15.4 Hansefit-ID Model-Validierung  (kann parallel zu Wave 1)
  Agent A → 15.5 Hansefit-ID Client-Validierung  (nach 15.3)
  Agent E → 15.6 Validierungs-Tests              (nach 15.4)

Wave 3 (sequenziell intern, kann parallel zu Wave 2 starten):
  Agent C → 15.7 Repository Search               (kann parallel zu Wave 1)
  Agent D → 15.8 Handler Search                   (nach 15.7)
  Agent A → 15.9 Admin Such-UI                    (nach 15.8)
  Agent E → 15.10 Such-Tests                      (nach 15.7 + 15.8)

Wave 4 (nach Wave 1-3):
  Agent R → 15.11 Finaler Review
```

**Optimierte Ausfuehrungsreihenfolge**:
- 15.1 + 15.2 + 15.3 + 15.4 + 15.7 koennen alle sofort parallel starten.
- 15.5 startet nach 15.3.
- 15.6 startet nach 15.4.
- 15.8 startet nach 15.7.
- 15.9 startet nach 15.8.
- 15.10 startet nach 15.7 + 15.8.
- 15.11 startet nach allem.

## Risiken und offene Punkte
- WhatsApp-Community-Link fehlt — Platzhalter wird vorbereitet, aber nicht aktiviert.
- Bereits gespeicherte Hansefit-IDs in der DB werden durch die neue Validierung nicht nachtraeglich geprueft. Bestehende Daten bleiben gueltig.
- Admin-Suche ohne DB-Index ist fuer kleine Datenmengen akzeptabel; bei Wachstum ueber 1000 Eintraege sollte ein `pg_trgm`-Index in Betracht gezogen werden.
- Aenderung der Formular-Reihenfolge (Vorname vor Nachname) ist ein Breaking Change fuer Nutzer, die sich an die alte Reihenfolge gewoehnt haben, aber entspricht der deutschen Konvention und behebt das Verwechslungs-Problem.

## Definition of Done
- [ ] Instagram-Link in Desktop- und Mobile-Nav sichtbar und klickbar.
- [ ] WhatsApp-Platzhalter vorbereitet, aber nicht sichtbar.
- [ ] "Nachricht senden"-Button hat weisse Schriftfarbe.
- [ ] Kontaktformular zeigt Vorname|Nachname in korrekter Reihenfolge.
- [ ] Hansefit-ID wird auf Format validiert (2 Ziffern Start, 1 Buchstabe min., 8 Zeichen).
- [ ] Admin-Suchfunktion filtert ueber alle sichtbaren Spalten.
- [ ] Tests sind gruen.
- [ ] Review ist abgeschlossen.
