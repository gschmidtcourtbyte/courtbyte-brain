# Courts Bad Zwischenahn — Padel Tennis Website

## Vault

Lies `.vault` in diesem Repo-Root um den Vault-Pfad zu ermitteln.
Planungsdateien liegen in: `{vault}/02-projects/padel-bz/`

### Session Start — immer in dieser Reihenfolge lesen

1. `.vault` lesen → `{vault}` Pfad ermitteln
2. `{vault}/02-projects/padel-bz/Rules.md`
3. `{vault}/02-projects/padel-bz/Projektplan.md`
4. `{vault}/02-projects/padel-bz/Iterations/Iteration-N/Iteration-N.md`
5. `{vault}/02-projects/padel-bz/Iterations/Iteration-N/Waves/Wave-X.md`
6. Skill aus der Wave-Datei aufrufen

---

## Projekt

Website für Padel-Tennis-Anlage in Bad Zwischenahn. Aktuell in der Bauphase,
Eröffnung Herbst 2026. Hauptfeature: Kontaktformular für Hansefit-Registrierung
+ CMS für Baufortschritte.

## Tech Stack

- **Backend**: Go (chi router, html/template, pgx, redis)
- **Frontend**: Server-Side Rendering, Tailwind CSS, Vanilla JS / Alpine.js
- **Datenbank**: PostgreSQL 16, Redis 7
- **Deployment**: Docker → Railway

## Projekt-Struktur

```
cmd/server/          — Einstiegspunkt
internal/config/     — Konfiguration (Env Vars)
internal/database/   — DB + Redis Connections
internal/handler/    — HTTP Handler + Middleware
internal/model/      — Domain Models
internal/repository/ — DB Queries
internal/service/    — Business Logic
migrations/          — SQL Migrations (golang-migrate Format)
web/templates/       — Go HTML Templates (layouts/, pages/, partials/, admin/)
web/static/          — CSS, JS, Bilder, Videos
```

## Kommandos

```bash
make dev             # Docker Compose up mit Hot Reload
make build           # Go Binary bauen
make test            # Tests ausführen
make migrate-up      # Migrationen anwenden
make migrate-down    # Letzte Migration zurückrollen
make lint            # Linter ausführen
```

## Konventionen

- Go Standard-Layout (cmd/, internal/)
- Alle Handler in internal/handler/, Repository Pattern für DB-Zugriffe
- Kein ORM — pgx mit Raw SQL
- Migrations nummeriert: 001_, 002_, etc.
- Templates: layouts/ für Basis, pages/ für Seiten, partials/ für Komponenten
- Environment Variables für Konfiguration (.env lokal, Railway Env in Prod)

## Design / UI

- Dunkles Theme, minimalistisch, sportlich — Orientierung an thecubepadel.de
- Mobile-first Responsive Design
- Tailwind CSS für Styling
