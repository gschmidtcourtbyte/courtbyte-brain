# Iteration 3 — Admin-CMS Optik + Responsive (Frontend & Admin)

**Status:** Planned
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-04-29

## Ziel

Zwei Liefergegenstände, die zusammen das Template "demoreif" machen:

1. **Admin-CMS-Optik** unter `/admin`: komplette UI-Shell 1:1 aus dem
   Claude-Design-Bundle (`Backend CMS.html`) mit allen sechs Sections und
   Mock-Daten. Persistenz, Uploads und Mail folgen in späteren Iterationen.
2. **Responsive Layout** für **Frontend & Admin**: das gesamte Template wird
   mobil nutzbar. Aktuell ist nichts responsive — Hero, Sections, Gallery,
   Footer, Navbar (Burger-Menü) im Frontend müssen angepasst werden, ebenso
   die neue Admin-Shell (Sidebar als Drawer, Master-Detail-Stacking,
   Tabellen-Scroll, Grid-Reflows).

## Scope-Shift

Der ursprüngliche Projektplan hatte Iteration 3 = "Frontend ausfeilen
(Responsive/A11y/SEO/Performance/i18n)" und CMS-UI ab Iteration 5/6. Die
Reihenfolge wird gedreht: CMS-Optik wird vorgezogen. Aus dem ursprünglichen
"Frontend-Polish" wird **nur Responsive** vorgezogen und in dieser Iteration
mit der Admin-Optik gebündelt — A11y, SEO, Performance, i18n bleiben in der
späteren Polish-Iteration. Persistenz-Iterationen bleiben weiterhin nach
diesem Schritt eingeplant.

## Scope

**In:**
- Admin-Theme-Tokens in `frontend/lib/theme.ts` (Dark-only-Namespace)
- Auth-Gate via Next.js Route-Group `app/admin/(authed)/layout.tsx`
- Login-Page Redesign (`/admin/login` mit Blob-Hintergrund + zentrierter Card)
- Sidebar + HeaderBar Komponenten unter `components/admin/`
- Logo-Slot in der Sidebar (SVG-Slot mit Initialen-Fallback aus `site-config`)
- Sechs Section-Routen mit Mock-Daten:
  - `/admin` (Dashboard) — Stats, BarChart, DonutChart, Recent-Listen
  - `/admin/anfragen` — Master-Detail Inbox mit Status-Toggle + Antwort-Form
  - `/admin/karriere` — Tabs Bewerbungen + Stellenanzeigen
  - `/admin/seiten` — Tabelle (Seite/URL/Status/Aufrufe/Geändert)
  - `/admin/galerie` — Upload-Zone + 3-spaltiges Bild-Grid
  - `/admin/einstellungen` — Settings-Form (Website + Social Media)
- Geteilte Atoms: StatusBadge, AdminCard, BarChart, DonutChart
- Mock-Daten in `frontend/lib/admin-mock.ts` (1:1-Port der Design-Arrays)
- Logout-Button in Sidebar-Footer hängt am bestehenden `/admin/api/logout`
- **Responsive für das gesamte Template** (Frontend + Admin):
  - Frontend: Navbar als Burger-Menü auf Mobile, Hero/Sections/Stats/Gallery/
    Karriere/Kontakt/Footer reflowen sauber auf Tablet (640–1023 px) und
    Mobile (< 640 px).
  - Admin: Sidebar wird auf < 1024 px zu einem Slide-in-Drawer mit Burger-
    Toggle in der HeaderBar; Stats/Charts/Recent-Listen stacken; Master-
    Detail-Views (Anfragen/Bewerbungen) zeigen auf Mobile **entweder Liste
    oder Detail**, nicht nebeneinander; Seiten-Tabelle wird unter < 768 px
    zur Card-Liste.
  - Geteilter `useMediaQuery`-Hook in `frontend/lib/hooks/useMediaQuery.ts`
    als Pattern für CSS-in-JS-Breakpoints.

**Out:**
- Persistenz (Postgres-CRUD für Inquiries/Applications/Jobs/Pages/Media/Settings)
- Datei-Uploads (Galerie + CV-Download)
- Mail-Versand
- Light-Variante des Admin-UI (Admin bleibt Dark-only)
- A11y-Audit, SEO-Optimierung, Performance-Tuning, i18n — bleiben in der
  späteren Polish-Iteration
- Responsive-Edge-Cases unter 360 px Breite (Mainstream-Ziel: ab 360 px aufwärts)

## Entscheidungen

- **Dark-only** — Kein Light-Modus für Admin. Frontend behält weiterhin
  Light/Dark-Tweak. Begründung: Match zum Design, geringer Aufwand,
  konsistente CMS-Optik bei allen Kunden.
- **Echte Routen statt SPA-State** — Pro Section eine eigene App-Router-Route.
  Bookmarkable, Server-Components-fähig in späteren Iterationen,
  Next-idiomatisch. Abweichung von der HTML-Prototyp-Struktur, identisches
  visuelles Ergebnis.
- **Logo-Slot mit Platzhalter** — Sidebar hat einen SVG-Slot. Default
  rendert Initialen aus `site-config.ts` (`MF`-Style). Kunde kann später
  ein echtes Logo via `public/admin-logo.svg` einsetzen, ohne Komponenten
  anzufassen.
- **Theme-Tokens** — Admin-Farben kommen ausschließlich aus `lib/theme.ts`
  unter neuem `admin`-Namespace. `primary` und `accent` werden aus dem
  bestehenden Theme wiederverwendet — Kunde stellt seine Farben weiterhin
  nur einmal ein.
- **Responsive über JS-Hook statt Media-Query-CSS** — Konsequenz aus dem
  CSS-in-JS-Inline-Style-Ansatz: ein `useMediaQuery(query)`-Hook liefert
  Booleans, Komponenten wählen darauf basierend Style-Objekte. Drei feste
  Breakpoints: `mobile` < 640 px, `tablet` 640–1023 px, `desktop` ≥ 1024 px.
  Kein Tailwind, kein CSS-Framework — passt zur Rules.md.
- **Admin-Mobile-Master-Detail** — Anfragen/Bewerbungen zeigen auf Mobile
  entweder die Liste **oder** das Detail (Detail überdeckt die Liste,
  Back-Button im Detail-Header). Auf Desktop bleibt das Two-Pane-Layout.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Admin-Theme-Tokens, Auth-Gate Layout, Login-Redesign | Done |
| Wave 2 | frontend-eng | Sidebar, HeaderBar, geteilte Atoms, Mock-Daten-Layer | Done |
| Wave 3 | frontend-eng | Dashboard-Page + Anfragen-Page (Master-Detail) | Done |
| Wave 4 | frontend-eng | Karriere (Tabs), Seiten, Galerie, Einstellungen | Done |
| Wave 5 | frontend-eng | Responsive für Frontend + Admin (Burger-Menü, Drawer, Reflows) | In Review |
| Wave 6 | testing-eng / review-eng | Railway Deploy, Smoke-Tests Desktop+Mobile, Visual-Review, Polish-Backlog | Planned |

**Hinweis Waves 1–4:** Diese bauen ausschließlich Desktop-Layouts. Mobile-
Anpassungen werden bewusst in Wave 5 gebündelt, damit die Sub-Agenten
fokussiert auf Pixel-Treue zum Design arbeiten können und das Responsive-
Pattern dann konsistent über alle Komponenten hinweg eingeführt wird.

## Acceptance Criteria

- [ ] Alle sechs Section-Routen + Login renderen 1:1 wie `Backend CMS.html`
      (Farben, Spacings, Schriften, Hover-Transitions, Animationen).
- [ ] Auth-Gate: ohne gültige Session → Redirect zu `/admin/login`;
      mit Session → Sidebar + Content.
- [ ] Logout-Button im Sidebar-Footer ruft `/admin/api/logout` und
      navigiert zurück zur Login-Page.
- [ ] Status-Toggles in Anfragen/Karriere ändern lokalen State
      (kein POST, kein Backend-Call).
- [ ] Firmenname und Logo-Initialen kommen aus `site-config.ts`,
      nicht hardcoded in Komponenten.
- [ ] Logo-Slot in der Sidebar: Component akzeptiert SVG-Override über Prop
      oder durch `public/admin-logo.svg`-Existenz, fällt sonst auf Initialen
      zurück.
- [ ] Admin-Theme-Tokens leben in `lib/theme.ts` unter `admin`-Namespace;
      keine Magic-Hex-Codes in Komponenten.
- [ ] Mock-Daten zentral in `lib/admin-mock.ts`; Komponenten importieren von
      dort, nicht inline.
- [ ] TypeScript strict bleibt grün; `npm run lint` clean.
- [ ] Railway `staging` Deploy erfolgreich; alle `/admin/*`-Routen liefern
      `200` mit gültigem Session-Cookie und Redirect zu `/admin/login` ohne.
- [ ] **Responsive Frontend:**
  - Navbar zeigt unter 768 px ein Burger-Menü; Click öffnet Slide-in-Panel
    mit allen Nav-Links + Social-Links.
  - Hero, Features, Stats, Leistungen, Gallery, Testimonials, Process,
    Karriere-Form, Kontaktformular und Footer reflowen ohne horizontalen
    Scroll auf 360, 414, 768, 1024 und 1440 px Breite.
- [ ] **Responsive Admin:**
  - Sidebar ist unter 1024 px ein Slide-in-Drawer; Burger in der
    HeaderBar öffnet/schließt; Overlay schließt bei Klick außerhalb.
  - Stats-Grid: 4 → 2 → 1 Spalten; Charts-Row stackt; Recent-Listen stacken.
  - Anfragen + Karriere/Bewerbungen: Master-Detail wird auf Mobile zu
    Single-Pane (Liste oder Detail mit Back-Button).
  - Seiten-Tabelle: Card-Layout auf Mobile (< 768 px) mit allen Daten
    pro Eintrag.
  - Galerie-Grid: 3 → 2 → 1 Spalten.
  - Einstellungen-Form bleibt nutzbar (Card-Padding mobile-tauglich).
- [ ] `useMediaQuery`-Hook in `frontend/lib/hooks/useMediaQuery.ts`
      typisiert + von Frontend- und Admin-Komponenten genutzt.

## Verification

Wird nach Wave 5 ergänzt — gleiches Format wie Iteration-2.md.

## Risiken

- **Visuelle Drift** — Inline-Styles in vielen Komponenten erhöhen das
  Risiko, dass kleinere Token-Fehler übersehen werden. Mitigation:
  Wave 5 macht Side-by-Side-Visual-Review gegen das HTML-Original.
- **Auth-Gate-Race** — `/admin/api/me` läuft client-side; bis die Antwort
  da ist, könnte Content kurz blitzen. Mitigation: Skeleton-State im
  Layout, kein Content-Render bevor Auth-Status bekannt.
- **Mock vs. Real-API-Drift** — Wenn Mock-Datenstrukturen später nicht
  zur Backend-Response passen, muss Iteration 4+ refactoren. Mitigation:
  Mock-Typen in `lib/admin-mock.ts` als TS-Interfaces definieren, die
  später als Contract für die Backend-Endpoints dienen.
- **SSR-Hydration mit `useMediaQuery`** — Auf dem Server gibt es kein
  `window.matchMedia`, der erste Client-Render kann anders aussehen als
  der Server-Render und Hydration-Mismatches erzeugen. Mitigation:
  Hook liefert beim ersten Render einen definierten Default (Desktop)
  und re-rendert nach Mount; betroffene Komponenten dürfen Layout nicht
  vom Hook-Wert beim ersten Paint abhängig machen, sondern sollen den
  Desktop-Default rendern und progressive enhancement betreiben.
- **Scope-Risiko Wave 5** — Responsive für das gesamte Template ist die
  größte Wave dieser Iteration. Mitigation: klare Sub-Tasks pro
  Komponente in Wave-5.md, klare Breakpoint-Definitionen, eine
  Smoke-Liste der Viewports in Wave 6 (360/414/768/1024/1440).
