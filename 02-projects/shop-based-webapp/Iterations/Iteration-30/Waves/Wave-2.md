# Wave 2 - Service und Persistenz fuer Kapazitaet und Anfrageoption

**Iteration:** [[Iteration-30]]
**Skill:** service-eng
**Status:** Done

## Ziel

Backend-Domain, Service-Logik und PostgreSQL-Repository tragen die neuen Camp-Kapazitaetsfelder durch alle Camp-Event-Use-Cases. Zusaetzlich akzeptiert der Booking-Service die Anfrageoption "Gruppenanfrage / Individualanfrage", ohne sie als sichtbares Pricing-Paket zu behandeln.

## Agent

Backend Service Engineer: Fokus auf Domain-Regeln, Use-Case-Kapselung, Repository-Vertraege, explizite Fehlerbehandlung und stabile Business-Contracts.

## Tasks

- [x] `internal/camps/domain/entities.go` erweitern: `CampEvent` erhaelt optionale Kapazitaetsfelder.
- [x] Domain-Validierung fuer Kapazitaet ergaenzen: keine negativen Werte, freie Plaetze nicht groesser als Gesamtplaetze, unvollstaendige Eingaben bewusst behandeln.
- [x] `internal/camps/app/service.go` erweitern: `CreateCampEventInput`, Create/Update-Mapping und Validation fuer Kapazitaet.
- [x] `internal/camps/adapters/repository/postgres/camp_event_repository.go` erweitern: SELECT, INSERT und UPDATE lesen/schreiben neue Spalten.
- [x] Package-/Anfrageoption-Validierung im Booking-Service so anpassen, dass "Gruppenanfrage / Individualanfrage" als Anfrage-Slug akzeptiert wird.
- [x] Sicherstellen, dass die neue Anfrageoption nicht in regulaere Public-Paketdefinitionen oder Preislogik fuer sichtbare Pakete gedrueckt wird.
- [x] Preis-/Snapshot-Verhalten fuer die Anfrageoption festlegen: erwartbar `0 EUR` oder interner neutraler Wert, ohne kundenseitigen Preisversprechen.
- [x] Bestehende Service-Tests fuer Camp Events und Bookings erweitern.

## Akzeptanzkriterien

- [x] Camp-Event-Domain und Service koennen optionale Kapazitaetsfelder erstellen und aktualisieren.
- [x] Ungueltige Kapazitaetswerte liefern kontrollierte Invalid-Input-Fehler.
- [x] Repository-Operationen verlieren Kapazitaetswerte weder bei List/Get noch bei Create/Update.
- [x] Eine Booking-Anfrage mit der neuen Anfrageoption wird akzeptiert.
- [x] Bestehende Paket-Slugs und Aliases funktionieren unveraendert.
- [x] Die neue Anfrageoption beeinflusst keine regulaere Paketempfehlung, keinen Camp-Preisoverride und keine Pricing-Liste.
- [x] Relevante Go-Service-/Repository-nahe Tests decken Happy Path und Invalid Cases ab.

## Abhaengigkeiten

Wave 1 muss die Migrationsbasis fuer die neuen Camp-Event-Spalten bereitstellen.

## Notes

Die Anfrageoption soll fachlich als Anfrageklassifikation behandelt werden. Sie darf technisch nur so weit als Package-Slug existieren, wie es fuer bestehende Booking-Persistenz und Admin-Lesbarkeit notwendig ist.

Umsetzung 2026-04-27:
- `CampEvent` und `CreateCampEventInput` um `CapacityAvailable` und `CapacityTotal` erweitert.
- Domain-Validierung ergaenzt: keine negativen Werte; bei vollstaendig gepflegter Kapazitaet darf `capacity_available` nicht groesser als `capacity_total` sein. Teilweise gepflegte Werte bleiben erlaubt und werden spaeter kundenseitig nicht angezeigt.
- `camp_event_repository.go` liest und schreibt die neuen Spalten in GetNextVisible, GetByID, List, Create und Update.
- `booking_repository.go` uebernimmt die Kapazitaetsfelder auch fuer eingebettete Camp Events in Booking-Responses.
- Neue anfrage-only Option `group-individual` mit Name `Gruppenanfrage / Individualanfrage`, `0 EUR` Snapshot-Wert und Slug-Aliases ergaenzt.
- Booking verwendet `resolveBookingPackage`, Camp-Empfehlungen verwenden weiter `resolvePackage`; dadurch bleibt die Anfrageoption aus regulaeren Empfehlungen ausgeschlossen.
- Service-Tests ergaenzt fuer Kapazitaets-Happy-Path, ungueltige Kapazitaet, anfrage-only Booking und Ablehnung als empfohlenes Paket.
- Verifikation: `go test ./internal/camps/app` gruen.
