# ShopSync — Coding Rules

## Architektur

- **App Router only** — kein Pages Router, keine Mischung
- **Server Components by default** — nur Client Components wenn nötig (`"use client"` explizit begründen)
- **API-Calls nur in Server Actions oder Route Handlers** — nie direkt im Client
- **Supabase-Client:** `createServerClient` für Server, `createBrowserClient` für Client

## Code-Stil

- **TypeScript strict mode** — kein `any`, kein `as unknown`
- **Keine barrel exports** (`index.ts`) — direkte Imports bevorzugen
- **Dateinamen:** kebab-case (`product-card.tsx`), Komponenten PascalCase
- **Fehlerbehandlung:** immer explizit — kein leeres `catch (e) {}`

## Datenbank

- **Migrations** in `supabase/migrations/` — nie direkt in der Supabase UI ändern
- **RLS aktiviert** auf allen User-Tabellen
- **Naming:** Tabellen snake_case, singular (`product`, nicht `products`)

## Tests

- Unit-Tests für Utility-Funktionen und Mapping-Logik
- Keine Tests für UI-Komponenten im MVP
- Test-Files direkt neben der Datei: `product-mapper.test.ts`

## Was nicht ins Repo gehört

- Keine Planungs-Markdowns
- Keine `.env`-Dateien (nur `.env.example`)
- Keine generierten Typen committen wenn sie auto-generiert werden
