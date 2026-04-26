# ShopSync — Projektplan

**Status:** In Progress
**Repo:** /path/to/shopsync (Git-Repo, Option A)
**Stack:** Next.js 14, Supabase, Tailwind CSS, TypeScript
**Ziel:** Web-App zum Synchronisieren von Produktdaten zwischen mehreren Online-Shops

---

## Scope

ShopSync erlaubt es Shop-Betreibern, Produkte (Name, Preis, Lagerstand, Bilder) aus einer Quelle zu importieren und automatisiert in verbundene Ziel-Shops zu pushen. MVP fokussiert auf Shopify als Quelle und WooCommerce als Ziel.

**In:**
- Produkt-Import aus Shopify via API
- Produkt-Export zu WooCommerce via API
- Mapping-Konfiguration (welches Feld geht wohin)
- Dashboard mit Sync-Status pro Produkt
- User-Authentifizierung (Supabase Auth)

**Out (MVP):**
- Bestand-Sync in Echtzeit (Webhooks — Iteration 3+)
- Weitere Shop-Systeme (Magento, OXID)
- Mobile App

---

## Iterationsplan

| Iteration | Ziel | Status |
|---|---|---|
| Iteration 1 | Foundation: Auth, DB, Grundgerüst UI | Done |
| Iteration 2 | Core: Import-Pipeline & Mapping | In Progress |
| Iteration 3 | Polish: Export, Notifications, Deploy | Todo |

---

## Tech-Entscheidungen

- **Supabase** für Auth + Postgres-DB — kein eigener Auth-Server nötig
- **Next.js App Router** — Server Components für Dashboard-Ansichten
- **Tailwind + shadcn/ui** — keine eigene Komponentenbibliothek bauen
- **Shopify Admin API 2024-01** — stabile Version für MVP
