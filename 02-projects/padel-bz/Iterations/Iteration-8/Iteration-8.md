# Iteration 8: Navbar-Layout mit linkem Slogan-Logo

## Ziel
Der Header bleibt unveraendert. Die Navbar wird neu austariert: Das zentrierte Scroll-Logo aus Iteration 7 wird entfernt, links erscheint das transparente Logo mit Slogan, und die Navigationspunkte stehen wieder rechts. Die horizontale Einrueckung der Navbar-Inhalte muss mit den Hauptinhalten der Webseite uebereinstimmen.

## Ausgangslage
- Iteration 7 hat das Hero-Logo vergroessert und ein zentriertes transparentes Logo in der Navbar eingefuehrt.
- Aktuelle Navbar:
  - `.nav-scroll-logo` ist zentriert und nutzt `logo_courts_transparent.svg`.
  - Textlinks und CTA existieren weiterhin.
  - Navbar selbst nutzt `padding: 0 var(--pad)`, aber keinen `max-width`-Wrapper wie die Seiteninhalte.
- Neues Ziel:
  - Header/Hero bleibt wie er ist.
  - Links in der Navbar: `logo/logo_courts_transparent_mit slogan.svg`.
  - Rechts in der Navbar: Navigationspunkte plus CTA.
  - Navbar-Inhalte erhalten denselben horizontalen Rahmen wie `.wrap` und andere Content-Bereiche: `max-width: var(--max)`, `padding-left/right: var(--pad)`, `margin: 0 auto`.

## Scope
- Asset-Bereitstellung: `logo/logo_courts_transparent_mit slogan.svg` -> `web/static/img/logo_courts_transparent_mit_slogan.svg`
- Navbar-Markup: `web/templates/partials/nav.html`
- Navbar-CSS: `web/static/css/style.css`
- Plan-/Statusdokumentation: `plan/iterations/iteration-8.md`
- Keine Aenderung am Hero, Footer, Backend oder Navigationstexten.

## Produktentscheidung
Das Logo wird beim Kopieren fuer Web-Referenzen ohne Leerzeichen benannt: `logo_courts_transparent_mit_slogan.svg`. Dadurch bleiben URLs stabil und unproblematisch. Das Original im `logo/`-Ordner bleibt unveraendert.

## Critical Path
1. Slogan-Logo als Web-Asset bereitstellen.
2. Zentriertes `.nav-scroll-logo` entfernen und neues linkes Brand-Logo einbauen.
3. Navbar-Inhalte in einen `max-width`-Wrapper bringen.
4. Nav-Links rechts ausrichten und mobile Breakpoints pruefen.
5. QA gegen Asset-Auslieferung, CSS-Syntax und Navigation durchfuehren.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Wave-Steuerung, Akzeptanzkriterien |
| frontend-eng | Navbar-Markup, Layout, responsive Verhalten |
| infrastructure-eng | Asset-Bereitstellung unter `web/static/img` |
| testing-eng | Syntaxchecks, HTTP-/Asset-Pruefung, Kontrollwerte |
| review-eng | Review von Layout, Klickbarkeit und Responsive-Risiken |
| documentation-eng | Status, QA-Ergebnis und Rest-Risiken im Iterationsplan |

## Task: 8.1 Slogan-Logo als Web-Asset bereitstellen
**Priority**: P0
**Blocked by**: none
**Skillset**: infrastructure-eng

**Akzeptanzkriterien**:
- [x] `web/static/img/logo_courts_transparent_mit_slogan.svg` existiert.
- [x] Quelle bleibt `logo/logo_courts_transparent_mit slogan.svg`.
- [x] Originaldatei im `logo/`-Ordner wird nicht veraendert.

**Output**:
- Neues statisches SVG-Asset.

## Task: 8.2 Navbar-Markup umbauen
**Priority**: P0
**Blocked by**: 8.1
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Zentriertes `.nav-scroll-logo` ist aus der Navbar entfernt.
- [x] Links steht ein Home-Link mit `logo_courts_transparent_mit_slogan.svg`.
- [x] Nav-Links und CTA bleiben erhalten.
- [x] Mobile-Menue bleibt im gleichen Alpine-Kontext bedienbar.

**Kontext**:
- Datei: `web/templates/partials/nav.html`
- Neuer Aufbau:
  - `<nav>`
  - `<div class="nav-inner">`
  - links `.brand`
  - rechts `.nav-links`
  - Mobile Toggle und Mobile Menu bleiben erhalten

**Output**:
- Markup-Anpassung in `web/templates/partials/nav.html`.

## Task: 8.3 Navbar-Inhalte auf Seitenraster ausrichten
**Priority**: P0
**Blocked by**: 8.2
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Navbar-Inhalte nutzen `max-width: var(--max)`.
- [x] Navbar-Inhalte nutzen horizontal `var(--pad)`.
- [x] Navbar-Inhalte sind mit Hero-/Sektionsinhalten ausgerichtet.
- [x] Nav-Links stehen rechts.
- [x] Logo steht links und bleibt sichtbar.

**Kontext**:
- Bestehende Content-Logik: `.wrap`, `.hero-grid`, andere Sections nutzen `max-width: var(--max)` und `padding: 0 var(--pad)`.
- `nav` soll die Hintergrundflaeche behalten; `.nav-inner` steuert Inhaltseinrueckung.

**Output**:
- CSS-Anpassungen in `web/static/css/style.css`.

## Task: 8.4 Responsive Navbar absichern
**Priority**: P1
**Blocked by**: 8.3
**Skillset**: frontend-eng, review-eng

**Akzeptanzkriterien**:
- [x] Desktop: Logo links, Links/CTA rechts.
- [x] Tablet: Textlinks verschwinden bei Bedarf, CTA und Mobile-Toggle bleiben bedienbar.
- [x] Mobile: Logo links, CTA und Mobile-Toggle kollidieren nicht.
- [x] Navbar bleibt auf der Startseite initial ausgeblendet und erscheint beim Scrollen.
- [x] Nicht-Hero-Seiten zeigen die Navbar direkt.

**Output**:
- Responsive CSS-Feinschliff.

## Task: 8.5 QA & Dokumentation
**Priority**: P1
**Blocked by**: 8.4
**Skillset**: testing-eng, review-eng, documentation-eng

**Akzeptanzkriterien**:
- [x] CSS-Brace-Check besteht.
- [x] JS-Syntaxcheck besteht, falls JS geaendert wurde.
- [x] App und neues Logo-Asset liefern im Container HTTP 200.
- [x] Keine Referenz auf `.nav-scroll-logo` bleibt in Navbar/CSS.
- [x] QA-Grenzen sind dokumentiert.

**Output**:
- QA-Status in dieser Datei.

## Execution Waves

### Wave 1 — Asset Foundation
**Skillsets**: product-own, infrastructure-eng
**Tasks**: 8.1
**Ziel**: Neues Slogan-Logo als statisches Web-Asset bereitstellen.

### Wave 2 — Navbar Markup
**Skillsets**: frontend-eng
**Tasks**: 8.2
**Ziel**: Zentriertes Logo entfernen, linkes Slogan-Logo einbauen, rechte Navigation erhalten.

### Wave 3 — Navbar Layout
**Skillsets**: frontend-eng, review-eng
**Tasks**: 8.3, 8.4
**Ziel**: Navbar-Inhalte an Webseitenraster ausrichten und responsive Bedienbarkeit sichern.

### Wave 4 — QA & Abschluss
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 8.5
**Ziel**: Auslieferung, Syntax und Rest-Risiken dokumentieren.

## Abhaengigkeits-Graph

```text
8.1 Asset
  -> 8.2 Markup
  -> 8.3 Layout
  -> 8.4 Responsive
  -> 8.5 QA/Doku
```

## Umsetzungsstatus

### Wave 1 — Asset Foundation: done
- `logo/logo_courts_transparent_mit slogan.svg` wurde nach `web/static/img/logo_courts_transparent_mit_slogan.svg` kopiert.
- Die Web-Kopie nutzt eine zugeschnittene `viewBox`, damit das Slogan-Logo in der Navbar als horizontales Logo funktioniert.
- Das Original im `logo/`-Ordner blieb unveraendert.

### Wave 2 — Navbar Markup: done
- `.nav-scroll-logo` wurde aus `web/templates/partials/nav.html` entfernt.
- Neue linke `.brand` nutzt `logo_courts_transparent_mit_slogan.svg`.
- Nav-Links, CTA, Mobile-Toggle und Mobile-Menue bleiben im gleichen `x-data`-Kontext.

### Wave 3 — Navbar Layout: done
- `nav` bleibt die volle gruene Flaeche.
- `.nav-inner` steuert die Inhaltseinrueckung mit `max-width: var(--max)`, `margin: 0 auto` und `padding: 0 var(--pad)`.
- `.nav-links` nutzt `margin-left:auto` und steht rechts.
- Auf Mobile wird `.nav-links` komplett ausgeblendet; die CTA bleibt im Mobile-Menue verfuegbar.
- Kontrollwerte:
  - Wide 1600px: Content-Innenbreite 1328px, Padding 56px, Logo 240px.
  - Desktop 1440px: Content-Innenbreite 1328px, Padding 56px, Logo 216px.
  - Laptop 1200px: Content-Innenbreite 1104px, Padding 48px, Logo 180px.
  - Tablet 820px: Content-Innenbreite 754px, Padding 33px, Logo 188px, Hamburger-Modus.
  - Mobile 390px: Content-Innenbreite 350px, Padding 20px, Logo 172px, Hamburger-Modus.

### Wave 4 — QA & Abschluss: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- Docker-Container laufen; `/`, `logo_courts_transparent_mit_slogan.svg`, `style.css` und `main.js` liefern im Container HTTP 200.
- `rg` bestaetigt: keine `.nav-scroll-logo`-Referenz mehr in Navbar/CSS.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.

## Definition of Done
- [x] Slogan-Logo ist als Web-Asset vorhanden.
- [x] Navbar zeigt links das transparente Slogan-Logo.
- [x] Zentriertes Scroll-Logo ist entfernt.
- [x] Navigationspunkte stehen rechts.
- [x] Navbar-Inhalte sind mit dem Seitenraster ausgerichtet.
- [x] Mobile Navigation bleibt bedienbar.
- [x] Startseiten-Navbar bleibt initial ausgeblendet und erscheint beim Scrollen.
- [x] QA-Status und Rest-Risiken sind dokumentiert.
