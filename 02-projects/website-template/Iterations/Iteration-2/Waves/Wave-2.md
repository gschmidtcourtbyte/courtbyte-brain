# Iteration 2 — Wave 2: Admin Persistence

**Skill:** migration-eng, database-eng
**Status:** Done

## Tasks

- [x] Migration für `admin_users`
- [x] Migration für `admin_audit_log`
- [x] Idempotenter Migration Runner im Backend
- [x] Admin-Bootstrap aus Environment Variables

## Acceptance

- Backend-Migrationen laufen beim Railway-Start idempotent gegen Postgres.
- Admin-Daten liegen in PostgreSQL, nicht in Code oder Frontend.
