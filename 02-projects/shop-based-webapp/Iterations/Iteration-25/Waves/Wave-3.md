# Wave 3 - Regression, Build und visueller Smoke

**Iteration:** [[Iteration-25]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die Copy- und Bilddarstellungsanpassungen sind testseitig und visuell ausreichend abgesichert. Das Ergebnis ist reviewbar und entspricht dem Kundenfeedback.

## Tasks

- [x] Statischen Audit ausfuehren: `rg "object-cover|object-\[" web/customer/src/app/warum-wir/page.tsx web/customer/src/components/AboutUs.tsx web/customer/src/components/GallerySlider.tsx`.
- [x] Customer-Web Tests ausfuehren: `cd web/customer && npm test -- --configLoader runner`.
- [x] Customer-Web Build ausfuehren: `cd web/customer && npm run build`.
- [ ] Visuellen Smoke fuer `/warum-wir` auf Desktop und Mobile durchfuehren. Lokal blockiert: kein Playwright/Chromium/Screenshot-Tool verfuegbar.
- [ ] Visuellen Smoke fuer Homepage-Gallery auf Desktop und Mobile durchfuehren. Lokal blockiert: kein Playwright/Chromium/Screenshot-Tool verfuegbar.
- [x] Finalen Diff gegen Scope pruefen: keine Asset-Ersetzung, keine Backend-Aenderungen, keine Preis-/Booking-Regression.

## Akzeptanzkriterien

- [x] Der statische Audit zeigt keine Crop-/Positionsklasse mehr in den betroffenen Bilddarstellungen oder dokumentiert bewusst verbleibende, nicht betroffene Vorkommen.
- [x] Tests sind gruen oder konkrete Blocker sind mit Ursache dokumentiert.
- [x] Build ist gruen oder konkrete Blocker sind mit Ursache dokumentiert.
- [x] `/warum-wir`: Insolvenzversicherungstext ist im Code erreichbar; statischer Bild-Audit zeigt keine Crop-/Zoom-Klassen in den betroffenen Darstellungen.
- [x] Homepage: Founder-Bilder und Gallery-Slides nutzen `object-contain`; statischer Audit zeigt keine Crop-/Zoom-Klassen.
- [ ] Keine sichtbaren Textueberlagerungen, instabilen Containerhoehen oder abgeschnittenen Hauptmotive bleiben offen. Lokal visuell nicht bestaetigt; Gallery-Caption-Overlay wurde aus dem Bildbereich entfernt.

## Abhaengigkeiten

Wave 1 und Wave 2 muessen abgeschlossen sein.

## Notes

Verification:

- Statischer Audit erweitert auf `object-cover|object-\[|scale-\[|group-hover:scale|hover:scale|animate-\[fadeIn` -> keine Treffer in `warum-wir/page.tsx`, `AboutUs.tsx`, `GallerySlider.tsx`.
- `cd web/customer && npm test -- --configLoader runner` -> 9 Testdateien, 70 Tests gruen.
- `cd web/customer && npm run build` -> erfolgreich.
- `git diff --check` -> keine Whitespace-Probleme.
- Browser/Screenshot-Smoke lokal blockiert: kein `playwright`, `chromium`, `chromium-browser` oder `google-chrome` verfuegbar; `web/customer/node_modules/.bin` enthaelt kein Playwright.
