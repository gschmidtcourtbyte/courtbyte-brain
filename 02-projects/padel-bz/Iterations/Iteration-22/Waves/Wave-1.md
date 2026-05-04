# Wave 1: Legal- und Aktionscontent vorbereiten

**Status:** Planned
**Skill:** documentation-eng
**Rolle:** documentation-eng
**Kann parallel ausgefuehrt werden:** bedingt
**Blocked by:** none

## Ziel

Die gelieferten AGB, die Datenschutzerklaerung und die Aktionsbedingungen werden so vorbereitet, dass sie in die vorhandene Server-Side-Rendering-Template-Struktur integriert werden koennen, ohne die gelieferten Inhalte fachlich umzuschreiben.

## Tasks

### Task 22.1: AGB-Inhalt in bestehende Template-Struktur uebernehmen
**Priority:** P0
**Blocked by:** none
**Agent:** Agent A (documentation-eng)

**Akzeptanzkriterien:**
- [ ] `docs/agb_courts.html` wurde als Quelle verwendet.
- [ ] `web/templates/pages/agb.html` enthaelt keinen Platzhaltertext mehr.
- [ ] Der fachliche AGB-Inhalt ist vollstaendig im bestehenden Base-Layout renderbar.
- [ ] Seitentitel und Headline benennen "Allgemeine Geschaeftsbedingungen".
- [ ] Links und Sonderzeichen sind HTML-sicher und template-kompatibel.

**Kontext:**
- Quelle: `docs/agb_courts.html`
- Ziel: `web/templates/pages/agb.html`
- Bestehende Legal-Styles in `web/static/css/style.css` nutzen.

**Output:**
- Aktualisiertes AGB-Template.

### Task 22.2: Datenschutzerklaerung mit gelieferter Version abgleichen/aktualisieren
**Priority:** P0
**Blocked by:** none
**Agent:** Agent A (documentation-eng)

**Akzeptanzkriterien:**
- [ ] `docs/datenschutzerklaerung_courts.html` wurde als Quelle verwendet.
- [ ] `web/templates/pages/datenschutz.html` bildet die gelieferte Datenschutzerklaerung vollstaendig ab.
- [ ] Keine fachlichen Inhalte werden ohne explizite Anforderung gekuerzt oder umgeschrieben.
- [ ] Rendering erfolgt ueber das bestehende Base-Layout.
- [ ] Datenschutz-Link im Kontaktformular bleibt korrekt.

**Kontext:**
- Quelle: `docs/datenschutzerklaerung_courts.html`
- Ziel: `web/templates/pages/datenschutz.html`

**Output:**
- Aktualisiertes Datenschutz-Template oder dokumentierte Bestaetigung, dass bestehender Inhalt deckungsgleich ist.

### Task 22.3: Aktionsbedingungen fuer Dialog vorbereiten
**Priority:** P0
**Blocked by:** none
**Agent:** Agent A (documentation-eng)

**Akzeptanzkriterien:**
- [ ] `docs/aktionsbedingungen-hansefit.html` wurde als Quelle verwendet.
- [ ] Der Aktionsbedingungen-Inhalt ist fuer Einbindung in einen Dialog vorbereitet.
- [ ] Der Dialog-Inhalt enthaelt Veranstalter, Teilnahmebedingungen, Aktionsumfang und Ausschluesse aus der Quelle.
- [ ] Es wird kein neuer oeffentlich verlinkter Content-Pfad benoetigt.
- [ ] Der Text bleibt juristisch unverfaelscht.

**Kontext:**
- Quelle: `docs/aktionsbedingungen-hansefit.html`
- Ziel spaeter: `web/templates/pages/coming-soon.html`

**Output:**
- Integrierbarer Aktionsbedingungen-Block fuer Wave 3.

## Wave-1-Exit-Gate

- [ ] AGB-Quelle verarbeitet.
- [ ] Datenschutz-Quelle verarbeitet.
- [ ] Aktionsbedingungen-Quelle verarbeitet.
- [ ] Keine Code-Repo-Dateien ausser Templates/CSS wurden fuer Legal-Content angefasst.

## Notes

- Die Dokumente sind vom Auftraggeber geliefert. Diese Wave nimmt keine Rechtsberatung vor.

## Abschluss

- [ ] Wave abgeschlossen
