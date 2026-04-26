# Iteration 3 — Polish: Export, Notifications & Deployment

**Status:** Todo
**Repo:** /path/to/shopsync
**Datum:** geplant ab 2026-05-01

## Ziel

Die App produktionsreif machen: WooCommerce-Export implementieren, E-Mail-Benachrichtigungen bei Sync-Fehlern, Deployment auf Vercel. Am Ende ist ShopSync öffentlich erreichbar und in einer echten Shop-Umgebung einsetzbar.

## Scope

**In:**
- WooCommerce REST API: Produkte anlegen / updaten
- Export-Pipeline (ShopSync-Schema → WooCommerce)
- E-Mail-Notification bei sync_log-Fehler (Resend)
- Vercel-Deployment mit Supabase-Production-Env
- Error-Boundary und 404-Seiten
- Performance: Tabellen-Queries mit Index optimieren

**Out:**
- Automatischer Cron-Sync (nächste Iteration)
- Multi-User / Team-Features
- Custom Mapping UI (Drag & Drop)

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | service-eng | WooCommerce-Client & Export-Pipeline | Todo |
| Wave 2 | infrastructure-eng | Notifications & Production-Hardening | Todo |

## Acceptance Criteria

- [ ] Importierte Produkte können zu WooCommerce exportiert werden
- [ ] Bei Sync-Fehlern erhält der User eine E-Mail
- [ ] App ist auf Vercel deployed und läuft stabil
- [ ] Alle DB-Queries haben passende Indices
- [ ] TypeScript baut ohne Fehler, keine offenen ESLint-Warnings
