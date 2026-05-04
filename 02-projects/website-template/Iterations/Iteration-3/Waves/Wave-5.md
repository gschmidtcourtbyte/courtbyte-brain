# Wave 5 — Responsive für Frontend + Admin

**Skill:** `/frontend-eng`
**Status:** Implemented; Browser-Visual-Verification pending (2026-04-29)
**Blocked by:** Wave 4 (Done)

## Outcome

- `frontend/lib/responsive.ts` mit zentralen Breakpoints und Spacing-Helpers
  angelegt.
- `frontend/lib/hooks/useMediaQuery.ts` SSR-safe implementiert inklusive
  semantischer Hooks (`useIsMobile`, `useIsTablet`, `useIsDesktop`,
  `useIsNarrow`, `useIsAdminNarrow`).
- Öffentliche Site:
  - Navbar nutzt Mobile-Drawer mit Overlay, Scroll-Lock und Link-Close.
  - Hero-Varianten, Feature-/Stats-/Leistungen-/Gallery-/Testimonials-/
    Process-/Footer-Layouts reflowen über Hook-gesteuerte Inline-Styles.
  - Marketing-Seiten `ueber-uns`, `ansprechpartner`, `karriere`, `kontakt`,
    `ressourcen` stacken Grids/Formulare auf Tablet/Mobile.
- Admin:
  - Sidebar wird unter 1024 px zum Drawer; HeaderBar zeigt Burger.
  - Dashboard, Galerie und allgemeine Grids reflowen 4/3/2/1-spaltig je
    Breakpoint.
  - Anfragen und Karriere-Bewerbungen wechseln unter 768 px auf Single-Pane
    Liste/Detail mit Back-Button.
  - Seiten-Tabelle wird unter 768 px zur Card-Liste.
  - Einstellungen/Login reduzieren Padding und Button-Breiten mobil.
- Verification lokal: `npm run lint`, `npm run typecheck`, `npm run build`
  clean; Dev-Server auf `http://127.0.0.1:3001`; HTTP-Smokes für Marketing-
  und Admin-Routen liefern `200`.

## Verification Notes

- Browser-/Screenshot-Verification für 360 / 414 / 768 / 1024 / 1440 px ist
  noch offen, weil in der aktuellen Umgebung kein Playwright/Browser installiert
  ist. Das gehört in Wave 6.

## Ziel

Das gesamte Template — bestehendes Frontend (aus Iteration 1) **und** das
neu gebaute Admin (Waves 1–4) — wird mobil nutzbar gemacht. Dazu werden ein
gemeinsamer `useMediaQuery`-Hook eingeführt und alle relevanten Komponenten
auf reflowfähige Layouts umgestellt.

## Breakpoints

```ts
mobile  : '(max-width: 639px)'      // < 640 px
tablet  : '(min-width: 640px) and (max-width: 1023px)'
desktop : '(min-width: 1024px)'
// zusätzlich für gezielte Stellen:
narrow  : '(max-width: 767px)'      // Burger / Admin-Sidebar-Drawer Schwelle (Frontend)
adminNarrow : '(max-width: 1023px)' // Admin-Sidebar-Drawer Schwelle
```

## Tasks

### 1. `frontend/lib/hooks/useMediaQuery.ts`

- Typed React Hook: `useMediaQuery(query: string): boolean`.
- SSR-safe: erster Render liefert einen Default (Desktop = `true` für
  Desktop-Query, sonst `false`); nach `useEffect`-Mount liest
  `window.matchMedia(query).matches` und re-rendert.
- Listener auf `change` registrieren, beim Unmount entfernen.
- Re-export von semantischen Constants:
  `useIsMobile()`, `useIsTablet()`, `useIsDesktop()`, `useIsAdminNarrow()`.

### 2. Frontend Responsive

**Komponenten-Liste** (alle unter `frontend/components/` aus Iteration 1):

#### Navbar
- Unter 768 px (`useIsAdminNarrow` ist hier nicht passend → eigene
  `useMediaQuery('(max-width: 767px)')`):
  - Logo links bleibt.
  - Nav-Links + Social-Icons + CTA werden in ein Burger-Menü migriert
    (Hamburger-Icon rechts).
  - Click → Slide-in-Panel von rechts (full-height, `width: 80vw`,
    `maxWidth: 320`), zeigt vertikal gestapelte Links.
  - Scroll-Lock auf `body` während Drawer offen ist.
  - Click auf Overlay oder Link schließt Drawer.

#### Hero (alle 3 Varianten)
- **Variante A (Split):** unter 1024 px stackt Text über Bild;
  Bild verkleinert sich, behält Aspect-Ratio.
- **Variante B (Vollbild):** Text-Container hat `maxWidth: 90vw`,
  Padding skaliert, kein Layout-Bruch.
- **Variante C (Floating Cards):** auf Mobile fällt eine der zwei Karten
  weg oder beide stacken untereinander.
- Hero-Padding skaliert: `120px 32px` → `80px 20px` auf Mobile.

#### Features / Stats / Leistungen-Vorschau
- 4-Spalten-Grids → 2 Spalten auf Tablet → 1 Spalte auf Mobile.
- Card-Padding skaliert moderat.

#### Gallery
- 3-Spalten-Grid → 2 Spalten auf Tablet → 1 Spalte auf Mobile.
- Filter-Tabs werden zu horizontalem Scroll-Strip auf Mobile.
- Lightbox: Vor/Zurück-Buttons werden zu Bottom-Bar auf Mobile;
  Thumbnails-Strip horizontal scrollbar.

#### Testimonials-Karussell
- Card-Width auf Mobile: `90vw` (statt `maxWidth: 720`).
- Auto-Advance bleibt; Dots-Indicator unten.

#### Prozess-Sektion (Timeline)
- Horizontale 4-Schritte-Timeline → vertikale Timeline auf Mobile
  (Linie verläuft links, Schritte rechts daneben).

#### Karriere-Page
- Job-Accordion bleibt 1-spaltig; Padding skaliert.
- Bewerbungsformular: 2-Spalten-Grids für Felder werden auf Mobile zu 1 Spalte.

#### Kontaktformular
- 2-Spalten-Layout (Form links, Kontaktdaten rechts) → Stack auf Tablet/Mobile.

#### Ressourcen-Seite
- Tabs (Leistungen/Blog/FAQ/Referenzen) bleiben horizontal mit
  Overflow-Scroll auf Mobile.
- Karten-Grids reflowen wie oben (3 → 2 → 1).

#### Footer
- 4-Spalten-Footer-Nav → 2 Spalten auf Tablet → 1 Spalte auf Mobile.
- Logo + Social-Icons-Block oben.

### 3. Admin Responsive

#### `components/admin/AdminShell.tsx`
- Unter 1024 px: Sidebar wird absoluter Slide-in-Drawer von links
  (`width: 280`, `transform: translateX(-100%)` wenn closed).
- Open-State liegt im Shell-Component (`useState` + Context für
  HeaderBar-Burger-Toggle, oder Lift via Prop).
- Overlay-Backdrop unter dem Drawer (`background: rgba(0,0,0,0.5)`),
  `pointer-events: auto` wenn open.
- Body scroll-lock während Drawer offen.

#### `components/admin/AdminHeaderBar.tsx`
- Unter 1024 px: Burger-Icon links neben Section-Titel; Click toggelt
  Sidebar-Drawer.
- Notification-Bell + Avatar bleiben rechts.

#### `components/admin/AdminSidebar.tsx`
- Logo-Header, Nav-List, Footer rendern unverändert.
- Auto-close des Drawers bei Nav-Click auf < 1024 px (UX: Klick auf
  Section schließt das Drawer, damit der Content sichtbar wird).

#### Dashboard (`/admin`)
- Stats-Grid: `repeat(4, 1fr)` → `repeat(2, 1fr)` auf Tablet → 1 Spalte
  auf Mobile.
- Charts-Row: `2fr 1fr` → Stack (BarChart oben, Donut unten) auf < 1024 px.
- Recent-Listen: `1fr 1fr` → Stack auf Tablet/Mobile.
- BarChart bleibt funktional (Bars schmaler), DonutChart-Legende rückt
  unter den SVG-Donut auf Mobile.

#### Anfragen-Page
- Master-Detail-Grid `1fr 1.4fr` → bei < 768 px:
  - Wenn `selected === null`: nur Liste full-width.
  - Wenn `selected != null`: nur Detail full-width mit Back-Button im
    Detail-Header (Pfeil-Icon links, ersetzt das `×`).
- Auf Tablet: Liste schmaler, Detail überdeckt nicht.

#### Karriere-Page
- Tab-Bar bleibt horizontal scrollbar auf sehr schmalen Viewports.
- "Bewerbungen"-Tab: gleiche Master-Detail-Logik wie Anfragen.
- "Stellen"-Tab: Job-Cards werden zu vertikal gestackten Layouts
  unter 768 px (Title oben, Tags + Counts gestapelt, Buttons full-width).

#### Seiten-Page (`/admin/seiten`)
- 5-Spalten-Tabelle auf Desktop bleibt.
- Unter 768 px: Tabelle wird zu Card-Liste — pro Eintrag eine Card mit
  Name (groß), URL (kleinerer Monospace), StatusBadge, Aufrufe + Datum
  als Meta-Zeile.
- Header-Row der Tabelle wird auf Mobile ausgeblendet.

#### Galerie-Page
- 3-Spalten-Grid → 2 Spalten auf Tablet → 1 Spalte auf Mobile.
- Upload-Zone bleibt full-width, Padding skaliert.

#### Einstellungen-Page
- `maxWidth: 720` bleibt; Card-Padding wird auf Mobile leicht reduziert
  (`24` → `20`); Inputs sind ohnehin `width: 100%`.
- Save-Button wird auf Mobile full-width.

#### Login-Page (`/admin/login`)
- Bereits zentriert mit `maxWidth: 420`; nur Padding-Anpassung
  (außer 24 px → 16 px auf Mobile).

### 4. Globale Helpers

- `frontend/lib/responsive.ts`: zentrale Breakpoint-Strings + `clamp()`-
  Spacing-Helpers (z. B. `clamp(20px, 4vw, 32px)` für Section-Padding).
- Verhindert Magic-Strings in den Komponenten.

## Akzeptanzkriterien

- [x] `useMediaQuery`-Hook implementiert + SSR-safe + alle Komponenten,
      die ihn nutzen, hydrieren ohne Console-Warnings.
- [ ] Frontend rendert ohne horizontalen Scroll bei
      360 / 414 / 768 / 1024 / 1440 px für alle Routen
      (`/`, `/ueber-uns`, `/karriere`, `/kontakt`, `/ressourcen`).
- [x] Burger-Menü auf Frontend funktioniert (öffnen, schließen via
      Overlay, schließen via Link-Click).
- [x] Admin-Sidebar wird unter 1024 px zum Drawer; Burger in HeaderBar
      öffnet/schließt; Overlay schließt; Nav-Click schließt.
- [x] Anfragen + Karriere-Bewerbungs-Master-Detail zeigt unter 768 px
      nur eine Pane; Detail-Header hat Back-Button.
- [x] Seiten-Tabelle ist unter 768 px Card-Liste; alle Daten weiterhin
      sichtbar.
- [x] Alle Grids (Stats, Charts, Recent, Galerie, Frontend-Sektionen)
      reflowen wie spezifiziert.
- [x] Tap-Targets ≥ 40 × 40 px für alle interaktiven Elemente
      (Buttons, Nav-Items, Drawer-Toggle, Tabs).
- [x] `npm run build` + `npm run lint` clean.

## Out of Scope (in Wave 5)

- A11y-Audit (Polish-Iteration)
- Performance-Tuning (Bundle-Splitting, Image-Optimierung) — Polish-Iteration
- Animation-Anpassungen für Reduced-Motion — Polish-Iteration
- Touch-Gesten (Swipe für Lightbox, Drawer-Swipe-to-Close) — optional, später

## Kontext

- **Bestehendes Frontend:** alle Komponenten unter `frontend/components/`
  + `frontend/app/(main)/*/page.tsx` (Routen aus Iteration 1)
- **Admin-Komponenten** aus Waves 1–4 unter `frontend/components/admin/`
  + `frontend/app/admin/(authed)/*/page.tsx`
- **Theme-Tokens** in `lib/theme.ts` — keine neuen Tokens nötig
- **Design-Bundle:** Das Original-Design ist nicht responsive; mobile
  Layouts sind eine eigene Design-Entscheidung dieser Wave und müssen
  nicht 1:1 zum Bundle passen — aber Farben, Schriften, Border-Radii,
  Hover-Idiome bleiben gleich.
