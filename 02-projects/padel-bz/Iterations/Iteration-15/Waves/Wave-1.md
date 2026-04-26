# Wave 1: Frontend Quick-Fixes (parallel, keine Abhaengigkeiten)

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-15.md`

## Legacy-Inhalt

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
