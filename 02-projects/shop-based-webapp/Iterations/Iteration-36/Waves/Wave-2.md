# Wave 2 - Build, Lint und Vitest pruefen

**Iteration:** [[Iteration-36]]
**Skill:** testing-eng
**Status:** Done

## Tasks

- [x] `npm run lint` im `web/customer`.
- [x] `npm run build` im `web/customer`.
- [x] `npm run test` im `web/customer` (Vitest-Suites) - blockiert durch vorbestehenden `node_modules/.vite-temp` Permission-Bug; Verifikation per tsc/Build akzeptiert.
- [x] Bundle-Inspection: pruefen dass die neuen CSS-Regeln (`backdrop-filter: blur(10px)`, `@supports (-webkit-touch-callout: none)`) im generierten `app/layout-*.css` enthalten sind.
- [x] Grep nach alten Werten (`blur-[90px]`, `blur-[120px]`, `blur(18px)`) im Build-Output - sollten nicht mehr vorkommen.

## Done Criteria

- [x] Lint gruen.
- [x] Build gruen, keine neuen Warnungen.
- [x] Vitest-Suites - blockiert durch Infrastruktur-Bug, dokumentiert.
- [x] Bundle-Inspection bestaetigt neue CSS-Werte und Abwesenheit der alten.

## Notes

`vitest run` war in Iteration 35 wegen root-owned `node_modules/.vite-temp` blockiert. Status unveraendert in Iteration 36 (EACCES auf `/web/customer/node_modules/.vite-temp/vitest.config.mts.timestamp-*.mjs`). Ersatzweise tsc + Next Build als Verifikation.

Umgesetzt am 2026-05-12. Ergebnis: `next lint` ohne Warnungen. `next build` gruen, 26 statische Seiten generiert, Homepage First Load JS unveraendert bei 172 kB. CSS-Bundle `bf67593f338b37da.css` enthaelt genau einen Treffer fuer `backdrop-filter:blur(10px)`, einen `@supports (-webkit-touch-callout:none)` Block und einen `will-change:transform` Eintrag. Keine Treffer fuer die alten Werte `blur(18px)`, `blur-[90px]`, `blur-[120px]`.
