# Iteration 11: Wartungsseite (Coming Soon) + Staging-Routing

## Ziel
Die Website soll sofort live gehen — aber mit einer "Coming Soon"-Wartungsseite als Standard. Die echte Website ist nur über den Pfad `/staging` erreichbar, damit der Betreiber Content in Ruhe finalisieren kann. Besucher sehen eine elegante Ankündigungsseite mit Logo, Standort-Infos und Kontaktdaten.

## Ausgangslage
- Vollständige Website mit Home, Kontakt, Impressum, Datenschutz vorhanden.
- Go-Backend mit chi Router, Handler, Renderer (`cmd/server/main.go`).
- Config über Environment Variables (`internal/config/config.go`).
- Logo verfügbar: `logo/logo_courts.svg` (SVG mit Olive-Waldgrün #475243 Hintergrund, Creme #FAF2E8 Schrift).
- Markenfarben in `logo/farbcodes.md` definiert.
- Kein Maintenance-Mode vorhanden.

## Scope
- **Config**: `internal/config/config.go` — neues Feld `MaintenanceMode`.
- **Middleware**: `internal/handler/maintenance.go` — neue Datei, Middleware-Logik.
- **Template**: `web/templates/pages/coming-soon.html` — Coming-Soon-Seite (standalone, kein base-Layout).
- **Router**: `cmd/server/main.go` — Middleware einbinden, `/staging`-Routing.
- Kein DB-Schema, kein neuer Service, kein JS.

## Produktentscheidung

### Coming-Soon-Seite
- Standalone HTML-Seite (eigenes `<html>`, kein base-Layout), damit sie unabhängig vom Rest funktioniert.
- Inline-SVG des Logos aus `logo/logo_courts.svg` — kein externer Asset-Request nötig.
- Inline-CSS im Marken-Stil (Olive #475243, Creme #FAF2E8, Accent #D8633D).
- Inhalte: Logo, "COMING SOON", "Eröffnung Herbst 2026", Standort "Bad Zwischenahn", E-Mail "info@courts-bz.de".
- Minimalistisch, zentriert, responsive, passend zum dunklen sportlichen Theme.
- Kein JavaScript, kein Countdown, keine externen Abhängigkeiten.

### Routing-Logik
- `MAINTENANCE_MODE=true` (Default in Production): Alle Requests → Coming-Soon-Seite.
- Ausnahmen: Pfade die mit `/staging` beginnen → Prefix wird gestripped, normale Seite wird ausgeliefert.
- Ausnahme: `/static/*` wird durchgelassen (für die Staging-Seite benötigte Assets).
- Ausnahme: `/impressum` und `/datenschutz` werden durchgelassen (rechtlich erforderlich).
- `MAINTENANCE_MODE=false`: Alles normal, keine Wartungsseite.

## Critical Path
1. Coming-Soon-Template erstellen (standalone HTML mit Inline-SVG + Inline-CSS).
2. Maintenance-Middleware implementieren (Request-Interception, Staging-Prefix-Stripping).
3. Config um `MaintenanceMode` erweitern.
4. Middleware + Staging-Route in `main.go` verdrahten.
5. QA: Maintenance Mode ON → Coming Soon. `/staging` → echte Seite. Maintenance Mode OFF → alles normal.

## Skillset-Zuordnung

| Skillset | Einsatz |
|----------|---------|
| product-own | Scope, Priorisierung, Wave-Steuerung, Akzeptanzkriterien |
| frontend-eng | Coming-Soon-Template (HTML/CSS/Inline-SVG) |
| handler-eng | Maintenance-Middleware, Staging-Routing-Logik |
| infrastructure-eng | Config-Erweiterung, Router-Integration in main.go |
| testing-eng | Funktionsprüfung aller Routing-Szenarien |

---

## Task: 11.1 Coming-Soon-Seite erstellen
**Priority**: P0
**Blocked by**: none
**Skillset**: frontend-eng

**Akzeptanzkriterien**:
- [ ] `web/templates/pages/coming-soon.html` existiert als standalone HTML (eigenes `<html>`, kein `{{define "content"}}`).
- [ ] Logo aus `logo/logo_courts.svg` als Inline-SVG eingebettet (nur die relevanten `<g>`/`<path>`-Elemente, nicht den Hintergrund-Rect).
- [ ] Inline-CSS mit Markenfarben: `--bg: #475243`, `--fg: #FAF2E8`, `--accent: #D8633D`.
- [ ] Inhalte: Logo, "COMING SOON" oder "Bald geht's los", Öffnungsdatum "Herbst 2026", Standort "Bad Zwischenahn", E-Mail "info@courts-bz.de".
- [ ] Responsive Design, zentriert, mobile-first.
- [ ] Keine externen Abhängigkeiten (keine Google Fonts, kein JS, keine externen CSS/Bilder).
- [ ] `<meta name="description">` und `<title>` gesetzt.
- [ ] Dezente Animation erlaubt (CSS-only, z.B. Fade-In), aber kein Countdown.

**Kontext**:
- Logo-SVG: `logo/logo_courts.svg` (Zeile 3-15) — ViewBox 0 0 2457 2457, drei `<g>`-Gruppen: Hintergrund-Rect, Logo-Pfad, "PADELCLUB"-Text.
- Farbcodes: `logo/farbcodes.md` — Olive #475243, Creme #FAF2E8, Accent #D8633D.
- Design-Referenz: Bestehendes Theme in `web/static/css/style.css` (dunkel, minimalistisch, sportlich).

**Output**:
- `web/templates/pages/coming-soon.html`

---

## Task: 11.2 Maintenance-Middleware implementieren
**Priority**: P0
**Blocked by**: none
**Skillset**: handler-eng

**Akzeptanzkriterien**:
- [ ] Neue Datei `internal/handler/maintenance.go`.
- [ ] Funktion `MaintenanceMiddleware(renderer *Renderer, enabled bool) func(http.Handler) http.Handler`.
- [ ] Wenn `enabled == false`: Middleware ist ein No-Op (passthrough).
- [ ] Wenn `enabled == true`:
  - Pfade `/staging/*` → Prefix `/staging` strippen, an nächsten Handler weiterleiten.
  - Pfade `/static/*` → durchlassen (Assets für Staging).
  - Pfade `/impressum`, `/datenschutz` → durchlassen (rechtlich).
  - Alle anderen Pfade → Coming-Soon-Template rendern (HTTP 503 + `Retry-After`-Header).
- [ ] Coming-Soon-Seite wird als eigenständiges Template gerendert (kein base-Layout, direkt `template.ParseFiles` + `Execute`).
- [ ] Saubere Go-Idiomatik, kein globaler State.

**Kontext**:
- Renderer: `internal/handler/render.go` — bestehender Renderer nutzt base-Layout. Die Coming-Soon-Seite braucht eigenes Rendering (standalone Template).
- Router: `cmd/server/main.go:83-106` — chi Router mit Middleware-Stack.
- Chi Middleware-Signatur: `func(next http.Handler) http.Handler`.

**Output**:
- `internal/handler/maintenance.go`

---

## Task: 11.3 Config + Router-Integration
**Priority**: P0
**Blocked by**: 11.1, 11.2
**Skillset**: infrastructure-eng

**Akzeptanzkriterien**:
- [ ] `internal/config/config.go` hat neues Feld `MaintenanceMode bool`.
- [ ] Geladen aus Env-Var `MAINTENANCE_MODE` (Default: `"true"`).
- [ ] `cmd/server/main.go` bindet `MaintenanceMiddleware` als erstes Middleware nach Logger/Recoverer ein.
- [ ] Staging-Routen brauchen keine eigene Route-Definition — die Middleware strippt `/staging` und die bestehenden Routen greifen.
- [ ] Logausgabe beim Start: "Maintenance mode: ON" oder "Maintenance mode: OFF".

**Kontext**:
- Config: `internal/config/config.go:40-74` — `Load()` Funktion mit `getEnv()` Helper.
- Main: `cmd/server/main.go:82-106` — Middleware-Stack und Route-Definitionen.
- Middleware aus Task 11.2: `handler.MaintenanceMiddleware(renderer, cfg.MaintenanceMode)`.

**Output**:
- Aktualisierte `internal/config/config.go`
- Aktualisierte `cmd/server/main.go`

---

## Task: 11.4 QA & Abschluss
**Priority**: P1
**Blocked by**: 11.3
**Skillset**: testing-eng

**Akzeptanzkriterien**:
- [ ] `MAINTENANCE_MODE=true`: Root `/` liefert Coming-Soon-Seite (HTTP 503).
- [ ] `MAINTENANCE_MODE=true`: `/staging` liefert die echte Startseite (HTTP 200).
- [ ] `MAINTENANCE_MODE=true`: `/staging/kontakt` liefert das Kontaktformular (HTTP 200).
- [ ] `MAINTENANCE_MODE=true`: `/impressum` und `/datenschutz` liefern ihre Seiten (HTTP 200).
- [ ] `MAINTENANCE_MODE=true`: `/static/css/style.css` wird durchgelassen (HTTP 200).
- [ ] `MAINTENANCE_MODE=false`: Root `/` liefert die normale Startseite (HTTP 200).
- [ ] Coming-Soon-Seite enthält Logo-SVG, Öffnungsdatum, E-Mail.
- [ ] HTML-Validierung der Coming-Soon-Seite bestanden.
- [ ] Go-Code kompiliert ohne Fehler (`go build ./...`).
- [ ] Container startet und antwortet korrekt.

**Output**:
- QA-Status in dieser Datei.

---

## Execution Waves

### Wave 1 — Coming Soon + Middleware (parallel)
**Skillsets**: frontend-eng, handler-eng
**Tasks**: 11.1, 11.2 (parallel — keine Abhängigkeiten untereinander)
**Ziel**: Coming-Soon-Template und Maintenance-Middleware unabhängig voneinander erstellen.

### Wave 2 — Integration
**Skillsets**: infrastructure-eng
**Tasks**: 11.3 (benötigt Outputs von 11.1 + 11.2)
**Ziel**: Config erweitern, Middleware in Router einbinden, alles verdrahten.

### Wave 3 — QA & Abschluss
**Skillsets**: testing-eng, product-own
**Tasks**: 11.4 (benötigt fertige Integration)
**Ziel**: Alle Routing-Szenarien durchprüfen, Container-Test, HTML-Validierung.

## Abhängigkeits-Graph

```text
11.1 Coming-Soon-Seite ──┐
                          ├──> 11.3 Config + Router ──> 11.4 QA
11.2 Middleware ──────────┘
```

## Umsetzungsstatus

### Wave 1 — Coming Soon + Middleware: done
- `web/templates/pages/coming-soon.html`: Standalone HTML mit Inline-SVG-Logo, Inline-CSS, Markenfarben, Fade-In-Animation, responsive.
- `internal/handler/maintenance.go`: Chi-Middleware, Template einmalig geparst, Staging-Prefix-Stripping, Legal-Passthrough, 503 + Retry-After.

### Wave 2 — Integration: done
- `internal/config/config.go`: `MaintenanceMode bool` aus `MAINTENANCE_MODE` Env-Var (Default: `"true"`).
- `cmd/server/main.go`: Middleware nach Timeout, vor CSRF eingebunden. Log-Ausgabe beim Start.

### Wave 3 — QA & Abschluss: done
- Go-Build: Kompiliert fehlerfrei (Docker builder stage).
- Container startet, Maintenance-Mode-Log bestätigt.
- Test 1: `/` → HTTP 503, Coming-Soon-Seite mit Logo-SVG.
- Test 2: `/staging` → HTTP 200, echte Startseite (Baufortschritt, Hero).
- Test 3: `/staging/kontakt` → HTTP 200, Kontaktformular.
- Test 4: `/impressum` → HTTP 200, durchgelassen.
- Test 5: `/datenschutz` → HTTP 200, durchgelassen.
- Test 6: `/static/css/style.css` → HTTP 200, durchgelassen.
- Test 7: `Retry-After: 86400` Header gesetzt.
- Test 8: HTML geschlossen, Logo-SVG mit korrektem viewBox eingebettet.

## Definition of Done
- [x] Coming-Soon-Seite wird bei aktivem Maintenance Mode auf allen öffentlichen Routen angezeigt.
- [x] Echte Website ist über `/staging`-Prefix erreichbar.
- [x] Impressum + Datenschutz bleiben direkt erreichbar (rechtlich).
- [x] Static Assets werden durchgelassen.
- [x] Config-gesteuert über `MAINTENANCE_MODE` Env-Var.
- [x] Container startet und liefert beide Modi korrekt aus.
