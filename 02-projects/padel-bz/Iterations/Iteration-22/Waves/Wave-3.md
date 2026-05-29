# Wave 3: Coming-Soon-Seite und Hansefit-Aktion

**Status:** Completed
**Skill:** frontend-eng
**Rolle:** frontend-eng
**Kann parallel ausgefuehrt werden:** nein
**Blocked by:** Wave 1, Wave 2

## Ziel

Die oeffentliche Vorschaltseite wird als markengerechte Coming-Soon-Seite mit Courts-Logo, Hansefit-CTA und Aktionshinweis umgesetzt. Die Aktionsbedingungen sind direkt auf der Seite per Popup/Dialog erreichbar.

## Tasks

### Task 22.7: Coming-Soon-Seite visuell neu gestalten
**Priority:** P0
**Blocked by:** Wave 1
**Agent:** Agent C (frontend-eng)

**Akzeptanzkriterien:**
- [x] `web/templates/pages/coming-soon.html` zeigt ein Courts-Logo als starkes First-Viewport-Signal.
- [x] Headline/Copy kommunizieren Eröffnung Herbst 2026 und Bad Zwischenahn.
- [x] Primärer CTA fuehrt zu `/kontakt` mit klarer Hansefitregistrierung.
- [x] Sekundaere Links fuehren nur zu `/impressum`, `/datenschutz`, `/agb`.
- [x] Seite funktioniert als Standalone-HTML ohne Base-Layout.
- [x] Design bleibt dunkel, minimalistisch, sportlich und mobile-first.
- [x] Text passt auf Mobile und Desktop ohne Ueberlappungen.

**Kontext:**
- `web/templates/pages/coming-soon.html`
- Logo-Assets: `web/static/img/logo_courts.svg`, `web/static/img/logo_courts_transparent.svg`, `web/static/img/logo_courts_transparent_mit_slogan.svg`

**Output:**
- Aktualisierte Coming-Soon-Seite.

### Task 22.8: Hansefit-Aktion prominent, aber kompakt bewerben
**Priority:** P0
**Blocked by:** 22.7
**Agent:** Agent C (frontend-eng)

**Akzeptanzkriterien:**
- [x] Der Text "Ersten 100 Registrierten erhalten Baelle gratis!" ist auf der Coming-Soon-Seite sichtbar.
- [x] Der Aktionstext wirkt wie ein Special/Badge, ohne die Hansefitregistrierung zu verdraengen.
- [x] Ein Link/Button "Aktionsbedingungen" oeffnet den Dialog.
- [x] Keine Tracking- oder Gewinnerlogik wird suggeriert, die technisch nicht existiert.

**Kontext:**
- Nutzer sollen bereits vorab das Formular ausfuellen koennen.
- Kein neues Datenmodell in dieser Iteration.

**Output:**
- Aktionshinweis auf der Coming-Soon-Seite.

### Task 22.9: Aktionsbedingungen als barrierearmen Dialog einbauen
**Priority:** P0
**Blocked by:** 22.3, 22.8
**Agent:** Agent C (frontend-eng)

**Akzeptanzkriterien:**
- [x] Dialog wird per Button/Link geoeffnet und geschlossen.
- [x] Dialog ist per Tastatur bedienbar.
- [x] Dialog nutzt `role="dialog"` oder native `<dialog>` mit sinnvollem Label.
- [x] Lange Inhalte sind in einem scrollbaren Bereich lesbar.
- [x] Inhalt basiert auf `docs/aktionsbedingungen-hansefit.html`.
- [x] Bei deaktiviertem JavaScript bleibt mindestens ein sichtbarer Hinweis oder ein fallback-lesbarer Bereich vorhanden.

**Kontext:**
- `web/templates/pages/coming-soon.html`
- `web/static/js/main.js` nur bei Bedarf anfassen; Vanilla JS bevorzugt.

**Output:**
- Aktionsbedingungen-Popup/Dialog auf der Coming-Soon-Seite.

## Wave-3-Exit-Gate

- [x] Coming-Soon-Seite ist visuell fertig.
- [x] Hansefit-CTA fuehrt zu `/kontakt`.
- [x] Aktionsbedingungen sind direkt erreichbar.
- [x] Keine oeffentlichen Links fuehren in gesperrte Inhalte.

## Notes

- Keine Landingpage-Sektion bauen; die Coming-Soon-Seite ist die erste und eigentliche oeffentliche Experience im Prelaunch.
- `web/templates/pages/coming-soon.html` wurde als Standalone-Prelaunch-Seite neu umgesetzt.
- Courts-Logo, Eröffnung Herbst 2026, Bad Zwischenahn und Hansefit-CTA sind im ersten Viewport sichtbar.
- Aktion "Ersten 100 Registrierten erhalten Bälle gratis!" ist als Special sichtbar.
- Aktionsbedingungen sind per native `<dialog>` integriert; Links zeigen nur auf `/kontakt`, `/datenschutz`, `/agb` oder Mail.
- Verification: Template-Parsing weiterhin erfolgreich via `go test ./internal/handler -run TestRendererRendersContactAndAGBPages`.

## Abschluss

- [x] Wave abgeschlossen
