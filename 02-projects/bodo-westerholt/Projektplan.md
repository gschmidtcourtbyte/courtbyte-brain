# bodo-westerholt - Projektplan

**Status:** Initial Setup
**Repo:** /home/andersen/git/bodo-westerholt
**Kunde:** Bodo Westerholt
**Letzte abgeschlossene Iteration:** 0
**Aktive Iteration:** 1 - Kunden-Initialisierung und Basis-Branding

## Ziel

Kundenwebsite fuer Bodo Westerholt auf Basis des Courtbyte Website-Templates.
Das Repo ist als Kunden-Clone vorbereitet und wird ueber Theme, Site Config,
Content und Assets kundenspezifisch angepasst.

## Ausgangsbasis

- Template-Code wurde aus `/home/andersen/git/website-template` uebernommen.
- Lokale Build-Artefakte, `node_modules`, `.next`, Docker-Daten und Template-Git-Historie wurden nicht uebernommen.
- `.vault` zeigt auf `/home/andersen/git/courtbyte-brain`.
- Projektplanung liegt in diesem Brain-Ordner.

## Tech Stack

- **Frontend:** Next.js 14 App Router, React 18, TypeScript.
- **Backend:** Go, chi-Router, pgx, raw SQL.
- **Datenbank:** PostgreSQL, Redis fuer Admin-Sessions.
- **Deployment:** Docker Compose lokal; Railway-Setup vorbereitet.

## Repo-Struktur

```text
frontend/                - Next.js App
  app/                   - Routes
  components/            - UI-Komponenten
  lib/                   - Theme, Daten, API-Client, Tweaks
  public/                - Logo, Bilder, statische Assets
backend/                 - Go API
  cmd/api/               - Einstiegspunkt
  internal/              - Handler, Auth, Config, Submission, Migrations
docker-compose.yml       - lokales Dev-Setup
docs/                    - Deployment- und Runbook-Dokumentation
```

## Anpassbare Stellen

| Bereich | Datei |
|---|---|
| Firmenname, Initialen, Kontakt | `frontend/lib/site-config.ts` |
| Farben und Theme | `frontend/lib/theme.ts` |
| Inhalte, Jobs, FAQ, Leistungen | `frontend/lib/data.ts` |
| SEO-Metadaten | `frontend/app/layout.tsx` |
| Assets | `frontend/public/` |
| Backend-Konfiguration | `backend/.env.example` als Vorlage |

## Iterations-Plan

| Iteration | Ziel |
|---|---|
| 1 | Kunden-Initialisierung, Basis-Branding, Content-/Asset-Luecken klaeren |
| 2 | Startseiten- und Unterseiteninhalte mit Kundenmaterial ausarbeiten |
| 3 | Kontakt-/Karriere-/CMS-Flows kundenspezifisch pruefen |
| 4 | Deployment, Smoke-Test, SEO/Legal-Check |

## Naechste Schritte

- Kundenmaterial sammeln: Logo, Farben, Fotos, Texte, rechtliche Angaben, Kontaktwege.
- Theme und Content anhand des Materials anpassen.
- Frontend und Backend lokal verifizieren.
- Deployment-Variablen fuer Staging vorbereiten.

## Was nicht ins Repo gehoert

- Planungsdateien bleiben in diesem Brain-Ordner.
- Secrets gehoeren in lokale `.env` oder Deployment-Variables, nicht in Git.
- Produktive Kundenbilder nur mit Freigabe und Nutzungsrechten einchecken.
