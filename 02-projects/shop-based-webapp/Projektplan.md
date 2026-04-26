# shop-based-webapp - Projektplan

**Status:** In Progress
**Letzte abgeschlossene Iteration:** 28
**Repo:** /home/andersen/git/shop-based-webapp
**GitHub:** git@github.com:Courtbyte/shop-based-webapp.git
**Produkt:** EasyEdge Platform / OUT Padel Customer Web

## Ziel

Dieses Projekt betreibt eine modulare Web-Plattform mit Go-Backend und Next.js-Customer-Web-App.
Der aktuelle fachliche Schwerpunkt liegt auf der OUT Padel Customer Journey: Camp-Pakete, Buchungsanfragen, redaktionelle Inhalte, Newsletter und ein Admin-/Operations-Workflow.

## Scope

**In:**
- Customer-Web-App unter `web/customer`
- Go API und Backend-Module unter `internal/*`
- SQL-Migrationen unter `migrations/`
- Railway-/Docker-Deployment-Konfiguration
- Technische Dokumentation im Code-Repo

**Out:**
- Planungs-Markdowns im Code-Repo fuer neue Iterationen
- Lokale Secrets, `.env` Dateien und maschinenspezifische Pfade
- Produktplanung ausserhalb der Vault

## Tech Stack

- **Backend:** Go, chi router, pgx, PostgreSQL, modulare Ports/Adapters-Architektur
- **Frontend:** Next.js, React, TypeScript, Tailwind CSS, Vitest
- **Datenbank:** PostgreSQL mit versionierten SQL-Migrationen
- **Deployment:** Docker, Railway

## Repo-Struktur

```text
cmd/server/        -> API server entrypoint
internal/          -> bounded contexts, app services, ports, adapters
pkg/               -> shared technical packages
migrations/        -> SQL migrations
web/customer/      -> customer-facing Next.js app
deploy/railway/    -> Railway deployment configuration
docs/              -> technical docs, ADRs, runbooks, API docs
```

## Workflow

Ab sofort liegen neue Iterations- und Wave-Planungen in dieser Vault:

```text
02-projects/shop-based-webapp/Iterations/Iteration-N/
```

Der Code-Repo-Root enthaelt nur die lokalen Workflow-Bruecken:

- `.vault` lokal und gitignored
- `AGENTS.md` fuer Codex
- `CLAUDE.md` fuer Claude Code

## Historische Planungsdateien

Historische Planungsdokumente existieren derzeit noch im Code-Repo unter `docs/plans/`.
Neue Planungen sollen nicht mehr dort entstehen. Eine spaetere Migration kann diese Dateien in die Vault-Struktur ueberfuehren.
