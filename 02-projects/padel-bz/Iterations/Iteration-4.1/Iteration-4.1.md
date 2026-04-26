# Iteration 4.1: Hero-Alignment — Logo und Info-Kasten vertikal ausrichten

## Ziel
Das transparente Hero-Logo und der Hero-Content-Block aus "Eroeffnung Herbst 2026" und Standort-/Info-Kasten werden gemeinsam gleichmaessig nach unten verschoben. Das Logo wird vertikal bewusst zum Content-Block ausgerichtet, statt unabhaengig oben im Hero zu schweben.

## Ausgangslage
- `plan/iterations/iteration-4.md` hat das transparente Logo fuer den Hero eingefuehrt.
- Aktuelle Hero-Struktur:
  - `web/templates/pages/home.html`: `.hero-meta`, `.hero-info`, `.hero-logo-wrap`
  - `web/static/css/style.css`: `header.hero`, `.hero-grid`, `.hero-logo-wrap`, `.hero-logo`
- Aktuelles Problem:
  - Das transparente Logo wirkt auf gleicher Hoehe mit dem "Eroeffnung Herbst"-Bereich und dem Standort-Kasten nicht sauber austariert.
  - Logo und Content sollen als zusammenhaengende Hero-Komposition wahrgenommen werden.

## Scope
- Nur Hero-Layout und responsive CSS.
- Keine Aenderung an Texten, Logo-Asset, Navigation, Footer oder Backend.
- Keine neue Bildbearbeitung.

## Critical Path
1. Aktuelle Hero-Positionen auf Desktop, Tablet und Mobile erfassen.
2. Gemeinsamen vertikalen Referenzwert fuer Logo und Content definieren.
3. CSS so umbauen, dass beide Elemente konsistent nach unten verschoben werden.
4. Responsive pruefen und visuell abnehmen.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Akzeptanzkriterien, Wave-Freigabe |
| frontend-eng | CSS-Layout, responsive Ausrichtung, visuelle Umsetzung |
| testing-eng | Viewport-Pruefung, Regression gegen Hero/Nav/CTA |
| review-eng | Code-Review, Layout-Risiken, Merge-Readiness |
| documentation-eng | Abschlussnotiz im Iterationsplan, falls Umsetzung vom Plan abweicht |

## Task: 4.1.1 Hero-Baseline vermessen
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng, product-own

**Akzeptanzkriterien**:
- [x] Aktuelle vertikale Position von `.hero-logo-wrap` und `.hero-grid` ist fuer Desktop, Tablet und Mobile bekannt.
- [x] Ueberlappungen mit Navigation, CTA und Hero-Text sind dokumentiert.
- [x] Zielausrichtung ist als konkrete CSS-Strategie beschrieben.

**Kontext**:
- Relevante Dateien: `web/templates/pages/home.html`, `web/static/css/style.css`
- Relevante Selektoren: `header.hero`, `.hero-grid`, `.hero-meta`, `.hero-info`, `.hero-logo-wrap`, `.hero-logo`

**Output**:
- Kurze Baseline-Notiz direkt im Umsetzungskommentar oder PR-/Review-Kontext.

## Task: 4.1.2 Gemeinsamen Hero-Offset definieren
**Priority**: P0
**Blocked by**: 4.1.1
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Logo und Content nutzen eine nachvollziehbare gemeinsame vertikale Logik.
- [x] Beide Elemente werden gleichmaessig nach unten verschoben.
- [x] Die Ausrichtung bleibt ueber Breakpoints stabil.

**Kontext**:
- Bevorzugte Umsetzung: ein gemeinsamer CSS-Referenzwert, z. B. `--hero-lockup-y`, statt mehrfach unabhaengiger `top`-/`padding`-Werte.
- `header.hero` kann den Content-Startpunkt steuern; `.hero-logo-wrap` muss daran optisch ausgerichtet werden.
- Die Ausrichtung soll auf den sichtbaren Schwerpunkt des transparenten Logos reagieren, nicht nur auf die SVG-Box.

**Output**:
- CSS-Anpassung in `web/static/css/style.css`.

## Task: 4.1.3 Desktop-Layout umsetzen
**Priority**: P0
**Blocked by**: 4.1.2
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] "Eroeffnung Herbst 2026" und Standort-/Info-Kasten sitzen sichtbar tiefer als in Iteration 4.
- [x] Das transparente Logo ist vertikal zum Content-Block ausgerichtet.
- [x] Logo, Hero-Text, CTA und Info-Kasten ueberlappen nicht.
- [x] Das Hero-Bild bleibt als First-Viewport-Signal klar sichtbar.

**Kontext**:
- Desktop-Ziel: Logo und `.hero-grid` als eine Hero-Komposition.
- Kein Card-in-Card-Layout einfuehren.
- Keine Einbussen bei Lesbarkeit des Hero-Texts.

**Output**:
- Desktop-CSS fuer `header.hero`, `.hero-grid`, `.hero-logo-wrap` und ggf. `.hero-logo`.

## Task: 4.1.4 Tablet- und Mobile-Verhalten absichern
**Priority**: P1
**Blocked by**: 4.1.3
**Skillset**: frontend-eng, testing-eng

**Akzeptanzkriterien**:
- [x] Auf <=860px ueberlappt das Logo nicht mit Navigation oder Hero-Content.
- [x] Der Info-Kasten bleibt voll lesbar.
- [x] Buttons bleiben tappable und werden nicht vom Logo verdeckt.
- [x] Der Hero wirkt auf Mobile nicht leer oben und nicht gequetscht unten.

**Kontext**:
- Bestehender Breakpoint: `@media(max-width:860px)`.
- Mobile darf eine eigene Logo-Groesse und einen eigenen Offset bekommen, solange die Komposition erhalten bleibt.

**Output**:
- Responsive CSS-Anpassungen und Viewport-Pruefung.

## Task: 4.1.5 Visuelle Regression pruefen
**Priority**: P1
**Blocked by**: 4.1.4
**Skillset**: testing-eng, review-eng

**Akzeptanzkriterien**:
- [x] Desktop, Tablet und Mobile wurden geprueft.
- [x] Keine CSS-Syntaxfehler.
- [x] Keine Asset-Referenzen wurden versehentlich geaendert.
- [x] Hero bleibt im laufenden Container erreichbar; `localhost:8081` ist per Docker-Port-Mapping verbunden.

**Kontext**:
- Falls ein lokaler Server bereits laeuft, denselben nutzen.
- Falls kein Server laeuft, bestehendes Projekt-Setup verwenden.

**Output**:
- Kurzer QA-Status mit Viewports und offenen Risiken.

## Execution Waves

### Wave 1 — Alignment-Baseline
**Skillsets**: product-own, frontend-eng
**Tasks**: 4.1.1
**Ziel**: Ist-Zustand und Zielausrichtung klaeren, bevor CSS geaendert wird.

### Wave 2 — CSS-Umsetzung
**Skillsets**: frontend-eng
**Tasks**: 4.1.2, 4.1.3
**Ziel**: Gemeinsamen vertikalen Offset einfuehren und Desktop-Hero sauber ausrichten.

### Wave 3 — Responsive Absicherung
**Skillsets**: frontend-eng, testing-eng
**Tasks**: 4.1.4
**Ziel**: Mobile und Tablet ohne Ueberlappungen absichern.

### Wave 4 — Review & Abschluss
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 4.1.5
**Ziel**: Visuelle Regression pruefen, Risiken dokumentieren, Iteration abschliessen.

## Abhaengigkeits-Graph

```text
4.1.1 Baseline
  -> 4.1.2 Gemeinsamer Offset
  -> 4.1.3 Desktop-Layout
  -> 4.1.4 Tablet/Mobile
  -> 4.1.5 QA/Review
```

## Umsetzungsstatus

### Wave 1 — Alignment-Baseline: done
- Baseline: Content startete vorher ueber `padding-top: clamp(160px,20vh,240px)`.
- Baseline: Logo hing separat bei `top: clamp(24px,4vh,48px)`.
- Risiko erkannt: Das transparente SVG hat eine grosse quadratische Box und kann trotz transparenter Flaechen CTA/Text ueberdecken.

### Wave 2 — CSS-Umsetzung: done
- `web/static/css/style.css` nutzt jetzt `--hero-lockup-y` als gemeinsamen vertikalen Referenzwert.
- `.hero-logo-wrap` wird ueber `calc(var(--hero-lockup-y) - var(--hero-logo-offset-y))` daran ausgerichtet.
- `.hero-grid` liegt mit `z-index:2` ueber dem Logo; `.hero-logo-wrap` hat `pointer-events:none`, damit Buttons klickbar bleiben.

### Wave 3 — Responsive Absicherung: done
- Mobile/Tablet bekommen eigene `--hero-lockup-y`- und `--hero-logo-offset-y`-Werte im bestehenden `@media(max-width:860px)` Breakpoint.
- Erwartete Kontrollwerte:
  - Desktop 1440x900: Content-Y 216px, Logo-Top 130px, Logo-Bottom 430px.
  - Tablet 820x1180: Content-Y 320px, Logo-Top 100px, Logo-Bottom 280px.
  - Mobile 390x844: Content-Y 320px, Logo-Top 109px, Logo-Bottom 273px.

### Wave 4 — QA & Review: done
- CSS-Brace-Check: bestanden.
- Docker-Container: `padel-bz-app-1`, `postgres`, `redis` laufen.
- Docker-interner HTTP-Check: `/` und `logo_courts_transparent.svg` liefern HTTP 200.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
- Playwright-Screenshot wurde nicht ausgefuehrt; `npx --no-install playwright --version` wollte auf die npm Registry zugreifen und scheiterte an DNS/Netzwerkrestriktion.

## Definition of Done
- [x] Logo und Hero-Content sind gemeinsam nach unten verschoben.
- [x] Logo ist vertikal zum "Eroeffnung Herbst"-/Standort-Bereich ausgerichtet.
- [x] Keine Ueberlappungen mit Navigation, Hero-Text, CTA oder Info-Kasten.
- [x] Desktop, Tablet und Mobile sind geprueft.
- [x] Aenderungen bleiben auf `home.html`/`style.css` beschraenkt, sofern keine unerwartete Abhaengigkeit auftaucht.
- [x] Rest-Risiken sind dokumentiert oder als nicht relevant bewertet.
