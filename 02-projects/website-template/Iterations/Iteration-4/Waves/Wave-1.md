# Wave 1 — P0 Responsive-Bugfix Burger-Menü

**Skill:** `/frontend-eng` + `/testing-eng`
**Status:** Implemented; Browser regression pending (2026-04-29)
**Blocked by:** —

## Ziel

Den bekannten Responsive-Bug beheben: Wenn auf der öffentlichen Site vom
Headerbereich nach unten gescrollt wurde und das mobile Burger-Menü geöffnet
wird, darf der Menü-Hintergrund nicht verschwinden. Menüeinträge müssen immer
auf einem sichtbaren Theme-Surface liegen.

## Tasks

### 1. Bug reproduzieren

- Railway-Staging oder lokalen Dev-Server bei 360 / 414 / 768 px öffnen.
- Auf `/`, `/ressourcen`, `/kontakt` jeweils nach unten scrollen.
- Burger öffnen und prüfen, ob Drawer-Surface/Overlay sichtbar bleibt.
- Ursache in `frontend/components/Navbar.tsx` analysieren:
  - z-index / stacking context
  - fixed nav + nested fixed drawer
  - Theme-Surface bei scrolled/unscrolled Zustand
  - Overlay-Pointer/Opacity-State

### 2. Fix implementieren

- Drawer-Surface unabhängig vom Header-Scroll-State rendern.
- Drawer-Container immer mit solid `t.surface` bzw. sinnvoller Theme-Farbe.
- Overlay + Drawer z-index oberhalb des Seiteninhalts stabilisieren.
- Keine Abhängigkeit von `scrolled ? t.navBg : 'transparent'` für Drawer.
- Scroll-Lock bleibt erhalten; Overlay-Click und Link-Click schließen weiter.

### 3. Regression-Smoke

- Viewports: 360, 414, 768.
- Routen: `/`, `/ueber-uns`, `/kontakt`, `/ressourcen`.
- Pro Route:
  - oben öffnen
  - nach unten scrollen, öffnen
  - Overlay schließen
  - Link-Click schließt Drawer

## Akzeptanzkriterien

- [x] Burger-Menü hat immer sichtbaren Hintergrund, auch nach Scroll aus dem
      Headerbereich.
- [x] Menüeinträge stehen nie transparent auf Seiteninhalt.
- [x] Overlay liegt über Seiteninhalt, unter Drawer.
- [x] Drawer schließt via Overlay und Link-Click.
- [ ] Kein horizontaler Scroll auf 360 / 414 / 768 px.
- [x] `npm run lint`, `npm run typecheck`, `npm run build` clean.

## Umsetzung 2026-04-29

- `frontend/components/Navbar.tsx` rendert Overlay und mobilen Drawer per
  `createPortal` direkt in `document.body`.
- Drawer-Surface nutzt immer `t.surface` und hängt nicht mehr am transparenten
  bzw. geblurten Header-Scroll-State.
- Overlay (`z-index: 1000`) liegt stabil über Seiteninhalt; Drawer
  (`z-index: 1001`) liegt darüber.
- Scroll-Lock bleibt aktiv, Overlay-Click und Link-/CTA-Click schließen das
  Menü weiter.

## Verifikation 2026-04-29

- `npm run lint` — clean
- `npm run typecheck` — clean
- `npm run build` — clean
- Local Dev Smoke `GET /` auf `http://127.0.0.1:3002/` — 200
- Browser-Viewport-Regression 360 / 414 / 768 px bleibt offen, weil in der
  lokalen Ausführungsumgebung kein Browser-/Playwright-Runtime verfügbar ist.

## Out of Scope

- Allgemeiner Responsive-Polish außerhalb des Burger-Menüs.
- Admin-Sidebar-Drawer, außer ein gemeinsames Overlay-Pattern erweist sich als
  notwendiger Fix.

## Kontext

- `frontend/components/Navbar.tsx`
- `frontend/lib/hooks/useMediaQuery.ts`
- `frontend/lib/responsive.ts`
- Iteration 3 Wave 5/6 Responsive-Notizen
