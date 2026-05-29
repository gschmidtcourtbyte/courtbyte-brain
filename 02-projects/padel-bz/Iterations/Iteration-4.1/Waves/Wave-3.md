# Wave 3: Responsive Absicherung

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, testing-eng
**Quelle:** `plan/iterations/iteration-4-1.md`

## Legacy-Inhalt

### Wave 3 — Responsive Absicherung
**Skillsets**: frontend-eng, testing-eng
**Tasks**: 4.1.4
**Ziel**: Mobile und Tablet ohne Ueberlappungen absichern.

## Legacy-Status

### Wave 3 — Responsive Absicherung: done
- Mobile/Tablet bekommen eigene `--hero-lockup-y`- und `--hero-logo-offset-y`-Werte im bestehenden `@media(max-width:860px)` Breakpoint.
- Erwartete Kontrollwerte:
  - Desktop 1440x900: Content-Y 216px, Logo-Top 130px, Logo-Bottom 430px.
  - Tablet 820x1180: Content-Y 320px, Logo-Top 100px, Logo-Bottom 280px.
  - Mobile 390x844: Content-Y 320px, Logo-Top 109px, Logo-Bottom 273px.
