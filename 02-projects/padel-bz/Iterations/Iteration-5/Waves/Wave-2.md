# Wave 2: Navbar-State

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-5.md`

## Legacy-Inhalt

### Wave 2 — Navbar-State
**Skillsets**: frontend-eng
**Tasks**: 5.2, 5.3
**Ziel**: Navbar initial auf Hero-Seiten verstecken und beim Scrollen sichtbar machen.

## Legacy-Status

### Wave 2 — Navbar-State: done
- `web/templates/layouts/base.html`: `html.js` wird frueh gesetzt, damit Hero-Seiten die Navbar initial ohne sichtbaren Flash verstecken koennen.
- `web/static/css/style.css`: Auf Hero-Seiten wird die Navbar fixed und initial per `transform`/`opacity` ausgeblendet; sichtbar bleibt sie im bestehenden gruenen Format.
- `web/static/js/main.js`: `body.has-hero-nav` und `body.nav-visible` steuern den Scroll-State ab `16px` Scroll-Offset.
