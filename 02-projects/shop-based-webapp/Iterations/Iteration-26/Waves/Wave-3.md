# Wave 3 - Backend-Service, Repository, Admin und Mail fuer Paketinteressen

**Iteration:** [[Iteration-26]]
**Skill:** service-eng
**Status:** Todo

## Ziel

Der Backend-Service validiert und speichert mehrere Paketinteressen. Admin- und Mail-Kontexte koennen diese Interessen anzeigen, ohne alte Buchungen zu brechen.

## Tasks

- [ ] `internal/camps/domain/entities.go`: `CampBooking` um Paketinteressen-Feld erweitern.
- [ ] `internal/camps/app/service.go`: `CreateBookingInput` um Paketinteressen erweitern.
- [ ] Service validiert Paketinteressen gegen bekannte Paket-Slugs, entfernt Duplikate und stellt sicher, dass mindestens ein Paket existiert.
- [ ] Wenn keine Paketinteressen uebergeben werden, wird das Primaerpaket als einziges Interesse verwendet.
- [ ] `internal/camps/adapters/repository/postgres/booking_repository.go`: Spalte schreiben und lesen.
- [ ] Admin-/Mail-Kontexte in `internal/camps/adapters/confirmation/email_sender.go` so erweitern, dass Paketinteressen sichtbar sind.

## Akzeptanzkriterien

- [ ] Bestehender Single-Package-Request bleibt gueltig.
- [ ] Multi-Package-Request speichert alle validen Paketinteressen.
- [ ] Ungueltige Paket-Slugs werden abgelehnt.
- [ ] Doppelte Paket-Slugs werden normalisiert.
- [ ] Response/Admin/Mail koennen Paketinteressen ausgeben.
- [ ] Bestehende Tests fuer Single-Package-Buchungen bleiben gruen.

## Abhaengigkeiten

Wave 2.

## Notes

Kein Gesamtpreis fuer mehrere Paketinteressen berechnen. Preise bleiben pro Paketlabel; `price_cents` des Primaerpakets bleibt Snapshot fuer Backward Compatibility.
