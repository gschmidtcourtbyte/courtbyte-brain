# Iteration 20: Sponsoren-Sektion umbauen — neue Logos und 5-Kachel-Layout

## Ziel

Die Sponsoren-Sektion wird von 6 auf 5 Kacheln reduziert. Das bestehende AXA-Versicherung-JPG wird durch ein neues AXA-SVG-Logo ersetzt. Zwei Platzhalter werden durch "Coming Soon"-SVGs und zwei weitere durch "Let's Play Together"-SVGs ersetzt. Das CSS-Grid wird fuer 5 Kacheln optimiert.

## Ausgangslage

### Sponsoren-Sektion (`web/templates/pages/home.html`, Zeilen 345-365)
- 6 Sponsor-Kacheln im Grid: 1 echtes Logo (AXA JPG), 5 CSS-Platzhalter.
- Grid: `repeat(auto-fit, minmax(168px, 1fr))`, Mobile: `repeat(2, minmax(0, 1fr))`.
- Echtes Logo nutzt `sponsor-logo-card-real`, Platzhalter nutzen `sponsor-placeholder` mit CSS-generierten Pseudo-Grafiken.

### CSS (`web/static/css/style.css`, Zeilen 910-973)
- `.sponsors`, `.sponsor-grid`, `.sponsor-logo-card`, `.sponsor-logo-card-real`, `.sponsor-placeholder`.
- Mobile-Breakpoint bei 520px.

### Neue Logos (im `logo/` Verzeichnis)
- `axa-logo-4.svg` — AXA-Logo als Vektor-Pfade (korrekte Darstellung garantiert).
- `coming soo.svg` — "COMING SOON" Text-SVG (Gobold Bold Font).
- `lets play together.svg` — "LET'S PLAY TOGETHER" Text-SVG (Gobold Bold Font).

## Scope

### Assets
- Drei SVG-Dateien aus `logo/` nach `web/static/img/sponsors/` kopieren.
- Alte AXA-JPG-Datei bleibt als Backup erhalten.

### HTML (`web/templates/pages/home.html`)
- 6 Kacheln auf 5 Kacheln reduzieren.
- AXA-JPG durch AXA-SVG ersetzen.
- 2 Platzhalter durch "Coming Soon"-SVG-Karten ersetzen.
- 2 Platzhalter durch "Let's Play Together"-SVG-Karten ersetzen.
- Alle 5 Kacheln behalten `reveal-on-scroll`.

### CSS (`web/static/css/style.css`)
- Grid-Layout fuer 5 Kacheln optimieren (Desktop: 5 Spalten).
- Mobile-Breakpoint anpassen (2er-Grid bleibt, letzte Kachel zentriert).
- Ggf. neue Klasse fuer die SVG-Platzhalter-Karten (nicht `sponsor-placeholder`, da kein CSS-Pseudo-Content).
- `sponsor-placeholder` CSS bleibt vorhanden (kein toter Code — wird nicht mehr genutzt, kann in Zukunft entfernt werden).

## Nicht im Scope

- Font-Embedding fuer die Text-SVGs (separate Aufgabe falls noetig).
- Entfernen der alten AXA-JPG-Datei.
- Aenderungen an anderen Sektionen.
- Neue Sponsoren-Seiten oder Detail-Ansichten.

## Produktentscheidungen

- **5 statt 6 Kacheln**: Weniger Platzhalter wirkt aufgeraeumter und realistischer.
- **AXA SVG statt JPG**: Bessere Qualitaet auf HiDPI-Displays, konsistenter Look mit den anderen SVGs.
- **"Coming Soon" und "Let's Play Together"**: Signalisieren aktiv, dass Sponsoring-Plaetze verfuegbar sind — besser als leere Platzhalter.
- **Reihenfolge**: AXA (real) → 2x Coming Soon → 2x Let's Play Together.
- **5-Spalten-Grid Desktop**: Alle Kacheln in einer Reihe, gleichmaessig verteilt.

## Critical Path

1. SVG-Assets nach `web/static/img/sponsors/` kopieren.
2. HTML: Kacheln austauschen (6→5, neue Logos).
3. CSS: Grid fuer 5 Spalten anpassen.
4. Visueller Test und Regressionspruefung.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Wave-Freigabe, Akzeptanzabnahme |
| Agent A | frontend-eng | Asset-Kopie, HTML-Umbau, CSS-Anpassung |
| Agent B | testing-eng | Regressionspruefung, Tests |

---

## Wave 1 — Assets vorbereiten

**Agenten**: Agent A (frontend-eng)
**Kann parallel ausgefuehrt werden**: ja (keine Abhaengigkeiten)
**Ziel**: SVG-Logos ins statische Verzeichnis kopieren.

### Task 20.1: SVG-Logos nach web/static/img/sponsors/ kopieren
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (frontend-eng)

**Akzeptanzkriterien**:
- [ ] `web/static/img/sponsors/axa-logo.svg` existiert (aus `logo/axa-logo-4.svg`).
- [ ] `web/static/img/sponsors/coming-soon.svg` existiert (aus `logo/coming soo.svg`).
- [ ] `web/static/img/sponsors/lets-play-together.svg` existiert (aus `logo/lets play together.svg`).
- [ ] Alle drei SVGs sind valide und darstellbar.

**Kontext**:
- Quelldateien im `logo/` Verzeichnis.
- Zielverzeichnis: `web/static/img/sponsors/`.

**Output**:
- Drei SVG-Dateien in `web/static/img/sponsors/`.

**Wave-1-Exit-Gate**:
- [ ] Alle drei SVGs liegen korrekt im Zielverzeichnis.

---

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

## Wave 3 — Testing und Verification

**Agenten**: Agent B (testing-eng)
**Kann parallel ausgefuehrt werden**: ja
**Blocked by**: Wave 2
**Ziel**: Regressionspruefung, Tests ausfuehren.

### Task 20.4: Regressionstests und Verifikation
**Priority**: P1
**Blocked by**: 20.3
**Agent**: Agent B (testing-eng)

**Akzeptanzkriterien**:
- [ ] `go test ./...` ist gruen.
- [ ] Render-Tests fuer `home.html` bestehen.
- [ ] SVG-Dateien sind valide.
- [ ] `git diff --check` ist gruen.
- [ ] Keine Whitespace-Fehler.

**Kontext**:
- `web/templates/pages/home.html`
- `web/static/css/style.css`
- `web/static/img/sponsors/`

**Output**:
- Gruene Tests, QA-Notiz.

**Wave-3-Exit-Gate**:
- [ ] Alle Tests gruen.
- [ ] Keine Regressionen.

---

## Abhaengigkeiten und Parallelisierung

| Wave | Parallel moeglich | Blockiert durch | Bemerkung |
|------|-------------------|-----------------|-----------|
| Wave 1 | ja | none | Asset-Kopie ist unabhaengig |
| Wave 2 | nein | Wave 1 | HTML und CSS muessen aufeinander abgestimmt sein |
| Wave 3 | ja | Wave 2 | Tests koennen nach Umbau laufen |

## Risiken

| Risiko | Impact | Umgang |
|--------|--------|--------|
| Gobold Bold Font nicht verfuegbar im Browser | Mittel | SVGs nutzen Fallback-Font; ggf. spaeter Font einbetten oder Text in Pfade wandeln |
| 5-Spalten-Grid auf Tablets unpassend | Niedrig | auto-fit Fallback oder Tablet-Breakpoint ergaenzen |
| SVG-ViewBox sehr gross (10417x4555) | Niedrig | object-fit: contain und max-width/max-height begrenzen die Darstellung |

## Definition of Done

- [ ] 5 Sponsoren-Kacheln in einer Reihe (Desktop).
- [ ] AXA-Logo als SVG (statt JPG).
- [ ] 2x "Coming Soon" SVG-Kacheln.
- [ ] 2x "Let's Play Together" SVG-Kacheln.
- [ ] Grid korrekt auf Desktop, Tablet und Mobile.
- [ ] `go test ./...` gruen.
- [ ] `git diff --check` gruen.
