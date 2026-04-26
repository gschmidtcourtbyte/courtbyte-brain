# Wave 1 — Projekt-Setup & Auth

**Iteration:** [[Iteration-1]]
**Skill:** handler-eng
**Status:** Done

## Tasks

- [x] `npx create-next-app` mit TypeScript, Tailwind, App Router — `./`
- [x] shadcn/ui initialisieren — `components.json`
- [x] Supabase-Projekt anlegen, `.env.local` mit Keys befüllen
- [x] `@supabase/ssr` installieren, Server- und Browser-Client konfigurieren — `lib/supabase/`
- [x] Login-Seite bauen — `app/(auth)/login/page.tsx`
- [x] Signup-Seite bauen — `app/(auth)/signup/page.tsx`
- [x] Auth-Callback Route — `app/auth/callback/route.ts`
- [x] Middleware für geschützte Routen — `middleware.ts`
- [x] Logout-Action — `app/actions/auth.ts`

## Done Criteria

- [x] Login/Logout funktioniert end-to-end
- [x] Nicht-eingeloggte User werden auf /login redirectet
- [x] Kein TypeScript-Fehler

## Notes

shadcn/ui v2 verwendet jetzt `components/ui/` statt `@/components/ui` — Imports entsprechend angepasst.
