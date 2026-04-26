# Wave 4 - Regression, Audit und Build

**Iteration:** [[Iteration-27]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die Redirect-, Admin-Layout- und Bilddarstellungsanpassungen sind gegen Regressionen abgesichert. Das Ergebnis ist reviewbar und erfuellt die Akzeptanzkriterien der Iteration.

## Agent

QA Engineer: Fokus auf zielgerichtete Regression, statische Audits und vorhandene Test-/Build-Gates.

## Tasks

- [x] Tests fuer Redirect-Helper oder Login-Redirect-Verhalten ergaenzen/anpassen, sofern die Logik testbar extrahiert wird.
- [x] Statischen Bild-Audit fuer `AboutUs.tsx` ausfuehren: keine `object-cover` oder `object-[...]` Klassen fuer Ariadna/Sascha.
- [x] Statischen Gallery-Audit fuer `GallerySlider.tsx` ausfuehren: kein `object-cover` fuer das Slide-Bild; keine Roundness am betroffenen Bildframe.
- [x] Customer-Web Tests ausfuehren: `cd web/customer && npm test -- --configLoader runner`.
- [x] Customer-Web Build ausfuehren: `cd web/customer && npm run build`.
- [x] Finalen Diff gegen Scope pruefen: keine Backend-Schema-, Preis-, Checkout- oder Asset-Aenderungen.
- [x] Falls lokal moeglich: visuellen Smoke fuer `/login`, `/admin`, Homepage-"Ueber uns" und Gallery auf Desktop/Mobile dokumentieren.

## Akzeptanzkriterien

- [x] Redirect-Regressionen sind per Test oder klar dokumentiertem manuellen Check abgedeckt.
- [x] Statischer Audit bestaetigt unverzoomte Ueber-uns Bilder ohne manuelle Ausschnittsteuerung.
- [x] Statischer Audit bestaetigt Gallery ohne Crop-Zoom und ohne betroffene Frame-Rundung.
- [x] Admin-Dashboard-Layout ist im Diff klar auf full-width Buchungsliste plus Traffic darunter begrenzt.
- [x] Customer-Web Tests sind gruen oder Blocker sind konkret dokumentiert.
- [x] Customer-Web Build ist gruen oder Blocker sind konkret dokumentiert.
- [x] Keine unerwuenschten Aenderungen ausserhalb des Iteration-27-Scopes bleiben offen.

## Abhaengigkeiten

Wave 1, Wave 2 und Wave 3 muessen integriert sein.

## Notes

Falls kein Browser-/Screenshot-Tool verfuegbar ist, wird der visuelle Smoke wie in Iteration 25 als lokaler Blocker dokumentiert und durch statische Audits plus Build/Test ergaenzt.

Umsetzung 2026-04-26:
- Redirect-Regressionen sind durch `web/customer/src/lib/post-login-redirect.test.ts` abgedeckt.
- Statischer Audit:
  - `rg -n "object-cover|object-\[" web/customer/src/components/AboutUs.tsx web/customer/src/components/GallerySlider.tsx` ohne Treffer.
  - `rg -n "rounded-2xl|rounded-\[--radius-xl\]|overflow-hidden rounded" web/customer/src/components/AboutUs.tsx web/customer/src/components/GallerySlider.tsx` ohne Treffer.
  - `web/customer/src/app/admin/page.tsx` zeigt `space-y-6`, `min-w-[1180px]`, "Neueste Buchungseingaenge" und "Traffic nach Seiten" untereinander.
- `npm test -- --configLoader runner`: 10 Testdateien / 79 Tests gruen.
- `npm run build`: erfolgreich.
- Finaler Scope-Check: Iteration-27-Codeaenderungen betreffen Login/Middleware Redirect, Admin Dashboard Layout, AboutUs, GallerySlider und den neuen Redirect-Testhelper. Keine Backend-Schema-, Preis-, Checkout- oder Asset-Aenderungen.
- Unrelated dirty files im Code-Repo bleiben vorhanden und wurden nicht veraendert: `.claude/settings.local.json`, `web/customer/src/lib/camp-packages.ts`.
- Browser-/Screenshot-Smoke lokal nicht ausgefuehrt: `@playwright/test` ist nicht installiert und kein Browser-/Screenshot-Tool ist in dieser Umgebung verfuegbar. Absicherung erfolgte ueber statische Audits, Tests und Build.
