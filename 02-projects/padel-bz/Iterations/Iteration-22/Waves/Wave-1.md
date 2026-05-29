# Wave 1: Legal- und Aktionscontent vorbereiten

**Status:** Completed
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
- [x] `docs/agb_courts.html` wurde als Quelle verwendet.
- [x] `web/templates/pages/agb.html` enthaelt keinen Platzhaltertext mehr.
- [x] Der fachliche AGB-Inhalt ist vollstaendig im bestehenden Base-Layout renderbar.
- [x] Seitentitel und Headline benennen "Allgemeine Geschaeftsbedingungen".
- [x] Links und Sonderzeichen sind HTML-sicher und template-kompatibel.

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
- [x] `docs/datenschutzerklaerung_courts.html` wurde als Quelle verwendet.
- [x] `web/templates/pages/datenschutz.html` bildet die gelieferte Datenschutzerklaerung vollstaendig ab.
- [x] Keine fachlichen Inhalte werden ohne explizite Anforderung gekuerzt oder umgeschrieben.
- [x] Rendering erfolgt ueber das bestehende Base-Layout.
- [x] Datenschutz-Link im Kontaktformular bleibt korrekt.

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
- [x] `docs/aktionsbedingungen-hansefit.html` wurde als Quelle verwendet.
- [x] Der Aktionsbedingungen-Inhalt ist fuer Einbindung in einen Dialog vorbereitet.
- [x] Der Dialog-Inhalt enthaelt Veranstalter, Teilnahmebedingungen, Aktionsumfang und Ausschluesse aus der Quelle.
- [x] Es wird kein neuer oeffentlich verlinkter Content-Pfad benoetigt.
- [x] Der Text bleibt juristisch unverfaelscht.

**Kontext:**
- Quelle: `docs/aktionsbedingungen-hansefit.html`
- Ziel spaeter: `web/templates/pages/coming-soon.html`

**Output:**
- Integrierbarer Aktionsbedingungen-Block fuer Wave 3.

## Wave-1-Exit-Gate

- [x] AGB-Quelle verarbeitet.
- [x] Datenschutz-Quelle verarbeitet.
- [x] Aktionsbedingungen-Quelle verarbeitet.
- [x] Keine Code-Repo-Dateien ausser Templates/CSS wurden fuer Legal-Content angefasst.

## Notes

- Die Dokumente sind vom Auftraggeber geliefert. Diese Wave nimmt keine Rechtsberatung vor.
- `web/templates/pages/agb.html` ersetzt den bisherigen Platzhalter durch die gelieferte AGB-Fassung aus `docs/agb_courts.html`.
- `web/templates/pages/datenschutz.html` wurde mit der gelieferten Fassung inklusive Videoüberwachung aktualisiert.
- Gemeinsame Legal-Styles fuer Inhaltsverzeichnis, Klausellisten, Hinweisboxen und Adressblöcke wurden in `web/static/css/style.css` ergänzt.
- Aktionsbedingungen aus `docs/aktionsbedingungen-hansefit.html` sind als Dialog-Content fuer Wave 3 vorbereitet; es wurde kein neuer oeffentlicher Pfad angelegt.
- Verification: `go test ./internal/handler -run TestRendererRendersContactAndAGBPages` ist gruen.

## Abschluss

- [x] Wave abgeschlossen
