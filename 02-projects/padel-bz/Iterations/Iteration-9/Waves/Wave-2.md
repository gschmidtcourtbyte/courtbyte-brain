# Wave 2: Token Foundation

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng, review-eng
**Quelle:** `plan/iterations/iteration-9.md`

## Legacy-Inhalt

### Wave 2 — Token Foundation
**Skillsets**: frontend-eng, review-eng
**Tasks**: 9.2
**Ziel**: CSS-Variablen so umstellen, dass alle aktiven Akzente zentral auf `#D8633D` basieren.

## Legacy-Status

### Wave 2 — Token Foundation: done
- `web/static/css/style.css` nutzt zentral:
  - `--accent: #D8633D`
  - `--accent-rgb: 216, 99, 61`
  - `--accent-hover: #E1734F`
  - `--accent-ink: #1a1714`
- `.btn-primary` nutzt jetzt `--accent` und `--accent-ink`.
- Kontrastcheck fuer Button-Text:
  - `#1a1714` auf `#D8633D`: 4.91:1
  - `#1a1714` auf `#E1734F`: 5.75:1
