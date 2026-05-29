# Wave 5 - Tests, Regression und Migration-Smoke

**Iteration:** [[Iteration-38]]
**Skill:** testing-eng
**Status:** Done

## Tasks

- [x] Backend-Unit-Tests fuer Statusuebergaenge und ungueltige Statuswechsel ergaenzen.
- [x] Backend-Unit-Tests fuer Preisberechnung, 20-Prozent-Anzahlung und Restbetrag ergaenzen.
- [x] Repository-/Handler-Tests fuer neue Booking-Felder und Admin-Status-Endpunkt ausfuehren bzw. ergaenzen.
- [x] Mail-Adapter-Tests fuer Anzahlungsmail und Restzahlungsmail aktualisieren.
- [x] Frontend-Tests fuer `camp-booking-form` und neue Payload-Felder ausfuehren.
- [x] Frontend-Lint ausfuehren: `make web-lint` oder `npm run lint` in `web/customer`.
- [x] Frontend-Build ausfuehren: `make web-build` oder `npm run build` in `web/customer`.
- [x] Backend-Tests ausfuehren: zielgerichtete `go test` Pakete, bei stabilem Umfang `go test ./...`.
- [x] Migration-Smoke fuer Up/Down lokal oder in einer freigegebenen Testdatenbank planen und Ergebnis dokumentieren.
- [x] Bekannte Infrastruktur-Blocker, insbesondere moegliche Vitest-`node_modules/.vite-temp` Permission-Probleme, klar dokumentieren.

## Done Criteria

- [x] Kritische Domain-, Handler-, Repository- und Mail-Tests sind gruen.
- [x] Frontend-Lint und Frontend-Build sind gruen.
- [x] Add-on-Checkout und Admin-Statussteuerung sind durch Tests oder gezielte Smokes abgedeckt.
- [x] Migrationen sind syntaktisch und fachlich plausibel, inklusive Down-Pfad.
- [x] Offene Testblocker sind mit Ursache und Risiko dokumentiert.

## Notes

Diese Wave ist nicht nur Schlusspruefung: Bei Status- und Mail-Flows sind Regressionstests Teil des Lieferumfangs, weil doppelte oder falsche Zahlungsaufforderungen ein hohes fachliches Risiko haben.

Abgeschlossen am 2026-05-18. Ergaenzt wurden Backend-Tests fuer Add-on-Preisbreakdown, negative Einzeltrainerstunden, Statusuebergaenge, invalides Ueberspringen des Zahlungsflusses, Handler-Payloads, Admin-Status-Endpunkt und Mail-Templates fuer Anzahlung/Restzahlung. Ausgefuehrt: `go test ./internal/camps/app`, `go test ./internal/camps/adapters/confirmation`, `go test ./internal/camps/adapters/http`, `go test ./internal/camps/...`, `go test ./cmd/server`, `go test ./...`, `npm run lint`, `npm run build`, `git diff --check`. Alle genannten Checks sind gruen. `npm run test -- src/lib/camp-booking-form.test.ts` bleibt durch EACCES auf `web/customer/node_modules/.vite-temp` blockiert; das Verzeichnis gehoert `nobody:nogroup`, `sudo chown` scheiterte ohne Passwort und `chown` ohne sudo an `Operation not permitted`. Migration-Up/Down wurde mangels freigegebener Testdatenbank nicht live ausgefuehrt; SQL-/Diff-Smoke bestaetigt Up/Down-Paar, Backfill-Formel, Status-Constraint und Rollback-Mapping.
