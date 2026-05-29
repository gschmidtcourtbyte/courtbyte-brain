# Wave 2 - Domain, Preisberechnung und Zahlungs-E-Mails

**Iteration:** [[Iteration-38]]
**Skill:** service-eng
**Status:** Done

## Tasks

- [x] `internal/camps/domain/entities.go`: `CampBooking` um Add-on- und Preis-Snapshot-Felder erweitern.
- [x] `internal/camps/domain/entities.go`: Statuskonstanten um `StatusDepositPaid` und `StatusBalanceRequested` erweitern.
- [x] `internal/camps/domain/entities.go`: `ValidateStatusTransition` fuer den neuen Fluss anpassen:
  - `pending` -> `confirmed`
  - `confirmed` -> `deposit_paid`
  - `deposit_paid` -> `balance_requested`
  - `balance_requested` -> `paid`
  - jeder Status -> `cancelled`
- [x] `internal/camps/app/service.go`: `CreateBookingInput` um Add-on-Felder erweitern und validieren.
- [x] `internal/camps/app/service.go`: Preisberechnung kapseln:
  - Basispreis aus bestehender Paket-/Camp-Event-Logik.
  - Add-on-Preise initial als zentrale Konstanten oder interne Konfiguration mit Wert `0`.
  - `total_price_cents`, `deposit_amount_cents` und `balance_amount_cents` berechnen.
- [x] `internal/camps/ports/repositories.go` und Postgres-Repository anpassen, damit neue Felder gespeichert und gelesen werden.
- [x] Bestehende `ConfirmBooking`-Logik fachlich als Anfrage-bearbeitet/Anzahlung-ausloesen weiterverwenden oder in allgemeine Statuswechsel-Logik ueberfuehren.
- [x] Neuen Service-Use-Case fuer Admin-Statuswechsel ergaenzen, damit Status und Mail-Trigger zentral gesteuert sind.
- [x] `internal/camps/ports/services.go`: ConfirmationSender um Restzahlungsaufforderung erweitern; falls noetig Anzahlungsmail explizit benennen.
- [x] `internal/camps/adapters/confirmation/email_sender.go`: Anzahlungsmail mit Gesamtbetrag, 20-Prozent-Anzahlung, Restbetrag, Bankdaten und Referenz rendern.
- [x] `internal/camps/adapters/confirmation/email_sender.go`: Restzahlungsmail mit Restbetrag, Gesamtbetrag, bereits angeforderter/geleisteter Anzahlung und Bankdaten rendern.
- [x] `internal/email/app/templates.go`: Templates fuer Anzahlungs- und Restzahlungsmail aktualisieren bzw. ergaenzen.
- [x] Dev-Logging-Sender in `cmd/server/runtime_adapters.go` an neue Sender-Methoden anpassen.

## Done Criteria

- [x] Add-on-Auswahl wird beim Erstellen einer Buchung validiert und im Domain-Modell abgebildet.
- [x] Preisberechnung ist deterministisch getestet und rundet die 20-Prozent-Anzahlung centgenau auf.
- [x] Statuswechsel sind serverseitig eindeutig und verhindern das Ueberspringen kritischer Zahlungsschritte.
- [x] `confirmed` sendet die Anzahlungsmail, `balance_requested` sendet die Restzahlungsmail.
- [x] Mail-Fehler werden wie bisher geloggt, ohne den Statuswechsel unkontrolliert doppelt auszufuehren.

## Notes

Der zentrale Entwurfspunkt ist ein allgemeiner Admin-Statuswechsel statt weiterer Einzelfunktionen fuer jeden Button. Dadurch bleibt der Mail-Trigger im Service testbar und das Frontend kann eine Statusauswahl anbieten.

Abgeschlossen am 2026-05-18. Domain und Service enthalten jetzt Add-on-Felder, Preis-Snapshots, Status `deposit_paid`/`balance_requested`, strikte Statusuebergaenge und einen generischen `UpdateBookingStatus`-Use-Case. `ConfirmBooking` fuehrt auf `confirmed` und sendet die 20-Prozent-Anzahlungsmail; `balance_requested` sendet die Restzahlungsmail; `paid` bleibt Abschluss-/Bestaetigungsmail. Postgres-Repository speichert und liest die neuen Felder. E-Mail-Templates und Dev-Logging-Sender wurden angepasst. Verifikation: `go test ./internal/camps/...` und `go test ./cmd/server` gruen.
