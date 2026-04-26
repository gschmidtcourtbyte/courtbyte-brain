# Wave 1 — WooCommerce-Client & Export-Pipeline

**Iteration:** [[Iteration-3]]
**Skill:** service-eng
**Status:** Todo

## Tasks

- [ ] WooCommerce-Client-Wrapper (Basic Auth, retry) — `lib/woocommerce/client.ts`
- [ ] WooCommerce-Typen für Produkt-Request/Response — `lib/woocommerce/types.ts`
- [ ] Produkt-Mapper: ShopSync-Schema → WooCommerce — `lib/woocommerce/product-mapper.ts`
- [ ] Unit-Tests für WooCommerce-Mapper — `lib/woocommerce/product-mapper.test.ts`
- [ ] Export-Funktion: create or update Produkt via REST — `lib/woocommerce/export-product.ts`
- [ ] Server Action: Export starten für eine Connection — `app/actions/export.ts`
- [ ] Export-Button in Products-Tabelle (pro Produkt) — `app/(dashboard)/products/page.tsx`
- [ ] WooCommerce-Connection-Typ in Connection-Formular ergänzen — `app/(dashboard)/connections/new/page.tsx`

## Done Criteria

- [ ] Produkt aus Shopify-Import kann zu WooCommerce exportiert werden
- [ ] Mapper-Tests grün
- [ ] Export schreibt sync_log-Einträge (Richtung: "export")
- [ ] Fehler beim Export brechen andere Produkte nicht ab

## Notes

WooCommerce REST API v3. Authentifizierung via Consumer Key + Secret (Basic Auth über HTTPS). Produkt-Matching via `sku` — falls SKU existiert: UPDATE, sonst: CREATE.
