# Iteration 17: Site-Passwortschutz fuer Testbetrieb und Email-Umstellung

## Ziel
Die Website geht testweise online. Alle oeffentlichen Seiten werden durch ein einfaches Site-Passwort geschuetzt, damit nur eingeweihte Personen die Seite sehen koennen. Die Passwort-Seite traegt bereits das Corporate Design mit zentriertem Logo. Zusaetzlich werden alle Emailadressen der gesamten Seite (Sender, Empfaenger, Kontaktanzeigen) auf `hallo@courts-padelclub.de` umgestellt.

## Ausgangslage
- Die Website laeuft aktuell mit Maintenance-Mode (`MAINTENANCE_MODE=true`), der eine Coming-Soon-Seite zeigt und den echten Content unter `/staging` zugaenglich macht.
- Ein Admin-Auth-System existiert bereits (`internal/handler/admin_auth.go`) mit Session-basierter Authentifizierung und bcrypt-Passwort-Hash. Dieses schuetzt nur `/admin/*`.
- Emailadressen sind aktuell `info@courts-bz.de` (in Templates) und `noreply@courts-bz.de` (als SMTP-Sender-Default).
- Das Logo `web/static/img/logo_courts_transparent.svg` existiert und wird bereits auf der Homepage verwendet.
- Referenzprojekt `shop-based-webapp` nutzt ein vollstaendiges User-Auth-System — fuer den Testbetrieb wird ein einfacherer Ansatz gewaehlt: ein Site-weites Passwort ohne Benutzername.
- Die Design-Tokens (Custom Properties) sind in `web/static/css/style.css` definiert: `--bg: #475243`, `--fg: #FAF2E8`, `--accent: #D8633D`, Fonts Space Grotesk / Fraunces / JetBrains Mono.

## Scope
- Neue Middleware `SitePasswordMiddleware` die alle oeffentlichen Routen hinter ein Passwort stellt.
- Neue Template-Seite `web/templates/pages/site-login.html` im Corporate Design.
- Config-Erweiterung um `SITE_PASSWORD_HASH` Environment Variable.
- Handler fuer GET/POST der Site-Login-Seite.
- Session-Handling: Nach einmaliger Passworteingabe bleibt der Zugang ueber eine Cookie-Session erhalten.
- Alle Emailadressen (Templates, Config-Defaults, .env.example, Docs) auf `hallo@courts-padelclub.de` aendern.
- Notification-Email (Empfaenger) und SMTP-From (Sender) werden beide ueber Config gesteuert — der Default aendert sich.
- Maintenance-Mode bleibt strukturell erhalten, wird aber fuer den Testbetrieb durch die Passwort-Middleware ersetzt.

## Nicht im Scope
- Vollstaendiges User-Auth-System mit Benutzername + Passwort + Registrierung.
- Rate-Limiting auf der Site-Login-Seite (kann in spaeterer Iteration ergaenzt werden).
- DNS/Domain-Setup fuer `courts-padelclub.de`.
- SMTP-Konfiguration fuer die neue Domain (wird in Railway Env gesetzt).
- Aenderung des Admin-Auth-Systems.
- SSL/TLS-Konfiguration.
- Deployment auf Railway (folgt als naechste Iteration).

## Produktentscheidungen
- **Nur Passwort, kein Benutzername**: Es ist ein Site-Gate, keine User-Authentifizierung. Ein einzelnes Passwort reicht.
- **Passwort**: `Courts2026!` — wird als bcrypt-Hash in `SITE_PASSWORD_HASH` konfiguriert.
- **Session-Dauer**: 30 Tage (`MaxAge: 86400 * 30`), damit Tester nicht staendig neu eingeben muessen.
- **Exempt Pfade**: `/static/*`, `/admin/*` (hat eigene Auth), `/impressum`, `/datenschutz`, `/agb` (rechtlich zugaenglich), `/site-login` (die Login-Seite selbst), `/videos/*`.
- **Maintenance-Mode**: Wird im Testbetrieb deaktiviert (`MAINTENANCE_MODE=false`). Die Passwort-Middleware uebernimmt den Schutz.
- **Email-Adresse**: `hallo@courts-padelclub.de` wird sowohl als Absender (SMTP_FROM) als auch als angezeigte Kontaktadresse in allen Templates verwendet. Die Notification-Email (Admin-Benachrichtigung bei neuer Kontaktanfrage) geht ebenfalls an `hallo@courts-padelclub.de`.
- **Notification-Empfaenger**: Aktuell sendet `sendNotificationEmail()` an `s.config.SMTPFrom` — also an sich selbst. Fuer die neue Adresse ist das korrekt: Absender und Empfaenger der Notification sind `hallo@courts-padelclub.de`.
- **Corporate Design der Login-Seite**: Dunkler Hintergrund (`--bg`), zentriertes Logo, minimales Formular mit einem Passwortfeld und Submit-Button im Accent-Stil (`--accent`). Keine Navigation, kein Footer — nur Logo + Formular + dezenter Copyright-Hinweis.

## Critical Path
1. Config um `SITE_PASSWORD_HASH` erweitern.
2. Site-Login-Handler (GET Formular, POST Validierung) implementieren.
3. Site-Password-Middleware implementieren.
4. Login-Template im Corporate Design erstellen.
5. Middleware und Handler in `main.go` verdrahten.
6. Alle Emailadressen in Templates, Config und Docs aendern.
7. Tests und QA.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Wave-Freigabe, Akzeptanzkriterien |
| Agent A | handler-eng | Site-Login-Handler (GET/POST), Session-Handling |
| Agent B | infrastructure-eng | Config-Erweiterung, Middleware, Router-Verdrahtung |
| Agent C | frontend-eng | Site-Login-Template im Corporate Design |
| Agent D | service-eng | Email-Adressen in Service/Config/Templates aendern |
| Agent E | testing-eng | Unit Tests, Integration Tests, Manuelle QA |
| Agent R | review-eng | Finaler Review |

---

## Wave 1 — Config, Middleware und Login-Handler (parallel)
**Agenten**: Agent A, Agent B, Agent C
**Kann parallel ausgefuehrt werden**: ja — drei unabhaengige Arbeitsstroeme
**Ziel**: Alle Bausteine fuer den Passwortschutz existieren als einzelne Komponenten.

### Task 17.1: Config um SITE_PASSWORD_HASH erweitern
**Priority**: P0
**Blocked by**: none
**Agent**: Agent B (infrastructure-eng)

**Akzeptanzkriterien**:
- [ ] `internal/config/config.go` hat ein neues Feld `SitePasswordHash string`.
- [ ] Default-Wert ist ein bcrypt-Hash von `Courts2026!` (als Konstante `defaultSitePasswordHash`).
- [ ] Environment Variable heisst `SITE_PASSWORD_HASH`.
- [ ] `.env.example` enthaelt `SITE_PASSWORD_HASH=` mit Kommentar.
- [ ] Bestehende Config-Tests bleiben gruen.

**Kontext**:
- `internal/config/config.go` — bestehendes Config-Pattern mit `getEnv()` und Default-Konstanten.
- `internal/config/config_test.go`
- `.env.example`

**Output**:
- Erweitertes Config-Struct und .env.example.

### Task 17.2: Site-Login-Handler implementieren
**Priority**: P0
**Blocked by**: none
**Agent**: Agent A (handler-eng)

**Akzeptanzkriterien**:
- [ ] Neue Datei `internal/handler/site_auth.go` mit `SiteAuthHandler` struct.
- [ ] `SiteAuthHandler` hat Felder: `templateDir string`, `config *config.Config`, `store *sessions.CookieStore`.
- [ ] Konstruktor `NewSiteAuthHandler(templateDir string, cfg *config.Config) *SiteAuthHandler` erstellt einen eigenen CookieStore.
- [ ] Session-Name: `padel_bz_site` (konfigurierbar oder Konstante).
- [ ] Session-MaxAge: `86400 * 30` (30 Tage).
- [ ] `LoginForm(w, r)` rendert das Site-Login-Template mit CSRF-Token. Wenn bereits authentifiziert, Redirect auf `/`.
- [ ] `Login(w, r)` validiert das Passwort per `bcrypt.CompareHashAndPassword` gegen `config.SitePasswordHash`.
- [ ] Bei korrektem Passwort: Session-Cookie setzen mit `authenticated = true`, Redirect auf `/`.
- [ ] Bei falschem Passwort: Template erneut rendern mit `Error: true`, HTTP 401.
- [ ] Methode `IsAuthenticated(r) bool` fuer Middleware-Nutzung (oeffentlich).
- [ ] Methode `RequireSitePassword(next http.Handler) http.Handler` als Middleware.
- [ ] Middleware prueft `IsAuthenticated(r)` — wenn nicht, Redirect auf `/site-login`.
- [ ] Middleware laesst exempt Pfade durch: `/static/`, `/admin/`, `/site-login`, `/impressum`, `/datenschutz`, `/agb`, `/videos/`.
- [ ] Template-Rendering nutzt `template.ParseFiles()` wie in `admin_auth.go`.

**Kontext**:
- `internal/handler/admin_auth.go` — Referenz-Implementierung fuer Session-basierte Auth.
- `internal/config/config.go` — Config-Struct.
- `cmd/server/main.go` — Router-Setup.

**Output**:
- `internal/handler/site_auth.go` — vollstaendiger Handler mit Middleware.

### Task 17.3: Site-Login-Template im Corporate Design
**Priority**: P0
**Blocked by**: none
**Agent**: Agent C (frontend-eng)

**Akzeptanzkriterien**:
- [ ] Neue Datei `web/templates/pages/site-login.html`.
- [ ] Eigenstaendiges HTML-Dokument (kein Base-Layout-Template, wie `admin/login.html`).
- [ ] Laedt die gleichen Google Fonts wie `base.html` (Space Grotesk, Fraunces, JetBrains Mono).
- [ ] Laedt `/static/css/style.css` fuer Custom Properties.
- [ ] Hintergrund: `var(--bg)` (#475243).
- [ ] Schriftfarbe: `var(--fg)` (#FAF2E8).
- [ ] Layout: Vertikal zentriert auf der Seite (Flexbox/Grid).
- [ ] Logo: `<img src="/static/img/logo_courts_transparent.svg" alt="courts padelclub">` zentriert ueber dem Formular.
- [ ] Logo-Breite: ca. 200-280px, responsive skaliert.
- [ ] Darunter: Einfaches Formular mit einem Passwort-Feld und einem Submit-Button.
- [ ] Passwort-Label: "Passwort" oder "Zugangspasswort".
- [ ] Submit-Button: `btn btn-primary` Stil (Accent-Farbe), Text "Zugang" oder "Eintreten".
- [ ] Fehlermeldung bei falschem Passwort: dezenter Alert im bestehenden `.alert .alert-error` Stil.
- [ ] CSRF-Token als Hidden Field: `<input type="hidden" name="gorilla.csrf.Token" value="{{.CSRFToken}}">`.
- [ ] Form-Action: `POST /site-login`.
- [ ] Dezenter Copyright-Hinweis am unteren Rand: "courts padelclub Bad Zwischenahn".
- [ ] Keine Navigation, kein Footer, kein Link zu anderen Seiten.
- [ ] Mobile-first: Formular ist auf kleinen Bildschirmen gut nutzbar.
- [ ] Kein externer JavaScript-Bedarf (nur CSS + HTML + CSRF).
- [ ] Favicon und Meta-Tags wie in `base.html`.
- [ ] `<meta name="robots" content="noindex, nofollow">` damit die Login-Seite nicht indexiert wird.

**Kontext**:
- `web/templates/admin/login.html` — Referenz fuer eigenstaendiges Login-Template.
- `web/templates/layouts/base.html` — Font-Imports und Meta-Tags.
- `web/static/css/style.css` — Design Tokens und bestehende Styles.
- `web/static/img/logo_courts_transparent.svg` — Logo-Datei.

**Output**:
- `web/templates/pages/site-login.html` — Corporate Design Login-Seite.

**Wave-1-Exit-Gate**:
- [ ] Config kennt `SITE_PASSWORD_HASH`.
- [ ] Handler kann Passwort validieren und Session setzen.
- [ ] Template existiert und nutzt Corporate Design.
- [ ] Alle drei Komponenten sind unabhaengig testbar.

---

## Wave 2 — Integration und Email-Umstellung (nach Wave 1)
**Agenten**: Agent B, Agent D
**Kann parallel ausgefuehrt werden**: ja (Router-Integration und Email-Aenderungen sind unabhaengig)
**Ziel**: Passwortschutz ist in den Router integriert und alle Emailadressen sind umgestellt.

### Task 17.4: Router-Integration des Passwortschutzes
**Priority**: P0
**Blocked by**: 17.1, 17.2, 17.3
**Agent**: Agent B (infrastructure-eng)

**Akzeptanzkriterien**:
- [ ] `cmd/server/main.go` erstellt einen `SiteAuthHandler` mit `NewSiteAuthHandler("web/templates", cfg)`.
- [ ] Route `GET /site-login` zeigt das Login-Formular (ausserhalb der Middleware-Gruppe).
- [ ] Route `POST /site-login` verarbeitet das Login (ausserhalb der Middleware-Gruppe).
- [ ] `SiteAuthHandler.RequireSitePassword` wird als Middleware vor den oeffentlichen Routen eingehaengt.
- [ ] Statische Dateien (`/static/*`, `/videos/*`) bleiben ohne Passwort erreichbar.
- [ ] Admin-Routen (`/admin/*`) bleiben bei ihrer eigenen Auth.
- [ ] Rechtliche Seiten (`/impressum`, `/datenschutz`, `/agb`) bleiben ohne Passwort erreichbar.
- [ ] Nach erfolgreichem Login landet der Nutzer auf `/` (Homepage).
- [ ] Nicht-authentifizierte Anfragen auf `/`, `/kontakt`, etc. werden auf `/site-login` umgeleitet.
- [ ] CSRF-Middleware muss VOR der Site-Auth-Middleware laufen, damit das Login-Formular CSRF-geschuetzt ist.
- [ ] Maintenance-Mode und Site-Password-Middleware koennen koexistieren, aber fuer den Testbetrieb wird `MAINTENANCE_MODE=false` empfohlen.

**Kontext**:
- `cmd/server/main.go` — aktuelles Router-Setup mit chi.
- `internal/handler/site_auth.go` (aus Task 17.2).
- `internal/handler/maintenance.go` — bestehende Maintenance-Middleware.

**Output**:
- Aktualisierte `cmd/server/main.go` mit Site-Auth-Integration.

### Task 17.5: Email-Adressen auf hallo@courts-padelclub.de umstellen
**Priority**: P1
**Blocked by**: none (kann parallel zu 17.4 laufen)
**Agent**: Agent D (service-eng)

**Akzeptanzkriterien**:
- [ ] `internal/config/config.go` Zeile 83: Default von `SMTP_FROM` aendern auf `hallo@courts-padelclub.de`.
- [ ] `.env.example` Zeile 28: `SMTP_FROM=hallo@courts-padelclub.de`.
- [ ] `web/templates/pages/home.html`: `info@courts-bz.de` → `hallo@courts-padelclub.de` (mailto und Anzeigetext).
- [ ] `web/templates/pages/contact.html`: `info@courts-bz.de` → `hallo@courts-padelclub.de` (mailto und Anzeigetext).
- [ ] `web/templates/pages/impressum.html`: `info@courts-bz.de` → `hallo@courts-padelclub.de`.
- [ ] `web/templates/pages/datenschutz.html`: Beide Vorkommen von `info@courts-bz.de` → `hallo@courts-padelclub.de`.
- [ ] `web/templates/pages/coming-soon.html`: `info@courts-bz.de` → `hallo@courts-padelclub.de`.
- [ ] `docs/Content.md`: Alle Vorkommen von `info@courts-bz.de` → `hallo@courts-padelclub.de`.
- [ ] Keine Vorkommen von `@courts-bz.de` verbleiben im gesamten Repo (ausser in Iterationsplaenen als historische Referenz).
- [ ] `sendNotificationEmail()` und `sendHansefitConfirmationEmail()` nutzen weiterhin `s.config.SMTPFrom` — keine Hardcoded-Adressen.

**Kontext**:
- Grep-Ergebnis: 10+ Vorkommen von `@courts-bz.de` in Templates, Config und Docs.
- `internal/service/contact_service.go` — Email-Versand nutzt bereits `s.config.SMTPFrom`, keine Hardcoded-Adressen.

**Output**:
- Aktualisierte Templates, Config und Docs.

**Wave-2-Exit-Gate**:
- [ ] Website ist hinter Passwort geschuetzt.
- [ ] Login mit `Courts2026!` funktioniert.
- [ ] Session bleibt nach Login erhalten.
- [ ] Exempt Pfade sind ohne Passwort erreichbar.
- [ ] Alle sichtbaren Emailadressen zeigen `hallo@courts-padelclub.de`.
- [ ] Config-Default fuer SMTP_FROM ist `hallo@courts-padelclub.de`.

---

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

## Parallelisierung

```text
Wave 1 (alle parallel, keine Abhaengigkeiten):
  Agent B -> 17.1 Config SITE_PASSWORD_HASH
  Agent A -> 17.2 Site-Login-Handler
  Agent C -> 17.3 Site-Login-Template (Corporate Design)

Wave 2 (nach Wave 1, untereinander parallel):
  Agent B -> 17.4 Router-Integration           (nach 17.1 + 17.2 + 17.3)
  Agent D -> 17.5 Email-Adressen umstellen      (unabhaengig, kann auch in Wave 1)

Wave 3 (nach Wave 2):
  Agent E -> 17.6 Unit-/Integrationstests       (nach 17.2 + 17.4)
  Agent E -> 17.7 Email-Verifikation            (nach 17.5)
  Agent E -> 17.8 Manuelle QA                   (nach 17.4 + 17.5)
  Agent R -> 17.9 Finaler Review                (nach 17.6 + 17.7 + 17.8)
```

**Optimierte Ausfuehrungsreihenfolge**:
- 17.1, 17.2, 17.3 und 17.5 koennen alle sofort parallel starten.
- 17.4 wartet auf die drei Wave-1-Tasks und integriert die Ergebnisse.
- 17.6 und 17.7 koennen parallel laufen.
- 17.8 benoetigt den Dev-Server mit allen Aenderungen.
- 17.9 ist der finale Gate-Keeper.

## Technische Details

### bcrypt-Hash fuer Courts2026!
Der Hash wird einmalig generiert und als Konstante `defaultSitePasswordHash` eingetragen:
```go
// Generierung:
// hash, _ := bcrypt.GenerateFromPassword([]byte("Courts2026!"), bcrypt.DefaultCost)
const defaultSitePasswordHash = "<bei Task 17.1 generieren>"
```

### Session-Architektur
```
Besucher -> GET /
  └── SitePasswordMiddleware prueft Session
      ├── Session vorhanden + authenticated=true -> weiter zu Handler
      └── Keine Session -> Redirect 303 auf /site-login

Besucher -> POST /site-login
  └── SiteAuthHandler.Login
      ├── bcrypt.CompareHashAndPassword(hash, password)
      │   ├── OK -> Session setzen, Redirect 303 auf /
      │   └── Fail -> Template mit Error, 401
```

### Exempt-Pfade der Middleware
```go
exemptPrefixes := []string{"/static/", "/admin/", "/site-login", "/videos/"}
exemptExact    := []string{"/impressum", "/datenschutz", "/agb"}
```

### Email-Aenderungen (vollstaendige Liste)
| Datei | Zeile | Alt | Neu |
|-------|-------|-----|-----|
| `internal/config/config.go` | 83 | `noreply@courts-bz.de` | `hallo@courts-padelclub.de` |
| `.env.example` | 28 | `noreply@courts-bz.de` | `hallo@courts-padelclub.de` |
| `web/templates/pages/home.html` | 323 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |
| `web/templates/pages/contact.html` | 228 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |
| `web/templates/pages/impressum.html` | 24 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |
| `web/templates/pages/datenschutz.html` | 25 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |
| `web/templates/pages/datenschutz.html` | 68 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |
| `web/templates/pages/coming-soon.html` | 169 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |
| `docs/Content.md` | 102, 148, 160, 176, 177 | `info@courts-bz.de` | `hallo@courts-padelclub.de` |

## Risiken und offene Punkte
- **Passwort im Klartext in der Iteration**: `Courts2026!` steht in diesem Plan. Der Plan sollte nicht oeffentlich zugaenglich sein. Im Code wird nur der bcrypt-Hash gespeichert.
- **Session-Secret**: Fuer Production muss `SESSION_SECRET` auf einen sicheren Wert gesetzt werden. Der Default `change-me-in-production-min-32-chars` ist nicht fuer den Testbetrieb geeignet.
- **SMTP-Konfiguration**: Die neue Adresse `hallo@courts-padelclub.de` muss im SMTP-Provider (Railway/Mailgun/etc.) konfiguriert werden, bevor Emails versendet werden koennen.
- **Maintenance-Mode vs. Site-Auth**: Beide Middlewares koennen gleichzeitig aktiv sein. Empfehlung: Fuer Testbetrieb `MAINTENANCE_MODE=false` setzen und nur Site-Auth nutzen.
- **Cookie-Sicherheit**: Im Development-Modus ist `Secure: false` (HTTP), in Production `Secure: true` (HTTPS). Der CookieStore nutzt `cfg.IsProduction()`.

## Definition of Done
- [ ] Alle oeffentlichen Seiten sind hinter dem Site-Passwort geschuetzt.
- [ ] Login mit `Courts2026!` funktioniert und setzt eine 30-Tage-Session.
- [ ] Login-Seite traegt Corporate Design mit zentriertem Logo.
- [ ] Exempt-Pfade (Static, Admin, Legal) sind ohne Passwort erreichbar.
- [ ] CSRF-Schutz ist auf der Login-Seite aktiv.
- [ ] Alle Emailadressen zeigen `hallo@courts-padelclub.de`.
- [ ] Config-Default fuer SMTP_FROM ist `hallo@courts-padelclub.de`.
- [ ] Passwort ist als bcrypt-Hash gespeichert, nie im Klartext im Go-Code.
- [ ] Unit-Tests fuer Site-Auth sind vorhanden und gruen.
- [ ] `go test ./...` ist gruen.
- [ ] Manuelle QA auf Desktop und Mobile bestanden.
- [ ] Finaler Review ist blockerfrei abgeschlossen.
