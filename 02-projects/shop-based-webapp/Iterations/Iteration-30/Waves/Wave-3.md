# Wave 3 - HTTP Contracts fuer Camp-Kapazitaet und Anfrageoption

**Iteration:** [[Iteration-30]]
**Skill:** handler-eng
**Status:** Done

## Ziel

Die HTTP-Schicht bildet die neuen Camp-Kapazitaetsfelder in Admin-Requests und Public/Admin-Responses ab. Booking-Anfragen mit "Gruppenanfrage / Individualanfrage" werden am API-Rand sauber validiert, gemappt und bei Fehlern zentral als Problem-Response ausgegeben.

## Agent

Handler/API Edge Engineer: Fokus auf DTOs, Request-Parsing, Response-Mapping, HTTP-Fehlerverhalten, CSRF-/Auth-Kontext und Contract-Tests.

## Tasks

- [x] `internal/camps/adapters/http/handler.go` erweitern: `campEventRequest` und `campEventResponse` enthalten die neuen Kapazitaetsfelder.
- [x] `parseCampEventInput` und `toCampEventResponse` fuer Kapazitaet mappen.
- [x] Booking-Request-Pfad pruefen: neue Anfrageoption wird ohne Sonderfall im Handler an den Service uebergeben.
- [x] Handler-Tests fuer Admin Camp Event Create/Update mit gueltiger Kapazitaet ergaenzen.
- [x] Handler-Tests fuer ungueltige Kapazitaet ergaenzen, soweit Fehler am HTTP-Rand oder ueber Service-Fehler sichtbar werden.
- [x] Handler-/API-Test fuer Booking-Anfrage mit neuer Anfrageoption ergaenzen.
- [x] Sicherstellen, dass Public `/api/v1/camp-events` und `/api/v1/camp-events/next` die Kapazitaetsfelder ausliefern.

## Akzeptanzkriterien

- [x] Admin Create/Update akzeptiert gueltige Kapazitaetswerte.
- [x] Admin/Public Camp Event Responses enthalten Kapazitaetsfelder, wenn vorhanden.
- [x] Leere Kapazitaetsfelder bleiben in JSON optional/null und brechen bestehende Clients nicht.
- [x] Ungueltige Kapazitaetswerte werden als Invalid Input und nicht als 500er gemappt.
- [x] Booking-Anfrage mit "Gruppenanfrage / Individualanfrage" liefert eine erfolgreiche Created-Response.
- [x] Bestehende Auth-/Admin- und CSRF-Anforderungen fuer mutierende Routen bleiben unveraendert.

## Abhaengigkeiten

Wave 2 muss Domain- und Service-Felder sowie die Anfrageoption bereitstellen.

## Notes

Die HTTP-Schicht soll keine eigene Business-Logik duplizieren. Sie mappt Typen, prueft Datums-/UUID-Parsing wie bisher und laesst fachliche Validierung im Service bzw. in der Domain.

Umsetzung 2026-04-27:
- `campEventRequest` und `campEventResponse` um `capacity_available` und `capacity_total` erweitert.
- `parseCampEventInput` und `toCampEventResponse` mappen die Kapazitaetsfelder durch.
- Booking-Pfad bleibt ohne Handler-Sonderlogik; die Anfrageoption wird an den Service uebergeben.
- Handler-Tests ergaenzt fuer Public List/Next Responses mit Kapazitaet, Admin Create/Update mit Kapazitaet, Invalid-Input-Mapping und Booking mit `group-individual`.
- Verifikation: `go test ./internal/camps/adapters/http` gruen.
