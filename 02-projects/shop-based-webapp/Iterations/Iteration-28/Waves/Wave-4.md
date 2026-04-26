# Wave 4 - Bild-Audit, Tests und Build

**Iteration:** [[Iteration-28]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die Bilddarstellungs-Korrekturen aus Wave 1 bis Wave 3 sind gegen Regressionen abgesichert. Statische Audits bestaetigen, dass die betroffenen Bilder boxbreit und ohne Crop-/Zoom-Mechaniken umgesetzt wurden.

## Agent

QA Engineer: Fokus auf statische Bildklassen-Audits, vorhandene Customer-Web Tests, Build und visuellen Smoke soweit lokal moeglich.

## Tasks

- [x] Statischen Audit fuer `web/customer/src/components/GallerySlider.tsx` ausfuehren: kein `object-cover`, keine manuelle `object-[...]` Positionierung, kein Scale-/Hover-Zoom fuer das sichtbare Slide-Bild.
- [x] Statischen Audit fuer `web/customer/src/components/AboutUs.tsx` ausfuehren: Ariadna/Sascha ohne `object-cover`, ohne manuelle `object-[...]` Positionierung, ohne Scale-/Hover-Zoom.
- [x] Statischen Audit fuer `web/customer/src/app/warum-wir/page.tsx` ausfuehren: Ariadna/Sascha ohne `object-cover`, ohne manuelle `object-[...]` Positionierung, ohne Scale-/Hover-Zoom.
- [x] Customer-Web Tests ausfuehren: `cd web/customer && npm test -- --configLoader runner`.
- [x] Customer-Web Build ausfuehren: `cd web/customer && npm run build`.
- [x] Finalen Diff gegen Scope pruefen: keine Backend-, API-, Datenbank-, Booking- oder Asset-Aenderungen.
- [x] Falls lokal moeglich: visuellen Smoke fuer Homepage-Gallery, Homepage-"Ueber uns" und `/warum-wir` auf Desktop/Mobile dokumentieren.

## Akzeptanzkriterien

- [x] Statischer Audit bestaetigt Gallery ohne Crop-/Zoom-Mechaniken im sichtbaren Slide-Bild.
- [x] Statischer Audit bestaetigt AboutUs-Founder-Bilder ohne Crop-/Zoom-Mechaniken.
- [x] Statischer Audit bestaetigt Warum-wir-Founder-Bilder ohne Crop-/Zoom-Mechaniken.
- [x] Gallery-, AboutUs- und Warum-wir-Bilder sind im Diff nachvollziehbar auf boxbreite, unverzoomte Darstellung begrenzt.
- [x] Customer-Web Tests sind gruen oder Blocker sind konkret dokumentiert.
- [x] Customer-Web Build ist gruen oder Blocker sind konkret dokumentiert.
- [x] Keine unerwuenschten Aenderungen ausserhalb des Iteration-28-Scopes bleiben offen.
- [x] Visueller Smoke ist dokumentiert oder ein lokaler Tooling-Blocker ist konkret benannt.

## Abhaengigkeiten

Wave 1, Wave 2 und Wave 3 muessen integriert sein.

## Notes

Falls kein Browser-/Screenshot-Tool verfuegbar ist, wird der visuelle Smoke als lokaler Blocker dokumentiert und durch statische Audits plus Tests/Build ergaenzt.

Umsetzung 2026-04-26:
- Statischer Audit `rg -n "object-cover|object-\[|scale-|hover:scale|group-hover:scale" web/customer/src/components/GallerySlider.tsx web/customer/src/components/AboutUs.tsx web/customer/src/app/warum-wir/page.tsx` ohne Treffer.
- Zusaetzlicher Bild-Audit bestaetigt `block h-auto w-full` plus echte `width`/`height` fuer Gallery, AboutUs-Founder und Warum-wir-Founder.
- `npm test -- --configLoader runner`: 10 Testdateien / 79 Tests gruen.
- `npm run build`: erfolgreich.
- Finaler Scope-Check: Codeaenderungen betreffen nur `GallerySlider.tsx`, `AboutUs.tsx`, `warum-wir/page.tsx`, `gallery.ts` und `gallery.test.ts`; keine Backend-, API-, Datenbank-, Booking- oder Asset-Aenderungen.
- Browser-/Screenshot-Smoke lokal nicht ausgefuehrt: kein Playwright, Chromium oder Google Chrome im Projekt/Systempfad verfuegbar.
