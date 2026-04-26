# Wave 2 — DB-Schema & Dashboard-Shell

**Iteration:** [[Iteration-1]]
**Skill:** database-eng
**Status:** Done

## Tasks

- [x] Migration: Tabelle `connection` (id, user_id, type, credentials_enc, created_at) — `supabase/migrations/001_connection.sql`
- [x] Migration: Tabelle `product` (id, connection_id, external_id, name, price, stock, synced_at) — `supabase/migrations/002_product.sql`
- [x] Migration: Tabelle `sync_log` (id, product_id, direction, status, error, created_at) — `supabase/migrations/003_sync_log.sql`
- [x] RLS-Policies für alle Tabellen — `supabase/migrations/004_rls.sql`
- [x] Dashboard-Layout mit Sidebar — `app/(dashboard)/layout.tsx`
- [x] Sidebar-Komponente — `components/sidebar.tsx`
- [x] Leere Seiten: `/dashboard`, `/products`, `/connections`, `/logs` — je `page.tsx`
- [x] Supabase-Typen generieren — `types/supabase.ts`

## Done Criteria

- [x] `supabase db push` läuft ohne Fehler
- [x] Dashboard-Layout rendert mit Sidebar
- [x] Alle Navigations-Links funktionieren

## Notes

`credentials_enc` wird in Iteration 2 mit Vault-Encryption befüllt — vorerst leer gelassen. RLS: jeder User sieht nur eigene Rows via `auth.uid() = user_id`.
