# Iteration 13: Feinschliff Baufortschritt und Hansefit-Admin

## Ziel
Iteration 13 fokussiert den Feinschliff der in Iteration 12 gelieferten Baufortschritt- und Hansefit-Admin-Funktionen. Der Adminbereich soll hochwertiger wirken, die Login-Maske zentraler und klarer erscheinen, das Dashboard soll bessere Uebersicht geben und Hansefit-Anfragen in einer sauberen Tabelle darstellen.

Auf der oeffentlichen Website wird Hansefit visuell zurueckgenommen, damit es als Partner-/Clubbestandteil sichtbar bleibt, aber nicht den Hauptfokus uebernimmt. Das Bautagebuch wird vereinheitlicht: alle Eintraege, inklusive "Aktueller Fortschritt", sind als auswaehlbare Baufortschritt-Eintraege bedienbar. Das zusaetzliche Bild neben dem Bauvideo wird entfernt.

Fachlich wird das Hansefit-Registrierungsformular um die Hansefit-ID erweitert. Diese ist Pflicht, wenn die Checkbox `Hansefitregistrierung` aktiv ist. Die E-Mail-Texte fuer Registrierende werden als editierbare Templates vorbereitet und in `docs/Content.md` verlinkt.

## Ausgangslage
- Iteration 12 ist abgeschlossen und dokumentiert in `plan/iterations/iteration-12.md`.
- Admin-Login existiert unter `web/templates/admin/login.html`, wirkt aber noch zu schlicht und nicht ausreichend zentral.
- Admin-Dashboard existiert unter `web/templates/admin/dashboard.html`, braucht aber bessere Struktur und visuelle Hierarchie.
- Hansefit-Uebersicht existiert unter `web/templates/admin/hansefit.html`, soll tabellarisch hochwertiger und vollstaendiger werden.
- Contact-Datenmodell enthaelt `playtomic_email`, aber noch keine `hansefit_id`.
- Contact-Service baut E-Mail-Texte aktuell inline in `internal/service/contact_service.go`.
- `docs/Content.md` dokumentiert Website-Texte, aber noch keine E-Mail-Template-Dateien.
- Baufortschritt liegt statisch in `web/templates/pages/home.html` mit JS-Toggle in `web/static/js/main.js`.

## Scope
- Admin-Login visuell ueberarbeiten und zentral auf der Seite darstellen.
- Admin-Dashboard hochwertiger strukturieren, inkl. sinnvoller Kennzahlen und Einstieg in die Hansefit-Tabelle.
- Hansefit-Admin-Tabelle optisch verbessern und fachlich um Hansefit-ID erweitern.
- Hansefit-Logo auf der Website schlanker, dezenter und unauffaelliger darstellen.
- Zusaetzliches Baufortschritt-Bild neben dem Video entfernen.
- Bautagebuch-Eintraege ueberarbeiten: `Aktueller Fortschritt` und `Hallenbau` als gleichartige, auswaehlbare Eintraege.
- Aktueller Fortschritt bleibt beim Seitenaufruf aktiv und behaelt die Status-Animation.
- Formularfeld `Hansefit-ID` ergaenzen.
- `Hansefit-ID` ist nur Pflicht, wenn `Hansefitregistrierung` angehakt ist.
- Hansefit-ID speichern, validieren, in Admin-Uebersicht anzeigen und in Betreiber-/Bestaetigungs-Mail verfuegbar machen.
- E-Mail an Registrierende aus Template-Datei rendern statt inline-String.
- E-Mail-Templates in `docs/Content.md` verlinken.

## Nicht im Scope
- Vollstaendiges CMS fuer Bautagebuch oder E-Mail-Texte.
- Video-Upload oder Admin-Pflege der Bauphasen.
- Neue Admin-Rollen oder Benutzerverwaltung.
- Hansefit-API-Anbindung oder externe ID-Verifikation.
- HTML-E-Mail-Design; Plain-Text reicht fuer diese Iteration.
- Inhaltliche finale Neuformulierung der E-Mail-Texte ohne gesonderte Content-Vorgaben.

## Produktentscheidungen
- `hansefit_id` wird nullable gespeichert, damit bestehende und normale Kontaktanfragen gueltig bleiben.
- Bei `hansefit_registration = true` sind `playtomic_email` und `hansefit_id` Pflichtfelder.
- Hansefit bleibt sichtbar, aber als ruhiger Partnerhinweis statt als grosser Hero-/Intro-Block.
- Bautagebuch bleibt statisch im Template. Die Vereinheitlichung erfolgt ueber Markup, CSS und JS, nicht ueber ein Datenmodell.
- Der aktuelle Baufortschritt wird im UI als `Aktueller Fortschritt` angeboten, kann aber intern weiter auf die Phase `rohbau` zeigen.
- E-Mail-Templates werden unter `web/templates/emails/` abgelegt und ueber `docs/Content.md` fuer spaetere Textpflege auffindbar gemacht.

## Critical Path
1. Datenmodell und Contact-Vertrag um `hansefit_id` erweitern.
2. Formular, Handler und Service-Validierung aktualisieren.
3. Admin-Tabellen und Dashboard auf neue Daten und bessere Darstellung bringen.
4. Bautagebuch-UI und dezente Hansefit-Darstellung ueberarbeiten.
5. E-Mail-Template-Struktur einziehen und `docs/Content.md` aktualisieren.
6. Regressionen, QA und Security-Review abschliessen.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe, Abnahmekriterien |
| Agent A | migration-eng + database-eng | Migration fuer `hansefit_id`, Query-/Index-Auswirkungen |
| Agent B | handler-eng + security-eng | Formular-Parsing, Admin-Handler, CSRF/Auth-Regression |
| Agent C | service-eng | Validierung, Contact-Service, E-Mail-Template-Rendering |
| Agent D | frontend-eng | Admin-UI, Login, Dashboard, Bautagebuch, dezentes Hansefit-Branding |
| Agent Doc | documentation-eng | `docs/Content.md`, Operations-/Template-Verweise |
| Agent E | testing-eng | Unit-/Regressionstests und manuelle QA-Matrix |
| Agent R | review-eng | Finaler Code-, Security- und Merge-Readiness-Review |

---

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

## Wave 2 - Formular, Hansefit-Branding und Bautagebuch
**Agenten**: Agent D, Agent B, Agent C
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Hansefit-ID-Speicherung
**Ziel**: Oeffentliche Website wirkt ruhiger, das Formular fragt die Hansefit-ID korrekt ab und Bautagebuch-Eintraege sind einheitlich auswaehlbar.

### Task: 13.3 Hansefit-Registrierungsformular erweitern
**Priority**: P0
**Blocked by**: 13.2
**Agent**: Agent D + Agent B

**Akzeptanzkriterien**:
- [ ] Formular enthaelt Feld `Hansefit-ID`.
- [ ] Hansefit-ID ist optisch und inhaltlich der Hansefit-Checkbox zugeordnet.
- [ ] Bei aktivierter `Hansefitregistrierung` ist Hansefit-ID Pflicht.
- [ ] Bei nicht aktivierter Checkbox darf das Formular ohne Hansefit-ID abgesendet werden.
- [ ] Validierungsfehler erscheinen im bestehenden Fehlerbereich.
- [ ] Werte bleiben nach Validierungsfehlern im Formular erhalten.
- [ ] Handler liest `hansefit_id` sicher und explizit aus.

**Kontext**:
- `web/templates/pages/contact.html`
- `internal/handler/contact.go`
- `internal/model/contact.go`

**Output**:
- Aktualisiertes Kontaktformular
- Aktualisierter Contact-Handler
- Validierungs-Regressionstest

### Task: 13.4 Hansefit-Logo dezenter integrieren
**Priority**: P1
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Hansefit-Logo wirkt kleiner und unauffaelliger als in Iteration 12.
- [ ] Logo bleibt sichtbar, aber dominiert weder Kontaktformular noch Startseite.
- [ ] Auf Mobile entsteht kein Layout-Bruch.
- [ ] Logo wird nicht als grosser eigener Content-Block verwendet, sondern als ruhiger Partnerhinweis.
- [ ] Kontrast und Lesbarkeit bleiben ausreichend.

**Kontext**:
- `web/templates/pages/contact.html`
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- Asset: `web/static/img/hansefit.png`

**Output**:
- Schlankere Hansefit-Darstellung
- CSS-Anpassungen fuer dezentes Branding

### Task: 13.5 Bautagebuch-Eintraege ueberarbeiten
**Priority**: P0
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Zusaetzliches Bild im Baufortschritt wird entfernt.
- [ ] `Aktueller Fortschritt` ist als auswaehlbarer Bautagebuch-Eintrag sichtbar.
- [ ] `Hallenbau` ist als weiterer Bautagebuch-Eintrag im gleichen UI-Muster sichtbar.
- [ ] Aktueller Fortschritt ist beim Seitenaufruf aktiv.
- [ ] Status-Animation bleibt nur beim aktuellen Fortschritt erhalten.
- [ ] Pro Eintrag wird weiterhin nur ein Video oder ein Platzhalter angezeigt.
- [ ] Hallenbau nutzt weiterhin den gestreiften Platzhalter.
- [ ] Ohne JavaScript bleibt der aktuelle Fortschritt sichtbar.
- [ ] Desktop und Mobile haben keine Ueberlappungen.

**Kontext**:
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- `web/static/js/main.js`
- Aktuelles Video: `/static/videos/construction.mp4`

**Output**:
- Ueberarbeiteter Bautagebuch-Bereich
- Entferntes Zusatzbild
- Konsistente Auswahl-Komponente fuer Eintraege

**Wave-Exit-Gate**:
- [ ] Kontaktformular zeigt und validiert Hansefit-ID korrekt.
- [ ] Hansefit ist visuell zurueckgenommen.
- [ ] Bautagebuch ist einheitlich bedienbar.

---

## Wave 3 - Admin-UI Feinschliff
**Agenten**: Agent D, Agent B
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Hansefit-ID-Anzeige
**Ziel**: Adminbereich wirkt hochwertiger, zentraler und besser strukturiert.

### Task: 13.6 Login-Maske zentral und hochwertiger gestalten
**Priority**: P1
**Blocked by**: none
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Login-Maske ist horizontal und vertikal sauber zentriert.
- [ ] Login nutzt eine reduzierte, professionelle Admin-Optik passend zur Website.
- [ ] Fehlermeldung ist sichtbar, aber nicht ueberbetont.
- [ ] Form-Felder, Button und Fokus-Stati sind auf Desktop und Mobile sauber.
- [ ] Keine sichtbaren technischen Erklaertexte oder unnoetigen Hinweise.
- [ ] CSRF und bestehende Login-Logik bleiben unveraendert funktionsfaehig.

**Kontext**:
- `web/templates/admin/login.html`
- `web/static/css/style.css`
- `internal/handler/admin_auth.go`

**Output**:
- Ueberarbeitetes Admin-Login
- Responsive Admin-Login-Styles

### Task: 13.7 Admin-Dashboard und Hansefit-Tabelle verbessern
**Priority**: P0
**Blocked by**: 13.2
**Agent**: Agent D + Agent B

**Akzeptanzkriterien**:
- [ ] Dashboard zeigt klarere Uebersicht statt nur einfachen Link.
- [ ] Dashboard enthaelt mindestens Kennzahlen fuer Hansefit-Anfragen: Gesamt, unbearbeitet, bearbeitet.
- [ ] Dashboard bietet direkten Einstieg in die Hansefit-Uebersicht.
- [ ] Hansefit-Uebersicht zeigt Inhalte in einer hochwertigen Tabelle.
- [ ] Tabelle enthaelt Vorname, Nachname, Playtomic-E-Mail, Hansefit-ID, Freitext, Status, Aktion.
- [ ] Tabelle bleibt auf Mobile nutzbar, z.B. durch horizontales Scrollen oder responsive Zeilen.
- [ ] Unbearbeitet/bearbeitet bleiben rot/gruen unterscheidbar, aber visuell ruhiger als harte Warnfarben.
- [ ] Leerer Zustand ist gepflegt und nicht technisch.

**Kontext**:
- `web/templates/admin/dashboard.html`
- `web/templates/admin/hansefit.html`
- `web/static/css/style.css`
- `internal/handler/admin_auth.go`
- `internal/handler/admin_hansefit.go`
- `internal/repository/contact_repo.go`

**Output**:
- Ueberarbeitetes Admin-Dashboard
- Ueberarbeitete Hansefit-Tabelle
- Ggf. Repository-/Service-Methode fuer Dashboard-Kennzahlen

**Wave-Exit-Gate**:
- [ ] Login und Dashboard wirken konsistent.
- [ ] Hansefit-ID ist im Admin sichtbar.
- [ ] Admin-Flows bleiben auth- und CSRF-geschuetzt.

---

## Wave 4 - E-Mail-Templates und Content-Dokumentation
**Agenten**: Agent C, Agent Doc
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1 fuer Hansefit-ID im Template-Kontext
**Ziel**: E-Mail-Texte fuer Registrierende sind als Template-Dateien pflegbar und in `docs/Content.md` auffindbar.

### Task: 13.8 Hansefit-Bestaetigungs-Mail als Template auslagern
**Priority**: P1
**Blocked by**: 13.2
**Agent**: Agent C

**Akzeptanzkriterien**:
- [ ] Hansefit-Bestaetigungs-Mail wird aus einer Template-Datei gerendert.
- [ ] Template liegt unter `web/templates/emails/`.
- [ ] Template kann Vorname, Nachname, Playtomic-E-Mail und Hansefit-ID nutzen.
- [ ] Bestehende Versandlogik und Empfaengerlogik bleiben erhalten.
- [ ] Bei Template-Parse- oder Renderfehler wird die Anfrage nicht faelschlich als bearbeitet markiert.
- [ ] Lokaler Betrieb ohne SMTP bleibt wie in Iteration 12 dokumentiert.

**Kontext**:
- `internal/service/contact_service.go`
- `docs/Operations.md`
- Neuer Zielpfad: `web/templates/emails/hansefit_confirmation.txt`

**Output**:
- Neues E-Mail-Template
- Template-Rendering im Contact-Service
- Regressionstest fuer Template-Daten, soweit ohne SMTP sinnvoll

### Task: 13.9 Content.md um E-Mail-Templates erweitern
**Priority**: P1
**Blocked by**: 13.8
**Agent**: Agent Doc

**Akzeptanzkriterien**:
- [ ] `docs/Content.md` enthaelt einen Abschnitt `E-Mail-Templates`.
- [ ] Hansefit-Bestaetigungs-Mail ist mit Dateipfad verlinkt.
- [ ] Bearbeitbare Textstellen im Template sind in der Tabelle auffindbar.
- [ ] Hinweise zu Variablen wie Vorname, Nachname, Playtomic-E-Mail und Hansefit-ID sind dokumentiert.
- [ ] Falls Betreiber-Benachrichtigung ebenfalls ausgelagert wird, ist sie ebenfalls verlinkt.

**Kontext**:
- `docs/Content.md`
- `web/templates/emails/hansefit_confirmation.txt`

**Output**:
- Aktualisierte Content-Dokumentation

**Wave-Exit-Gate**:
- [ ] Bestaetigungs-Mail ist template-basiert.
- [ ] Content-Verantwortliche finden die Mailtexte in `docs/Content.md`.

---

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

## Abhaengigkeits-Graph

```text
Wave 1: Datenvertrag Hansefit-ID
   |--> Wave 2: Formular + Bautagebuch + Branding
   |--> Wave 3: Admin-UI Feinschliff
   `--> Wave 4: E-Mail-Templates + Content.md

Wave 2 + Wave 3 + Wave 4
   `--> Wave 5: Tests + Review
```

## Definition of Done
- [ ] Hansefit-ID wird gespeichert und ist bei Hansefitregistrierung Pflicht.
- [ ] Formular zeigt Hansefit-ID sauber und erhaelt Werte nach Validierungsfehlern.
- [ ] Admin-Login ist zentral, responsive und visuell hochwertiger.
- [ ] Dashboard zeigt sinnvolle Kennzahlen und wirkt wie ein echter Admin-Einstieg.
- [ ] Hansefit-Anfragen werden in einer hochwertigen Tabelle inkl. Hansefit-ID angezeigt.
- [ ] Hansefit-Logo ist sichtbar, aber deutlich dezenter integriert.
- [ ] Baufortschritt enthaelt kein zusaetzliches Bild neben dem Video.
- [ ] Bautagebuch-Eintraege sind einheitlich auswaehlbar; `Aktueller Fortschritt` ist initial aktiv.
- [ ] Hansefit-Bestaetigungs-Mail wird aus Template-Datei gerendert.
- [ ] `docs/Content.md` verlinkt die E-Mail-Templates.
- [ ] `go test ./...` ist gruen oder Abweichungen sind begruendet dokumentiert.
- [ ] Security- und Merge-Review sind abgeschlossen.

## Umsetzungsstatus
- Wave 1: done
- Wave 2: done
- Wave 3: done
- Wave 4: done
- Wave 5: done
