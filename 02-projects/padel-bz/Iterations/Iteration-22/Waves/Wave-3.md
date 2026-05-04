# Wave 3: Coming-Soon-Seite und Hansefit-Aktion

**Status:** Planned
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
- [ ] `web/templates/pages/coming-soon.html` zeigt ein Courts-Logo als starkes First-Viewport-Signal.
- [ ] Headline/Copy kommunizieren Eröffnung Herbst 2026 und Bad Zwischenahn.
- [ ] Primärer CTA fuehrt zu `/kontakt` mit klarer Hansefitregistrierung.
- [ ] Sekundaere Links fuehren nur zu `/impressum`, `/datenschutz`, `/agb`.
- [ ] Seite funktioniert als Standalone-HTML ohne Base-Layout.
- [ ] Design bleibt dunkel, minimalistisch, sportlich und mobile-first.
- [ ] Text passt auf Mobile und Desktop ohne Ueberlappungen.

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
- [ ] Der Text "Ersten 100 Registrierten erhalten Baelle gratis!" ist auf der Coming-Soon-Seite sichtbar.
- [ ] Der Aktionstext wirkt wie ein Special/Badge, ohne die Hansefitregistrierung zu verdraengen.
- [ ] Ein Link/Button "Aktionsbedingungen" oeffnet den Dialog.
- [ ] Keine Tracking- oder Gewinnerlogik wird suggeriert, die technisch nicht existiert.

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
- [ ] Dialog wird per Button/Link geoeffnet und geschlossen.
- [ ] Dialog ist per Tastatur bedienbar.
- [ ] Dialog nutzt `role="dialog"` oder native `<dialog>` mit sinnvollem Label.
- [ ] Lange Inhalte sind in einem scrollbaren Bereich lesbar.
- [ ] Inhalt basiert auf `docs/aktionsbedingungen-hansefit.html`.
- [ ] Bei deaktiviertem JavaScript bleibt mindestens ein sichtbarer Hinweis oder ein fallback-lesbarer Bereich vorhanden.

**Kontext:**
- `web/templates/pages/coming-soon.html`
- `web/static/js/main.js` nur bei Bedarf anfassen; Vanilla JS bevorzugt.

**Output:**
- Aktionsbedingungen-Popup/Dialog auf der Coming-Soon-Seite.

## Wave-3-Exit-Gate

- [ ] Coming-Soon-Seite ist visuell fertig.
- [ ] Hansefit-CTA fuehrt zu `/kontakt`.
- [ ] Aktionsbedingungen sind direkt erreichbar.
- [ ] Keine oeffentlichen Links fuehren in gesperrte Inhalte.

## Notes

- Keine Landingpage-Sektion bauen; die Coming-Soon-Seite ist die erste und eigentliche oeffentliche Experience im Prelaunch.

## Abschluss

- [ ] Wave abgeschlossen
