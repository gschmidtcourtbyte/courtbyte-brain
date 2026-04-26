# Wave 3: Navbar Layout

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, review-eng
**Quelle:** `plan/iterations/iteration-8.md`

## Legacy-Inhalt

### Wave 3 — Navbar Layout
**Skillsets**: frontend-eng, review-eng
**Tasks**: 8.3, 8.4
**Ziel**: Navbar-Inhalte an Webseitenraster ausrichten und responsive Bedienbarkeit sichern.

## Legacy-Status

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
