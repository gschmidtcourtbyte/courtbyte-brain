# Wave 3 - API, Handler und OpenAPI

**Iteration:** [[Iteration-38]]
**Skill:** handler-eng
**Status:** Done

## Tasks

- [x] `internal/camps/adapters/http/handler.go`: `createBookingRequest` um Add-on-Felder erweitern:
  - `airport_transfer_requested`
  - `full_board_requested`
  - `private_training_hours`
- [x] `internal/camps/adapters/http/handler.go`: `bookingResponse` um Add-on- und Preis-Snapshot-Felder erweitern.
- [x] Admin-Endpunkt fuer generische Statusaenderung ergaenzen, z. B. `PATCH /api/v1/admin/camps/bookings/{id}/status` mit Body `{ "status": "..." }`.
- [x] Bestehende Endpunkte `/confirm` und `/confirm-payment` entweder kompatibel lassen und intern auf den neuen Statuswechsel mappen oder sauber als legacy behandeln.
- [x] Request-Validierung und Fehler-Mapping fuer ungueltige Statuswerte und ungueltige Add-on-Eingaben sicherstellen.
- [x] `docs/api/openapi.yaml`: CampBookingRequest, CampBookingResponse, Status-Enum und Admin-Status-Endpunkt aktualisieren.
- [x] Handler-Tests fuer Add-on-Payload, Response-Felder und Statuswechsel-Endpoint ergaenzen.

## Done Criteria

- [x] Public Booking API akzeptiert optionale Add-ons und gibt gespeicherte Werte zurueck.
- [x] Admin API kann alle geplanten Statuswerte setzen, sofern der Domain-Statusfluss es erlaubt.
- [x] Ungueltige Statuswechsel liefern 422/Problem-Response statt stiller Mutation.
- [x] OpenAPI beschreibt neue Felder, Statuswerte und Mail-ausloesende Statuswechsel ausreichend fuer Frontend-Implementierung.
- [x] Bestehende Admin-Buttons brechen nicht, solange das Frontend noch nicht migriert ist.

## Notes

Die Handler-Wave soll schlank bleiben: HTTP validiert und mappt, Statuslogik und Mailentscheidung bleiben im Service.

Abgeschlossen am 2026-05-18. `createBookingRequest` und `bookingResponse` enthalten Add-ons und Preis-Snapshots. Neuer Admin-Endpunkt: `PATCH /api/v1/admin/camps/bookings/{id}/status` mit `{ "status": "..." }`. Der bestehende Legacy-Endpunkt `/confirm-payment` mappt fuer alte Admin-Clients auf `deposit_paid`, damit der neue Restzahlungsprozess nicht uebersprungen wird. OpenAPI wurde fuer Request-/Response-Schemas, Status-Enum und den Status-Endpunkt aktualisiert. Verifikation: `go test ./internal/camps/adapters/http` und `go test ./internal/camps/...` gruen.
