# padel-bz — Projektplan

**Status:** In Progress
**Repo:** /home/andersen/git/padel-bz
**Kunde:** Courts Bad Zwischenahn
**Eröffnung:** Herbst 2026
**Letzte abgeschlossene Iteration:** 23

## Ziel

Website für Padel-Tennis-Anlage in Bad Zwischenahn während der Bauphase.
Hauptfeatures: Kontaktformular (Hansefit-Registrierung) + CMS für Baufortschritte.

## Seitenstruktur

```
/                → Landingpage (Hero, About, Features, Sponsoren, FAQ, Kontakt)
/kontakt         → Kontaktformular (Hansefit-Registrierung)
/baufortschritt  → Bau-Updates mit Fotos/Videos (CMS-gesteuert)
/impressum       → Impressum
/datenschutz     → Datenschutz

/admin/login     → Admin-Login
/admin/dashboard → Admin-Dashboard
/admin/updates   → Bau-Updates verwalten (CRUD)
/admin/media     → Medien-Upload (Fotos, Videos)
```

## Tech Stack

- **Backend**: Go (chi router, html/template, pgx, redis)
- **Frontend**: Server-Side Rendering, Tailwind CSS, Vanilla JS / Alpine.js
- **Datenbank**: PostgreSQL 16, Redis 7
- **Deployment**: Docker → Railway

## Was bereits steht (nach Iteration 20)

- Landingpage vollständig (Hero, About, Features, FAQ, Footer)
- Kontaktformular mit CSRF, Rate Limiting, E-Mail-Versand
- Admin-Authentifizierung (bcrypt, Redis-Sessions)
- CMS: Bau-Updates erstellen / bearbeiten / löschen
- Medien-Upload (Fotos, Videos)
- Sponsoren-Sektion (5 Kacheln: AXA SVG + 2x Coming Soon + 2x Let's Play Together)
- Railway Deployment aktiv
- SEO: Meta-Tags, OG-Tags, Sitemap

## Historische Planungsdateien

Iterationen 1–20 wurden aus `plan/iterations/` in die Vault-Struktur `Iterations/Iteration-N/` migriert.
Ab Iteration 21 werden alle neuen Planungsdateien direkt in dieser Vault gepflegt.
