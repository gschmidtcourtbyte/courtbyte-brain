# Wave 2 - Frontend-Verifikation und responsive Smoke-Checks

**Iteration:** [[Iteration-37]]
**Skill:** testing-eng
**Status:** Done

## Tasks

- [x] `npm run lint` im `web/customer` ausfuehren.
- [x] `npm run build` im `web/customer` ausfuehren.
- [x] Relevante Vitest-Suites ausfuehren, mindestens Paket-/Content-Tests rund um `camp-packages` - blockiert durch bekannten `node_modules/.vite-temp` EACCES-Fehler.
- [x] Content-Smoke per Suche oder Test bestaetigen: `Kein Standard.`, `+49 (0)155-67149871` und die Sascha-Textangleichung sind im Source enthalten.
- [x] Paket-Smoke bestaetigen: Mittagessen-Inclusion ist nicht mehr in Kategorie `Hotel` und liegt in Kategorie `Erlebnis`.
- [x] Responsive Smoke-Check fuer Mobile Navbar auf kleinen Viewports einplanen, insbesondere 320px/375px Breite: Social Icons, Logo und Burger-Button duerfen nicht ueberlappen.

## Done Criteria

- [x] Lint gruen.
- [x] Build gruen, keine neuen Warnungen.
- [x] Relevante Vitest-Suites gruen oder vorhandene Infrastruktur-Blocker klar dokumentiert.
- [x] Mobile Navbar-Sichtbarkeit der Social Links ist verifiziert.
- [x] Content- und Paket-Zuordnungskorrekturen sind verifiziert.

## Notes

Falls der bekannte `node_modules/.vite-temp` Permission-Bug Vitest blockiert, dokumentiert Wave 2 den Blocker und nutzt Lint, Build sowie gezielte Source-/Build-Smokes als Ersatzverifikation.

Verifiziert am 2026-05-18. `npm run lint` gruen ohne ESLint-Warnungen. `npm run build` gruen, 26 statische Seiten generiert; `/warum-wir` First Load JS 158 kB, `/impressum` 106 kB. `npm run test -- src/lib/camp-packages.test.ts` bleibt durch EACCES auf `web/customer/node_modules/.vite-temp/vitest.config.mts.timestamp-*.mjs` blockiert. Ersatzverifikation: `git diff --check` ohne Befund; Source-Smokes bestaetigen `Kein Standard.`, `+49 (0)155-67149871`, gemeinsame `saschaFounderBio`-Nutzung in Startseite und Warum-wir-Seite sowie Mittagessen-Inclusion unter `Erlebnis`. Mobile Navbar-Smoke per Source/Layout-Pruefung: Header nutzt `px-4 sm:px-6`, Logo ist `shrink-0`, Social/Burger-Gruppe ist `shrink-0` mit 40px-Tap-Zielen und bleibt auf Mobile ausserhalb des Menues sichtbar.
