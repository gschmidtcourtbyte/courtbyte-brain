# Wave 2: Admin-Bypass fuer Maintenance- und Site-Auth-Gate

**Status:** Completed
**Skill:** handler-eng
**Rolle:** handler-eng
**Kann parallel ausgefuehrt werden:** ja, parallel zu Wave 1
**Blocked by:** —

## Ziel

Eingeloggte Admins koennen alle internen Endpoints erreichen, ohne `/site-login` zu durchlaufen oder die Coming-Soon-Seite zu sehen. `/admin/login` bleibt per direkter URL erreichbar — auch bei `MAINTENANCE_MODE=true` und gesetztem `SITE_PASSWORD`.

## Tasks

### Task 23.4: `AdminAuthHandler.IsAdmin` exportieren
**Priority:** P0
**Blocked by:** —
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [ ] `AdminAuthHandler` bietet eine exportierte Methode `IsAdmin(r *http.Request) bool`.
- [ ] `IsAdmin` ist semantisch identisch zur bestehenden `isAuthenticated` (Cookie-Prufung gegen `cfg.AdminSessionName`).
- [ ] Bestehende Aufrufer von `isAuthenticated` werden auf die neue Methode umgestellt oder bleiben unveraendert (interner Aufruf darf bleiben).
- [ ] Keine Aenderung am Login-/Logout-Flow.

**Kontext:**
- Datei: `internal/handler/admin_auth.go`.
- `isAuthenticated` (Zeile 175 ff.) ist die Basis.

**Output:**
- `internal/handler/admin_auth.go` mit der neuen Methode.

### Task 23.5: Bypass in `MaintenanceMiddleware`
**Priority:** P0
**Blocked by:** 23.4
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [ ] `MaintenanceMiddleware` akzeptiert einen optionalen `adminCheck func(*http.Request) bool` (Signatur erweitern).
- [ ] Wenn `adminCheck != nil` und `adminCheck(r) == true`, wird der Request unmittelbar an `next` weitergereicht — vor allen Allowlist-Pruefungen und vor dem Coming-Soon-Render.
- [ ] Ohne gesetzten `adminCheck` (oder bei `false`) bleibt das aktuelle Verhalten exakt erhalten.
- [ ] Bestehende Allowlist (`/staging`, `/admin`, `/static`, `/kontakt`, `/impressum`, `/datenschutz`, `/agb`) bleibt unveraendert.

**Kontext:**
- Datei: `internal/handler/maintenance.go`.
- Aktuell: `MaintenanceMiddleware(templateDir string, enabled bool) func(http.Handler) http.Handler`.
- Neue Signatur (Vorschlag): `MaintenanceMiddleware(templateDir string, enabled bool, adminCheck func(*http.Request) bool) func(http.Handler) http.Handler`.

**Output:**
- Aktualisierte `internal/handler/maintenance.go`.
- Bestehende Tests koennen weitergenutzt werden (Aufrufe ggf. mit `nil` als `adminCheck` ergaenzen).

### Task 23.6: Bypass in `SiteAuthHandler.RequireSitePassword`
**Priority:** P0
**Blocked by:** 23.4
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [ ] `SiteAuthHandler` kann mit einem optionalen `adminCheck func(*http.Request) bool` konfiguriert werden (per Setter `SetAdminCheck` oder Konstruktor-Parameter).
- [ ] In `RequireSitePassword` wird `adminCheck(r) == true` als Bypass behandelt: Request wird ohne Site-Password-Pruefung an `next` weitergereicht.
- [ ] `LoginForm` und `Login` (`/site-login` GET/POST) bleiben unveraendert — auch ein Admin darf das Site-Login bedienen, falls er es bewusst aufruft.
- [ ] Ohne gesetzten `adminCheck` aendert sich das Verhalten von `RequireSitePassword` nicht.
- [ ] Exempt-Listen fuer `/static`, `/admin`, `/site-login`, `/videos`, `/kontakt`, `/impressum`, `/datenschutz`, `/agb` bleiben erhalten.

**Kontext:**
- Datei: `internal/handler/site_auth.go`.

**Output:**
- Aktualisierte `internal/handler/site_auth.go`.

### Task 23.7: Verdrahtung in `cmd/server/main.go`
**Priority:** P0
**Blocked by:** 23.5, 23.6
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [ ] `MaintenanceMiddleware` wird mit `adminAuthHandler.IsAdmin` als Bypass aufgerufen.
- [ ] `siteAuthHandler` erhaelt `adminAuthHandler.IsAdmin` als Bypass (per Konstruktor oder Setter).
- [ ] Initialisierungsreihenfolge bleibt deterministisch: `adminAuthHandler` wird vor `siteAuthHandler` und vor dem Middleware-Setup angelegt.
- [ ] `/admin/login` ist weiterhin als ungeschuetzte Route registriert (kein Site-Password-Schutz davor).
- [ ] Manueller Smoke (`go run ./cmd/server`) startet ohne Fehler bei `MAINTENANCE_MODE=true` und mit gesetztem `SITE_PASSWORD`.

**Kontext:**
- Datei: `cmd/server/main.go`.
- Heutige Reihenfolge: Maintenance -> CSRF -> Routen, mit `RequireSitePassword` nur in der `protected`-Group.

**Output:**
- Aktualisierte `cmd/server/main.go` mit korrekt verdrahteten Bypass-Hooks.
- Kurz-Notiz im Plan oder als Code-Kommentar (max. 1 Zeile), die das Bypass-Konzept erklaert.

### Task 23.8: Betriebsnotiz fuer Admin-Bypass dokumentieren
**Priority:** P1
**Blocked by:** 23.7
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [ ] `docs/Operations.md` (oder vergleichbar bestehende Notiz) erklaert in maximal 10 Zeilen:
  - URL `/admin/login` ist nur per direktem Aufruf erreichbar.
  - Nach Login wirkt der Admin-Cookie automatisch als Bypass fuer Maintenance- und Site-Password-Gate.
  - Logout via `/admin/logout` (POST) entfernt den Bypass.
- [ ] Keine Secrets oder Cookie-Werte im Dokument.

**Kontext:**
- Bestehende Datei pruefen: `docs/Operations.md` wurde in Iteration 22 angelegt.

**Output:**
- Aktualisierte Betriebsnotiz.

## Wave-2-Exit-Gate

- [ ] `IsAdmin` exportiert.
- [ ] Beide Middlewares unterstuetzen den Bypass.
- [ ] `cmd/server/main.go` verdrahtet die Hooks.
- [ ] `/admin/login` ist bei `MAINTENANCE_MODE=true` und gesetztem `SITE_PASSWORD` direkt erreichbar.
- [ ] Bestehende Allowlist-Tests weiter gruen (mit `nil`-Bypass).

## Notes

- Reine Backend-Wave. Keine Aenderung an Templates, CSS oder JS.
- Keine neuen Routen, keine neuen Cookies.
- Bypass ist ausschliesslich an die bestehende Admin-Session gekoppelt.
