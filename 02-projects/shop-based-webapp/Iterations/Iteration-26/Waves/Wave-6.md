# Wave 6 - Tests, Build und Review

**Iteration:** [[Iteration-26]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die visuellen Ueber-uns-Aenderungen und die flexible Paketauswahl sind regressionssicher, contract-konform und buildbar.

## Tasks

- [x] Backend Service-Tests fuer Paketinteressen ergaenzen/ausfuehren.
- [x] Handler-Tests fuer `package_interest_slugs` ergaenzen/ausfuehren.
- [x] Frontend Helper-Tests in `camp-booking-form.test.ts` fuer Paketinteressen ergaenzen.
- [x] Customer-Web relevante Tests ausfuehren.
- [x] Backend relevante Tests ausfuehren.
- [x] Customer-Web Build ausfuehren.
- [x] Statischen UI-Audit fuer AboutUs: Crop/Zoom nur dort wiederhergestellt, nicht global.
- [x] Review gegen Scope: keine Pricing-/Payment-/Warum-wir-Gallery-Reverts.

## Akzeptanzkriterien

- [x] Service lehnt ungueltige Paketinteressen ab und normalisiert Duplikate.
- [x] Handler akzeptiert Single- und Multi-Package-Payloads.
- [x] Frontend Payload-Builder sendet Paketinteressen korrekt.
- [x] `AboutUs.tsx` nutzt Crop/Zoom mit softer Roundness.
- [x] `warum-wir/page.tsx` und `GallerySlider.tsx` bleiben ausser Scope unveraendert.
- [x] Tests/Build sind gruen oder konkrete Blocker sind dokumentiert.

## Abhaengigkeiten

Waves 1 bis 5.

## Notes

Abgeschlossen am 2026-04-26.

- Beim Wave-6-Review zusaetzlich Checkout-Initialisierung korrigiert: URL-Paket oder Default sind synchron vorausgewaehlt; die Camp-Empfehlung zieht nach async Camp-Load nach, solange Nutzer nicht editiert.
- Service-Tests fuer Paketinteressen ergaenzt: Default auf Primaerpaket, Alias-/Duplikat-Normalisierung sowie unbekannte/leere Slugs als Invalid-Input.
- Handler-Contract fuer Single- und Multi-Package-Payloads sowie invalides `package_interest_slugs` geprueft; Frontend-Helper-Tests fuer Include/Omit von `package_interest_slugs` geprueft.
- Customer-Web Tests liefen mit `npm test -- --configLoader runner`, weil der Standard-Vitest-Bundle-Loader an `node_modules/.vite-temp`-Rechten (`nobody:nogroup`) scheitert.
- Verifikation gruen: `go test ./...`, `npm test -- --configLoader runner`, `npm run build`, `git diff --check`.
- Statischer UI-/Scope-Audit: `AboutUs.tsx` nutzt `object-cover` mit `object-position` und `rounded-2xl`; `warum-wir/page.tsx` und `GallerySlider.tsx` bleiben bei `object-contain`; keine Pricing-/Payment-/Warum-wir-Gallery-Reverts.
- Browser-Screenshot-Smoke nicht ausgefuehrt, weil kein lokales Playwright/Chromium/Chrome-Tooling verfuegbar ist; Build und statischer Audit herangezogen.
