# Wave 3 - Pricing- und Frontend-Regression

**Iteration:** [[Iteration-39]]
**Skill:** testing-eng
**Status:** Done

## Tasks

- [x] Relevante Go-Tests fuer Camp-Pricing, Booking-Status und Zahlungs-Mail-Pfade ausfuehren.
- [x] Relevante Customer-Web-Tests fuer FAQ-/Camp-Formularlogik aktualisieren oder ausfuehren.
- [x] Frontend-Lint und Frontend-Build ausfuehren.
- [x] UI-Smoke fuer FAQ, Instagram-Link, optionalen Checkout-Step und Admin-Preislesbarkeit dokumentieren.
- [x] Bekannte Testinfrastruktur-Blocker klar dokumentieren.

## Done Criteria

- [x] Backend- und Frontend-Regression sind gruen oder mit konkretem Blocker dokumentiert.
- [x] FAQ-, Social- und Add-on-Pricing-Akzeptanzkriterien sind gegen den Code-Stand pruefbar.

## Notes

Der bekannte Vitest-Permission-Blocker aus Iteration 37/38 ist erneut zu pruefen, nicht stillschweigend zu ignorieren.

Verifikation am 2026-05-21:
- Backend gruen mit `go test ./internal/camps/app ./internal/camps/adapters/http ./internal/camps/adapters/confirmation ./internal/email/app`.
- Relevante Frontend-Tests gruen mit `npm test -- --configLoader runner src/lib/social-links.test.ts src/lib/faq-content.test.ts`; die volle Suite bestaetigt zusaetzlich die gruenen Camp-Formular-Tests.
- Frontend-Lint und Production-Build gruen mit `npm run lint` und `npm run build`.
- UI-Code-Smoke: FAQ rendert Antworten ohne Accordion-State, Instagram-Konfiguration zeigt auf `padel.out`, Checkout-Step 2 bleibt optional und zeigt Extras-Summe, Admin-Tabelle zeigt Add-on-Preisaufschluesselung neben Gesamt-/Anzahlungs-/Restbetrag.
- Standard-Vitest-Configloader bleibt lokal durch `EACCES` auf `web/customer/node_modules/.vite-temp` blockiert, weil der Ordner `nobody:nogroup` gehoert. Laufweg mit `--configLoader runner` ist gruen.
- Volle Vitest-Suite mit `npm test -- --configLoader runner` endet bei `src/lib/gallery.test.ts`: Test-Expectation driftet gegen vorhandene Gallery-Slides mit `blurDataURL` und aktualisierten Bilddimensionen. Ergebnis: 99/100 Tests gruen, Blocker nicht durch Iteration-39-Aenderungen verursacht.
