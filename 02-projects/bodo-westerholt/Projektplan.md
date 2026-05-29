# bodo-westerholt - Projektplan

**Status:** Iteration 1 Code-Tasks erledigt — visuelle Abnahme offen
**Repo:** /home/andersen/git/bodo-westerholt
**Kunde:** Bodo Westerholt Unternehmensgruppe (Tiefbau)
**Letzte abgeschlossene Iteration:** 0 (Iteration 1 wartet auf visuelle Abnahme)
**Aktive Iteration:** [[Iteration-2]] - CD-Manual-Alignment
**Corporate Design:** [[CD-Manual]] (verbindlich) · [[Corporate-Design]] (Interim)
**Offene Asset-Liste:** [[Open-Assets]]

## Ziel

Kundenwebsite fuer Bodo Westerholt Unternehmensgruppe auf Basis des
Courtbyte Website-Templates. Das Repo ist als Kunden-Clone vorbereitet
und wird ueber Theme, Site Config, Content und Assets kundenspezifisch
angepasst. Branche ist Tiefbau (Erdbau, Kanalbau, Strassenbau, Rueckbau).

## Ausgangsbasis

- Template-Code wurde aus `/home/andersen/git/website-template` uebernommen.
- Lokale Build-Artefakte, `node_modules`, `.next`, Docker-Daten und
  Template-Git-Historie wurden nicht uebernommen.
- `.vault` zeigt auf `/home/andersen/git/courtbyte-brain`.
- Projektplanung liegt in diesem Brain-Ordner.
- Logo, Primaer-/Sekundaerfarbe sowie Branche sind seit 2026-05-29 bekannt
  und in [[Corporate-Design]] festgehalten.

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
| 1 | [[Iteration-1]] - Frontend-Kundentouch: Theme, Logo, Fonts, Komponenten-Refresh, Tiefbau-Content |
| 2 | [[Iteration-2]] - CD-Manual-Alignment: Archivo + IBM Plex Mono, strikt Rot/Anthrazit, Wortmarke + "Die Schraege" |
| 3 | Backend- und Deployment-Basischeck + erste Unterseiten-Ausarbeitung mit Kundenmaterial |
| 4 | Kontakt-/Karriere-/CMS-Flows kundenspezifisch pruefen; Deployment, Smoke-Test, SEO/Legal-Check |

## Naechste Schritte

- [[Iteration-2]] ausfuehren (Wave 1 -> 2 -> 3 -> 4); CD-Manual ist verbindlich.
- Nach Iteration 2: [[Corporate-Design]] auf das [[CD-Manual]] nachziehen.
- Open-Assets-Liste mit Kunde durchgehen.
- Kundenmaterial sammeln: echte Bilder, finale Texte, rechtliche Angaben.

## Was nicht ins Repo gehoert

- Planungsdateien bleiben in diesem Brain-Ordner.
- Secrets gehoeren in lokale `.env` oder Deployment-Variables, nicht in Git.
- Produktive Kundenbilder nur mit Freigabe und Nutzungsrechten einchecken.
- Das Logo aus `Bilder/` ist Repo-extern; in [[Wave-1]] wird es nach
  `frontend/public/` kopiert (Nutzungsrecht durch Kunde implizit gegeben).
