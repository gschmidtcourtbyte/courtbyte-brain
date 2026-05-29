# Wave 1: SVG-Karte & Animationen

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-10.md`

## Legacy-Inhalt

### Wave 1 — SVG-Karte & Animationen
**Skillsets**: frontend-eng
**Tasks**: 10.1
**Ziel**: Stilisierte SVG-Karte mit sequenzieller Straßen-Glow-Animation erstellen und in die Seite integrieren.

## Legacy-Status

### Wave 1 — SVG-Karte & Animationen: done
- Inline-SVG in `web/templates/pages/home.html` mit viewBox 600x450.
- Straßen: A28 (Doppellinie, 4px), Oldenburger Str. (2.5px), Haarenstr./Lange Str./Bahnhofstr. (1.5-1.8px), Auf dem Hohen Ufer/Peterstr. (1.2px), Kranstraße (1.5px).
- `@keyframes road-glow` mit gestaffelten Delays: A28 (0s), Oldenburger (2s), Lokal (4s), Kranstraße (6s) — 8s Zyklus.
- Standort-Pin (Kranstraße 1a): Drei Kreise (outer ring r=18, inner ring r=12, dot r=6) mit `svg-pulse` und `svg-ring-pulse` Animationen.
- Labels: A28, Oldenburger Str., Haarenstr., Kranstraße in `--fg-mute`, 9px Mono.
- Grid-Pattern als SVG `<defs><pattern>` statt CSS ::before.
- `prefers-reduced-motion` wird durch bestehende Wildcard-Regel abgedeckt.
- Adresse in Kontakt-Info aktualisiert auf "Kranstraße 1a, Bad Zwischenahn".
