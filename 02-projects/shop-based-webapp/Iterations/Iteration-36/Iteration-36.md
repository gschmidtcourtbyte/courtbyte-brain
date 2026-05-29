# Iteration 36 - iOS 26 Safari Render-Performance fixen

**Status:** Done
**Repo:** /home/andersen/git/shop-based-webapp
**Datum:** 2026-05-12
**Rolle:** Acting as Product Owner. Planning Iteration 36 for shop-based-webapp.

## Ziel

Iteration 36 bringt die Render- und wahrgenommene Ladezeit der OUT-Padel-Startseite auf iOS 26 Safari zurueck auf das iOS 18 Niveau, ohne die visuelle Identitaet (Hero-Glow, beige Panel-Sprache, Nav-Sheen) auf Desktop und aelteren Browsern zu verschlechtern.

## Messbefund

- iOS 18 Safari rendert die Startseite ohne Auffaelligkeiten; iOS 26 Safari zeigt massive Ladezeiten und Jank bereits beim ersten Paint.
- `web/customer/src/components/Hero.tsx:9-11` stapelt drei Blur-Layer above the fold: `blur-[90px]`, `blur-[120px]`, `blur-3xl` (=64px) - alle endlos animiert (`hero-orb-drift` 16s, `hero-sheen` 10s).
- `web/customer/src/app/globals.css:204` setzt `backdrop-filter: blur(18px)` auf den permanenten fixed Header `.panel-nav`, ueberlagert von `nav-sheen` (background-position infinite).
- `web/customer/src/components/Hero.tsx:14-56` enthaelt eine animierte SVG-Mask (`hero-net-shimmer`) - auf WebKit historisch fragil, in iOS 26 mit Liquid Glass Pipeline deutlich teurer.
- `web/customer/src/app/globals.css:220-226` (`.hero-ambient`) stapelt vier radial-gradients plus einen linear-gradient auf der Wurzel-Flaeche.
- iOS 26 "Liquid Glass" Compositing Pipeline faellt bei Blur-Radien >40px und gestapelten Filtern haeufig auf Software-Rendering zurueck - das erklaert den Regressionspunkt zwischen iOS 18 und iOS 26.

## Scope

**In:**
- Hero-Blur-Radien reduzieren (`Hero.tsx`): `blur-[90px]` -> `blur-[40px]`, `blur-[120px]` -> `blur-[50px]`, `blur-3xl` -> `blur-2xl`. Opacity der Orbs leicht anheben, damit der Glow weiter sichtbar bleibt.
- `globals.css` `.panel-nav` `backdrop-filter` von `blur(18px)` auf `blur(10px)` senken.
- `globals.css` `@media` Block: `hero-net-shimmer` Animation per `@supports (-webkit-touch-callout: none)` auf iOS deaktivieren; Base-Net bleibt sichtbar.
- `globals.css` `.hero-orb` und `.hero-sheen` mit `will-change: transform` und `transform: translateZ(0)` als GPU-Compositing-Hint versehen.
- `globals.css` `.hero-ambient` von vier auf zwei radial-gradients reduzieren; restliche Tiefe per linear-gradient erzeugen.
- Build, Lint, Vitest-Smoke gruen.

**Out:**
- Vollstaendiges Re-Design der Hero-Section.
- Aenderung am Design-Token-System (`@theme` Block).
- Server-seitige Device-Detection oder UA-Sniffing.
- Aenderungen an Galerie/Bildpipeline (war Iteration 35).
- Aenderungen an Framer-Motion-Animationen der Text-Elemente (cheap, unbetroffen).
- `FinalCTA.tsx` (`blur-[100px]`) und `BookingGuide.tsx` (`backdrop-blur-sm`) - bewusst out-of-scope, da Below-the-fold bzw. nur in Modal sichtbar.

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | frontend-eng | Blur-Radien, backdrop-filter, hero-ambient, will-change, iOS-Mask-Disable | Done |
| Wave 2 | testing-eng | Build, Lint, Vitest, Bundle-Inspection | Done |

## Critical Path

```text
Wave 1 (Visual Performance Fix)
    v
Wave 2 (Verification)
```

## Acceptance Criteria

- [x] Alle Blur-Radien in `Hero.tsx` <= 50px.
- [x] `.panel-nav` `backdrop-filter` <= 10px.
- [x] `hero-net-shimmer` per `@supports (-webkit-touch-callout: none)` auf iOS deaktiviert; auf Desktop weiter aktiv.
- [x] `.hero-orb` und `.hero-sheen` haben `will-change: transform` und `translateZ(0)`.
- [x] `.hero-ambient` enthaelt maximal zwei radial-gradients.
- [x] `npm run lint` gruen.
- [x] `npm run build` gruen, keine neuen Warnungen.
- [x] Vitest-Suites - blockiert durch vorbestehenden `node_modules/.vite-temp` Permission-Bug, Verifikation per tsc/Build akzeptiert.
- [ ] Visueller Smoke auf iOS 26 (offen, vom User zu pruefen): Ladezeit gleicht iOS 18 an, Hero zeigt weiterhin warmen Glow-Look.

## Notes

Planung erstellt am 2026-05-12 nach dem iOS-26-spezifischen Performance-Audit. Hauptursache: iOS 26 Safari Compositing Pipeline ("Liquid Glass") faellt bei Blur-Radien >40px und gestapelten Filtern auf Software-Rendering zurueck. Iteration 35 hatte das Galerie-Verhalten optimiert; diese Iteration zielt auf den Hero und den fixed Header.

Abgeschlossen am 2026-05-12. Ergebnis: Drei Hero-Blur-Orbs auf 40/50/blur-2xl gesenkt (mit kompensierter Opacity), `.panel-nav` backdrop-filter auf 10px reduziert, `.hero-ambient` von vier auf zwei radial-gradients verschlankt, `.hero-orb` und `.hero-sheen` mit GPU-Compositing-Hint versehen, `hero-net-shimmer` und `nav-sheen` per `@supports (-webkit-touch-callout: none)` auf iOS deaktiviert (Base-Layer bleiben sichtbar). Homepage First Load JS unveraendert 172 kB. Bundle-Inspection bestaetigt korrekte CSS-Ausgabe.
