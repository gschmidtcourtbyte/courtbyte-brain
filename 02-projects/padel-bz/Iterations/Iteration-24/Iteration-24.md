# Iteration 24: Coming-Soon Layout Cleanup

**Status:** Planned
**Rolle:** Acting as Product Owner. Planning Iteration 24 for padel-bz.
**Datum:** 2026-05-04

## Ziel

Die Coming-Soon-Seite wird visuell reduziert und klarer ausgerichtet. Der Content steht nicht mehr in einem sichtbaren Kasten, die obere Textmarke wird entfernt, und der Eroeffnungshinweis steht direkt unter dem Logo. Logo, Eroeffnungshinweis und Text wirken als eine buendige Content-Gruppe.

## Ausgangslage

- Die oeffentliche Prelaunch-Ansicht rendert die Coming-Soon-Seite.
- Der aktuelle Content ist visuell in einem Kasten bzw. einer Card gefasst.
- Oberhalb des Logos steht der Text `Courts Padelclub Bad Zwischenahn`.
- `Eröffnung Herbst 2026` soll naeher an die Marke ruecken und direkt unter dem Logo erscheinen.
- Die Aenderung betrifft ausschliesslich die Coming-Soon-Seite; Backend, Admin, CMS, Legal-Seiten und Navigation bleiben unveraendert.

## Scope

### Frontend / Template / CSS

- Den sichtbaren Kasten bzw. Card-Rahmen um den Coming-Soon-Content entfernen.
- Den Text `Courts Padelclub Bad Zwischenahn` ganz oben ersatzlos entfernen.
- `Eröffnung Herbst 2026` direkt unter dem Logo platzieren.
- Logo und nachfolgende Texte optisch buendig ausrichten.
- Spacing auf Mobile und Desktop so anpassen, dass die Seite ruhig, reduziert und sauber lesbar bleibt.

### Verification

- Coming-Soon-Seite lokal rendern bzw. ueber bestehenden Render-/Build-Check absichern.
- Pruefen, dass keine Aenderungen an anderen Seiten oder Routen noetig sind.

## Nicht im Scope

- Keine neuen Assets.
- Keine inhaltlichen Aenderungen ausser Entfernen/Verschieben der genannten Texte.
- Keine Aenderungen an Landingpage, Kontakt, Legal-Seiten, Admin, CMS oder Navigation.
- Keine Backend-, Middleware-, Datenbank- oder Deployment-Aenderungen.
- Keine Migrationen.

## Produktentscheidungen

- Die Coming-Soon-Seite soll als reduzierte, offene Prelaunch-Flaeche wirken, nicht als eingerahmte Karte.
- `Eröffnung Herbst 2026` wird als sekundaere Markeninformation direkt an das Logo gekoppelt.
- Der bisherige obere Label-Text entfaellt, weil Logo und Eroeffnungshinweis die noetige Orientierung liefern.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Abnahme, Wave-Freigabe |
| Agent A | frontend-eng | Coming-Soon-Template und Styling aktualisieren, responsive Pruefung |

## Waves

| Wave | Skill | Ziel | Status |
|------|-------|------|--------|
| Wave 1 | frontend-eng | Coming-Soon-Content entrahmen, Textmarke entfernen, Eroeffnungshinweis unter Logo platzieren | Planned |

## Critical Path

1. Wave 1 ausfuehren.
2. Render-/Build-Check durchfuehren.
3. PO-Abnahme: Coming-Soon-Seite auf Mobile und Desktop visuell pruefen.

## Risiken

| Risiko | Impact | Umgang |
|--------|--------|--------|
| Entfernen des Kastens reduziert Kontrast/Lesbarkeit vor dem Hintergrund | Mittel | Textkontrast und Abstaende explizit in Wave 1 pruefen; bei Bedarf subtile Layout-/Spacing-Anpassung ohne Card-Rahmen |
| Buendigkeit ist je nach Logo-Breite optisch unklar | Niedrig | Logo, Eroeffnungshinweis und Text an einer gemeinsamen Content-Achse ausrichten |
| Aenderung greift versehentlich auf andere Seiten durch globale CSS-Klassen | Mittel | CSS moeglichst auf Coming-Soon-spezifische Selektoren begrenzen und Diff pruefen |

## Definition of Done

- [ ] Coming-Soon-Content hat keinen sichtbaren Kasten, Rahmen oder Card-Hintergrund mehr.
- [ ] `Courts Padelclub Bad Zwischenahn` ist oberhalb des Logos entfernt.
- [ ] `Eröffnung Herbst 2026` steht direkt unter dem Logo.
- [ ] Logo, Eroeffnungshinweis und Text sind optisch buendig ausgerichtet.
- [ ] Mobile und Desktop zeigen keine Ueberlappungen, abgeschnittenen Texte oder Layout-Spruenge.
- [ ] Keine Code-Aenderungen ausserhalb der Coming-Soon-Template-/Style-Flaeche.
- [ ] Relevanter Render-/Build-Check ist gruen oder eine bekannte Ausnahme ist dokumentiert.

## Abschlussnotes

- Offen bis zur Umsetzung.
