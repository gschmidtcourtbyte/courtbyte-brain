# Iteration 10: Animierte SVG-Karte — Anfahrt Kranstraße 1a

## Ziel
Die bestehende CSS-only Map-Sektion wird durch eine stilisierte, animierte SVG-Karte ersetzt. Die Karte zeigt die Lage von Kranstraße 1a in Bad Zwischenahn mit abstrahierten Hauptverkehrsstraßen (A28, Oldenburger Straße, weitere lokale Straßen). Die Straßen leuchten sequenziell in der Highlightfarbe `#D8633D` auf und verblassen wieder — eine optisch ansprechende, rein dekorative Animation ohne echte Kartendaten.

## Ausgangslage
- `web/templates/pages/home.html:278` enthält `<div class="map reveal-on-scroll"></div>`.
- `web/static/css/style.css:720-748` definiert die aktuelle CSS-only Map: Grid-Muster mit pulsendem Accent-Dot.
- Design Tokens: `--accent: #D8633D`, `--accent-rgb: 216, 99, 61`, `--bg-deep: #3a4437`, `--line-soft: #536250`.
- Bestehende `@keyframes pulse` Animation wird weiterverwendet.

## Scope
- **HTML**: `web/templates/pages/home.html` — Map-Div durch Inline-SVG ersetzen.
- **CSS**: `web/static/css/style.css` — Neue Animationen für Straßen-Glow, bestehende `.map`-Styles anpassen.
- Kein Backend-, JS- oder Datenbankumbau.

## Produktentscheidung
Die Karte ist dekorativ — kein Mapbox, kein Google Maps, kein Leaflet. Die SVG zeigt ein stilisiertes Straßennetz, das die geografische Lage andeutet, ohne kartografisch exakt zu sein. Die Animation erzeugt einen Premium-Eindruck passend zum dunklen, sportlichen Theme.

### Straßen im SVG
| Straße | Stil | Animation |
|--------|------|-----------|
| A28 (Autobahn) | Breiteste Linie, leicht gebogen | Glow 1 (zuerst) |
| Oldenburger Straße | Mittlere Linie, nord-süd | Glow 2 |
| Haarenstraße / Lange Straße | Dünnere Linien | Glow 3 |
| Kranstraße | Dünne Linie zum Ziel | Glow 4 (zuletzt) |
| Standort-Pin | Pulsender Accent-Dot | Dauerhaft (bestehende pulse Animation) |

### Animation-Sequenz
Jede Straße leuchtet nacheinander auf (opacity + glow), hält kurz, verblasst, dann folgt die nächste. Der gesamte Zyklus wiederholt sich endlos. Gesamtdauer: ~8s pro Zyklus.

## Critical Path
1. SVG-Pfade für stilisiertes Straßennetz erstellen.
2. CSS-Animationen mit gestaffelten `animation-delay`-Werten definieren.
3. Inline-SVG in `home.html` integrieren, bestehende `.map`-Div ersetzen.
4. Bestehende `.map`-CSS-Regeln anpassen (Grid-Muster beibehalten, Dot entfernen da im SVG).
5. QA: Syntax, Responsive, Container-Check.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Wave-Steuerung, Akzeptanzkriterien |
| frontend-eng | SVG-Erstellung, CSS-Animationen, HTML-Integration |
| testing-eng | Syntax-, Responsive- und Container-Prüfungen |

## Task: 10.1 SVG-Karte erstellen und CSS-Animationen definieren
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Inline-SVG mit stilisierten Straßenpfaden (A28, Oldenburger Str., lokale Straßen, Kranstraße).
- [x] Straßen nutzen `--line-soft` als Basisfarbe, Glow in `--accent` / `--accent-rgb`.
- [x] Sequenzielle Glow-Animation: Straßen leuchten nacheinander auf und verblassen.
- [x] Standort-Pin (Kranstraße 1a) mit bestehender Pulse-Animation.
- [x] Dezente Straßen-Labels (A28, Oldenburger Str.) in `--fg-mute`.
- [x] SVG ist responsive (viewBox, keine festen Dimensionen).

**Output**:
- Aktualisierte `web/templates/pages/home.html` (SVG inline).
- Neue CSS-Keyframes und Styles in `web/static/css/style.css`.

## Task: 10.2 QA & Abschluss
**Priority**: P1
**Blocked by**: 10.1
**Skillset**: testing-eng, product-own

**Akzeptanzkriterien**:
- [x] CSS-Brace-Check besteht.
- [x] HTML-Syntax ist valide (kein ungeschlossenes SVG).
- [x] Container liefert Startseite mit neuer Karte aus (HTTP 200).
- [x] Animation respektiert `prefers-reduced-motion`.
- [x] Responsive: Karte skaliert auf Mobile korrekt (viewBox-basiert).

**Output**:
- QA-Status in dieser Datei.

## Execution Waves

### Wave 1 — SVG-Karte & Animationen
**Skillsets**: frontend-eng
**Tasks**: 10.1
**Ziel**: Stilisierte SVG-Karte mit sequenzieller Straßen-Glow-Animation erstellen und in die Seite integrieren.

### Wave 2 — QA & Abschluss
**Skillsets**: testing-eng, product-own
**Tasks**: 10.2
**Ziel**: Syntax, Responsiveness, Container-Auslieferung und Barrierefreiheit prüfen.

## Abhängigkeits-Graph

```text
10.1 SVG-Karte & Animationen
  -> 10.2 QA & Abschluss
```

## Umsetzungsstatus

### Wave 1 — SVG-Karte & Animationen: done
- Inline-SVG in `web/templates/pages/home.html` mit viewBox 600x450.
- Straßen: A28 (Doppellinie, 4px), Oldenburger Str. (2.5px), Haarenstr./Lange Str./Bahnhofstr. (1.5-1.8px), Auf dem Hohen Ufer/Peterstr. (1.2px), Kranstraße (1.5px).
- `@keyframes road-glow` mit gestaffelten Delays: A28 (0s), Oldenburger (2s), Lokal (4s), Kranstraße (6s) — 8s Zyklus.
- Standort-Pin (Kranstraße 1a): Drei Kreise (outer ring r=18, inner ring r=12, dot r=6) mit `svg-pulse` und `svg-ring-pulse` Animationen.
- Labels: A28, Oldenburger Str., Haarenstr., Kranstraße in `--fg-mute`, 9px Mono.
- Grid-Pattern als SVG `<defs><pattern>` statt CSS ::before.
- `prefers-reduced-motion` wird durch bestehende Wildcard-Regel abgedeckt.
- Adresse in Kontakt-Info aktualisiert auf "Kranstraße 1a, Bad Zwischenahn".

### Wave 2 — QA & Abschluss: done
- CSS-Brace-Check: 218 open, 218 close — balanced.
- JS-Syntaxcheck (`node --check`): bestanden.
- Container-Auslieferung: Startseite rendert korrekt (HTTP 200 im Container).
- SVG-Elemente verifiziert: 2x road-a28, 1x road-oldenburger, 5x road-local, 1x road-kranstrasse, 3x map-pin, 4x map-label.
- CSS-Animationen (road-glow, svg-pulse, svg-ring-pulse) werden im Stylesheet ausgeliefert.

## Definition of Done
- [x] Bestehende Grid-Map ist durch animierte SVG-Karte ersetzt.
- [x] Hauptstraßen leuchten sequenziell in Highlightfarbe auf.
- [x] Standort-Pin markiert Kranstraße 1a.
- [x] Animation läuft endlos und respektiert reduced-motion.
- [x] Container liefert die Seite fehlerfrei aus.
