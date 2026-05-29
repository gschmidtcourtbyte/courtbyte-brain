# Iteration 2: Design-Overhaul — courts padelclub Branding

## Ziel
Kompletterneuerung des Frontends basierend auf dem professionellen Design-Handoff aus Claude Design. Das neue Design ersetzt die Iteration-1-Landingpage vollständig mit dem offiziellen "courts padelclub" Branding.

**Iteration 2.1 (aktuell):** Hero-SVG durch offizielles Padel-Court-Diagramm ersetzen + "Baufortschritt"-Sektion mit Bauvideo und Drohnenfoto hinzufügen.

## Design-Quelle
- **Handoff-Bundle**: Claude Design Export (courts.html)
- **Logo SVG**: `/logo/courts - padelclub (standalone).html` — Padel-Court-Diagramm mit "courts" Wordmark + "PADELCLUB" Monospace
- **Logo JPG**: `/logo/courts.jpg` — Tannengrün mit Off-White Wordmark
- **Stil**: Editorial, Premium, dunkles Tannengrün, monospace Labels, Fraunces + Space Grotesk + JetBrains Mono
- **Baufortschritt-Inspiration**: thecubepadel.de — minimalistisch, schwarz/weiß, großzügige Abstände, Media-fokussiert

## Design-System (aus courts.html extrahiert)

### Farben
| Token | Hex | Verwendung |
|-------|-----|-----------|
| --bg | #2e3c36 | Tannengrün Haupthintergrund |
| --bg-deep | #243029 | Tieferer Hintergrund |
| --bg-elev | #364942 | Erhöhte Elemente (Cards) |
| --fg | #e7e1d5 | Off-White Haupttext |
| --fg-dim | #b6b2a6 | Gedimmter Text |
| --fg-mute | #8a8879 | Labels, Monospace |
| --line | #4a5a52 | Trennlinien |
| --line-soft | #3b4942 | Subtile Linien |
| --accent | #d9e1a6 | Court-Line Gelbgrün |
| --accent-deep | #c3cc86 | Tieferer Akzent |

### Typografie
| Font | Verwendung |
|------|-----------|
| Space Grotesk (400/500/600) | Display, UI, Headlines |
| Fraunces (300, italic) | Serif-Akzente, Zitate, Subtitel |
| JetBrains Mono (400/500) | Monospace Labels, technische Angaben |

### Sektionen (Reihenfolge)
1. **Nav** — Sticky, Glassmorphism, Brand "c" Mark + "courts padelclub"
2. **Hero** — Info-Rail (Standort/Courts/Öffnung/Partner) + großer SVG Wordmark "courts"
3. **Courts** — Feature Center Court + 3 Double + 1 Single, Stats-Zeile
4. **Hansefit Strip** — Eigenständiger Band-Block
5. **Events** — GPS Tour Featured Card + Terminliste
6. **Team** — 3 Mitglieder mit Platzhalter-Portraits
7. **Kontakt** — Abstrakte Map + Info-Zeilen + CTAs
8. **Footer** — Großes "courts" Wordmark + Spalten

## Scope

### Epic 2.1: Assets & Vorbereitung
**Agent**: infrastructure-eng
**Priority**: P0

- [ ] Logo kopieren: `logo/courts.jpg` → `web/static/img/courts-logo.jpg`
- [ ] Design-Logo kopieren: `/tmp/courts/project/assets/courts-logo.jpg` → `web/static/img/courts-logo.jpg`
- [ ] Veraltete Iteration-1 CSS/JS entfernen (werden ersetzt)

### Epic 2.2: Base Layout & Design System
**Agent**: frontend-eng
**Priority**: P0

- [ ] `web/templates/layouts/base.html` — Komplett neu:
  - Google Fonts: Space Grotesk, Fraunces, JetBrains Mono
  - Kein Tailwind CDN mehr — Custom CSS Design-Tokens
  - Alpine.js beibehalten für Interaktivität
- [ ] `web/static/css/style.css` — Komplett neu:
  - Alle CSS aus courts.html extrahieren
  - Design Tokens als CSS Custom Properties
  - Responsive Breakpoints beibehalten

### Epic 2.3: Navigation & Footer
**Agent**: frontend-eng
**Priority**: P0

- [ ] `web/templates/partials/nav.html` — Neu:
  - Brand: Circle "c" Mark + "courts padelclub"
  - Links: Courts, Events, Team, Kontakt
  - CTA: "Court buchen" (Playtomic-Link Platzhalter)
  - Mobile: Links ausblenden, nur CTA
- [ ] `web/templates/partials/footer.html` — Neu:
  - Großes "courts" Wordmark
  - 3 Spalten: Club, Spielen, Standort
  - Bottom Bar: Copyright, Impressum, Datenschutz, AGB

### Epic 2.4: Homepage
**Agent**: frontend-eng
**Priority**: P0

- [ ] `web/templates/pages/home.html` — Komplett neu nach Design:
  - Hero mit Info-Rail + SVG Wordmark
  - Courts-Grid (Feature + 4 Courts)
  - Hansefit Strip
  - Events mit GPS Tour Card + Terminliste
  - Team-Sektion
  - Kontakt mit Map-Platzhalter

### Epic 2.5: Kontaktformular-Anpassung
**Agent**: frontend-eng
**Priority**: P1

- [ ] `web/templates/pages/contact.html` — An neues Design anpassen:
  - Gleiches Farbschema/Fonts
  - Formular in neuem Stil

### Epic 2.6: Statische Seiten anpassen
**Agent**: frontend-eng
**Priority**: P2

- [ ] `web/templates/pages/impressum.html` — An neues Design
- [ ] `web/templates/pages/datenschutz.html` — An neues Design

### Epic 2.7: JS aktualisieren
**Agent**: frontend-eng
**Priority**: P2

- [ ] `web/static/js/main.js` — Anpassen für neues Design

## Execution Waves (Iteration 2.0 — abgeschlossen)

```
Wave 1 (Parallel):  ✅
  ├── infrastructure-eng  → Epic 2.1: Assets kopieren, Vorbereitung
  └── frontend-eng        → Epic 2.2 + 2.3 + 2.4 + 2.5 + 2.6 + 2.7:
                             Komplettes Frontend Redesign

Wave 2 (nach Wave 1):  ✅
  └── infrastructure-eng  → Docker Build & Start auf localhost:8081
```

## Execution Waves (Iteration 2.1 — Hero SVG + Baufortschritt)

### Epic 2.8: Hero SVG Update
**Agent**: frontend-eng
**Priority**: P0

- [ ] Hero-Wordmark SVG ersetzen durch Padel-Court-Diagramm aus `logo/courts - padelclub (standalone).html`
  - Court-Linien (Rechteck, Mittellinie, Aufschlaglinie) als SVG-Pfade
  - "courts" Text unterhalb des Courts
  - "PADELCLUB" in Monospace darunter
  - Beibehaltung der CSS Custom Properties (--fg, --accent)

### Epic 2.9: Baufortschritt-Sektion
**Agent**: frontend-eng
**Priority**: P0

- [ ] Neue Sektion "Baufortschritt" zwischen Hero und Courts einfügen
- [ ] Sektion enthält:
  - Sec-Head mit Nummer "01.5" und Titel "Baufortschritt"
  - Vollbreites Video-Element mit `construction.mp4` (autoplay, muted, loop)
  - Drohnenfoto `construction-aerial.jpg` als großes Bild
  - Beschreibungstext zum Baufortschritt
  - Design orientiert an thecubepadel.de: clean, großzügig, Media-fokussiert
- [ ] CSS für Baufortschritt-Sektion:
  - Video mit border-radius, optional overlay
  - Bild-Grid oder gestapeltes Layout
  - Responsive Breakpoints

```
Wave 1 (frontend-eng):
  └── Epic 2.8 + 2.9: Hero SVG + Baufortschritt (HTML + CSS)

Wave 2 (nach Wave 1):
  └── Docker Rebuild & Restart auf localhost:8081
```

## Abhängigkeits-Graph
```
Epic 2.1 (Assets) ──→ Epic 2.2-2.7 (Frontend) ──→ Epic 2.8+2.9 (Hero+Baufortschritt) ──→ Docker Restart
```

## Definition of Done — Iteration 2
- [x] Neues Design 1:1 nach courts.html Handoff implementiert
- [x] Logo "courts padelclub" im Header und Footer
- [x] Tannengrün-Farbschema durchgängig
- [x] Space Grotesk / Fraunces / JetBrains Mono Typografie
- [x] Alle Sektionen: Hero, Courts, Hansefit, Events, Team, Kontakt, Footer
- [x] Kontaktformular funktioniert weiterhin
- [x] Responsive auf Mobile/Tablet/Desktop
- [x] Anwendung läuft unter localhost:8081

## Definition of Done — Iteration 2.1
- [ ] Hero zeigt Padel-Court-Diagramm SVG statt Text-Wordmark
- [ ] Baufortschritt-Sektion mit Video und Drohnenfoto sichtbar
- [ ] Video autoplayed (muted, loop)
- [ ] Responsive auf Mobile/Tablet/Desktop
- [ ] Anwendung läuft unter localhost:8081
