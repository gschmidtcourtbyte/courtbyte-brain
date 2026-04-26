# Wave 2 — Notifications & Production-Hardening

**Iteration:** [[Iteration-3]]
**Skill:** infrastructure-eng
**Status:** Todo

## Tasks

- [ ] Resend installieren, API-Key in Env — `lib/email/client.ts`
- [ ] E-Mail-Template für Sync-Fehler-Report — `lib/email/templates/sync-error.tsx`
- [ ] Nach Import/Export: wenn sync_log Fehler enthält, E-Mail senden — `app/actions/import.ts`, `app/actions/export.ts`
- [ ] DB-Indices: `product(connection_id)`, `sync_log(product_id, created_at)` — `supabase/migrations/005_indices.sql`
- [ ] Error-Boundary für Dashboard — `app/(dashboard)/error.tsx`
- [ ] 404-Seite — `app/not-found.tsx`
- [ ] `next.config.ts` für Produktion konfigurieren (CSP Headers, Image Domains)
- [ ] Vercel-Projekt anlegen, Environment Variables setzen
- [ ] Supabase-Production-Projekt anlegen, Migrations deployen
- [ ] Deploy und E2E-Smoketest: Login → Import → Export

## Done Criteria

- [ ] Sync-Fehler-E-Mail kommt an
- [ ] Vercel-Deploy ist grün
- [ ] Supabase-Production läuft mit RLS
- [ ] Lighthouse Performance Score > 80

## Notes

Resend kostenlos bis 3.000 Mails/Monat — reicht für MVP. E-Mail nur bei Fehlern, nicht bei jedem erfolgreichen Sync (würde sonst spammen).
