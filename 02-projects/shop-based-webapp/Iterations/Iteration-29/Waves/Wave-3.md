# Wave 3 - Bildgroessen-Audit, Tests und Build

**Iteration:** [[Iteration-29]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die Bildgroessen-Korrektur aus Wave 1 und Wave 2 ist gegen Regressionen abgesichert. Statische Audits, responsive Sichtpruefung und Customer-Web Quality Gates bestaetigen, dass Ariadna und Sascha in beiden Bereichen dieselbe sichtbare Bildbox haben.

## Agent

QA Engineer: Fokus auf CSS-/Markup-Audit, responsive Smoke-Pruefung, vorhandene Customer-Web Tests, Build und Scope-Kontrolle.

## Tasks

- [x] Statischen Audit fuer `web/customer/src/components/AboutUs.tsx` ausfuehren: Ariadna/Sascha verwenden dieselben Bildbox-Constraints fuer Breite, Hoehe und Seitenverhaeltnis.
- [x] Statischen Audit fuer `web/customer/src/app/warum-wir/page.tsx` ausfuehren: Ariadna/Sascha verwenden dieselben Bildbox-Constraints fuer Breite, Hoehe und Seitenverhaeltnis.
- [x] Pruefen, dass zwischen "Ueber uns" und `/warum-wir` keine unbegruendete Drift in der Founder-Bildlogik entstanden ist.
- [x] Customer-Web Tests ausfuehren: `cd web/customer && npm test -- --configLoader runner`.
- [x] Customer-Web Build ausfuehren: `cd web/customer && npm run build`.
- [x] Finalen Diff gegen Scope pruefen: keine Backend-, API-, Datenbank-, Booking-, Admin-, Gallery- oder Asset-Aenderungen.
- [x] Falls lokal moeglich: visuellen Smoke fuer Homepage-"Ueber uns" und `/warum-wir` auf Desktop und Mobile dokumentieren.

## Akzeptanzkriterien

- [x] Statischer Audit bestaetigt gleiche Bildbox-Breite und -Hoehe fuer Ariadna/Sascha in `AboutUs.tsx`.
- [x] Statischer Audit bestaetigt gleiche Bildbox-Breite und -Hoehe fuer Ariadna/Sascha in `warum-wir/page.tsx`.
- [x] "Ueber uns" und `/warum-wir` nutzen konsistente Founder-Bildregeln oder eine begruendete, dokumentierte lokale Abweichung.
- [x] Customer-Web Tests sind gruen oder Blocker sind konkret dokumentiert.
- [x] Customer-Web Build ist gruen oder Blocker sind konkret dokumentiert.
- [x] Keine unerwuenschten Aenderungen ausserhalb des Iteration-29-Scopes bleiben offen.
- [x] Visueller Smoke ist dokumentiert oder ein lokaler Tooling-Blocker ist konkret benannt.

## Abhaengigkeiten

Wave 1 und Wave 2 muessen integriert sein.

## Notes

Falls kein Browser-/Screenshot-Tool verfuegbar ist, wird der visuelle Smoke als lokaler Blocker dokumentiert und durch statische Audits plus Tests/Build ergaenzt.

Umsetzung 2026-04-26:
- Statischer Audit `rg -n "FounderImageFrame|aspectRatio|fill|object-cover|object-bottom|width=|height=|aspect-\[2/3\]" web/customer/src/components/AboutUs.tsx web/customer/src/app/warum-wir/page.tsx web/customer/src/components/FounderImageFrame.tsx` bestaetigt gemeinsame Frame-Nutzung.
- `AboutUs.tsx`: Ariadna/Sascha verwenden dieselbe `FounderImageFrame`-Komponente.
- `warum-wir/page.tsx`: Ariadna/Sascha verwenden dieselbe `FounderImageFrame`-Komponente.
- `FounderImageFrame.tsx`: gemeinsamer Frame mit `aspectRatio: "2 / 3"`, `relative`, `w-full`, `overflow-hidden`, `Image fill` und `object-cover`.
- Finaler Scope-Check: Codeaenderungen betreffen nur `AboutUs.tsx`, `warum-wir/page.tsx` und die neue Komponente `FounderImageFrame.tsx`; keine Backend-, API-, Datenbank-, Booking-, Admin-, Gallery- oder Asset-Aenderungen.
- Customer-Web Tests: `npm test -- --configLoader runner` mit 10 Testdateien / 79 Tests gruen.
- Customer-Web Build: `npm run build` erfolgreich.
- Browser-/Screenshot-Smoke lokal nicht ausgefuehrt: kein Playwright, Chromium oder Google Chrome im Systempfad verfuegbar.
