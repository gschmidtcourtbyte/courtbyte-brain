# Wave 2 - Komponenten-Refresh & Icons

**Iteration:** [[Iteration-1]]
**Skill:** frontend-eng
**Status:** Done (2026-05-30)
**Referenz:** [[Corporate-Design]]

## Tasks

- [x] `Hero.tsx` (Split/Vollbild/Karte): perspective tilt entfernt,
      Foto-Kachel mit hartem `--shadow-hard`, Floating-Badge ersetzt durch
      `ProjectStamp` (Mono, "SEIT 20+ JAHRE IM TIEFBAU"). Vollbild-Variante
      hat jetzt schwarzen Gradient + roten Vertikalstreifen statt
      Petrol-Gradient. Karten-Variante hat roten Mono-Stempel
      ("QUALITAET · Termin · Sicher") statt 98% Badge
- [x] `Blobs.tsx`: Bunte Blobs ersetzt durch Punkt-Raster (24 px Grid),
      zwei diagonale Hazard-Streifen in Ecken, Mono-Koordinaten-Marker.
      `ScrollHint` jetzt statisch mit Mono-Label "WEITER" + Pfeil-SVG
- [x] `Navbar.tsx`: Nav-Links in Display-Font Caps, scharfkantiges
      Dropdown mit Mono-Items, Mobile-Menu mit hartem Schatten und
      DIN-Trennlinien
- [x] `Footer.tsx`: Hazard-Stripe als Top-Akzent, Mono-Spaltentitel mit
      1 px-Underline, Mono-Adresse/Telefon/Mail-Block, Logo invertiert
      mit groesserer Hoehe (40 px), Mono-Copyright
- [x] `StatsBar.tsx`: Vollflaechig Tiefschwarz, oben 4 px roter Trennstrich,
      Zahlen in Display-Font (clamp 56-96 px), Suffix in Mono-Rot,
      Spalten-Trennlinien, Mono-Sektionslabel "KENNZAHLEN"
- [x] `FeaturesStrip.tsx`: drei Features mit IcExcavator/IcSurveyor/IcHelmet,
      Display-Font Titel, Spalten-Trennlinien statt Karten
- [x] `LeistungenPreview.tsx`: Karten-Raster mit 1 px-Trennlinien,
      Mono-Nummerierung pro Karte ("01-04"), Icon via getIcon(l.icon),
      Display-Font Titel, Sektion mit Mono-Label "01 · LEISTUNGEN" und
      Big-Shoulders H2
- [x] `Process.tsx`: 4-Schritte mit `01-04` in Display-Font (XL), oben
      pro Spalte 3 px Akzentlinie (erster Schritt rot), Mono-Sublabel
      "SCHRITT", Display-Titel, keine Emoji
- [x] `Gallery.tsx`: Filter-Pills jetzt scharfkantig in Mono-Caps,
      Galerie-Karten ohne Rundung mit rotem Eck-Marker (8x56 px) oben
      links, Anthrazit-Hover-Overlay, Mono-Code in Caption
      (`KATEGORIE · 001`), Lightbox scharfkantig mit hartem Schatten,
      Mono-Index-Badge
- [x] `PageHeader.tsx`: Vollflaechig Tiefschwarz mit Punkt-Raster und
      rotem Vertikalstreifen links, optionaler Mono-Breadcrumb-Prop,
      Big-Shoulders Title (clamp 56-96 px), 4 px roter Trennstrich unten
- [x] `components/icons.tsx`: 9 neue Tiefbau-Strich-Icons
      (IcExcavator, IcPipe, IcRoad, IcDemolition, IcHelmet, IcSurveyor,
      IcPylon, IcShovel, IcManhole) plus IcArrowDown/IcArrowRight,
      1.5 px stroke, ICON_MAP + getIcon(key) Helper

## Done Criteria

- [x] Keine Emoji mehr in `frontend/components/*.tsx` und
      `frontend/app/(marketing)/*` (admin-mock ausgenommen, das ist
      Dark-CMS, Out-of-Scope)
- [x] Border-Radius in oeffentlichen Komponenten konsistent <= 8 px
      (CSS-Variable `--radius-card: 4px` ueberall, Filter/Tags 0 px)
- [x] Hero, Navbar, Footer, StatsBar, Process visuell konsistent mit
      Industrial-Editorial-Richtung aus [[Corporate-Design]]
- [x] `npm run lint` sauber
- [x] `npm run typecheck` sauber
- [x] `npm run build` sauber (27 Routen statisch generiert)
- [ ] **Offen: visuelle Sichtkontrolle Mobile (375 px)** — manuell im
      Browser pruefen, ob neue Spalten-Layouts (Stats, Leistungen,
      Process, Features) sauber zu Single-Column kollabieren

## Notes

Eine Komponente nach der anderen anfassen, jeweils auf `npm run dev`
gegenpruefen. Ziel ist nicht Pixel-Perfektion auf Anhieb, sondern
konsistente Form-Sprache. Wenn ein Layout-Detail unklar ist, lieber
einfacher und gridiger als verspielt. [[Corporate-Design]] ist die
Referenz.

Falls Komponenten Werte hardcoden, die in `theme.ts` als Token gehoeren
(Farben, Radien, Schatten, Schriftgroessen-Skalen), erst Token-Luecke
schliessen, dann Komponente konsumiert das Token - die "goldene Regel"
aus CLAUDE.md gilt auch hier.
