# Wave 1 — Connection-Setup & Shopify-Client

**Iteration:** [[Iteration-2]]
**Skill:** handler-eng
**Status:** In Progress

## Tasks

- [x] Connection-Formular UI — `app/(dashboard)/connections/new/page.tsx`
- [x] Server Action: Connection speichern mit AES-Encryption für API-Key — `app/actions/connections.ts`
- [ ] Shopify-Client-Wrapper (fetch + retry + rate-limit) — `lib/shopify/client.ts`
- [ ] Shopify-Typen für Produkt-Response — `lib/shopify/types.ts`
- [ ] "Connection testen" Button + Server Action (GET /products.json?limit=1) — `app/actions/connections.ts`
- [ ] Connections-Übersichtsseite mit Status-Badge — `app/(dashboard)/connections/page.tsx`

## Done Criteria

- [ ] User kann Shopify-Connection anlegen
- [ ] API-Key wird verschlüsselt in DB gespeichert
- [ ] "Testen" gibt Success/Error-Feedback in der UI
- [ ] Shopify-Client-Wrapper handelt 429-Responses mit exponential backoff

## Notes

AES-Encryption-Key aus `ENCRYPTION_KEY` Env-Variable (32 Bytes, Base64). Noch kein Key-Rotation-Konzept für MVP.
