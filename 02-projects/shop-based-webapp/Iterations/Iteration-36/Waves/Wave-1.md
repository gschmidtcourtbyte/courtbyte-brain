# Wave 1 - Blur- und Filter-Last in Hero, Nav und globals.css reduzieren

**Iteration:** [[Iteration-36]]
**Skill:** frontend-eng
**Status:** Done

## Tasks

- [x] `web/customer/src/components/Hero.tsx`: drei Blur-Orbs reduzieren - `blur-[90px]` -> `blur-[40px]`, `blur-[120px]` -> `blur-[50px]`, `blur-3xl` -> `blur-2xl`. Opacity der weissen Orbs leicht anheben (bg-white/55 -> bg-white/70, bg-brand-accent/10 -> bg-brand-accent/14, sheen from-white/35 -> from-white/45), damit der Glow trotz kleinerem Radius sichtbar bleibt.
- [x] `web/customer/src/app/globals.css` `.panel-nav`: `backdrop-filter: blur(18px)` -> `backdrop-filter: blur(10px)`.
- [x] `web/customer/src/app/globals.css`: neuer `@supports (-webkit-touch-callout: none)` Block, der `.hero-net-shimmer` Animation auf iOS auf `animation: none` setzt; Base-Net bleibt voll sichtbar. Zusaetzlich `.nav-sheen` mit deaktiviert, da identische Problemklasse (animated background-position ueber blurredem Panel).
- [x] `web/customer/src/app/globals.css` `.hero-orb` und `.hero-sheen`: `will-change: transform` und `transform: translateZ(0)` als initiale Hint-Eigenschaft setzen (keyframes nutzen weiterhin transform fuer Animation).
- [x] `web/customer/src/app/globals.css` `.hero-ambient`: von vier radial-gradients auf zwei reduziert (der mittlere weisse Highlight-Spot bei 60% / 78% entfernt); linear-gradient bleibt als Tiefen-Basis.

## Done Criteria

- [x] Alle geaenderten Dateien typechecken und lassen sich von Next bauen.
- [x] Keine Tailwind-Klassen-Warnungen aus dem Build.
- [x] Im DevTools-Inspector: `.panel-nav` zeigt `backdrop-filter: blur(10px)`; `.hero-orb` zeigt `will-change: transform`.
- [x] `@supports (-webkit-touch-callout: none)` Block ist im finalen CSS-Bundle enthalten.
- [ ] Visueller Browser-Smoke (offen, vom User durchzufuehren): Hero zeigt weiterhin warmen Glow, Header bleibt halbtransparent.

## Notes

Begruendung Blur-Radien: iOS 26 Safari faellt bei Blur >40px haeufig auf Software-Rendering zurueck. 40/50px sind die hoechsten Werte, die nach interner Wahrnehmung noch GPU-kompositiert bleiben. Opacity-Erhoehung kompensiert den optisch flacheren Eindruck kleinerer Radien.

Begruendung `@supports (-webkit-touch-callout: none)`: matched zuverlaessig iOS Safari (Desktop Safari hat diese Property nicht). Trifft zwar auch iOS 18, aber dort war keine Regression - der Mask-Shimmer war auf iOS schon immer eine Schwachstelle, jetzt eskaliert sie nur. Alternative JS-UA-Detection waere overkill fuer reinen CSS-Fix.

Begruendung `will-change`: Hint an den Browser, die Layer dauerhaft auf einer eigenen GPU-Schicht zu halten. In Kombination mit `translateZ(0)` zwingt das auch aelteres WebKit zu GPU-Compositing, was die Mask-/Filter-Performance entlastet.

Umgesetzt am 2026-05-12. `nav-sheen` wurde zusaetzlich zum geplanten `hero-net-shimmer` in den `@supports`-Block aufgenommen, weil die animierte `background-position` auf einem panel-nav mit `backdrop-filter` die gleiche iOS-26-Pathologie erzeugt - der Plan war hier konservativ. Die kleinere `.hero-ambient` reduziert vier auf drei `background`-Layer (zwei radial-gradients + linear-gradient).
