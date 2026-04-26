# padel-bz — Coding Rules

## Architektur

- **Go Standard-Layout** — cmd/, internal/, keine Abweichungen
- **Handler** ausschließlich in internal/handler/
- **Repository Pattern** für alle DB-Zugriffe — kein direktes SQL in Handlers oder Services
- **Kein ORM** — pgx mit Raw SQL
- **Server-Side Rendering** — keine Client-Side-Frameworks, kein React/Vue

## Datenbank

- **Migrations** nummeriert: 001_, 002_, etc. in migrations/
- **golang-migrate** Format: `.up.sql` / `.down.sql`
- Nie direkt in Railway-DB ohne Migration ändern

## Templates

- `layouts/` — Basis-Layouts
- `pages/` — eine Datei pro Route
- `partials/` — wiederverwendbare Komponenten
- `admin/` — Admin-spezifische Templates

## Code-Stil

- **Environment Variables** für alle Konfiguration (.env lokal, Railway in Prod)
- **Structured Logging** mit slog
- **Graceful Shutdown** implementiert
- **CSRF-Schutz** auf allen POST-Routen
- **Rate Limiting** via Redis

## Frontend

- **Tailwind CSS** für Styling — kein Custom CSS außer wo Tailwind nicht ausreicht
- **Vanilla JS / Alpine.js** — kein Build-Step für JS im MVP
- **Mobile-first** — alle Layouts zuerst für Mobile designen
- **Design-Referenz**: thecubepadel.de — dunkel, minimalistisch, sportlich

## Deployment

- **Docker** lokal via docker-compose
- **Railway** für Produktion
- Keine Secrets in Git — Railway Env Variables nutzen

## Was nicht ins Repo gehört

- Keine Planungs-Markdowns (ab Iteration 21 — in Vault)
- Keine .env-Dateien (nur .env.example)
- Keine .vault Datei
