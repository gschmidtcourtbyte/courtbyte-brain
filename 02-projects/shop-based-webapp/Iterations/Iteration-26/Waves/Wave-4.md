# Wave 4 - HTTP Contract und Frontend API Types

**Iteration:** [[Iteration-26]]
**Skill:** handler-eng
**Status:** Todo

## Ziel

Der HTTP-Contract akzeptiert und liefert Paketinteressen. Frontend-Types und API-Dokumentation sind mit dem Runtime-Verhalten synchron.

## Tasks

- [ ] `internal/camps/adapters/http/handler.go`: Request DTO um `package_interest_slugs` erweitern.
- [ ] Handler mappt `package_interest_slugs` in den Service-Input.
- [ ] Booking Response enthaelt Paketinteressen inklusive Slug-Liste und, falls lokal sinnvoll, Paketnamen.
- [ ] `web/customer/src/types/api.ts`: `CampBookingRequest` und `CampBooking` erweitern.
- [ ] `docs/api/openapi.yaml`: Camp-Booking Request/Response dokumentieren.
- [ ] Handler-Tests fuer Single- und Multi-Package-Request ergaenzen.

## Akzeptanzkriterien

- [ ] Clients koennen weiterhin nur `package_slug` senden.
- [ ] Clients koennen zusaetzlich `package_interest_slugs` senden.
- [ ] Response enthaelt die gespeicherten Paketinteressen.
- [ ] OpenAPI ist mit Request/Response synchron.
- [ ] Fehlerfaelle fuer ungueltige Paketinteressen sind getestet.

## Abhaengigkeiten

Wave 3.

## Notes

Namen konsistent halten. Falls Backend intern eine andere Spaltenbezeichnung nutzt, sollte die API trotzdem lesbar bleiben: `package_interest_slugs`.
