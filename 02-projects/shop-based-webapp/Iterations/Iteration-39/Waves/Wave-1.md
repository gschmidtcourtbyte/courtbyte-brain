# Wave 1 - Add-on-Pricing aktivieren

**Iteration:** [[Iteration-39]]
**Skill:** service-eng
**Status:** Done

## Tasks

- [x] Bestehende Camp-Add-on-Preislogik aus Iteration 38 lokalisieren und die vorlaeufigen Cent-Werte zentral setzen.
- [x] Flughafentransfer mit `5000` Cents, Vollpension mit `40000` Cents und Einzeltrainerstunden mit `3000` Cents je Stunde in der Campsumme beruecksichtigen.
- [x] Snapshot-Werte fuer Add-on-Summe, Gesamtpreis, 20-Prozent-Anzahlung und Restbetrag gegen die neuen Preise pruefen.
- [x] Sicherstellen, dass Anzahlungs- und Restzahlungs-Mail-Trigger weiter Snapshot-Betraege verwenden.
- [x] Backend-Tests fuer Preisberechnung und relevante Booking-/Mail-Pfade aktualisieren.

## Done Criteria

- [x] Neue Camp-Anfragen speichern die Add-on-Preiswerte und resultierenden Summen nachvollziehbar.
- [x] Anfrage ohne Add-ons bleibt preislich kompatibel.
- [x] Pricing-Tests decken Transfer, Vollpension, Einzelstunden und kombinierte Summen ab.

## Notes

Die Preise sind fachlich vorlaeufige Content-Werte. Alte Buchungs-Snapshots duerfen nicht nachtraeglich neu berechnet werden.

Umsetzung:
- `internal/camps/app/service.go` setzt Add-on-Preise zentral auf Transfer `5000`, Vollpension `40000` und Einzeltrainerstunde `3000` Cents.
- Der vorhandene Service-Test fuer Add-on-Snapshots prueft jetzt Gesamtpreis `233900`, Anzahlung `46780` und Restzahlung `187120` fuer Single plus alle drei Add-ons.
- Snapshot-Mail-Verhalten bleibt in den vorhandenen Confirmation-Tests unveraendert.

Verifikation:
- `go test ./internal/camps/app`
- `go test ./internal/camps/adapters/confirmation`
