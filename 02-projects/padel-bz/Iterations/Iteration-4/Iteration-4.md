# Iteration 4: Hero-Logo — Transparentes Logo mit neuer Positionierung

## Ziel
Das Hero-Logo wird durch die transparente Variante (`logo_courts_transparenmt.svg`) ersetzt und von der zentrierten Position unterhalb des Hero-Grids nach oben-links verschoben, leicht in Richtung Mitte. Das transparente Logo ueberlagert direkt das Hero-Hintergrundbild und erzeugt einen hochwertigen, markenstarken Eindruck.

## Aenderungs-Uebersicht

### Vorher (Iteration 3)
- Logo: `logo_courts.svg` (mit olive-gruenem Hintergrund-Rechteck)
- Position: Zentriert unterhalb des Hero-Grids (`.hero-logo-wrap` mit `text-align: center`)
- Das Logo wirkt wie ein separater Block unter dem Content

### Nachher (Iteration 4)
- Logo: `logo_courts_transparenmt.svg` (transparent, nur Creme-Pfade)
- Position: Oben-links im Hero, leicht zur Mitte versetzt
- Das Logo schwebt ueber dem Hero-Bild als Wasserzeichen/Brand-Element
- Erzeugt visuellen Zusammenhang zwischen Bild und Marke

## Scope

### Epic 4.1: Asset-Vorbereitung
**Priority**: P0 (Blocker)
**Blocked by**: none
**Agent**: infrastructure-eng

**Tasks**:
1. **Transparentes Logo bereitstellen**
   - `logo/logo_courts_transparenmt.svg` nach `web/static/img/logo_courts_transparent.svg` kopieren
   - Hinweis: Dateiname wird beim Kopieren korrigiert (transparenmt → transparent)

**Akzeptanzkriterien**:
- [ ] Transparentes Logo liegt unter `web/static/img/logo_courts_transparent.svg`

---

### Epic 4.2: Hero-Logo austauschen & repositionieren
**Priority**: P0 (Blocker)
**Blocked by**: Epic 4.1
**Agent**: frontend-eng

**Tasks**:
1. **HTML: Logo-Quelle aendern** (`home.html`)
   - Im Hero-Bereich: `src="/static/img/logo_courts.svg"` aendern zu `src="/static/img/logo_courts_transparent.svg"`

2. **CSS: Logo-Position umbauen** (`style.css`)
   - `.hero-logo-wrap`: Von `text-align: center` zu absoluter/relativer Positionierung
   - Neue Position: Oben-links im Hero, aber nicht ganz am Rand — leicht zur Mitte versetzt
   - Vorgeschlagenes CSS:
     ```css
     .hero-logo-wrap {
         position: absolute;
         top: clamp(24px, 4vh, 48px);
         left: clamp(40px, 8vw, 120px);  /* leicht zur Mitte */
         z-index: 1;
         text-align: left;
         padding: 0;
         margin-top: 0;
     }
     ```
   - `.hero-logo`: Groesse anpassen fuer neue Position
     - Desktop: `width: clamp(200px, 22vw, 320px)` (etwas groesser da transparent)
     - Mobile: `width: clamp(140px, 35vw, 200px)`
   - Optional: Leichter `drop-shadow` oder `filter: drop-shadow(0 2px 8px rgba(0,0,0,0.3))` fuer bessere Sichtbarkeit auf hellem Bildbereich

3. **Hero-Grid Padding anpassen**
   - Da das Logo jetzt oben-links schwebt, muss der Hero-Grid ggf. mehr `padding-top` bekommen damit Logo und Text sich nicht ueberlappen
   - `header.hero` padding-top erhoehen: `padding-top: clamp(140px, 18vh, 220px)` (Platz fuer Logo)

4. **Mobile-Responsive**
   - Auf Mobile (<=860px): Logo-Position anpassen
   - Ggf. zentriert auf Mobile oder kleiner oben-links
   - Sicherstellen dass Logo nicht mit der Navigation ueberlappt (Nav ist sticky, Hero scrollt)

**Akzeptanzkriterien**:
- [ ] Hero zeigt das transparente Logo (ohne Hintergrund-Rechteck)
- [ ] Logo ist oben-links positioniert, leicht zur Mitte versetzt
- [ ] Logo ueberlappt nicht mit Navigation oder Hero-Text
- [ ] Logo ist auf dem Hero-Bild gut sichtbar (Kontrast)
- [ ] Responsive: Korrekte Darstellung auf Mobile/Tablet/Desktop

---

### Epic 4.3: Preload & Cleanup
**Priority**: P1 (Critical)
**Blocked by**: Epic 4.2
**Agent**: frontend-eng

**Tasks**:
1. **Preload aktualisieren** (`base.html`)
   - Logo-Preload von `logo_courts.svg` auf `logo_courts_transparent.svg` aendern
   - Hinweis: Das nicht-transparente Logo wird weiterhin in Nav und Footer verwendet — Preload fuer beide behalten oder nur das haeufigere

2. **Pruefen ob altes Logo noch gebraucht wird**
   - `logo_courts.svg` (mit Hintergrund) wird weiterhin in Nav und Footer verwendet
   - Es darf NICHT entfernt werden
   - Nur die Hero-Referenz aendert sich

**Akzeptanzkriterien**:
- [ ] Preload in base.html ist aktualisiert
- [ ] Altes Logo bleibt in Nav und Footer erhalten
- [ ] Keine verwaisten Asset-Referenzen

---

### Epic 4.4: QA & Docker Restart
**Priority**: P2 (Normal)
**Blocked by**: Epic 4.3
**Agent**: testing-eng

**Tasks**:
1. **Visuelle Pruefung**
   - Alle Asset-Referenzen existieren
   - Template-Struktur konsistent
   - Keine CSS-Syntaxfehler

2. **Docker Container neu bauen und starten**
   - `docker compose down && docker compose up -d --build`
   - Pruefen ob App auf localhost:8081 erreichbar ist (HTTP 200)

**Akzeptanzkriterien**:
- [ ] Alle Assets vorhanden und korrekt referenziert
- [ ] Docker Container laufen
- [ ] App erreichbar auf localhost:8081

---

## Abhaengigkeits-Graph

```
Epic 4.1 (Asset) ──→ Epic 4.2 (Logo-Tausch + Positionierung) ──→ Epic 4.3 (Preload/Cleanup) ──→ Epic 4.4 (QA + Docker)
```

## Execution Waves

```
Wave 1 — Asset (keine Abhaengigkeiten):
  └── infrastructure-eng  → Epic 4.1: Transparentes Logo kopieren

Wave 2 — Logo-Tausch + Positionierung (nach Wave 1):
  └── frontend-eng        → Epic 4.2 + Epic 4.3: Logo austauschen,
                             CSS-Position umbauen, Preload aktualisieren

Wave 3 — QA + Docker (nach Wave 2):
  └── testing-eng         → Epic 4.4: Visuelle Pruefung + Docker Restart
```

### Wave-Detail

| Wave | Agent | Epics | Komplexitaet |
|------|-------|-------|--------------|
| Wave 1 | infrastructure-eng | 4.1 | Niedrig (1 Datei kopieren) |
| Wave 2 | frontend-eng | 4.2 + 4.3 | Mittel (CSS-Positionierung, HTML-Aenderung, Preload) |
| Wave 3 | testing-eng | 4.4 | Niedrig (Pruefung + Docker) |

## Definition of Done — Iteration 4
- [ ] Transparentes Logo im Hero sichtbar (ohne Hintergrund-Rechteck)
- [ ] Logo-Position: Oben-links, leicht zur Mitte versetzt
- [ ] Logo ueberlappt nicht mit Nav oder Hero-Text
- [ ] Nav und Footer verwenden weiterhin das Logo mit Hintergrund
- [ ] Responsive auf Mobile/Tablet/Desktop
- [ ] App laeuft auf localhost:8081
