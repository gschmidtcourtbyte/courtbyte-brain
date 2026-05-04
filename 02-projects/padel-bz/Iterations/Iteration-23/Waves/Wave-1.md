# Wave 1: Navbar-Revert und Legal-Layout-Fix

**Status:** Completed
**Skill:** frontend-eng
**Rolle:** frontend-eng
**Kann parallel ausgefuehrt werden:** ja, parallel zu Wave 2
**Blocked by:** —

## Ziel

Die Navbar wird auf den Stand vor Iteration 22 zurueckgesetzt. Die Inhaltsverzeichnisse auf `/agb` und `/datenschutz` ueberlappen weder mit der Sticky-Navbar noch mit dem nachfolgenden Flieusstext. Es existiert kein sichtbarer UI-Link/Button auf `/admin/login`.

## Tasks

### Task 23.1: Navbar auf vor-Iteration-22-Stand zuruecksetzen
**Priority:** P0
**Blocked by:** —
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Desktop-Links sind `/#courts`, `/#events`, `/#kontakt` (in dieser Reihenfolge).
- [ ] Instagram-Icon-Link auf `https://www.instagram.com/courts.padelclub/` ist wieder vorhanden.
- [ ] WhatsApp-Icon bleibt mit `style="display:none"` als Platzhalter erhalten.
- [ ] Hansefit-Registrierung-CTA bleibt unveraendert (`/kontakt`, `btn btn-primary`, Pfeil-SVG).
- [ ] Mobile-Menue spiegelt die Desktop-Reihenfolge (Anker-Links, Instagram, Hansefit-CTA).
- [ ] Keine Verlinkung auf `/datenschutz`, `/agb`, `/impressum` in der Navbar (Footer behaelt diese).
- [ ] Keine Verlinkung auf `/admin` oder `/admin/login`.

**Kontext:**
- Quelle fuer den Zielzustand: Diff `f9a1575..ef9663e -- web/templates/partials/nav.html` zeigt klar, was wegfaellt und was zurueckkommt.
- Datei: `web/templates/partials/nav.html`.
- Footer-Datei (zur Kontrolle, dass Legal-Links dort weiterhin existieren): `web/templates/partials/footer.html`.

**Output:**
- Aktualisierte `web/templates/partials/nav.html` mit den alten Anker-Links.

### Task 23.2: Legal-TOC-/Layout-Fix
**Priority:** P0
**Blocked by:** —
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Auf Desktop und Mobile rutscht das Inhaltsverzeichnis auf `/agb` und `/datenschutz` nicht hinter die Sticky-Navbar (`position:sticky; top:0; height:64px`).
- [ ] Beim Klick auf einen TOC-Link landet die jeweilige Section sichtbar UNTER der Navbar (genug `scroll-margin-top`).
- [ ] Es gibt keine visuelle Ueberlappung zwischen TOC und der nachfolgenden Section bzw. zwischen TOC und der `<div class="sec-head">`-Headline.
- [ ] Keine Aenderung am semantischen Markup (Heading-Hierarchie, `<nav class="legal-toc">`, Section-IDs bleiben).
- [ ] CSS-Aenderungen sind in `web/static/css/style.css` lokalisiert; kein Inline-Style in den Page-Templates.
- [ ] `go test ./internal/handler -run TestRendererRendersContactAndAGBPages` bleibt gruen.

**Kontext:**
- Templates: `web/templates/pages/agb.html`, `web/templates/pages/datenschutz.html`. Beide nutzen `web/templates/layouts/base.html` mit `<main>` direkt nach der Navbar.
- Bestehende Regeln in `web/static/css/style.css`:
  - `.legal-content { max-width:800px; margin:0 auto; padding:0 var(--pad); }`
  - `.legal-toc { … margin-bottom:clamp(30px,5vw,52px); }`
  - `.legal-section { … scroll-margin-top:110px; }`
  - Navbar: `nav { position:sticky; top:0; z-index:100; height:64px; }`
- Hypothese: Es fehlt ausreichender Top-Abstand zwischen `<main>` und der ersten Section. Loesung kann ueber `main { padding-top: ...; }` oder ueber einen Top-Margin auf `.legal-content` bzw. dem umschliessenden `<section>` der Legal-Pages erfolgen. Reine CSS-Loesung bevorzugen.
- Mobile-Check: TOC darf nicht ueber `.sec-head` liegen, wenn das Grid auf 1 Spalte umbricht (siehe `@media`-Block ab Zeile ~1697 in `style.css`).

**Output:**
- Aktualisiertes `web/static/css/style.css` mit den noetigen Layout-Korrekturen.
- Falls noetig: minimaler Padding/Spacing-Fix in `web/templates/pages/agb.html` und `web/templates/pages/datenschutz.html` (nur Klassen-Anpassung, kein Strukturumbau).

### Task 23.3: Sicherstellen, dass kein UI-Element auf `/admin/login` verlinkt
**Priority:** P1
**Blocked by:** —
**Agent:** Agent A (frontend-eng)

**Akzeptanzkriterien:**
- [ ] Grep ueber `web/templates/` (ausser `web/templates/admin/`) zeigt keinen Treffer fuer `/admin/login` oder `/admin`.
- [ ] Coming-Soon-Seite (`web/templates/pages/coming-soon.html`) enthaelt keinen Admin-Link.
- [ ] Footer (`web/templates/partials/footer.html`) enthaelt keinen Admin-Link.

**Kontext:**
- Admin bleibt nur per direktem URL-Aufruf nutzbar.
- Falls bestehende Templates einen Admin-Link enthalten, ersatzlos entfernen.

**Output:**
- Bereinigte Templates ohne UI-Links auf den Admin-Bereich.

## Wave-1-Exit-Gate

- [ ] Navbar entspricht dem alten Stand.
- [ ] Legal-Pages zeigen TOC und Sections ohne Ueberlappung.
- [ ] Keine UI-Links auf den Admin-Bereich.
- [ ] Templates parsen weiterhin (`go test ./internal/handler -run TestRendererRendersContactAndAGBPages` gruen).

## Notes

- Reine Frontend-Wave; keine Aenderungen an Go-Code, Routen oder Middleware.
- Keine neuen Assets oder externen Abhaengigkeiten.
