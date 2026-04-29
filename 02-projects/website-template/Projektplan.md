# website-template — Projektplan

**Status:** In Progress
**Repo:** /home/andersen/git/website-template
**Kunde:** intern (Master-Template, wird je Kunde geklont)
**Letzte abgeschlossene Iteration:** —

## Ziel

Wiederverwendbares Master-Template um Mittelstands-Websites in Tagen statt Wochen
auszuliefern. Pro Kunde wird das Repo geklont und über klar isolierte Theme-Tokens,
Logos und Content-Files angepasst – ohne Komponenten-Code anzufassen.

Über das Frontend hinaus liefert dieses Repo eine eigene Backend-API + ein CMS,
sodass Kunden Inhalte (Anfragen, Bewerbungen, Stellen, Galerie, Seiten) ohne
externes Drittsystem pflegen können.

## Quelle

Initiales Design: Claude-Design-Bundle „Website Template Mittelstand"
- `Website Template.html` (Frontend SPA, ~1485 Zeilen)
- `Backend CMS.html` (Admin-UI, ~960 Zeilen)
- `tweaks-panel.jsx` (Live-Tweaks-Shell)

Chat-Transkript bestätigt: dezent + freundlich + tech, modern, subtile Animation,
Grid-Gallery + Lightbox, Karriere mit Upload, Kontakt, Ressourcen (Leistungen/Blog/FAQ/Referenzen),
Backend-CMS mit Login + Dashboard + Anfragen + Karriereportal + Seiten + Galerie + Settings.

## Seitenstruktur

```
/                  → Landingpage (Hero[3 Varianten], Features, Stats, Leistungen, Gallery, Testimonials, Process)
/ueber-uns         → Story + Werte
/ansprechpartner   → Team-Grid
/karriere          → Job-Accordion + Bewerbungsformular (Upload)
/kontakt           → Kontaktformular + Standortdaten
/ressourcen        → Tabs: Leistungen / Blog / FAQ / Referenzen
/blog/[slug]       → einzelne Blog-Posts (CMS)
/impressum         → Pflichtseite
/datenschutz       → Pflichtseite

/admin/login       → CMS-Login
/admin             → Dashboard (Visitors, offene Anfragen, offene Bewerbungen)
/admin/anfragen    → Inbox-Style Kontaktanfragen
/admin/karriere    → Bewerbungs-Inbox + Stellenverwaltung
/admin/seiten      → Seiten/Inhalte
/admin/galerie     → Medien-Upload + Grid
/admin/settings    → Firmendaten + Social
```

## Tech Stack

- **Frontend**: Next.js 14 (App Router, TypeScript, React 18), CSS-in-JS via Theme-Context (1:1-Port der Design-Inline-Styles), DM Sans
- **Backend**: Go (chi-Router, pgx, raw SQL), opaque Redis-Sessions für Admin
- **Datenbank**: PostgreSQL 18 auf Railway
- **Cache**: Redis auf Railway für Admin-Sessions
- **Storage**: lokaler Volume-Mount für Uploads (CV, Galerie-Bilder); S3/Spaces optional pro Kunde
- **Mail**: SMTP (Provider pro Kunde – Postmark / Mailjet) für Eingangs-Bestätigungen + Admin-Notifications
- **Deployment**: Docker Compose lokal; pro Kunde Hosting frei wählbar (Railway / VPS / Vercel + Fly)

## Repo-Struktur

```
frontend/                   — Next.js App
  app/                      — Routes (App Router)
  components/               — UI-Komponenten (Hero, Gallery, …)
  lib/                      — Theme, Daten-Stubs, API-Client
  public/                   — Logo-Platzhalter, Fonts, statische Assets
backend/                    — Go API
  cmd/api/                  — main.go
  internal/handler/         — HTTP-Handler
  internal/service/         — Business-Logik
  internal/repository/      — pgx-DB-Zugriff
  internal/model/           — Domain-Modelle
  internal/middleware/      — auth, logger, ratelimit, cors
  migrations/               — golang-migrate (.up.sql / .down.sql)
docker-compose.yml          — lokales dev (postgres, api, web)
README.md
.vault                      — Pfad zum Vault (für Claude-Code-Sessions)
CLAUDE.md                   — Onboarding für Coding-Sessions
```

## Anpassbarkeit pro Kunde

Das ist das Herzstück. Per Kunde austauschbar **ohne Komponenten anzufassen**:

1. `frontend/lib/theme.ts` — Light/Dark-Palette, Primär- + Akzentfarbe
2. `frontend/lib/site-config.ts` — Firmenname, Logo-Initialen, Social-Links
3. `frontend/content/` — Markdown / TS-Module für Texte (Hero, Über uns, Werte, …)
4. `frontend/public/` — Logo-SVG, Hero-Bild, Galerie-Bilder
5. `backend/.env` — DB-URL, SMTP, Admin-Initial-Account
6. (optional) `frontend/lib/data.ts` — Initiale Job-, FAQ-, Leistungs-Listen, falls noch kein CMS-Content

## Iterations-Plan (grob)

| Iteration | Goal |
|---|---|
| **1** | Frontend bootstrappen + Design 1:1 portieren (Frontend-only, statische Daten) |
| **2** | Admin-Login unter `/admin`, Postgres-Persistenz, Redis-Session-Cache, Railway-only Verification |
| 3 | Frontend ausfeilen: Responsive, A11y, SEO, Performance, i18n-Basis |
| 4 | Contact + Careers POST mit Persistenz, Rate Limits und Admin-Inbox-Grundlage |
| 5 | Admin-CMS-UI für Anfragen + Karriere-Inbox |
| 6 | Admin-CMS-UI für Stellen + Seiten + Galerie + Settings |
| 7 | Mail-Versand (Bestätigung Eingang, Admin-Notification) |
| 8 | Deployment-Templates finalisieren + Kunden-Clone Runbook |
| 9 | Erster Kunden-Clone + Tweak-Workflow dokumentieren |

## Was nicht ins Repo gehört

- Keine Planungs-Markdowns im Code-Repo — Planung liegt in der Vault unter `02-projects/website-template/Iterations/`
- Keine .env-Dateien (nur .env.example)
- Keine .vault-Datei mit absolutem Pfad in Git committen (`.vault` ist gitignored)
- Keine Kunden-spezifischen Inhalte / Logos / Bilder im Master — diese gehören erst im Kunden-Clone rein
