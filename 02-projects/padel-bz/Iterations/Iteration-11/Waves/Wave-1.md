# Wave 1: Coming Soon + Middleware (parallel)

**Status:** Done (Legacy-Migration)
**Skill:** handler-eng, frontend-eng
**Quelle:** `plan/iterations/iteration-11.md`

## Legacy-Inhalt

### Wave 1 — Coming Soon + Middleware (parallel)
**Skillsets**: frontend-eng, handler-eng
**Tasks**: 11.1, 11.2 (parallel — keine Abhängigkeiten untereinander)
**Ziel**: Coming-Soon-Template und Maintenance-Middleware unabhängig voneinander erstellen.

## Legacy-Status

### Wave 1 — Coming Soon + Middleware: done
- `web/templates/pages/coming-soon.html`: Standalone HTML mit Inline-SVG-Logo, Inline-CSS, Markenfarben, Fade-In-Animation, responsive.
- `internal/handler/maintenance.go`: Chi-Middleware, Template einmalig geparst, Staging-Prefix-Stripping, Legal-Passthrough, 503 + Retry-After.
