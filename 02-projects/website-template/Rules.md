# website-template — Coding Rules

## Architektur

- **Monorepo** mit `frontend/` (Next.js) und `backend/` (Go) als gleichwertige Top-Level-Ordner
- **Frontend**: Next.js App Router + TypeScript + React 18; keine Pages-Router-Routen
- **Backend**: Go Standard-Layout — `cmd/`, `internal/` (handler, service, repository, model, middleware)
- **Repository Pattern** im Backend für alle DB-Zugriffe — kein direktes SQL in Handlers / Services
- **Kein ORM** — pgx mit Raw SQL

## Frontend-Konventionen

- **TypeScript strict** — `noImplicitAny`, `strictNullChecks` an
- **Styling**: CSS-in-JS via Inline-Styles + Theme-Context (1:1-Port der Designvorgabe). Keine Tailwind, kein styled-components, kein Emotion. Tokens leben ausschließlich in `lib/theme.ts`.
- **Komponenten** unter `components/`, eine Komponente pro Datei, PascalCase-Filename
- **Daten-Stubs** unter `lib/data.ts` — werden später durch API-Calls ersetzt
- **Theme-Tweaks** über `TweaksProvider` + `<TweaksPanel/>` — nur in Dev-Mode sichtbar (`process.env.NODE_ENV !== 'production'`)
- **Bilder**: Platzhalter via `picsum.photos`; Kunde tauscht durch eigene Assets in `public/`
- **Routing**: deutsche Slugs (`/ueber-uns`, `/karriere`, …) — Default-Sprache de-DE
- **Keine Client-Side-Auth-Tokens im LocalStorage** — Cookie-basiert (HttpOnly) sobald Backend angebunden

## Backend-Konventionen

- **Migrations** nummeriert: `001_`, `002_`, … in `backend/migrations/`
- **golang-migrate** Format: `.up.sql` / `.down.sql`
- **Structured Logging** mit slog
- **Graceful Shutdown** implementiert
- **CSRF-Schutz** auf allen state-changing Routen aus dem Browser
- **Rate Limiting** auf `/api/contact` und `/api/applications`
- **Auth**: JWT in HttpOnly-Cookie für Admin; Public-Endpoints ohne Auth aber rate-limited

## Was explizit NICHT verwendet wird

- Kein Tailwind, kein UI-Framework (Radix/shadcn/MUI/etc.) — wir wollen 1:1 das Design ohne Lib-Aesthetik
- Kein Pages Router (Next.js)
- Kein ORM (gorm, sqlx mit Reflection o.Ä.)
- Kein Drittanbieter-CMS (Strapi/Directus) — wir bauen das selbst, sonst lohnt das Backend nicht

## Anpassbarkeit pro Kunde

Goldene Regel: **Nichts Kunden-Spezifisches in Komponenten**. Texte/Farben/Logos/Bilder
fließen ausschließlich über `lib/theme.ts`, `lib/site-config.ts`, `content/`, `public/`.
Wenn ein Kunden-Wunsch eine Komponenten-Änderung erfordert, ist das ein Hinweis dass die
Komponente eine Konfigurations-Lücke hat — Lücke schließen, dann anpassen.

## Was nicht ins Code-Repo gehört

- Keine Planungs-Markdowns (alles in `{vault}/02-projects/website-template/`)
- Keine `.env`-Dateien (nur `.env.example`)
- Keine Kunden-Logos / -Bilder
