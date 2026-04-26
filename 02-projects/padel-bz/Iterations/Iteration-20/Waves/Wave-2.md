# Wave 2: HTML und CSS anpassen

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-20.md`

## Legacy-Inhalt

## Wave 2 — HTML und CSS anpassen

**Agenten**: Agent A (frontend-eng)
**Kann parallel ausgefuehrt werden**: nein (HTML und CSS muessen aufeinander abgestimmt sein)
**Blocked by**: Wave 1
**Ziel**: Sponsoren-Sektion von 6 auf 5 Kacheln umbauen, neue Logos einbinden, Grid anpassen.

### Task 20.2: HTML — Sponsoren-Kacheln austauschen
**Priority**: P0
**Blocked by**: 20.1
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Nur noch 5 `sponsor-logo-card` Elemente im Grid.
- [ ] Kachel 1: AXA-Logo als SVG (`/static/img/sponsors/axa-logo.svg`), Klasse `sponsor-logo-card-real`.
- [ ] Kachel 2-3: "Coming Soon" SVG (`/static/img/sponsors/coming-soon.svg`), eigene Klasse `sponsor-logo-card-soon`.
- [ ] Kachel 4-5: "Let's Play Together" SVG (`/static/img/sponsors/lets-play-together.svg`), eigene Klasse `sponsor-logo-card-soon`.
- [ ] Alle Kacheln haben `reveal-on-scroll`.
- [ ] Korrektes `alt`-Attribut auf allen `<img>` Tags.
- [ ] Keine `sponsor-placeholder` Elemente mehr vorhanden.

**Kontext**:
- `web/templates/pages/home.html`, Zeilen 355-364.

**Output**:
- Aktualisiertes `web/templates/pages/home.html`.

### Task 20.3: CSS — Grid fuer 5 Kacheln optimieren
**Priority**: P0
**Blocked by**: 20.2
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Desktop: `grid-template-columns: repeat(5, 1fr)` fuer `.sponsor-grid`.
- [ ] Mobile (<=520px): `grid-template-columns: repeat(2, minmax(0, 1fr))` bleibt, letzte Kachel zentriert sich.
- [ ] Neue Klasse `.sponsor-logo-card-soon` mit Styling fuer die SVG-Platzhalter-Karten (dunkler Hintergrund, passend zum Design).
- [ ] SVG-Bilder in den neuen Karten skalieren korrekt (max-width, max-height, object-fit).
- [ ] Kein Layout-Bruch auf allen Bildschirmgroessen.

**Kontext**:
- `web/static/css/style.css`, Zeilen 910-973.
- Design: dunkles Theme, minimalistisch.

**Output**:
- Aktualisiertes `web/static/css/style.css`.

**Wave-2-Exit-Gate**:
- [ ] Sponsoren-Sektion zeigt 5 Kacheln in einer Reihe (Desktop).
- [ ] AXA-Logo, 2x Coming Soon, 2x Let's Play Together korrekt sichtbar.
- [ ] Mobile: 2er-Grid, letzte Kachel zentriert.
- [ ] Kein visueller Bruch.

---
