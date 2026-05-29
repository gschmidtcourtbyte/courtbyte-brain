# Wave 3: Tests, QA und Review (nach Wave 2)

**Status:** Done (Legacy-Migration)
**Skill:** testing-eng, review-eng
**Quelle:** `plan/iterations/iteration-17.md`

## Legacy-Inhalt

## Wave 3 — Tests, QA und Review (nach Wave 2)
**Agenten**: Agent E, Agent R
**Kann parallel ausgefuehrt werden**: Tests und QA ja, Review nach Tests
**Ziel**: Alles ist getestet, reviewed und ready fuer den Testbetrieb.

### Task 17.6: Unit- und Integrationstests fuer Site-Auth
**Priority**: P0
**Blocked by**: 17.2, 17.4
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Test: Nicht-authentifizierter Request auf `/` wird auf `/site-login` umgeleitet.
- [ ] Test: Nicht-authentifizierter Request auf `/kontakt` wird auf `/site-login` umgeleitet.
- [ ] Test: Request auf `/static/css/style.css` ist ohne Auth erreichbar.
- [ ] Test: Request auf `/admin/login` ist ohne Site-Auth erreichbar (hat eigene Auth).
- [ ] Test: Request auf `/impressum` ist ohne Site-Auth erreichbar.
- [ ] Test: Request auf `/datenschutz` ist ohne Site-Auth erreichbar.
- [ ] Test: Request auf `/agb` ist ohne Site-Auth erreichbar.
- [ ] Test: Request auf `/site-login` (GET) liefert 200 mit Login-Formular.
- [ ] Test: POST `/site-login` mit korrektem Passwort setzt Session-Cookie und redirected auf `/`.
- [ ] Test: POST `/site-login` mit falschem Passwort liefert 401 mit Fehlermeldung.
- [ ] Test: Request mit gueltigem Session-Cookie auf `/` liefert 200 (Homepage).
- [ ] Test: Bestehende Tests bleiben gruen (`go test ./...`).

**Kontext**:
- `internal/handler/site_auth.go`
- `internal/handler/admin_auth.go` — Referenz fuer Test-Pattern.
- `internal/handler/render_test.go`, `internal/handler/admin_hansefit_test.go` — bestehende Tests.

**Output**:
- `internal/handler/site_auth_test.go` — umfassende Tests.

### Task 17.7: Email-Adressen-Verifikation
**Priority**: P1
**Blocked by**: 17.5
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] `grep -r "@courts-bz\.de"` im Repo liefert nur Treffer in `plan/iterations/` (historische Referenzen).
- [ ] Alle Templates zeigen `hallo@courts-padelclub.de` an der korrekten Stelle.
- [ ] Config-Default fuer `SMTPFrom` ist `hallo@courts-padelclub.de`.
- [ ] `.env.example` zeigt `hallo@courts-padelclub.de`.

**Kontext**:
- Alle Template-Dateien in `web/templates/pages/`.
- `internal/config/config.go`
- `.env.example`
- `docs/Content.md`

**Output**:
- Verifikationsnotiz oder Grep-Ausgabe.

### Task 17.8: Manuelle QA der Login-Seite und des Passwortschutzes
**Priority**: P0
**Blocked by**: 17.4, 17.5
**Agent**: Agent E (testing-eng)

**Akzeptanzkriterien**:
- [ ] Dev-Server starten (`make dev`), Browser oeffnen.
- [ ] Aufruf von `http://localhost:8080/` redirected auf `/site-login`.
- [ ] Login-Seite zeigt das Logo zentriert ueber dem Formular.
- [ ] Hintergrund ist dunkelgruen (`#475243`), Schrift ist hell (`#FAF2E8`).
- [ ] Passwortfeld und Button sind sauber dargestellt.
- [ ] Falsches Passwort zeigt Fehlermeldung.
- [ ] Korrektes Passwort (`Courts2026!`) leitet auf Homepage weiter.
- [ ] Nach Login sind alle Seiten erreichbar (Home, Kontakt, etc.).
- [ ] Impressum, Datenschutz, AGB sind auch ohne Login erreichbar.
- [ ] Admin-Login ist weiterhin unter `/admin/login` erreichbar.
- [ ] Emailadressen auf Home, Kontakt, Impressum, Datenschutz zeigen `hallo@courts-padelclub.de`.
- [ ] Mobile-Viewport: Login-Seite sieht gut aus.
- [ ] Desktop-Viewport: Login-Seite sieht gut aus.

**Kontext**:
- Lokaler Dev-Server.
- Browser Desktop und Mobile Viewport.

**Output**:
- QA-Notiz mit Screenshots oder Beschreibungen.

### Task 17.9: Finaler Review
**Priority**: P0
**Blocked by**: 17.6, 17.7, 17.8
**Agent**: Agent R (review-eng), PO Lead

**Akzeptanzkriterien**:
- [ ] Passwort wird als bcrypt-Hash gespeichert, nie im Klartext im Code.
- [ ] Session-Cookie ist HttpOnly und Secure (in Production).
- [ ] CSRF-Schutz ist aktiv auf der Login-Seite.
- [ ] Keine Hardcoded Emailadressen im Go-Code (nur in Config-Defaults und Templates).
- [ ] Middleware-Exemptionen sind korrekt: kein Leak von geschuetzten Inhalten.
- [ ] Kein Zugang zu geschuetzten Seiten ohne gueltige Session.
- [ ] Alle Tests sind gruen.
- [ ] Keine ungewollten Aenderungen ausserhalb des Iterationsscopes.
- [ ] Maintenance-Middleware und Site-Auth-Middleware koennen koexistieren.
- [ ] SMTP_FROM-Default ist konsistent in Config und .env.example.

**Kontext**:
- Vollstaendiger Diff der Iteration.
- Test- und QA-Ergebnisse.

**Output**:
- Review-Findings.
- Merge-Readiness-Entscheidung.

**Wave-3-Exit-Gate**:
- [ ] Alle P0-Tests gruen.
- [ ] QA der Login-Seite bestanden.
- [ ] Email-Verifikation bestanden.
- [ ] Review blockerfrei.
- [ ] PO Lead gibt Iteration 17 frei.

---
