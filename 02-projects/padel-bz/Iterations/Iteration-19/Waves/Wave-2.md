# Wave 2: Sequentielle Draw-In-Animation implementieren

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-19.md`

## Legacy-Inhalt

## Wave 2 — Sequentielle Draw-In-Animation implementieren
**Agenten**: Agent A (frontend-eng)
**Kann parallel ausgefuehrt werden**: nein (CSS und JS muessen zusammen entwickelt werden)
**Ziel**: Die Hauptarbeit — die bisherige Glow-Animation wird durch eine professionelle, sequentielle Draw-In-Animation ersetzt.

### Task 19.3: CSS road-glow entfernen und Draw-In-Grundstruktur vorbereiten
**Priority**: P0
**Blocked by**: 19.2
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] `@keyframes road-glow` ist entfernt.
- [ ] Die vier `.road-*` Animations-Zuweisungen (Zeilen 826-829) sind entfernt.
- [ ] Strassen haben initial `opacity: 0` oder `stroke-dashoffset` auf volle Laenge (unsichtbar).
- [ ] Pin und Pin-Ringe haben initial `opacity: 0`.
- [ ] Labels haben initial `opacity: 0`.
- [ ] Eine CSS-Klasse `.map-animate` (oder vergleichbar) aktiviert die Draw-In-Transition per Klassen-Toggle.
- [ ] Transition-Dauer und Easing sind definiert: `stroke-dashoffset` mit `cubic-bezier` oder `ease-in-out`.
- [ ] Keine visuellen Regressionen auf anderen Seitenteilen.

**Kontext**:
- `web/static/css/style.css`, Zeilen 813-850
- SVG-Pfade in `web/templates/pages/home.html`

**Output**:
- Aktualisiertes `web/static/css/style.css`.

### Task 19.4: JavaScript Draw-In-Animation mit IntersectionObserver
**Priority**: P0
**Blocked by**: 19.3
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Neuer Abschnitt in `web/static/js/main.js` fuer die Map-Animation.
- [ ] IntersectionObserver auf `.map` Container, threshold ca. 0.3.
- [ ] Bei Intersection: Klasse `.map-animate` wird auf das SVG oder den Container gesetzt.
- [ ] Strassen werden sequentiell per `stroke-dashoffset` Animation eingezeichnet:
  1. **A28** (road-a28, Hauptstrang) — zeichnet sich von oben nach unten (~1s).
  2. **Autobahnabfahrt-Kreis** — wird sichtbar (fade-in, ~0.3s).
  3. **Oldenburger Str.** (road-oldenburger, vom Kreisverkehr nach links) — zeichnet sich ein (~0.8s).
  4. **Strasse Richtung Kranstrasse** (road-oldenburger, nach Sueden) — zeichnet sich ein (~0.6s).
  5. **Nebenstrassen** (road-local) — faden nacheinander ein oder zeichnen sich leicht ein (~0.4s je).
  6. **Kranstrasse** (road-kranstrasse) — zeichnet sich zum Pin (~0.5s).
  7. **Location Pin** — erscheint mit Pulse-Start (~0.3s).
- [ ] `stroke-dasharray` und `stroke-dashoffset` werden per JS berechnet (via `getTotalLength()`).
- [ ] Labels faden zeitversetzt nach ihren Strassen ein.
- [ ] Animation spielt nur einmal (Observer disconnected nach Trigger).
- [ ] Gesamtdauer ca. 4-6 Sekunden.
- [ ] Kein jQuery, kein externes Framework — Vanilla JS.
- [ ] `prefers-reduced-motion: reduce` wird respektiert (sofort sichtbar, keine Animation).

**Kontext**:
- `web/static/js/main.js` — bestehender IntersectionObserver als Vorlage.
- `web/templates/pages/home.html` — SVG-Pfad-Klassen und IDs.
- `web/static/css/style.css` — Draw-In CSS aus Task 19.3.

**Output**:
- Erweitertes `web/static/js/main.js` mit Map-Draw-In-Logik.

### Task 19.5: SVG-Pfade fuer Draw-In optimieren
**Priority**: P1
**Blocked by**: 19.2
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Jeder Hauptstrassenpfad hat eine eindeutige Klasse oder ID fuer gezielte JS-Selektion.
- [ ] Falls Pfade fuer die Animation in der richtigen Zeichenrichtung sein muessen (Start → Ziel), werden sie ggf. umgekehrt.
- [ ] Oldenburger Str. zeichnet sich vom A28-Kreisverkehr (rechts) nach links — Pfad-Richtung pruefen.
- [ ] Die Strasse von Haarenstroth nach Sueden (Richtung Kranstrasse) zeichnet sich von oben nach unten.
- [ ] Kranstrasse zeichnet sich vom Abzweig nach unten zum Pin.
- [ ] Autobahnabfahrt-Kreis bekommt eine eigene Klasse (z.B. `road-exit`) wenn noetig.
- [ ] Neben-/Lokalstrassen koennen gruppiert bleiben.

**Kontext**:
- `web/templates/pages/home.html`, SVG-Bereich

**Output**:
- Ggf. angepasste SVG-Pfade in `web/templates/pages/home.html`.

**Wave-2-Exit-Gate**:
- [ ] Die Karte baut sich beim Scrollen sequentiell auf — Strasse fuer Strasse.
- [ ] A28 zuerst, Autobahnabfahrt, dann Zufahrtsstrassen, dann Kranstrasse, dann Pin.
- [ ] Labels erscheinen zeitversetzt.
- [ ] Animation spielt nur einmal.
- [ ] Bei `prefers-reduced-motion` ist alles sofort sichtbar.

---
