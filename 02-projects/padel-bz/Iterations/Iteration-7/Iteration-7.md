# Iteration 7: Hero-Logo-Scale & Sticky Navbar-Logo

## Ziel
Das transparente Logo im Hero wird gegenueber Iteration 6 um ca. 20% vergroessert. Beim Scrollen und Erscheinen der Navbar bleibt das transparente Logo in kleinerer Form zentriert an der Navbar haengen. Das alte nicht-transparente Navbar-Logo wird entfernt.

## Ausgangslage
- Iteration 6:
  - Hero-Logo ist zentral ueber dem Hero-Content positioniert.
  - Hero-Logo-Groesse: Desktop `clamp(230px,24vw,360px)`, Mobile `clamp(172px,48vw,230px)`.
  - Navbar ist initial auf der Startseite ausgeblendet und erscheint beim Scrollen.
  - Navbar enthaelt noch das alte `logo_courts.svg` links.
- Kundenfeedback:
  - Groesse und Richtung gefallen.
  - Hero-Logo darf noch ca. 20% groesser werden.
  - Beim Scrollen soll das transparente Logo kleiner werden und zentriert an/bei der Navbar kleben.
  - Es muss nicht komplett in die Navbar hineingehen; die Zentrierung soll erhalten bleiben.

## Scope
- Navbar-Markup: `web/templates/partials/nav.html`
- Hero-/Navbar-CSS: `web/static/css/style.css`
- Scroll-State-JS nur falls zusaetzlicher State noetig wird: `web/static/js/main.js`
- Plan-/Statusdokumentation: `plan/iterations/iteration-7.md`
- Keine neuen Assets, keine Backend-Aenderungen, keine Textaenderungen an Navigation oder Hero.

## Produktentscheidung
Fuer die Umsetzung wird ein zweites transparentes Logo als zentrierter Navbar-/Scroll-Anker verwendet. Das Hero-Logo bleibt gross im Hero-Stack. Beim Scroll-State wird das kleine Navbar-Logo eingeblendet und zentriert fixiert. Das erzeugt die gewuenschte Wahrnehmung von "kleiner werden und an der Navbar kleben", ohne riskantes DOM-Reparenting oder scrollbasierte Pixelanimation.

## Critical Path
1. Hero-Logo um ca. 20% vergroessern und Hero-Abstaende pruefen.
2. Altes Navbar-Logo entfernen.
3. Transparentes kleines Logo als zentrierten Navbar-Anker einfuegen.
4. Einblend-/Klebeverhalten an bestehenden `nav-visible` Scroll-State koppeln.
5. Desktop/Mobile gegen Ueberlappungen, Klickbarkeit und Zentrierung pruefen.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Wave-Steuerung, Akzeptanzkriterien |
| frontend-eng | Navbar-Markup, Logo-Skalierung, CSS-Positionierung, responsive Verhalten |
| testing-eng | Syntaxchecks, HTTP-/Asset-Pruefung, Viewport-Kontrollwerte |
| review-eng | Review von CSS-Spezifitaet, Klickbarkeit, Mobile-Menue und Layout-Risiken |
| documentation-eng | Status, QA-Ergebnis und Rest-Risiken im Iterationsplan |

## Task: 7.1 Hero-Logo um ca. 20% vergroessern
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Hero-Logo ist ca. 20% groesser als in Iteration 6.
- [x] Hero-Logo bleibt horizontal zentriert.
- [x] Logo steht weiterhin klar ueber dem Hero-Content.
- [x] Hero-Text, CTA und Info-Kasten werden nicht ueberdeckt.
- [x] Mobile Darstellung bleibt ausgewogen.

**Kontext**:
- Iteration-6-Werte:
  - Desktop: `clamp(230px,24vw,360px)`
  - Mobile: `clamp(172px,48vw,230px)`
- Zielwerte als Startpunkt:
  - Desktop: `clamp(276px,28.8vw,432px)`
  - Mobile: `clamp(206px,57.6vw,276px)`
- Je nach Viewport kann ein konservativer Max-Wert noetig sein, damit Hero-Content nicht nach unten gequetscht wird.

**Output**:
- CSS-Aenderung in `.hero-logo`.

## Task: 7.2 Altes Navbar-Logo entfernen
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] `logo_courts.svg` wird nicht mehr als sichtbares Navbar-Logo verwendet.
- [x] Navigation bleibt semantisch bedienbar.
- [x] Startseiten-Link bleibt ueber das neue transparente Scroll-Logo oder einen barrierearmen Link erreichbar.
- [x] Desktop- und Mobile-Nav behalten ihre bestehenden Links und CTA.

**Kontext**:
- Aktuelles Markup in `web/templates/partials/nav.html`:
  - `<a href="/" class="brand"><img src="/static/img/logo_courts.svg" ...></a>`
- Das alte Logo darf aus der Navbar entfernt werden; das Asset selbst wird nicht geloescht, weil es weiterhin an anderen Stellen genutzt werden kann.

**Output**:
- Markup-Anpassung in `web/templates/partials/nav.html`.

## Task: 7.3 Kleines transparentes Navbar-Logo einfuehren
**Priority**: P0
**Blocked by**: 7.2
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [x] Kleines transparentes Logo nutzt `logo_courts_transparent.svg`.
- [x] Logo ist horizontal zentriert zur Viewport-/Navbar-Mitte.
- [x] Logo erscheint beim Scrollen zusammen mit der Navbar.
- [x] Logo wirkt an/bei der Navbar klebend und darf leicht ueber die Navbar-Kante hinausragen.
- [x] Logo blockiert keine Nav-Links, CTA oder Mobile-Menue-Bedienung.

**Kontext**:
- Bevorzugte Umsetzung:
  - Neues Element z. B. `.nav-scroll-logo` als zentrierter Link in der Navbar.
  - Positionierung via `position:absolute; left:50%; transform:translateX(-50%)`.
  - Kleine Groesse z. B. Desktop `clamp(96px,9vw,150px)`, Mobile `clamp(82px,24vw,116px)`.
  - Sichtbarkeit an `body.nav-visible` koppeln.
- Es muss keine perfekte Morph-Animation sein; wichtig ist der visuelle Uebergang grosses Hero-Logo -> kleines zentriertes Navbar-Logo.

**Output**:
- Navbar-Markup und CSS fuer `.nav-scroll-logo`.

## Task: 7.4 Scroll-Uebergang und Zentrierung absichern
**Priority**: P1
**Blocked by**: 7.1, 7.3
**Skillset**: frontend-eng, review-eng

**Akzeptanzkriterien**:
- [x] Beim initialen Startseitenaufruf ist die Navbar weiterhin ausgeblendet.
- [x] Beim Scrollen erscheint die Navbar inklusive kleinem transparentem Logo.
- [x] Das kleine Logo bleibt beim Scrollen zentriert.
- [x] Beim Zurueckscrollen nach oben verschwindet die Navbar wieder.
- [x] Nicht-Hero-Seiten behalten eine direkt sichtbare Navbar und ein sinnvolles Logo-/Home-Link-Verhalten.

**Kontext**:
- Bestehender Scroll-State aus Iteration 6:
  - `body.has-hero-nav`
  - `body.nav-visible`
  - `navRevealOffset = 16`
- Falls Nicht-Hero-Seiten ohne sichtbares Logo unklar wirken, kann `.nav-scroll-logo` dort dauerhaft sichtbar bleiben.

**Output**:
- CSS-/JS-Feinschliff, falls noetig.

## Task: 7.5 QA & Regression
**Priority**: P1
**Blocked by**: 7.4
**Skillset**: testing-eng, review-eng

**Akzeptanzkriterien**:
- [x] JS-Syntaxcheck besteht, falls JS geaendert wurde.
- [x] CSS-Brace-Check besteht.
- [x] App und relevante Assets liefern im Container HTTP 200.
- [x] Desktop/Tablet/Mobile-Kontrollwerte sind dokumentiert.
- [x] Navigation ist bedienbar: Links, CTA, Mobile-Menue, Home-Link.
- [x] Tooling- oder Sandbox-Grenzen sind dokumentiert.

**Output**:
- QA-Status in dieser Iterationsdatei.

## Execution Waves

### Wave 1 — Hero-Logo-Scale
**Skillsets**: product-own, frontend-eng
**Tasks**: 7.1
**Ziel**: Das zentrale Hero-Logo ca. 20% groesser darstellen, ohne den Hero-Content zu ueberdecken.

### Wave 2 — Navbar-Logo-Markup
**Skillsets**: frontend-eng
**Tasks**: 7.2, 7.3
**Ziel**: Altes Navbar-Logo entfernen und transparentes zentriertes Scroll-Logo einfuehren.

### Wave 3 — Scroll-/Responsive-Verhalten
**Skillsets**: frontend-eng, review-eng
**Tasks**: 7.4
**Ziel**: Logo klebt beim Scrollen zentriert an der Navbar; Links und Mobile-Menue bleiben bedienbar.

### Wave 4 — QA & Dokumentation
**Skillsets**: testing-eng, review-eng, documentation-eng, product-own
**Tasks**: 7.5
**Ziel**: Ergebnis pruefen, Risiken dokumentieren und Iteration abschliessen.

## Abhaengigkeits-Graph

```text
7.1 Hero-Logo-Scale
7.2 Altes Navbar-Logo entfernen
  -> 7.3 Transparentes Navbar-Logo einfuehren
7.1 + 7.3
  -> 7.4 Scroll/Responsive
  -> 7.5 QA/Doku
```

## Umsetzungsstatus

### Wave 1 — Hero-Logo-Scale: done
- `.hero-logo` wurde von `clamp(230px,24vw,360px)` auf `clamp(276px,28.8vw,432px)` vergroessert.
- Mobile `.hero-logo` wurde von `clamp(172px,48vw,230px)` auf `clamp(206px,57.6vw,276px)` vergroessert.
- Das Hero-Logo bleibt im bestehenden zentrierten Stack ueber dem Hero-Content.

### Wave 2 — Navbar-Logo-Markup: done
- Das alte sichtbare Navbar-Logo `logo_courts.svg` wurde aus `web/templates/partials/nav.html` entfernt.
- Neuer Home-Link `.nav-scroll-logo` nutzt `logo_courts_transparent.svg`.
- Nav-Links, CTA und Mobile-Menue-Markup bleiben erhalten.

### Wave 3 — Scroll-/Responsive-Verhalten: done
- `.nav-scroll-logo` ist absolut zur Navbar-/Viewport-Mitte positioniert.
- Auf Hero-Seiten bleibt das kleine Logo mit der Navbar initial ausgeblendet und erscheint mit `body.nav-visible`.
- Auf Nicht-Hero-Seiten bleibt die Navbar direkt sichtbar; das transparente Logo dient dort als zentrierter Home-Link.
- Nav-Links und Mobile-Toggle liegen mit `z-index:102` ueber dem Logo, damit Bedienung bei engen Breiten nicht blockiert wird.
- Unter `1040px` werden die Textlinks ausgeblendet, damit das zentrierte Logo nicht mit der Linkgruppe kollidiert.

### Wave 4 — QA & Dokumentation: done
- `node --check web/static/js/main.js`: bestanden.
- CSS-Brace-Check: bestanden.
- Docker-Container laufen; `/`, `logo_courts_transparent.svg`, `style.css` und `main.js` liefern im Container HTTP 200.
- Ausgelieferte CSS-Datei enthaelt `.nav-scroll-logo`, `clamp(276px,28.8vw,432px)` und `clamp(206px,57.6vw,276px)`.
- Kontrollwerte:
  - Desktop 1440x900: Hero-Logo 415px, Content-Y 541px, Navbar-Logo 130px.
  - Narrow 960x900: Hero-Logo 276px, Content-Y 402px, Navbar-Logo 96px.
  - Tablet 820x1180: Hero-Logo 276px, Content-Y 394px, Navbar-Logo 116px.
  - Mobile 390x844: Hero-Logo 225px, Content-Y 326px, Navbar-Logo 94px.
- Host-Curl auf `127.0.0.1:8081` war aus der Sandbox nicht erreichbar, obwohl Docker den Port mapped.
- `go test ./...` konnte lokal nicht laufen, weil `go` auf dem Host nicht installiert ist.
- Playwright-Screenshot wurde nicht ausgefuehrt; `npx --no-install playwright --version` wollte auf die npm Registry zugreifen und scheiterte an DNS/Netzwerkrestriktion.

## Definition of Done
- [x] Hero-Logo ist ca. 20% groesser und weiterhin zentriert.
- [x] Altes `logo_courts.svg` ist aus der sichtbaren Navbar entfernt.
- [x] Kleines transparentes Logo erscheint beim Scrollen zentriert an/bei der Navbar.
- [x] Navbar bleibt initial auf der Startseite ausgeblendet.
- [x] Navigation, CTA und Mobile-Menue bleiben bedienbar.
- [x] Nicht-Hero-Seiten behalten eine nutzbare Navbar.
- [x] QA-Status und Rest-Risiken sind dokumentiert.
