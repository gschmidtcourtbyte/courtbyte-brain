# Wave 2: Integration

**Status:** Done (Legacy-Migration)
**Skill:** infrastructure-eng
**Quelle:** `plan/iterations/iteration-11.md`

## Legacy-Inhalt

### Wave 2 — Integration
**Skillsets**: infrastructure-eng
**Tasks**: 11.3 (benötigt Outputs von 11.1 + 11.2)
**Ziel**: Config erweitern, Middleware in Router einbinden, alles verdrahten.

## Legacy-Status

### Wave 2 — Integration: done
- `internal/config/config.go`: `MaintenanceMode bool` aus `MAINTENANCE_MODE` Env-Var (Default: `"true"`).
- `cmd/server/main.go`: Middleware nach Timeout, vor CSRF eingebunden. Log-Ausgabe beim Start.
