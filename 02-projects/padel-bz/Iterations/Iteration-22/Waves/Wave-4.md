# Wave 4: Tests und Regression

**Status:** Planned
**Skill:** testing-eng
**Rolle:** testing-eng
**Kann parallel ausgefuehrt werden:** nein
**Blocked by:** Wave 2, Wave 3

## Ziel

Das neue Prelaunch-Verhalten wird durch fokussierte Tests abgesichert: oeffentliche Allowlist, Coming-Soon-Fallback, Legal-Rendering und Kontaktformular bleiben korrekt.

## Tasks

### Task 22.10: Middleware- und SiteAuth-Tests aktualisieren
**Priority:** P0
**Blocked by:** Wave 2
**Agent:** Agent D (testing-eng)

**Akzeptanzkriterien:**
- [ ] Tests belegen, dass `/kontakt` GET ohne Site-Passwort erlaubt ist.
- [ ] Tests belegen, dass `/kontakt` POST ohne Site-Passwort erlaubt ist.
- [ ] Tests belegen, dass `/agb`, `/datenschutz`, `/impressum` erlaubt sind.
- [ ] Tests belegen, dass nicht erlaubte Pfade im Maintenance Mode die Coming-Soon-Seite erhalten.
- [ ] Bestehende SiteAuth-Tests sind an das neue Produktverhalten angepasst.

**Kontext:**
- `internal/handler/site_auth_test.go`
- Neue oder bestehende Tests fuer `internal/handler/maintenance.go`

**Output:**
- Aktualisierte Handler-Tests.

### Task 22.11: Render-Tests fuer Legal- und Coming-Soon-Seiten
**Priority:** P1
**Blocked by:** Wave 1, Wave 3
**Agent:** Agent D (testing-eng)

**Akzeptanzkriterien:**
- [ ] `agb.html` rendert und enthaelt gelieferte Kernbegriffe.
- [ ] `datenschutz.html` rendert und enthaelt gelieferte Kernbegriffe.
- [ ] `coming-soon.html` rendert im Maintenance-Kontext.
- [ ] Der Aktionstext und der Aktionsbedingungen-Trigger sind im HTML vorhanden.

**Kontext:**
- `internal/handler/render_test.go`
- `web/templates/pages/coming-soon.html`

**Output:**
- Render-Testabdeckung fuer neue Inhalte.

### Task 22.12: Regression und Quality Gate ausfuehren
**Priority:** P0
**Blocked by:** 22.10, 22.11
**Agent:** Agent D (testing-eng)

**Akzeptanzkriterien:**
- [ ] `go test ./...` ist gruen.
- [ ] `git diff --check` ist gruen.
- [ ] Kontaktformular-Validierung fuer Hansefit bleibt gruen.
- [ ] CSRF- und Rate-Limiting-Pfade werden nicht deaktiviert.
- [ ] Keine Template-Parse-Fehler.

**Kontext:**
- Gesamtes Code-Repo.

**Output:**
- QA-Notiz mit ausgefuehrten Checks und Ergebnis.

## Wave-4-Exit-Gate

- [ ] Relevante Tests sind angepasst.
- [ ] `go test ./...` ist gruen.
- [ ] `git diff --check` ist gruen.

## Notes

- Falls externe Ressourcen wie Google Fonts nicht erreichbar sind, zaehlt das nicht als Testblocker fuer SSR-Rendering, solange Template- und Routenverhalten korrekt sind.

## Abschluss

- [ ] Wave abgeschlossen
