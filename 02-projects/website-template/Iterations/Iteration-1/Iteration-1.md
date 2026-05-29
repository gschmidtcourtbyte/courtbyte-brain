# Iteration 1 — Frontend Bootstrap + Design-Port

**Status:** In Progress
**Repo:** /home/andersen/git/website-template
**Datum:** 2026-04-29

## Ziel

Frontend von Null auf bringen: Next.js + TypeScript-Skelett, Theme-System, alle
Komponenten + Seiten der Designvorlage 1:1 portiert, statische Daten aus
`lib/data.ts`. Backend bleibt in dieser Iteration nur als ungefülltes Skelett.

## Scope

**In:**
- Monorepo-Bootstrap (`frontend/`, `backend/`, root configs, .vault, CLAUDE.md)
- Next.js App-Router-Setup, TypeScript, ESLint, DM-Sans-Font
- Theme-System (Light/Dark, Primary, Accent) via React Context
- Tweaks-Panel (Dev-only) mit Live-Switching
- Komponenten: Logo, Navbar, Footer, Hero (3 Varianten), Blobs, ScrollHint, FeaturesStrip, Stats-Counter, Leistungen-Vorschau, Testimonials-Karussell, Process-Timeline, Gallery (Filter + Lightbox), PageHeader
- Seiten: `/`, `/ueber-uns`, `/ansprechpartner`, `/karriere` (mit Bewerbungs-Modal + Upload-UI), `/kontakt`, `/ressourcen` (Tabs: Leistungen/Blog/FAQ/Referenzen)
- Statische Daten in `lib/data.ts` (NAV_ITEMS, GALLERY, TEAM, JOBS, …)
- Go-Backend-Skelett (kompiliert, `/healthz` OK, sonst leer)

**Out:**
- Reale API-Calls (Forms posten zu `/api/...` aber Backend antwortet noch mit 501)
- Datenbank, Migrations, CMS-UI, Auth, Mail
- Production-Deployment / Dockerfiles für Prod
- Pixel-Pass auf <768px Mobile (Designvorlage ist Desktop-first; Mobile wird Iteration 2)

## Waves

| Wave | Skill | Focus | Status |
|---|---|---|---|
| Wave 1 | infrastructure-eng | Monorepo + Next.js + Go-Skelett bootstrappen | In Progress |
| Wave 2 | frontend-eng | Theme-System, Tweaks-Panel, Layout (Navbar/Footer/Logo) | Todo |
| Wave 3 | frontend-eng | Hero (3 Varianten), Home-Sektionen, Gallery+Lightbox | Todo |
| Wave 4 | frontend-eng | Content-Seiten (Über uns, Ansprechpartner, Karriere, Kontakt, Ressourcen) | Todo |

## Acceptance Criteria

- [ ] `cd frontend && npm run dev` startet auf :3000 ohne Fehler
- [ ] `cd backend && go run ./cmd/api` startet, `/healthz` antwortet `200 OK`
- [ ] Alle Seiten aus dem Design sind in Next.js erreichbar und visuell deckungsgleich (Desktop)
- [ ] Theme-Toggle Hell/Dunkel + Primär-/Akzentfarben-Picker funktionieren live
- [ ] Hero-Variante (Split / Vollbild / Floating-Card) ist über Tweaks umschaltbar
- [ ] Gallery-Filter + Lightbox + Pfeiltasten-Navigation funktionieren
- [ ] Karriere-Bewerbungs-Modal öffnet, Datei-Drop-Zone visuell vorhanden, Submit zeigt Erfolgs-State
- [ ] `lib/site-config.ts` enthält Firmenname/Initialen/Social-URLs an einem Ort

## Notes

- Designvorlage nutzt komplett Inline-Styles mit Theme-Werten — wir behalten das bei
  (kein Tailwind), so dass die Vorlage 1:1 portiert ist und der Theme-Switch live wirkt.
- React-Komponenten werden als Client Components markiert (`'use client'`), weil das
  ganze Template stateful + interaktiv ist. Server-Components-Optimierung kommt in Iteration 2.
- Bilder bleiben vorerst `picsum.photos` mit den im Design verwendeten Seeds —
  visuelle Parität mit der Vorlage erhalten.
