# Iteration 5: Hero-Logo-Zentrierung & Scroll-Navbar

## Ziel
Das transparente Logo steht zentral ueber dem Hero-Content und bildet mit dem darunterliegenden Text-/Info-Block eine klare vertikale Komposition. Beim ersten Aufruf der Startseite ist die Navbar nicht sichtbar, damit der Header vollflaechig wirken kann. Erst nach dem Scrollen erscheint die gruene Navbar im bestehenden Format.

## Ausgangslage
- Iteration 4.1 hat Logo und Hero-Content ueber einen gemeinsamen Offset gekoppelt.
- Aktuell steht das transparente Logo optisch noch als separates absolut positioniertes Element im Hero.
- Die Navbar ist sticky und direkt beim Seitenaufruf sichtbar.

## Scope
- Startseiten-Hero: `web/templates/pages/home.html`
- Hero-/Navbar-CSS: `web/static/css/style.css`
- Scroll-State fuer Navbar: `web/static/js/main.js`
- Keine Aenderung an Assets, Backend, Navigationstexten oder Footer.

## Critical Path
1. Hero-Logo in die vertikale Hero-Komposition aufnehmen.
2. Logo zentral ueber dem Hero-Content positionieren.
3. Navbar auf der Startseite initial ausblenden und aus dem Layoutfluss nehmen.
4. Navbar beim Scrollen wieder als gruene Navbar im bestehenden Format anzeigen.
5. Mobile, Tablet und Desktop gegen Ueberlappungen und Klickbarkeit pruefen.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Wave-Steuerung, Akzeptanzkriterien |
| frontend-eng | Hero-Layout, Navbar-Transition, responsive Umsetzung |
| testing-eng | HTTP-/Asset-Pruefung, CSS-/JS-Syntax, Viewport-Risiken |
| review-eng | Layout- und Interaktionsreview, Regressionen |
| documentation-eng | Iterationsstatus, Rest-Risiken, QA-Ergebnis |

## Task: 5.1 Hero-Logo zentral ueber Content setzen
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Das transparente Logo steht horizontal zentriert im Hero.
- [x] Das Logo steht sichtbar ueber `.hero-grid`, nicht auf gleicher Hoehe mit "Eroeffnung Herbst 2026" oder dem Standort-Kasten.
- [x] Logo, Hero-Text, CTA und Info-Kasten ueberlappen nicht.
- [x] Die Hero-Komposition bleibt auf Desktop, Tablet und Mobile ausgewogen.

**Kontext**:
- Relevante Dateien: `web/templates/pages/home.html`, `web/static/css/style.css`
- Bestehende Selektoren: `header.hero`, `.hero-logo-wrap`, `.hero-logo`, `.hero-grid`

**Output**:
- HTML-Reihenfolge und CSS fuer vertikale Logo-Content-Komposition.

## Task: 5.2 Navbar initial ausblenden
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Beim initialen Laden der Startseite ist die Navbar nicht sichtbar.
- [x] Der Hero beginnt oben im Viewport und wird nicht durch Navbar-Hoehe nach unten gedrueckt.
- [x] Auf Nicht-Hero-Seiten bleibt die Navbar direkt sichtbar.
- [x] Die bestehende gruene Navbar-Gestaltung bleibt unveraendert, wenn sie sichtbar ist.

**Kontext**:
- Relevante Dateien: `web/templates/partials/nav.html`, `web/templates/layouts/base.html`, `web/static/css/style.css`
- Startseite ist erkennbar ueber `header.hero` im `main`.

**Output**:
- CSS-State fuer Hero-Seiten, der die Navbar initial versteckt und aus dem Layoutfluss nimmt.

## Task: 5.3 Navbar beim Scrollen anzeigen
**Priority**: P0
**Blocked by**: 5.2
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Nach geringem Scrollen erscheint die Navbar.
- [x] Beim Zurueckscrollen an den Seitenanfang verschwindet die Navbar wieder.
- [x] Mobile Menu bleibt bedienbar, sobald die Navbar sichtbar ist.
- [x] Anchor-/Hash-Navigation bleibt nutzbar.

**Kontext**:
- Relevante Datei: `web/static/js/main.js`
- Bestehende Smooth-Scroll-Logik muss erhalten bleiben.

**Output**:
- Scroll-State auf `body`, z. B. `nav-visible`, nur fuer Seiten mit Hero.

## Task: 5.4 Responsive und Regression pruefen
**Priority**: P1
**Blocked by**: 5.1, 5.3
**Skillset**: testing-eng, review-eng

**Akzeptanzkriterien**:
- [x] CSS- und JS-Syntax sind plausibel geprueft.
- [x] App liefert Startseite und relevante Assets im Container mit HTTP 200 aus.
- [x] Desktop, Tablet und Mobile wurden gegen die geplanten Positionen geprueft.
- [x] Offene Tooling- oder Sandbox-Grenzen sind dokumentiert.

**Kontext**:
- Falls Playwright lokal verfuegbar ist, Screenshot-Pruefung nutzen.
- Falls nicht verfuegbar, statische und containerinterne Pruefung dokumentieren.

**Output**:
- QA-Status in dieser Iterationsdatei.

## Execution Waves

### Wave 1 — Hero-Komposition
**Skillsets**: product-own, frontend-eng
**Tasks**: 5.1
**Ziel**: Logo in eine zentrale, vertikale Hero-Komposition bringen.

### Wave 2 — Navbar-State
**Skillsets**: frontend-eng
**Tasks**: 5.2, 5.3
**Ziel**: Navbar initial auf Hero-Seiten verstecken und beim Scrollen sichtbar machen.

### Wave 3 — Responsive Absicherung
**Skillsets**: frontend-eng, testing-eng
**Tasks**: 5.4
**Ziel**: Desktop/Tablet/Mobile ohne Ueberlappung und mit bedienbarer Navigation absichern.

### Wave 4 — Review & Abschluss
**Skillsets**: review-eng, documentation-eng, product-own
**Tasks**: 5.4
**Ziel**: Ergebnis gegen Akzeptanzkriterien pruefen, Status und Rest-Risiken dokumentieren.

## Abhaengigkeits-Graph

```text
5.1 Hero-Komposition
5.2 Navbar initial ausblenden
  -> 5.3 Navbar beim Scrollen anzeigen
5.1 + 5.3
  -> 5.4 Responsive/QA/Review
```

## Umsetzungsstatus

### Wave 1 — Hero-Komposition: done
- `web/templates/pages/home.html`: `.hero-logo-wrap` steht nun vor `.hero-grid`.
- `web/static/css/style.css`: Hero nutzt eine vertikale Stack-Komposition mit zentriertem Logo und darunterliegendem Grid.
- Das Logo ist nicht mehr absolut links positioniert und steht nicht auf gleicher Hoehe mit Text oder Standort-Kasten.

### Wave 2 — Navbar-State: done
- `web/templates/layouts/base.html`: `html.js` wird frueh gesetzt, damit Hero-Seiten die Navbar initial ohne sichtbaren Flash verstecken koennen.
- `web/static/css/style.css`: Auf Hero-Seiten wird die Navbar fixed und initial per `transform`/`opacity` ausgeblendet; sichtbar bleibt sie im bestehenden gruenen Format.
- `web/static/js/main.js`: `body.has-hero-nav` und `body.nav-visible` steuern den Scroll-State ab `16px` Scroll-Offset.

### Wave 3 — Responsive Absicherung: done
- Erwartete Kontrollwerte:
  - Desktop 1440x900: Logo-Top 90px, Logo-Breite 288px, Content-Y 414px.
  - Tablet 820x1180: Logo-Top 84px, Logo-Breite 190px, Content-Y 308px.
  - Mobile 390x844: Logo-Top 68px, Logo-Breite 164px, Content-Y 265px.
- Mobile nutzt den bestehenden `@media(max-width:860px)` Breakpoint mit kompakterem Top-Padding und Gap.

### Wave 4 — QA & Review: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- Docker-Container laufen; `/`, `hero-header.jpg`, `logo_courts_transparent.svg`, `style.css` und `main.js` liefern im Container HTTP 200.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
- Playwright-Screenshot wurde nicht ausgefuehrt; `npx --no-install playwright --version` wollte auf die npm Registry zugreifen und scheiterte an DNS/Netzwerkrestriktion.

## Definition of Done
- [x] Transparentes Logo steht zentral ueber dem Hero-Content.
- [x] Logo steht nicht auf gleicher Hoehe mit Hero-Text oder Standort-Kasten.
- [x] Navbar ist beim initialen Startseitenaufruf nicht sichtbar.
- [x] Hero startet ohne Navbar-Offset oben im Viewport.
- [x] Navbar erscheint beim Scrollen im bestehenden gruenen Format.
- [x] Mobile Navigation bleibt nach Scroll sichtbar und bedienbar.
- [x] QA-Status und Rest-Risiken sind dokumentiert.
