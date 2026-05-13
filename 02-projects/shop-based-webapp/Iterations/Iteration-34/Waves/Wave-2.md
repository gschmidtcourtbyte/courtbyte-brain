# Wave 2 - Runtime- und API-Ladepfade entlasten

**Iteration:** [[Iteration-34]]
**Skill:** service-eng
**Status:** Done

## Tasks

- [x] Oeffentliche Camp-Event-Endpoints mit kurzer Cache-Control versehen - `internal/camps/adapters/http/handler.go`
- [x] Cache-Header in Handler-Tests absichern - `internal/camps/adapters/http/handler_test.go`
- [x] Next-API-Proxy mit kontrolliertem Timeout absichern - `web/customer/src/app/api/[...path]/route.ts`
- [x] Browser-API-Client mit Request-Timeout versehen - `web/customer/src/lib/api-client.ts`
- [x] Navigation auf oeffentlichen Seiten ohne Auth-Ladeskeleton rendern - `web/customer/src/components/Navigation.tsx`

## Done Criteria

- [x] Langsame Backend-Calls koennen nicht dauerhaft den Next-Proxy blockieren.
- [x] Oeffentliche Camp-Daten koennen von Browser/Edge kurzzeitig wiederverwendet werden.
- [x] Ausgeloggte Nutzer sehen keine Auth-Ladezustands-Regressions.

## Notes

Ein Teil dieser Wave wurde bereits vor Iterationsanlage als Untersuchungsfix vorbereitet und wird in diese Iteration uebernommen.

Umgesetzt am 2026-05-12: oeffentliche Camp-Event-Endpunkte liefern `Cache-Control: public, max-age=60, stale-while-revalidate=300`. Der Next-Proxy bricht Upstream-Calls nach 5 Sekunden mit `504 api_proxy_timeout` ab, der Browser-API-Client setzt fuer Requests standardmaessig 15 Sekunden Timeout, und die oeffentliche Navigation rendert waehrend Auth-Loading keine Skeleton-Platzhalter mehr.
