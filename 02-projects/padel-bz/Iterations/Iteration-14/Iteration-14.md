# Iteration 14: Kontaktformular Pflichtfelder und Hansefit-Gating

## Ziel
Iteration 14 korrigiert das in Iteration 13 angepasste Kontaktformular fachlich und UX-seitig. Das Formular soll fuer normale Kontaktanfragen schlank bleiben und Hansefit-spezifische Felder erst anzeigen, wenn die Hansefit-Registrierung aktiv gewaehlt wurde.

Immer verpflichtend sind:
- Name/Nachname
- Vorname
- E-Mail-Adresse
- Zustimmung zur Datenschutzerklaerung
- Zustimmung zu den AGB

Nur bei aktivierter Checkbox `Hansefit Registrierung` verpflichtend sind:
- Hansefit-ID
- Playtomic-E-Mail-Adresse

Immer optional sichtbar ist:
- Freitext/Nachricht

Ohne alle jeweils relevanten Pflichtfelder darf kein Absenden moeglich sein. Client-seitige Validierung ist UX, server-seitige Validierung bleibt verbindlich.

## Ausgangslage
- Iteration 13 ist dokumentiert in `plan/iterations/iteration-13.md`.
- Das Kontaktformular liegt in `web/templates/pages/contact.html`.
- Die aktuelle Hansefit-Logik in `web/static/js/main.js` setzt Pflichtfelder dynamisch, blendet die Hansefit-Felder aber nicht wirklich aus.
- Das Formular nutzt aktuell `novalidate`; dadurch blockiert native Browser-Validierung das Absenden nicht.
- Backend-Validierung fuer Vorname, Nachname, E-Mail, Datenschutzerklaerung, Playtomic-E-Mail und Hansefit-ID existiert in `internal/model/contact.go`.
- Eine AGB-Zustimmung existiert im Datenmodell, Handler und Template noch nicht.
- Eine AGB-Seite oder Route ist aktuell nicht vorhanden; der Footer enthaelt nur einen Platzhalter-Link.

## Scope
- Kontaktformular auf die gewuenschten Pflichtfelder reduzieren bzw. klar markieren.
- Hansefit-ID und Playtomic-E-Mail nur anzeigen, wenn `Hansefit Registrierung` angehakt ist.
- Hansefit-ID und Playtomic-E-Mail nur dann als Pflichtfelder behandeln.
- Optionales Freitextfeld dauerhaft anzeigen.
- Datenschutzerklaerung und AGB als verpflichtende Checkboxen vor Absenden abfragen.
- AGB-Zustimmung im Backend validieren und persistent speichern.
- Versteckte Hansefit-Felder bei deaktivierter Checkbox nicht absenden oder serverseitig ignorieren.
- Bestehende Hansefit-Admin- und Bestaetigungsfluesse duerfen nicht regressieren.
- Tests und Doku auf den neuen Formularvertrag aktualisieren.

## Nicht im Scope
- Hansefit-API-Anbindung oder externe Verifikation der Hansefit-ID.
- Neuer Admin-Workflow fuer normale Kontaktanfragen.
- Final juristisch freigegebene AGB-Texte, sofern diese nicht geliefert werden.
- Vollstaendige Neugestaltung der Kontaktseite ausserhalb des Formularbereichs.
- Entfernen des Telefonfelds, sofern es weiterhin optional bleibt und nicht in den Pflichtfeldpfad eingreift.

## Produktentscheidungen
- Der Begriff `Name` wird technisch als `last_name`/Nachname verstanden; `Vorname` bleibt `first_name`.
- `message` bleibt das immer sichtbare optionale Freitextfeld.
- Wenn `hansefit_registration = false`, werden `playtomic_email` und `hansefit_id` nicht gespeichert, auch wenn ein manipulierter Request Werte sendet.
- AGB-Zustimmung wird analog zur Datenschutzerklaerung als Boolean gespeichert.
- Falls kein finaler AGB-Text vorliegt, darf die Technik vorbereitet werden; Release ist dann durch Content/Legal blockiert.

## Critical Path
1. Daten- und Validierungsvertrag fuer AGB-Zustimmung festlegen.
2. Migration, Model, Repository, Handler und Service-Validierung erweitern.
3. Kontaktformular und JS-Gating umstellen.
4. Tests fuer Pflichtfeldmatrix und Hansefit-Konditionen ergaenzen.
5. Doku, QA und Review abschliessen.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe, Akzeptanzkriterien |
| Agent A | migration-eng + database-eng | AGB-Zustimmung in Schema und Repository absichern |
| Agent B | service-eng | Domain-/Service-Validierung und Normalisierung |
| Agent C | handler-eng + security-eng | Formular-Parsing, Consent-Handling, CSRF-/Rate-Limit-Regression |
| Agent D | frontend-eng | Kontaktformular-UX, Hansefit-Gating, Accessibility |
| Agent E | testing-eng | Unit-, Handler- und manuelle QA-Matrix |
| Agent Doc | documentation-eng | Content-/Operations-Doku und AGB-Content-Blocker |
| Agent R | review-eng | Finaler Code-, Security- und Merge-Readiness-Review |

---

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

## Wave 2 - Backend-Validierung und HTTP-Rand
**Agenten**: Agent B, Agent C, Agent E
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 1
**Ziel**: Manipulierte oder unvollstaendige Requests werden serverseitig korrekt abgewiesen.

### Task: 14.4 Server-seitige Pflichtfeldmatrix absichern
**Priority**: P0
**Blocked by**: 14.2, 14.3
**Agent**: Agent B

**Akzeptanzkriterien**:
- [ ] Fehlender Nachname erzeugt Validierungsfehler.
- [ ] Fehlender Vorname erzeugt Validierungsfehler.
- [ ] Fehlende oder ungueltige E-Mail-Adresse erzeugt Validierungsfehler.
- [ ] Fehlende Datenschutzerklaerung erzeugt Validierungsfehler.
- [ ] Fehlende AGB-Zustimmung erzeugt Validierungsfehler.
- [ ] Ohne Hansefit-Checkbox sind Hansefit-ID und Playtomic-E-Mail nicht erforderlich.
- [ ] Mit Hansefit-Checkbox sind Hansefit-ID und Playtomic-E-Mail erforderlich.
- [ ] Mit Hansefit-Checkbox wird eine ungueltige Playtomic-E-Mail abgewiesen.

**Kontext**:
- `internal/model/contact.go`
- `internal/model/contact_test.go`
- `internal/service/contact_service.go`

**Output**:
- Erweiterte Domain-Tests fuer die komplette Pflichtfeldmatrix.

### Task: 14.5 Handler-Parsing und Consent-Mapping aktualisieren
**Priority**: P0
**Blocked by**: 14.3
**Agent**: Agent C

**Akzeptanzkriterien**:
- [ ] Handler liest `terms`/`terms_accepted` aus dem Formular.
- [ ] Handler erhaelt Formularwerte nach Validierungsfehlern.
- [ ] Handler setzt Hansefit-Felder nur, wenn Hansefit aktiv ist, oder Service verwirft sie deterministisch.
- [ ] CSRF-Token bleibt unveraendert erforderlich.
- [ ] Rate-Limit-Verhalten bleibt unveraendert.
- [ ] Fehler werden weiterhin im vorhandenen Fehlerbereich ausgegeben.

**Kontext**:
- `internal/handler/contact.go`
- `web/templates/pages/contact.html`

**Output**:
- Aktualisierter Contact-Handler.
- Handler-Test oder manuelle Testmatrix, falls keine Handler-Teststruktur existiert.

### Task: 14.6 AGB-Seite oder AGB-Link klaeren
**Priority**: P1
**Blocked by**: 14.1
**Agent**: Agent C + Agent Doc

**Akzeptanzkriterien**:
- [ ] Kontaktformular verlinkt auf eine erreichbare AGB-URL.
- [ ] Footer-AGB-Link zeigt auf dieselbe URL.
- [ ] Wenn finaler AGB-Text vorhanden ist, existieren Template und Route.
- [ ] Wenn finaler AGB-Text fehlt, ist der Release-Blocker dokumentiert.

**Kontext**:
- `cmd/server/main.go`
- `internal/handler/public.go`
- `web/templates/partials/footer.html`
- `docs/Content.md`

**Output**:
- Erreichbare AGB-Route oder klar dokumentierter Content-Blocker.

**Wave-Exit-Gate**:
- [ ] Server lehnt alle unvollstaendigen Pflichtfeld-Kombinationen ab.
- [ ] Consent-Felder werden korrekt gemappt.
- [ ] AGB-Link ist technisch geloest oder als Release-Blocker sichtbar.

---

## Wave 3 - Formular-UX und Hansefit-Gating
**Agenten**: Agent D, Agent C, Agent E
**Kann einzeln ausgefuehrt werden**: ja, nach Wave 2
**Ziel**: Nutzer koennen das Formular nur mit den jeweils relevanten Pflichtfeldern absenden; Hansefit-Felder erscheinen erst nach Checkbox-Aktivierung.

### Task: 14.7 Kontaktformular-Markup umbauen
**Priority**: P0
**Blocked by**: 14.4, 14.5
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] Formular enthaelt Nachname, Vorname und E-Mail als immer sichtbare Pflichtfelder.
- [ ] Freitext/Nachricht ist immer sichtbar und optional.
- [ ] Datenschutzerklaerung und AGB sind als verpflichtende Checkboxen sichtbar.
- [ ] Hansefit-Registrierung ist eine Checkbox.
- [ ] Hansefit-ID und Playtomic-E-Mail sind initial nicht sichtbar.
- [ ] Hansefit-ID und Playtomic-E-Mail erscheinen erst nach aktivierter Hansefit-Checkbox.
- [ ] Bei Server-Validierungsfehlern bleiben Hansefit-Felder sichtbar, wenn Hansefit aktiv war.
- [ ] Labels und Hilfetexte sind eindeutig, kurz und nicht widerspruechlich.

**Kontext**:
- `web/templates/pages/contact.html`
- `web/static/css/style.css`

**Output**:
- Ueberarbeitetes Kontaktformular-Template.
- Aktualisierte Formularstyles.

### Task: 14.8 Client-seitige Blockade des Absenden-Buttons
**Priority**: P0
**Blocked by**: 14.7
**Agent**: Agent D

**Akzeptanzkriterien**:
- [ ] `novalidate` wird entfernt oder durch eine eigene, vollstaendige Validierungslogik ersetzt.
- [ ] Native `required`-Attribute blockieren Absenden fuer immer verpflichtende Felder.
- [ ] Hansefit-Felder erhalten `required` nur bei aktivierter Hansefit-Checkbox.
- [ ] Hansefit-Felder sind bei deaktivierter Checkbox `hidden` und `disabled`.
- [ ] Beim Deaktivieren der Hansefit-Checkbox werden Hansefit-Feldwerte geloescht oder serverseitig ignoriert.
- [ ] Submit-Button bleibt bedienbar, aber Browser/JS verhindern ungueltige Submission.
- [ ] Ohne JavaScript bleibt die serverseitige Validierung korrekt; progressive Enhancement bricht den Flow nicht.

**Kontext**:
- `web/static/js/main.js`
- `web/templates/pages/contact.html`

**Output**:
- Aktualisierte Hansefit-JS-Logik.
- Manuell pruefbare Client-Validierung.

### Task: 14.9 Accessibility und mobile Formular-QA
**Priority**: P1
**Blocked by**: 14.7, 14.8
**Agent**: Agent D + Agent E

**Akzeptanzkriterien**:
- [ ] Checkboxen sind per Tastatur erreichbar und klar fokussierbar.
- [ ] `aria-controls` und `aria-expanded` spiegeln den Hansefit-Zustand korrekt.
- [ ] Versteckte Hansefit-Felder werden nicht per Tab-Fokus erreicht.
- [ ] Mobile Layout hat keine Ueberlappungen oder zu kleine Touch-Ziele.
- [ ] Fehlermeldungen bleiben fuer Screenreader nachvollziehbar.

**Kontext**:
- `web/templates/pages/contact.html`
- `web/static/css/style.css`
- `web/static/js/main.js`

**Output**:
- Accessibility-Checkliste in QA-Notizen.

**Wave-Exit-Gate**:
- [ ] Hansefit-Felder erscheinen nur nach aktiver Checkbox.
- [ ] Client verhindert ungueltiges Absenden fuer alle Pflichtfelder.
- [ ] Server bleibt Quelle der Wahrheit fuer Validierung.

---

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

## Parallelisierung
- Wave 1.1 kann parallel zu technischer Analyse laufen, blockiert aber finale Abnahme.
- Wave 1.2 und Wave 1.3 koennen parallel starten, sobald 14.1 bestaetigt ist.
- Wave 2.6 kann parallel zu 14.4/14.5 laufen, solange der AGB-Link erst in Wave 3 final eingebaut wird.
- Wave 3 bleibt sequenziell nach Backend-Vertrag, damit Template, JS und Server-Validierung nicht auseinanderlaufen.
- Wave 4 startet erst nach der fertigen Formular-UX.

## Risiken und offene Punkte
- AGB-Content fehlt aktuell. Ohne finalen AGB-Text ist ein produktiver Release fachlich/rechtlich blockiert.
- Die bestehende Migration `001_contact_submissions.up.sql` setzt `privacy_accepted` default auf `TRUE`; Iteration 14 sollte dieses Verhalten nicht ausweiten und fuer `terms_accepted` bewusst `FALSE` verwenden.
- Das aktuelle Formular nutzt `novalidate`; wenn das nicht entfernt wird, ist die Anforderung "kein Absenden ohne Pflichtfelder" client-seitig nicht erfuellt.
- Wenn Hansefit-Felder nur optisch versteckt, aber nicht disabled/ignoriert werden, koennen normale Kontaktanfragen versehentlich Hansefit-Daten speichern.

## Definition of Done
- [x] Formular zeigt Hansefit-ID und Playtomic-E-Mail nur bei aktivierter Hansefit-Registrierung.
- [x] Immer verpflichtende Felder sind client- und serverseitig abgesichert.
- [x] Konditionale Hansefit-Pflichtfelder sind client- und serverseitig abgesichert.
- [x] Datenschutzerklaerung und AGB muessen vor Absenden akzeptiert werden.
- [x] Optionales Freitextfeld ist immer sichtbar und nie verpflichtend.
- [x] AGB-Zustimmung wird gespeichert oder ein bewusst dokumentierter Legal-Blocker verhindert Release.
- [ ] Tests und manuelle QA decken normale Anfrage und Hansefit-Anfrage ab.
- [x] Doku und Iterationsstatus sind aktualisiert.

## Umsetzungsstatus 2026-04-21
- Wave 1 abgeschlossen: `terms_accepted` ist per Migration ergänzt, Model/Repository/Service speichern die AGB-Zustimmung.
- Wave 2 abgeschlossen: Handler liest AGB-Zustimmung, Hansefit-Felder werden bei normaler Kontaktanfrage ignoriert, `/agb` ist als Platzhalter-Route erreichbar.
- Wave 3 abgeschlossen: Kontaktformular nutzt native Pflichtfeldvalidierung, Hansefit-Felder werden nur bei aktivierter Registrierung sichtbar und per JS required/enabled gesetzt.
- Wave 4 technisch abgeschlossen: Domain-Tests und Renderer-Test ergänzt, `go test ./...` läuft im Go-Container grün.
- Restrisiko: Manuelle Browser-QA wurde in dieser Umsetzung nicht ausgeführt.
- Restrisiko: Finaler AGB-Text fehlt weiterhin und muss vor produktiver Freigabe in `web/templates/pages/agb.html` ersetzt werden.
