# Iteration 16: Admin-Suchfix, Team-Ausblendung und Sponsoren-Section

## Ziel
Iteration 16 behebt die in Iteration 15 eingefuehrte, aber aktuell nicht korrekt funktionierende Admin-Suche. Nach einer Suche sollen in der Hansefit-Admin-Tabelle ausschliesslich Zeilen angezeigt werden, deren sichtbare Daten zum Suchbegriff passen.

Zusaetzlich wird die Section "Das Team" im Frontend vorerst ausgeblendet, weil sie fachlich erst spaeter relevant wird. Als letzte sichtbare Content-Section der Website wird eine neue Section "Sponsoren" ergaenzt. Sponsoren werden dort nur ueber Logos dargestellt; fuer noch offene Sponsorenplaetze werden Platzhalter genutzt.

## Ausgangslage
- Iteration 15 hat eine Admin-Suche fuer `/admin/hansefit?q=...` geplant und teilweise umgesetzt.
- In der aktuellen Codebasis existieren bereits Suchpfade in:
  - `internal/handler/admin_hansefit.go`
  - `internal/service/contact_service.go`
  - `internal/repository/contact_repo.go`
  - `web/templates/admin/hansefit.html`
- Der Nutzer berichtet: Nach Eingabe in das Suchfeld liefert die Suche kein korrektes Ergebnis.
- Erwartetes Verhalten: Bei nicht-leerer Suche zeigt die Tabelle nur passende Hansefit-Zeilen; bei leerer Suche zeigt sie weiterhin alle Hansefit-Anfragen.
- Die Team-Section ist aktuell in `web/templates/pages/home.html` sichtbar und wird ueber `/#team` in Navigation und Footer verlinkt.
- Die Website endet aktuell mit der Kontakt-Section vor dem Footer.
- Ein erstes Sponsorenlogo liegt unter `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg`.
- Dateien unter `logo/` werden nicht automatisch als Web-Asset ausgeliefert; fuer Frontend-Auslieferung muss das Logo in einen passenden Pfad unter `web/static/img/...` uebernommen werden.

## Scope
- Admin-Suche reproduzieren, Fehlerursache isolieren und korrigieren.
- Admin-Tabelle bei Suchbegriff serverseitig filtern.
- Nur Zeilen rendern, die in den relevanten sichtbaren Datenfeldern einen Treffer enthalten.
- Suchfeld-Wert nach Absenden beibehalten.
- "Keine Treffer"-Zustand sauber anzeigen, wenn kein Datensatz passt.
- Leere Suche zeigt weiterhin die vollstaendige Hansefit-Liste.
- Team-Section auf der Homepage ausblenden.
- Team-Links aus Desktop-Navigation, Mobile-Navigation und Footer entfernen.
- Kontakt-Section als sichtbare Section nach Team-Ausblendung konsistent nummerieren.
- Neue Sponsoren-Section als letzte Content-Section vor dem Footer einbauen.
- AXA-Logo als erstes echtes Sponsorenlogo integrieren.
- 4-6 erwartete Sponsoren beruecksichtigen; Layout muss auch mit weniger oder mehr Logos stabil bleiben.
- Platzhalter fuer noch offene Sponsorenplaetze einbauen.
- Tests und manuelle QA fuer Suche und Frontend-Layout ergaenzen.
- Relevante Content-/Operations-Dokumentation aktualisieren.

## Nicht im Scope
- Sponsor-CMS oder Admin-Verwaltung fuer Sponsoren.
- Sponsor-Texte, Beschreibungen, Deeplinks oder Tracking.
- Finale Team-Inhalte oder Team-Fotos.
- Vollstaendiger Landingpage-Redesign.
- Pagination der Admin-Tabelle.
- Neue Suchindizes, solange die erwartete Datenmenge klein bleibt.
- Externe Sponsor-Logo-Recherche.

## Produktentscheidungen
- Die Admin-Suche bleibt ein serverseitiger GET-Flow auf `/admin/hansefit?q=<suchbegriff>`.
- Die Suche filtert ueber die relevanten sichtbaren Datenfelder der Tabelle: Vorname, Nachname, E-Mail/Playtomic-E-Mail, Hansefit-ID und Freitext.
- Button-/Aktionsspalten werden nicht durchsucht, damit Begriffe wie "Bestaetigen" keine fachlich nutzlosen Treffer erzeugen.
- Status-Suche wird fuer diese Iteration nicht priorisiert, weil "bearbeitet" als Teilstring auch "unbearbeitet" treffen wuerde und dadurch leicht missverstaendliche Ergebnisse entstehen.
- Suchbegriffe werden getrimmt und auf maximal 200 Zeichen begrenzt.
- SQL bleibt parametrisiert; Sonderzeichen wie `%` und `_` duerfen nicht dazu fuehren, dass eine Suche unbeabsichtigt alle Zeilen trifft.
- "Das Team" wird sichtbar entfernt oder serverseitig nicht gerendert. CSS darf vorerst bestehen bleiben, wenn es spaetere Reaktivierung erleichtert.
- `/#team` darf nach der Aenderung nicht mehr sichtbar verlinkt sein.
- Die Sponsoren-Section wird hinter "Kontakt & Anfahrt" und vor dem Footer eingefuegt.
- Sponsoren werden logo-only dargestellt: keine Sponsor-Beschreibung, keine Marketingtexte pro Sponsor.
- Startzustand: 1 echtes AXA-Logo plus Platzhalter bis zu insgesamt ca. 6 Slots.
- Sponsor-Logos werden lokal ausgeliefert, lazy geladen und nicht verzerrt.

## Critical Path
1. Suchproblem reproduzieren und den brechenden Layer bestimmen.
2. Suchvertrag in Repository/Service/Handler stabilisieren.
3. Admin-Template so pruefen, dass nur gefilterte Rows gerendert werden.
4. Tests fuer Treffer, Nicht-Treffer, leere Suche und Sonderzeichen absichern.
5. Team-Section und Team-Links aus der sichtbaren Frontend-Navigation entfernen.
6. Sponsoren-Asset vorbereiten und Sponsoren-Section als letzte Section designen.
7. Desktop-/Mobile-QA und finaler Review.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe, Akzeptanzkriterien |
| Agent A | testing-eng | Reproduktion, Testfaelle, Regressionen |
| Agent B | database-eng | SQL-Suchlogik, NULL-/Wildcard-Verhalten, Performance-Bewertung |
| Agent C | service-eng | Service-Vertrag fuer Suchfunktion |
| Agent D | handler-eng | Query-Parsing, Response-Mapping, Admin-Flow |
| Agent E | frontend-eng | Admin-Such-UI, Team-Ausblendung, Sponsoren-Design |
| Agent F | documentation-eng | Content-/Operations-Doku aktualisieren |
| Agent R | review-eng | Finaler risikoorientierter Review |

---

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

## Wave 2 - Homepage-Struktur: Team ausblenden, Sponsoren vorbereiten
**Agenten**: Agent E, Agent F
**Kann einzeln ausgefuehrt werden**: ja, parallel zu Wave 1 nach kurzer Abstimmung
**Ziel**: Die oeffentliche Homepage zeigt vorerst keine Team-Inhalte und bekommt die Grundlage fuer die Sponsoren-Section.

### Task 16.5: Team-Section sichtbar ausblenden
**Priority**: P1
**Blocked by**: none
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] `#team` ist auf der Homepage nicht mehr sichtbar.
- [ ] Desktop-Navigation enthaelt keinen sichtbaren Link zu `/#team`.
- [ ] Mobile-Navigation enthaelt keinen sichtbaren Link zu `/#team`.
- [ ] Footer enthaelt keinen sichtbaren Link zu `/#team`.
- [ ] Es bleiben keine sichtbaren Dead-Anchor-Links auf `/#team`.
- [ ] Kontakt-Section wird nach der Ausblendung konsistent nummeriert.
- [ ] CSS fuer spaetere Team-Reaktivierung darf bestehen bleiben, wird aber nicht als sichtbarer Inhalt genutzt.

**Kontext**:
- `web/templates/pages/home.html`
- `web/templates/partials/nav.html`
- `web/templates/partials/footer.html`
- `web/static/css/style.css`
- `docs/Content.md`

**Output**:
- Homepage ohne sichtbare Team-Section.
- Navigation und Footer ohne Team-Link.

### Task 16.6: Sponsoren-Assets vorbereiten
**Priority**: P1
**Blocked by**: none
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] AXA-Logo wird aus `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg` in einen webfaehigen Pfad unter `web/static/img/sponsors/` uebernommen.
- [ ] Browserpfad fuer das Logo ist eindeutig dokumentiert, z.B. `/static/img/sponsors/axa-versicherung-haake-haake-ohg.jpg`.
- [ ] `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg:Zone.Identifier` wird nicht verwendet und nicht ausgeliefert.
- [ ] Platzhalter fuer weitere Sponsoren sind ohne externe Abhaengigkeiten verfuegbar.
- [ ] Asset-Namen sind stabil und sprechend.
- [ ] Bildgroesse/Dateigroesse wird geprueft; bei Bedarf wird eine optimierte Web-Kopie erstellt.

**Kontext**:
- `logo/axa-versicherung-haake-haake-ohg_full_1584974628.jpg`
- `web/static/img/`
- `web/templates/pages/home.html`

**Output**:
- Webfaehiges Sponsorenlogo.
- Struktur fuer weitere Sponsor-Assets.

### Task 16.7: Sponsoren-Section als letzte Section designen
**Priority**: P1
**Blocked by**: 16.6
**Agent**: Agent E (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Neue Section mit `id="sponsoren"` steht als letzte Content-Section vor dem Footer.
- [ ] Section-Titel lautet "Sponsoren".
- [ ] Sponsorendarstellung ist logo-only: keine Beschreibungen und keine CTA-Texte pro Sponsor.
- [ ] AXA-Logo wird als reales Sponsorlogo angezeigt.
- [ ] Weitere Slots sind als klare Logo-Platzhalter dargestellt.
- [ ] Startlayout deckt 4-6 Sponsoren sauber ab.
- [ ] Layout bleibt stabil bei 1, 4, 6 und 8 Sponsor-Slots.
- [ ] Logos werden nicht verzerrt; `object-fit: contain` oder gleichwertige Loesung wird genutzt.
- [ ] Bilder sind lazy geladen, ausser eine spaetere Performance-Pruefung zeigt einen Grund dagegen.
- [ ] Mobile Ansicht hat keine ueberlappenden Logos oder Texte.
- [ ] Desktop Ansicht wirkt hochwertig und passt zur bestehenden dunklen visuellen Sprache.
- [ ] Keine externen Bildquellen oder CDN-Abhaengigkeiten.

**Kontext**:
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- `web/static/img/sponsors/`

**Output**:
- Neue Sponsoren-Section.
- CSS fuer responsive Logo-Grid/Logo-Tiles.

**Wave-Exit-Gate**:
- [ ] Team-Inhalte und Team-Links sind nicht sichtbar.
- [ ] Sponsoren-Section ist letzte sichtbare Section vor dem Footer.
- [ ] AXA-Logo wird ausgeliefert.
- [ ] Platzhalter decken die erwartete Sponsorenzahl ab.

---

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

## Parallelisierung

```text
Wave 1:
  Agent A + Agent D -> 16.1 Reproduktion Admin-Suche
  Agent B           -> 16.2 Repository-Suche      (nach 16.1)
  Agent C + Agent D -> 16.3 Service/Handler Flow  (nach 16.1)
  Agent E           -> 16.4 Admin-Template        (nach 16.2 + 16.3)

Wave 2:
  Agent E -> 16.5 Team ausblenden          (kann parallel zu Wave 1 starten)
  Agent E -> 16.6 Sponsoren-Assets         (kann parallel zu 16.5 starten)
  Agent E -> 16.7 Sponsoren-Section        (nach 16.6)

Wave 3:
  Agent A + Agent B -> 16.8 Such-Tests      (nach 16.2 + 16.3 + 16.4)
  Agent A + Agent E -> 16.9 Manuelle QA     (nach 16.4 + 16.5 + 16.7)
  Agent F           -> 16.10 Doku           (nach 16.5 + 16.7)
  Agent R + PO Lead -> 16.11 Review         (nach 16.8 + 16.9 + 16.10)
```

**Optimierte Ausfuehrungsreihenfolge**:
- 16.1 startet sofort.
- 16.5 und 16.6 koennen parallel zu 16.1 laufen, da sie unabhaengig von der Admin-Suche sind.
- 16.2 und 16.3 starten nach der Reproduktion, damit nicht am falschen Layer korrigiert wird.
- 16.4 startet erst, wenn klar ist, welches Ergebnis das Backend liefert.
- 16.7 startet nach Asset-Vorbereitung.
- 16.8 und 16.9 sind Quality Gates vor Review.

## Risiken und offene Punkte
- Die Ursache der defekten Suche ist noch nicht bestaetigt. Moegliche Ursachen: falsche Query-Semantik, Datenzustand, Template-Rendering, Handler-Flow oder fehlende Testdaten.
- Wenn `%` oder `_` aktuell als SQL-Wildcards wirken, kann eine Suche unbeabsichtigt zu viele Zeilen liefern. Das muss in dieser Iteration explizit abgesichert werden.
- Falls lokale Integrationstests eine echte PostgreSQL-Instanz benoetigen, muss die Teststrategie an die vorhandene Docker-/CI-Struktur angepasst werden.
- Team-Inhalte bleiben fachlich deferred. Wenn sie nur ausgeblendet und nicht geloescht werden, muss spaetere Reaktivierung bewusst erfolgen.
- Sponsor-Platzhalter sind nur Uebergangsinhalt. Sobald finale Logos vorliegen, muessen Platzhalter ersetzt und Bildrechte/Dateiformate geprueft werden.
- Das AXA-Logo liegt als JPG vor. Falls die Datei sehr gross ist oder unruhige Raender hat, kann eine optimierte Web-Kopie noetig sein.

## Definition of Done
- [ ] Admin-Suche zeigt bei Suchbegriff nur passende Hansefit-Zeilen.
- [ ] Admin-Suche zeigt bei Nicht-Treffer eine klare Keine-Treffer-Meldung.
- [ ] Leere Admin-Suche zeigt alle Hansefit-Anfragen.
- [ ] SQL-Suche ist parametrisiert und behandelt Wildcard-Sonderzeichen kontrolliert.
- [ ] Automatisierte Tests fuer die Suchfunktion sind vorhanden und gruen.
- [ ] "Das Team" ist auf der Website nicht sichtbar.
- [ ] Navigation und Footer enthalten keinen Team-Link mehr.
- [ ] "Sponsoren" ist die letzte Content-Section vor dem Footer.
- [ ] AXA-Logo ist in der Sponsoren-Section sichtbar.
- [ ] Platzhalter fuer weitere Sponsoren sind eingebaut.
- [ ] Sponsoren-Grid funktioniert responsiv fuer weniger, 4-6 und mehr Sponsoren.
- [ ] `docs/Content.md` und ggf. `docs/Operations.md` sind aktualisiert.
- [ ] Finaler Review ist abgeschlossen.
