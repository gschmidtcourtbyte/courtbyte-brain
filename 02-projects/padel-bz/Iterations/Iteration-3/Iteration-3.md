# Iteration 3: Frontend Refinement — Brand Colors, Logo & Hero-Relaunch

## Ziel
Durchgaengige Integration der offiziellen Markenfarben aus dem Logo, Einbindung des neuen SVG-Logos und Relaunch des Hero-Bereichs mit dem Header-Bild der Padel-Halle. Das Ergebnis ist eine visuell konsistente, markentreue Website die den Premium-Charakter der Anlage transportiert.

## Design-Richtung
- **Farbschema**: Olive-Waldgruen (#475243) + Warmes Creme (#FAF2E8) aus `logo/logo_courts.svg`
- **Hero**: Full-bleed Foto (Header-Bild.jpeg) mit Gradient-Overlay, inspiriert von thecubepadel.de
- **Logo**: Offizielles SVG-Logo (`logo_courts.svg`) in Navigation, Hero und Footer
- **Stil**: Premium, sportlich, editorial — Tannengruen wird zu Olive-Waldgruen verschoben

## Farbmigration

### Bisherige Palette (Iteration 2)
| Token | Alt (Hex) |
|-------|-----------|
| --bg | #2e3c36 |
| --bg-deep | #243029 |
| --bg-elev | #364942 |
| --fg | #e7e1d5 |
| --fg-dim | #b6b2a6 |
| --fg-mute | #8a8879 |
| --line | #4a5a52 |
| --line-soft | #3b4942 |

### Neue Palette (Iteration 3 — abgeleitet aus Logo)
| Token | Neu (Hex) | Quelle |
|-------|-----------|--------|
| --bg | #475243 | Logo-Hintergrund `rgb(71,82,67)` |
| --bg-deep | #3a4437 | Abgedunkelt |
| --bg-elev | #546350 | Aufgehellt |
| --fg | #FAF2E8 | Logo-Text `rgb(250,242,232)` |
| --fg-dim | #d4cdc2 | Gedaempft |
| --fg-mute | #a8a293 | Stark gedaempft |
| --line | #5f6e5a | Abgeleitet |
| --line-soft | #536250 | Abgeleitet |
| --accent | #d9e1a6 | Beibehalten (Court-Line Gelbgruen) |
| --accent-deep | #c3cc86 | Beibehalten |

---

## Scope

### Epic 3.1: Asset-Vorbereitung & Farbdokumentation
**Priority**: P0 (Blocker)
**Blocked by**: none
**Agent**: infrastructure-eng

**Tasks**:
1. **Logo-Asset bereitstellen**
   - `logo/logo_courts.svg` nach `web/static/img/logo_courts.svg` kopieren
   - Header-Bild optimieren: `logo/Header-Bild.jpeg` nach `web/static/img/hero-header.jpg` kopieren
   - Bildkompression fuer Web-Auslieferung (max 300KB fuer Hero-Bild)

2. **Favicon aus Logo generieren**
   - SVG-basiertes Favicon erstellen (favicon.svg)
   - Fallback favicon.ico (32x32) generieren
   - In `base.html` einbinden

**Akzeptanzkriterien**:
- [ ] Logo-SVG liegt unter `/static/img/logo_courts.svg`
- [ ] Hero-Bild liegt unter `/static/img/hero-header.jpg` (optimiert)
- [ ] Favicon ist eingebunden und sichtbar im Browser-Tab

---

### Epic 3.2: Design-Token-Update — Farbschema
**Priority**: P0 (Blocker)
**Blocked by**: none
**Agent**: frontend-eng

**Tasks**:
1. **CSS Custom Properties aktualisieren**
   - Alle 8 Farb-Tokens in `:root` auf neue Werte umstellen (siehe Farbmigration)
   - `--accent` und `--accent-deep` beibehalten (harmonieren mit neuem Gruen)
   - `meta[name="theme-color"]` in base.html auf `#475243` aendern

2. **Transparenz-Werte anpassen**
   - Nav-Hintergrund: `rgba(71,82,67,.65)` (statt rgba(46,60,54,.65))
   - Mobile-Menu: `rgba(58,68,55,.97)` (statt rgba(36,48,41,.97))
   - Alle `rgba`-Werte in style.css die auf alten Farben basieren updaten

3. **Selection & Scrollbar anpassen**
   - `::selection` Background-Farbe pruefen
   - Scrollbar-Thumb/Track an neue Palette anpassen

**Akzeptanzkriterien**:
- [ ] Alle CSS Custom Properties verwenden die neuen Farbwerte
- [ ] Keine hartcodierten alten Farbwerte (#2e3c36, #243029, etc.) im CSS
- [ ] Transparente Overlays basieren auf neuen RGB-Werten
- [ ] Konsistentes Erscheinungsbild — keine Farbbrueche zwischen Sektionen

---

### Epic 3.3: Hero-Relaunch mit Header-Bild
**Priority**: P0 (Blocker)
**Blocked by**: Epic 3.1 (Assets muessen bereitstehen)
**Agent**: frontend-eng

**Design-Briefing** (inspiriert von thecubepadel.de):
Der Hero soll das imposante Padel-Hallen-Foto (`Header-Bild.jpeg`) als Full-Bleed-Hintergrundbild zeigen. Darüber liegt ein Gradient-Overlay (von dunkel-transparent zu Hintergrundfarbe), sodass Text lesbar bleibt. Das Logo steht prominent im Hero. Der Besucher soll sofort den Premium-Charakter der Anlage spueren.

**Tasks**:
1. **Hero-Struktur umbauen**
   - Hero bekommt `background-image: url(/static/img/hero-header.jpg)`
   - `background-size: cover`, `background-position: center`
   - Mindesthoehe: `min-height: 85vh` (Desktop), `min-height: 60vh` (Mobile)
   - Gradient-Overlay: `linear-gradient(to bottom, rgba(58,68,55,0.3) 0%, rgba(58,68,55,0.7) 60%, #3a4437 100%)`
   - Das Overlay sorgt fuer Uebergang vom Bild zum Seiteninhalt

2. **Logo im Hero platzieren**
   - `logo_courts.svg` als `<img>` oder inline SVG im Hero-Bereich
   - Positionierung: zentriert ueber dem Bild oder links im Hero-Meta-Bereich
   - Groesse: ca. 180px-240px Breite (Desktop), 120px-160px (Mobile)
   - Das SVG nutzt die Markenfarben (Olive-Gruen-Hintergrund + Creme-Text)

3. **Info-Rail beibehalten & stylen**
   - Info-Rail (Standort, Courts, Oeffnung, Partner) bleibt rechts
   - Leicht transparenter Hintergrund fuer bessere Lesbarkeit ueber dem Bild
   - Glassmorphism-Effekt: `background: rgba(71,82,67,0.6); backdrop-filter: blur(8px)`

4. **Hero-Tagline & CTA ueber dem Bild**
   - Text-Elemente (Kicker, Lede, CTAs) muessen ueber dem Bild lesbar sein
   - Text-Shadow oder Background-Blur fuer Kontrast
   - CTA-Buttons behalten ihr Accent-Styling

5. **SVG Wordmark entfernen/ersetzen**
   - Den bisherigen Court-Diagramm SVG Wordmark aus dem Hero entfernen
   - Stattdessen das Header-Bild als visuelles Element nutzen
   - Optional: Logo SVG als dezentes Wasserzeichen ueber dem Bild

**Akzeptanzkriterien**:
- [ ] Hero zeigt Header-Bild.jpeg als Full-Bleed-Hintergrund
- [ ] Gradient-Overlay sorgt fuer lesbaren Text
- [ ] Logo ist sichtbar und prominent im Hero
- [ ] Info-Rail ist weiterhin vorhanden und lesbar
- [ ] CTA-Buttons sind klickbar und sichtbar
- [ ] Responsive: Bild skaliert korrekt auf Mobile/Tablet/Desktop
- [ ] Kein Layout-Shift beim Laden des Bildes (aspect-ratio oder min-height)

---

### Epic 3.4: Logo in Navigation & Footer
**Priority**: P1 (Critical)
**Blocked by**: Epic 3.1, Epic 3.2
**Agent**: frontend-eng

**Tasks**:
1. **Navigation: Logo ersetzen**
   - Bisheriges Text-Brand (`<span class="mark">c</span> courts padelclub`) durch Logo-SVG ersetzen
   - Logo inline als `<img src="/static/img/logo_courts.svg">` oder inline SVG
   - Hoehe: 36-40px in der Nav-Bar
   - SVG-Farben muessen zum dunklen Nav-Hintergrund passen
   - Auf kleinen Bildschirmen Logo ggf. auf Icon-Groesse reduzieren

2. **Footer: Logo ersetzen**
   - Bisheriges Text-Wordmark (`<span class="wordmark">courts</span>`) durch Logo ersetzen
   - Groessere Darstellung im Footer (ca. 200-300px Breite)
   - Alternativ: Logo + Text-Wordmark nebeneinander

**Akzeptanzkriterien**:
- [ ] Logo-SVG ist in der Navigation sichtbar und korrekt dimensioniert
- [ ] Logo-SVG ist im Footer sichtbar
- [ ] Klick auf Nav-Logo fuehrt zur Startseite
- [ ] Responsive Darstellung auf allen Breakpoints

---

### Epic 3.5: Sektions-Farbkonsistenz
**Priority**: P1 (Critical)
**Blocked by**: Epic 3.2
**Agent**: frontend-eng

**Tasks**:
1. **Baufortschritt-Sektion**
   - Hintergrund-Farbe auf `--bg-deep` (neu) pruefen
   - Status-Badge Farben verifizieren
   - Video/Foto-Border-Farben anpassen

2. **Courts-Grid**
   - Placeholder-Streifen an neue Palette anpassen
   - Stat-Boxen Border und Akzentfarben pruefen
   - Court-Labels Farben verifizieren

3. **Hansefit-Strip**
   - Hintergrundmuster (`repeating-linear-gradient`) mit neuen Farben
   - Text und Button-Kontrast pruefen

4. **Events-Sektion**
   - GPS-Card Hintergrund und Border
   - Badge-Farben
   - Event-Liste Trennlinien

5. **Team-Sektion**
   - Avatar-Placeholder-Hintergrund
   - Text-Farben

6. **Kontakt-Sektion**
   - Abstrakte Map Hintergrund und Grid-Linien
   - Kontakt-Zeilen Farben
   - Map-Marker Puls-Animation mit neuen Farben

7. **Formular-Styles (contact.html)**
   - Form-Card Hintergrund
   - Input-Fields Border und Focus-Ring
   - Alert-Styles (Success/Error)

8. **Legale Seiten (Impressum, Datenschutz)**
   - Typografie-Farben pruefen
   - Ueberschriften und Body-Text

**Akzeptanzkriterien**:
- [ ] Alle Sektionen verwenden konsistent die neuen Markenfarben
- [ ] Keine Farbbrueche oder verwaisten alten Farbwerte
- [ ] Kontrast-Verhaeltnis WCAG AA-konform (mind. 4.5:1 fuer Text)
- [ ] Puls-Animationen und Hover-States nutzen neue rgba-Werte

---

### Epic 3.6: Responsive & Performance-Optimierung
**Priority**: P2 (Normal)
**Blocked by**: Epic 3.3, Epic 3.4
**Agent**: frontend-eng

**Tasks**:
1. **Hero-Bild Responsive**
   - `<picture>`-Element mit srcset fuer verschiedene Aufloesung (optional)
   - Mobile: ggf. anderer `background-position` fuer besseren Bildausschnitt
   - `loading="eager"` fuer Hero (above-the-fold)

2. **Logo-Responsive**
   - Logo-SVG skaliert sauber auf allen Breakpoints
   - Nav: Logo verkleinern auf kleinen Screens
   - Hero: Logo-Groesse per clamp() responsive

3. **Performance**
   - Hero-Bild Preload via `<link rel="preload">` in base.html
   - Logo-SVG inline einbetten statt externer Request (optional)
   - Pruefen ob Cumulative Layout Shift durch Bild-Laden entsteht

**Akzeptanzkriterien**:
- [ ] Hero-Bild laedt schnell und ohne Layout-Shift
- [ ] Logo ist auf allen Geraeten scharf (SVG-Vorteil)
- [ ] Lighthouse Performance Score >= 80
- [ ] Mobile: Kein horizontaler Overflow

---

### Epic 3.7: QA & Abnahme
**Priority**: P2 (Normal)
**Blocked by**: Epic 3.5, Epic 3.6
**Agent**: testing-eng

**Tasks**:
1. **Visuelles Audit**
   - Alle Seiten im Browser pruefen (Desktop 1440px, Tablet 768px, Mobile 375px)
   - Farbkonsistenz ueber alle Sektionen verifizieren
   - Logo-Darstellung in Nav, Hero, Footer

2. **Funktionalitaet**
   - Navigation: Smooth Scroll, Mobile Menu
   - Kontaktformular: Submit, Validierung, CSRF
   - Links: Alle internen Links funktionsfaehig

3. **Accessibility-Check**
   - Farbkontrast pruefen (WCAG AA)
   - Alt-Texte fuer Hero-Bild und Logo
   - Focus-Ring Sichtbarkeit mit neuem Farbschema

**Akzeptanzkriterien**:
- [ ] Keine visuellen Regressionen
- [ ] Kontaktformular funktioniert weiterhin end-to-end
- [ ] Alle Links funktionieren
- [ ] WCAG AA Kontrast eingehalten

---

## Abhaengigkeits-Graph

```
Epic 3.1 (Assets) ──────────┬──→ Epic 3.3 (Hero-Relaunch)
                             │
Epic 3.2 (Design-Tokens) ───┼──→ Epic 3.4 (Logo Nav/Footer)
                             │
                             ├──→ Epic 3.5 (Sektions-Farben)
                             │
                             └──→ Epic 3.6 (Responsive)
                                        │
                                        └──→ Epic 3.7 (QA)
```

## Execution Waves

```
Wave 1 — Foundation (Parallel, keine Abhaengigkeiten):
  ├── infrastructure-eng  → Epic 3.1: Assets kopieren, Bild optimieren, Favicon
  └── frontend-eng        → Epic 3.2: CSS Design-Tokens auf neue Farben umstellen

Wave 2 — Hero & Logo (nach Wave 1):
  └── frontend-eng        → Epic 3.3: Hero-Relaunch mit Header-Bild
                           + Epic 3.4: Logo in Navigation und Footer

Wave 3 — Konsistenz (nach Wave 2):
  └── frontend-eng        → Epic 3.5: Alle Sektionen auf neue Farben pruefen
                           + Epic 3.6: Responsive & Performance

Wave 4 — QA (nach Wave 3):
  └── testing-eng         → Epic 3.7: Visuelles Audit, Funktionstest, Accessibility
```

### Wave-Detail

#### Wave 1: Foundation
| Agent | Epic | Tasks | Geschaetzte Komplexitaet |
|-------|------|-------|-------------------------|
| infrastructure-eng | 3.1 | Assets kopieren, Bild optimieren, Favicon | Niedrig |
| frontend-eng | 3.2 | CSS Custom Properties umstellen, rgba-Werte | Mittel |

**Parallelisierbar**: Ja — beide Agents arbeiten an unabhaengigen Dateien.

#### Wave 2: Hero & Logo
| Agent | Epic | Tasks | Geschaetzte Komplexitaet |
|-------|------|-------|-------------------------|
| frontend-eng | 3.3 | Hero umbauen (HTML + CSS), Bild einbinden, Overlay | Hoch |
| frontend-eng | 3.4 | Nav-Logo ersetzen, Footer-Logo ersetzen | Mittel |

**Sequentiell**: Ein Agent (frontend-eng), aber Epics koennen nacheinander im selben Wave abgearbeitet werden.

#### Wave 3: Konsistenz
| Agent | Epic | Tasks | Geschaetzte Komplexitaet |
|-------|------|-------|-------------------------|
| frontend-eng | 3.5 | 8 Sektionen Farb-Audit + Anpassung | Mittel |
| frontend-eng | 3.6 | Responsive-Checks, Preload, Performance | Niedrig |

**Sequentiell**: Farb-Konsistenz zuerst, dann Performance-Tuning.

#### Wave 4: QA
| Agent | Epic | Tasks | Geschaetzte Komplexitaet |
|-------|------|-------|-------------------------|
| testing-eng | 3.7 | Visuelles Audit, Funktions-Tests, Accessibility | Mittel |

---

## Definition of Done — Iteration 3
- [ ] Farbschema basiert durchgaengig auf Logo-Farben (#475243 / #FAF2E8)
- [ ] Offizielles Logo (logo_courts.svg) in Navigation, Hero und Footer
- [ ] Hero zeigt Header-Bild.jpeg als Full-Bleed-Hintergrund mit Gradient-Overlay
- [ ] Keine verwaisten alten Farbwerte im CSS
- [ ] Responsive auf Mobile (375px), Tablet (768px), Desktop (1440px)
- [ ] Kontaktformular funktioniert weiterhin end-to-end
- [ ] WCAG AA Farbkontrast eingehalten
- [ ] Favicon aus Logo eingebunden
- [ ] Anwendung laeuft unter localhost:8081
