# Wave 2: Integration und Email-Umstellung (nach Wave 1)

**Status:** Done (Legacy-Migration)
**Skill:** infrastructure-eng, service-eng
**Quelle:** `plan/iterations/iteration-17.md`

## Legacy-Inhalt

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
