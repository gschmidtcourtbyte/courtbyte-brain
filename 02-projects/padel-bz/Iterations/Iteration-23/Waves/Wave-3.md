# Wave 3: Tests fuer Allowlist, Admin-Bypass und Render-Smoke

**Status:** Completed
**Skill:** testing-eng
**Rolle:** testing-eng
**Kann parallel ausgefuehrt werden:** nein
**Blocked by:** Wave 1, Wave 2

## Ziel

Die Aenderungen aus Wave 1 und Wave 2 sind durch Tests abgesichert. Bestehende Allowlist-Tests bleiben gruen. Neue Tests beweisen, dass eine gueltige Admin-Session beide Middlewares passiert und dass ohne Admin-Cookie das alte Verhalten exakt erhalten bleibt.

## Tasks

### Task 23.9: Maintenance-Middleware-Bypass testen
**Priority:** P0
**Blocked by:** Wave 2
**Agent:** Agent C (testing-eng)

**Akzeptanzkriterien:**
- [ ] Neuer Testfall: Mit gueltigem Admin-Cookie liefert `MaintenanceMiddleware` (mit `enabled=true`) **nicht** die Coming-Soon-Seite, sondern reicht den Request an `next` weiter (z.B. 200 fuer `/`).
- [ ] Negativ-Test: Ohne Admin-Cookie bleibt das Verhalten identisch (503 + Coming-Soon-Body fuer `/`).
- [ ] Bestehende Tests in `internal/handler/maintenance_test.go` bleiben gruen.
- [ ] `nil`-`adminCheck` wird wie bisher behandelt (kein Bypass).

**Kontext:**
- Datei: `internal/handler/maintenance_test.go`.
- Hilfsfunktion bauen, die einen Stub fuer den `adminCheck` setzt (z.B. `func(_ *http.Request) bool { return true/false }`), damit der Test unabhaengig vom echten Cookie-Setup ist.

**Output:**
- Erweiterte `maintenance_test.go`.

### Task 23.10: Site-Auth-Bypass testen
**Priority:** P0
**Blocked by:** Wave 2
**Agent:** Agent C (testing-eng)

**Akzeptanzkriterien:**
- [ ] Neuer Testfall: Mit `adminCheck`, das `true` zurueckgibt, leitet `RequireSitePassword` einen Request an `/` an `next` weiter, ohne 302 auf `/site-login`.
- [ ] Negativ-Test: Mit `adminCheck`, das `false` zurueckgibt, bleibt der 302-Redirect auf `/site-login` erhalten.
- [ ] Exempt-Pfade (`/static/`, `/admin/`, `/site-login`, `/videos/`, `/kontakt`, `/impressum`, `/datenschutz`, `/agb`) verhalten sich unveraendert.
- [ ] Bestehende Tests in `internal/handler/site_auth_test.go` bleiben gruen.

**Kontext:**
- Datei: `internal/handler/site_auth_test.go`.

**Output:**
- Erweiterte `site_auth_test.go`.

### Task 23.11: Render-Smoke fuer Legal-Seiten weiter gruen halten
**Priority:** P1
**Blocked by:** Wave 1
**Agent:** Agent C (testing-eng)

**Akzeptanzkriterien:**
- [ ] `go test ./internal/handler -run TestRendererRendersContactAndAGBPages` ist gruen.
- [ ] Neuer Smoke-Check, dass `/datenschutz` rendert (falls nicht schon abgedeckt).
- [ ] Optional: Test, dass `nav.html` keine Substring-Treffer fuer `/admin` enthaelt.

**Kontext:**
- Datei: `internal/handler/render_test.go`.

**Output:**
- Aktualisierte `render_test.go` (falls noetig).

### Task 23.12: Gesamttestlauf
**Priority:** P0
**Blocked by:** 23.9, 23.10, 23.11
**Agent:** Agent C (testing-eng)

**Akzeptanzkriterien:**
- [ ] `go test ./...` ist gruen.
- [ ] `go vet ./...` ist gruen.
- [ ] `git diff --check` ist gruen.

**Output:**
- Testlog im Wave-Abschluss als Beleg.

## Wave-3-Exit-Gate

- [ ] Neue Bypass-Tests vorhanden und gruen.
- [ ] Bestehende Allowlist-Tests unveraendert gruen.
- [ ] `go test ./...` insgesamt gruen.

## Notes

- Reine Test-Wave. Keine Code-Aenderung an `internal/handler/` ausser in Test-Dateien.
- Wenn die Implementierung in Wave 2 die Funktionssignaturen anders waehlt (z.B. Methoden-Setter statt Konstruktor-Parameter), Tests entsprechend anpassen — die Aussage des Tests bleibt gleich: Bypass nur bei `IsAdmin == true`.
