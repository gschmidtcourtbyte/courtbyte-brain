# Wave 2 - Legal-Seiten technisch verifizieren

**Iteration:** [[Iteration-32]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die aktualisierten Rechtstexte bauen stabil, sind ueber die bestehenden Seiten erreichbar und verursachen keine Customer-Web-Regression.

## Agent

QA Engineer: Fokus auf zielgerichtete Testauswahl, Build-/Lint-Verifikation, Legal-Link-Smoke, Content-Sichtbarkeit und Dokumentation konkreter Blocker.

## Tasks

- [x] Relevante Customer-Web Tests fuer Content, Legal Pages oder Navigation ausfuehren.
- [x] Falls keine spezifischen Tests existieren, sinnvolle Smoke-/Content-Tests ergaenzen oder als bewusstes Test-Gap dokumentieren.
- [x] `npm run build` in `web/customer` ausfuehren.
- [x] `npm run lint` in `web/customer` ausfuehren, sofern verfuegbar.
- [x] Datenschutz-Seite per statischem oder Browser-Smoke pruefen.
- [x] AGB-Seite per statischem oder Browser-Smoke pruefen.
- [x] Bestehende Footer-/Navigation-Links auf Datenschutz und AGB pruefen.
- [x] Sicherstellen, dass lange Paragrafen, Listen, E-Mail-Adresse, Telefonnummer, URL und Sonderzeichen nicht abgeschnitten werden.
- [x] Ergebnis mit konkreten Kommandos und eventuellen Blockern dokumentieren.

## Akzeptanzkriterien

- [x] Customer-Web Tests sind gruen oder konkrete Blocker sind dokumentiert.
- [x] Customer-Web Build ist gruen oder konkrete Blocker sind dokumentiert.
- [x] Lint ist gruen oder bekannte Ausnahmen sind dokumentiert.
- [x] Datenschutz-Route ist erreichbar und zeigt die aktualisierte Fassung.
- [x] AGB-Route ist erreichbar und zeigt die aktualisierte Fassung.
- [x] Links auf Datenschutz und AGB funktionieren weiter.
- [x] Umlaute, `§`, Euro-Betraege, Prozentwerte, `info@padel-out.de`, `www.bfdi.bund.de` und `https://ec.europa.eu/consumers/odr` werden korrekt dargestellt.

## Abhaengigkeiten

Wave 1 muss umgesetzt sein.

## Notes

Wenn kein lokaler Browser-Smoke moeglich ist, muss mindestens ein Build-/Route- und Content-Audit aus den generierten Artefakten oder Quelltexten dokumentiert werden.

Umsetzung 2026-05-05:
- Legal-Content-Test ergaenzt und gruen: `npm test -- --configLoader runner src/lib/legal-content.test.ts` mit 3 Tests.
- Customer-Web Tests gruen: `npm test -- --configLoader runner` mit 12 Testdateien / 91 Tests.
- Customer-Web Build gruen: `npm run build`; `/agb` und `/datenschutz` werden statisch prerendered.
- Customer-Web Lint gruen: `npm run lint`; Next.js meldet nur die bekannte `next lint` Deprecation, keine Warnungen oder Fehler.
- Statischer Build-Smoke gegen `.next/server/app/agb.html`, `.next/server/app/datenschutz.html` und `.next/server/app/index.html`: Rechtstexte und Footer-Links sichtbar.
- Keine offenen technischen Blocker.
