# Wave 3: UI-Anwendung

**Status:** Done (Legacy-Migration)
**Skill:** frontend-eng
**Quelle:** `plan/iterations/iteration-9.md`

## Legacy-Inhalt

### Wave 3 — UI-Anwendung
**Skillsets**: frontend-eng
**Tasks**: 9.3
**Ziel**: Direkte Hellgruen-Verwendungen durch Highlight-Tokens ersetzen.

## Legacy-Status

### Wave 3 — UI-Anwendung: done
- Direkte Hellgruen-RGBA-Werte im aktiven CSS wurden durch `rgba(var(--accent-rgb),...)` ersetzt.
- Betroffen sind Status-Badge, GPS-Badge, Map-Pin, Pulse-Animation, Formular-Fokus und Success-Alert.
- Link- und Formular-Hover nutzen `--accent-hover`.
