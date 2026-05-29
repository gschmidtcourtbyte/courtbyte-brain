# Wave 2: Navbar-Reveal

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-6.md`

## Legacy-Inhalt

### Wave 2 — Navbar-Reveal
**Skillsets**: frontend-eng
**Tasks**: 6.2, 6.3
**Ziel**: Navbar initial verstecken und beim Scrollen zuverlaessig sichtbar machen.

## Legacy-Status

### Wave 2 — Navbar-Reveal: done
- Hidden-Regeln nutzen jetzt `:not(.nav-visible)`, damit die versteckte Navbar nicht gegen den sichtbaren Scroll-State gewinnt.
- `body.has-hero-nav.nav-visible > nav` kann die Navbar wieder auf `transform: translateY(0)`, `opacity:1` und `pointer-events:auto` setzen.
- `web/static/js/main.js` nutzt `getScrollTop()` mit Fallbacks fuer `window.scrollY`, `document.documentElement.scrollTop` und `document.body.scrollTop`.
- `updateHeroNav()` laeuft direkt, via `requestAnimationFrame`, bei `scroll`, `resize` und `hashchange`.
