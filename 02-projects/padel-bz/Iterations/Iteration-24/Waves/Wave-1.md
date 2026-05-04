# Wave 1: Coming-Soon Layout Cleanup

**Status:** Planned
**Skill:** frontend-eng
**Rolle:** frontend-eng
**Kann parallel ausgefuehrt werden:** nein
**Blocked by:** -

## Ziel

Die Coming-Soon-Seite wird als offene, reduzierte Prelaunch-Flaeche umgesetzt. Der Content steht ohne sichtbaren Kasten, die obere Textmarke entfaellt, und `Eröffnung Herbst 2026` sitzt direkt unter dem Logo. Logo und Text folgen einer gemeinsamen optischen Achse.

## Tasks

### Task 24.1: Coming-Soon-Kasten entfernen
**Priority:** P0
**Blocked by:** -
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Der zentrale Coming-Soon-Content hat keinen sichtbaren Card-/Box-Rahmen.
- [ ] Es gibt keinen auffaelligen Card-Hintergrund, der den Content als Kasten wirken laesst.
- [ ] Lesbarkeit und Kontrast bleiben auf Mobile und Desktop erhalten.
- [ ] Hintergrund und Content wirken weiterhin bewusst gestaltet, nicht wie ungestyltes Markup.

**Kontext:**
- Vermutete Hauptdatei: `web/templates/pages/coming-soon.html`.
- Styling vermutlich in `web/static/css/style.css` oder ueber bestehende Utility-Klassen im Template.
- Aenderungen moeglichst Coming-Soon-spezifisch halten, damit Legal-, Admin- und Landingpage-Layouts nicht betroffen sind.

**Output:**
- Aktualisiertes Coming-Soon-Template und/oder lokal begrenzte CSS-Anpassung.

### Task 24.2: Obere Textmarke entfernen
**Priority:** P0
**Blocked by:** -
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Der Text `Courts Padelclub Bad Zwischenahn` erscheint nicht mehr ganz oben im Coming-Soon-Content.
- [ ] Entfernen des Textes hinterlaesst keinen ungewollten Leerraum.
- [ ] Seitentitel, Meta-Daten oder andere technische Identifikatoren werden nicht veraendert, sofern sie nicht sichtbar zur Coming-Soon-Flaeche gehoeren.

**Kontext:**
- Es geht um den sichtbaren Text oberhalb des Logos, nicht um SEO-/Meta-Tags.

**Output:**
- Bereinigtes Coming-Soon-Markup.

### Task 24.3: Eroeffnungshinweis unter das Logo verschieben
**Priority:** P0
**Blocked by:** 24.2
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] `Eröffnung Herbst 2026` steht direkt unter dem Logo.
- [ ] Abstand zwischen Logo und Eroeffnungshinweis ist bewusst und kompakt.
- [ ] Der nachfolgende Fliesstext bleibt klar darunter gruppiert.
- [ ] Die visuelle Hierarchie ist: Logo, Eroeffnungshinweis, Haupttext/weitere Inhalte.

**Kontext:**
- Eroeffnungstermin bleibt inhaltlich unveraendert.
- Kein neues Label oder zusaetzlicher Text hinzufuegen.

**Output:**
- Aktualisierte Reihenfolge im Coming-Soon-Markup.

### Task 24.4: Buendigkeit und Responsive-Verhalten pruefen
**Priority:** P0
**Blocked by:** 24.1, 24.3
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Logo, `Eröffnung Herbst 2026` und Text sind optisch buendig ausgerichtet.
- [ ] Desktop-Ansicht wirkt ruhig und nicht zu breit laufend.
- [ ] Mobile-Ansicht zeigt keine Ueberlappungen, abgeschnittenen Texte oder unerwuenschte horizontale Scrollbars.
- [ ] Text passt in seine Container und bleibt gut lesbar.

**Kontext:**
- Bei zentriertem Layout bedeutet buendig: gemeinsame Mittelachse bzw. konsistente Text-/Logo-Ausrichtung.
- Bei linksbuendigem Layout bedeutet buendig: gemeinsame linke Kante von Logo und Textblock.
- Entscheidung an bestehendem Coming-Soon-Design ausrichten.

**Output:**
- Finaler Layoutzustand der Coming-Soon-Seite.

### Task 24.5: Verification
**Priority:** P1
**Blocked by:** 24.4
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Relevanter Render-Test oder Build-Check laeuft gruen.
- [ ] `git diff --check` laeuft gruen.
- [ ] Diff zeigt keine Aenderungen ausserhalb der Coming-Soon-Template-/Style-Flaeche, ausser sie sind technisch zwingend begruendet.
- [ ] Abschlussnote dokumentiert, welche Checks ausgefuehrt wurden.

**Kontext:**
- Wenn kein spezifischer Coming-Soon-Test existiert, mindestens den bestehenden Handler-/Template-Render-Check oder `go test ./...` ausfuehren.

**Output:**
- Test-/Check-Ergebnis in den Wave-Notes.

## Wave-1-Exit-Gate

- [ ] Coming-Soon-Content ist entrahmt.
- [ ] Obere Textmarke ist entfernt.
- [ ] `Eröffnung Herbst 2026` steht unter dem Logo.
- [ ] Logo und Text sind buendig.
- [ ] Mobile/Desktop wurden geprueft.
- [ ] Relevanter Check ist gruen oder eine Ausnahme ist dokumentiert.

## Notes

- Reine Frontend-Wave.
- Keine Aenderungen an Go-Routen, Middleware, Admin, CMS, Datenbank oder Deployment.
- Keine neuen Assets oder externen Abhaengigkeiten.
