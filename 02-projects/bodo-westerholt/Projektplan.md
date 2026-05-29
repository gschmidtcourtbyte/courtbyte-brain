# bodo-westerholt - Projektplan

**Status:** Iteration 1 Code-Tasks erledigt — visuelle Abnahme offen
**Repo:** /home/andersen/git/bodo-westerholt
**Kunde:** Bodo Westerholt Unternehmensgruppe (Tiefbau)
**Letzte abgeschlossene Iteration:** 0 (Iteration 1 wartet auf visuelle Abnahme)
**Aktive Iteration:** [[Iteration-1]] - Frontend-Kundentouch (Tiefbau Branding)
**Corporate Design:** [[Corporate-Design]]
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
| 2 | Backend- und Deployment-Basischeck (verschoben aus Iter. 1) + erste Unterseiten-Ausarbeitung mit Kundenmaterial |
| 3 | Kontakt-/Karriere-/CMS-Flows kundenspezifisch pruefen |
| 4 | Deployment, Smoke-Test, SEO/Legal-Check |

## Naechste Schritte

- [[Iteration-1]] ausfuehren ([[Wave-1]] -> [[Wave-2]] -> [[Wave-3]]).
- Open-Assets-Liste (entsteht in [[Wave-3]]) mit Kunde durchgehen.
- Kundenmaterial sammeln: echte Bilder, finale Texte, rechtliche Angaben.

## Was nicht ins Repo gehoert

- Planungsdateien bleiben in diesem Brain-Ordner.
- Secrets gehoeren in lokale `.env` oder Deployment-Variables, nicht in Git.
- Produktive Kundenbilder nur mit Freigabe und Nutzungsrechten einchecken.
- Das Logo aus `Bilder/` ist Repo-extern; in [[Wave-1]] wird es nach
  `frontend/public/` kopiert (Nutzungsrecht durch Kunde implizit gegeben).
