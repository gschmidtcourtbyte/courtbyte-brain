# Wave 2 — Import-Pipeline & DB-Write

**Iteration:** [[Iteration-2]]
**Skill:** service-eng
**Status:** Todo

## Tasks

- [ ] Shopify-Produkt-Fetcher (paginiert, cursor-based) — `lib/shopify/fetch-products.ts`
- [ ] Produkt-Mapper: Shopify → ShopSync-Schema — `lib/shopify/product-mapper.ts`
- [ ] Unit-Tests für Mapper — `lib/shopify/product-mapper.test.ts`
- [ ] Upsert-Funktion: Produkte in `product`-Tabelle schreiben — `lib/db/upsert-products.ts`
- [ ] sync_log-Einträge bei Erfolg und Fehler schreiben — `lib/db/write-sync-log.ts`
- [ ] Server Action: Import starten (ruft Fetcher + Mapper + Upsert) — `app/actions/import.ts`
- [ ] Import-Button auf Connections-Detailseite — `app/(dashboard)/connections/[id]/page.tsx`

## Done Criteria

- [ ] Import liest alle Produkte (auch >250 via Pagination)
- [ ] Mapper-Tests grün
- [ ] Jeder Import-Run schreibt sync_log-Einträge
- [ ] Fehler in einem Produkt brechen den Gesamt-Import nicht ab

## Notes

Shopify pagination via `page_info` Cursor — nicht offset-basiert. Max 250 Produkte pro Request.
