# Wave 2: CSS-Umsetzung

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-4-1.md`

## Legacy-Inhalt

### Wave 2 — CSS-Umsetzung
**Skillsets**: frontend-eng
**Tasks**: 4.1.2, 4.1.3
**Ziel**: Gemeinsamen vertikalen Offset einfuehren und Desktop-Hero sauber ausrichten.

## Legacy-Status

### Wave 2 — CSS-Umsetzung: done
- `web/static/css/style.css` nutzt jetzt `--hero-lockup-y` als gemeinsamen vertikalen Referenzwert.
- `.hero-logo-wrap` wird ueber `calc(var(--hero-lockup-y) - var(--hero-logo-offset-y))` daran ausgerichtet.
- `.hero-grid` liegt mit `z-index:2` ueber dem Logo; `.hero-logo-wrap` hat `pointer-events:none`, damit Buttons klickbar bleiben.
