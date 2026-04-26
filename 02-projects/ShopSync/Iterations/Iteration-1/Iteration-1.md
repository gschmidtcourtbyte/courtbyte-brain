# Iteration 1 — Foundation

**Status:** Done
**Repo:** /path/to/shopsync
**Datum:** 2026-04-01

## Ziel

Grundgerüst der App aufbauen: Authentifizierung, Datenbankschema, Navigation und leere Dashboard-Shell. Am Ende kann sich ein User einloggen und sieht das Dashboard (noch ohne Daten).

## Scope

**In:**
- Supabase-Projekt aufsetzen (Auth, DB)
- Next.js-Projekt initialisieren (App Router, Tailwind, shadcn/ui)
- Login / Logout Flow
- DB-Schema: `user`, `connection`, `product`, `sync_log`
- Dashboard-Layout mit Sidebar-Navigation
- Leere Placeholder-Seiten für: Products, Connections, Logs

**Out:**
- Echte API-Verbindungen (kommt Iteration 2)
- Datenbefüllung

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | handler-eng | Projekt-Setup & Auth | Done |
| Wave 2 | database-eng | DB-Schema & Dashboard-Shell | Done |

## Acceptance Criteria

- [x] User kann sich registrieren und einloggen
- [x] Nach Login landet User auf /dashboard
- [x] Sidebar zeigt Navigation zu allen Hauptseiten
- [x] Supabase-Migrations laufen lokal durch
- [x] TypeScript baut ohne Fehler
