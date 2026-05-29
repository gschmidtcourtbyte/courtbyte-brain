# Wave 1: Config, Middleware und Login-Handler (parallel)

**Status:** Done (Legacy-Migration)
**Skill:** infrastructure-eng, handler-eng, frontend-eng
**Quelle:** `plan/iterations/iteration-17.md`

## Legacy-Inhalt

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
