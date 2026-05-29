# Wave 2 — Sidebar, HeaderBar, geteilte Atoms, Mock-Daten-Layer

**Skill:** `/frontend-eng`
**Status:** Done (2026-04-29)
**Blocked by:** Wave 1 (Done)

## Outcome

- Marketing-Layout-Konflikt gelöst: öffentliche Seiten liegen in
  `app/(marketing)/`, Root-Layout bleibt minimal und `/admin` rendert ohne
  Navbar/Footer.
- `AdminSessionProvider` eingeführt; `/admin/api/me` wird einmal im authed
  Layout geladen und per `useAdminSession()` an Dashboard-Stub, Sidebar und
  HeaderBar weitergegeben.
- `AdminShell`, `AdminSidebar`, `AdminHeaderBar` und geteilte Atoms
  `AdminCard`, `StatusBadge`, `BarChart`, `DonutChart` angelegt.
- `frontend/lib/admin-mock.ts` enthält typisierte Mock-Daten für Dashboard,
  Anfragen, Bewerbungen, Jobs, Seiten, Galerie, Einstellungen sowie Nav- und
  Titel-Mapping.
- Admin-Hover-/Motion-Globals (`fadeUp`, `pulse`, `card-hover`,
  `sidebar-item`, `table-row`, `btn`, `tab-btn`) in `app/admin/admin.css`.
- Wave-5-Prep: Sidebar-/Header-Signaturen akzeptieren Drawer-/Menu-Callbacks.
- Verification: `npm run lint`, `npm run typecheck`, `npm run build` clean.

## Übergabe-Notizen aus Wave 1

- **Marketing-Layout-Konflikt:** Das Root-`app/layout.tsx` rendert weiterhin
  Navbar/Footer der öffentlichen Site auch um `/admin`-Routen. Wave 2 muss
  das auflösen — bevorzugte Lösung: Marketing-Routen in eine Route Group
  `app/(marketing)/` ziehen, die ihr eigenes Layout mit Navbar/Footer hat;
  Root-Layout bleibt minimal (TweaksProvider, Fonts, html/body). Alternative:
  Conditional-Rendering im Root-Layout per `usePathname()` — pragmatischer,
  aber weniger sauber.
- **Doppelter `/admin/api/me`-Fetch:** Layout fetcht für Auth-Gate, Stub-Page
  fetcht erneut für die Email/Role-Anzeige. Wave 2 führt einen
  `AdminSessionProvider`-Context ein, der die Session einmal im Layout fetcht
  und via `useAdminSession()` an Sidebar-Footer (Email + Logout), HeaderBar
  (Avatar) und Sections weiterreicht.
- **`tweaks.logoInitials`** aus TweaksProvider funktioniert (Root-Layout-mounted).

## Ziel

AdminShell (Sidebar + HeaderBar) bauen, geteilte Atoms (StatusBadge, AdminCard,
BarChart, DonutChart) implementieren, zentrale Mock-Daten-Datei anlegen.
Nach dieser Wave kann jede Section-Route ihre Inhalte einfach in den Shell
einsetzen.

## Tasks

### 1. `components/admin/AdminShell.tsx`

- Outermost Layout für `(authed)`-Bereich.
- Zwei-Spalten-Layout: `<AdminSidebar>` links (`width: 240`, sticky, full-height),
  `<main>` rechts mit `<AdminHeaderBar>` oben + scrollendem Content.
- Akzeptiert `children` und (optional) `headerTitle` als Prop;
  fallback: leite Titel aus dem aktuellen Pathname ab.

### 2. `components/admin/AdminSidebar.tsx`

- Sidebar 1:1 aus `Backend CMS.html` (`Sidebar` ab Zeile 737).
- **Logo-Header:** SVG-Slot oben links — Component akzeptiert `<LogoSlot>`-Prop;
  Default-Implementierung rendert die `site-config`-Initialen (`MF`-Style 36×36
  Square mit `primary`-Background). Wenn `public/admin-logo.svg` existiert,
  rendert sie statt der Initialen (via `<img>`-Tag, Fallback bei 404).
- **Nav-Items:** Liste aus `NAV_ITEMS` (Dashboard / Anfragen / Karriere /
  Seiten / Galerie / Einstellungen) — exakt die Labels aus dem Design.
- Active-State per `usePathname()` matching:
  - `/admin` → Dashboard
  - `/admin/anfragen` → Anfragen
  - usw.
- Badges (`item.badge`) initial mit Mock-Counts aus `lib/admin-mock.ts`
  (Anfragen: ungelesene Count, Karriere: neue-Bewerbungen-Count).
- **Footer:** Avatar mit Email aus `/admin/api/me`-Response + Logout-Button
  (`⏻`); Click → POST `/admin/api/logout` → `router.replace('/admin/login')`.

### 3. `components/admin/AdminHeaderBar.tsx`

- 60px-hoher Header (`HeaderBar` ab Zeile 901 im Design).
- Links: Section-Titel (Mapping aus `pathname`).
- Rechts: Notification-Bell (mit roten Punkt — visual nur),
  Avatar-Square (Initialen aus `me.email`).

### 4. Geteilte Atoms in `components/admin/`

- **`StatusBadge.tsx`** — exakte Color-Map aus dem Design (Neu / In Bearbeitung /
  Erledigt / Im Gespräch / Abgelehnt / Angenommen / Veröffentlicht / Entwurf).
  Akzeptiert Status-String + fallback auf neutral.
- **`AdminCard.tsx`** — `background: theme.admin.surface`, `borderRadius: 14`,
  `border`, `padding: 24`. Akzeptiert `style`-Override.
- **`BarChart.tsx`** — 7-Tage-Balken-Chart, exakte Implementierung aus dem
  Design (`BarChart` ab Zeile 137). Akzeptiert `data: { day, v }[]`.
- **`DonutChart.tsx`** — SVG-Donut mit Polar-Math, Total in der Mitte,
  Legende rechts (`DonutChart` ab Zeile 161). Akzeptiert
  `data: { label, v, color }[]`.

### 5. `frontend/lib/admin-mock.ts`

- Zentrale Mock-Daten + Type-Interfaces. Alle Arrays 1:1 aus
  `Backend CMS.html` (Zeilen 58–104):
  - `STATS` (Dashboard-KPIs)
  - `CHART_DATA` (7-Tage-Visitors)
  - `INQUIRIES` (Anfragen-Liste)
  - `APPLICATIONS` (Bewerbungen)
  - `JOBS` (Stellen — aus `KarriereView` `useState`-Initial extrahieren)
  - `PAGES_DATA` (Seiten-Tabelle)
  - `GALLERY_ITEMS` (9-Item-Mock mit `picsum.photos`-Seeds)
  - `SETTINGS_DATA` (Website + Social-Media-Felder)
- Types als `export interface Inquiry`, `export interface Application`,
  `export interface Job`, etc. — diese werden später als Backend-Contract
  wiederverwendet.

### 6. CSS-Globals für Hover-Animationen

Animation-Keyframes aus dem Design (`fadeUp`, `spin`, `pulse`) und Hover-Klassen
(`sidebar-item:hover`, `card-hover:hover`, `table-row:hover`, `btn:hover`,
`tab-btn:hover`) in `app/admin/admin.css` oder als Style-Tag im
`(authed)/layout.tsx` einsetzen.

## Akzeptanzkriterien

- [x] AdminShell rendert mit Sidebar + HeaderBar + Content-Area.
- [x] Sidebar-Active-State folgt `usePathname()` korrekt für alle 6 Sections.
- [x] LogoSlot fällt auf `site-config`-Initialen zurück, wenn keine
      `public/admin-logo.svg` existiert.
- [x] Sidebar-Footer Logout funktioniert (Cookie weg, Redirect zu Login).
- [x] StatusBadge / AdminCard / BarChart / DonutChart sind als
      eigenständige Komponenten exportiert und importierbar.
- [x] `lib/admin-mock.ts` liefert typisierte Mock-Daten für alle Sections.
- [x] Hover-Transitions matching Design (Sidebar-Items, Cards, Table-Rows).
- [x] `npm run build` + `npm run lint` clean.

## Out of Scope (in Wave 2)

- Section-spezifische Pages (Wave 3 + 4)
- Echte API-Calls (alles bleibt mock)
- **Mobile-Layouts** — die Sidebar wird in Wave 2 Desktop-only (sticky,
  240 px) gebaut; der Drawer-Mode für < 1024 px wird in Wave 5 nachgezogen.
  Wave 2 kann aber die Komponenten-Signatur bereits so anlegen, dass sie
  einen `isOpen` + `onClose` Prop akzeptiert (default-no-op auf Desktop) —
  das vereinfacht Wave 5.

## Kontext

- **Design-Referenz:** `Backend CMS.html` Zeilen 41–205 (Palette + Atoms),
  737–924 (Sidebar + HeaderBar)
- **Vorhandene Theme-Tokens** aus Wave 1
- **`lib/site-config.ts`** für Firmenname + Initialen
