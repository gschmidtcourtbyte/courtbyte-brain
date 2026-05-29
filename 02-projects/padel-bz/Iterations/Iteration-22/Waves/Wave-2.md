# Wave 2: Public-Allowlist und Prelaunch-Gate

**Status:** Completed
**Skill:** handler-eng
**Rolle:** handler-eng
**Kann parallel ausgefuehrt werden:** nein
**Blocked by:** Wave 1 fuer Legal-Pfade, sonst none

## Ziel

Das Routing bildet den Prelaunch-Zustand korrekt ab: oeffentlich erreichbar sind nur Coming Soon, Hansefitregistrierung, Impressum, Datenschutz, AGB und statische Assets. Der restliche Website-Content bleibt fuer Besucher gesperrt.

## Tasks

### Task 22.4: Maintenance-Allowlist erweitern
**Priority:** P0
**Blocked by:** none
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [x] Bei `MAINTENANCE_MODE=true` wird `/kontakt` GET an den Kontakt-Handler durchgereicht.
- [x] Bei `MAINTENANCE_MODE=true` wird `/kontakt` POST an den Kontakt-Handler durchgereicht.
- [x] Bei `MAINTENANCE_MODE=true` wird `/agb` durchgereicht.
- [x] `/impressum`, `/datenschutz`, `/static/*`, `/admin/*` bleiben durchgereicht.
- [x] `/` und nicht freigegebene Marketing-/Content-Pfade zeigen die Coming-Soon-Seite.
- [x] Kommentar/Code-Doku der Middleware nennt die aktuelle Allowlist korrekt.

**Kontext:**
- `internal/handler/maintenance.go`
- `cmd/server/main.go`
- CSRF-Middleware bleibt global aktiv.

**Output:**
- Korrigierte Maintenance-Middleware.

### Task 22.5: Site-Password-Gate fuer Hansefitregistrierung freigeben
**Priority:** P0
**Blocked by:** none
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [x] `/kontakt` GET ist ohne Site-Passwort erreichbar.
- [x] `/kontakt` POST ist ohne Site-Passwort erreichbar.
- [x] Nicht erlaubte Content-Routen bleiben ohne Site-Passwort gesperrt oder laufen in Maintenance auf Coming Soon.
- [x] Admin-Routen bleiben nur ueber Admin-Auth geschuetzt.
- [x] Bestehende Preview-/Staging-Funktionalitaet bleibt fuer Betreiber erhalten.

**Kontext:**
- Aktuell liegt `/kontakt` in der password-geschuetzten Route-Gruppe.
- Option: Kontakt-Routen ausserhalb der protected Group registrieren und SiteAuth-Tests anpassen.

**Output:**
- Router-/Middleware-Anpassung fuer oeffentliche Hansefitregistrierung.

### Task 22.6: Unerwuenschte oeffentliche Links entfernen/entschaerfen
**Priority:** P1
**Blocked by:** 22.4, 22.5
**Agent:** Agent B (handler-eng)

**Akzeptanzkriterien:**
- [x] Coming-Soon-Template verlinkt oeffentlich nur erlaubte Seiten.
- [x] Keine oeffentliche Navigation zeigt Links auf die gesperrte Landingpage oder Baufortschritt.
- [x] Falls Site-Preview erreichbar bleibt, wird er nicht auf der oeffentlichen Coming-Soon-Seite beworben.

**Kontext:**
- `web/templates/pages/coming-soon.html`
- `web/templates/partials/nav.html`
- `web/templates/partials/footer.html`

**Output:**
- Konsistentes Link-Verhalten fuer Prelaunch.

## Wave-2-Exit-Gate

- [x] Oeffentliche Allowlist ist implementiert.
- [x] `/kontakt`, `/impressum`, `/datenschutz`, `/agb` sind ohne Site-Passwort erreichbar.
- [x] Restlicher Content bleibt gesperrt/vorgeschaltet.

## Notes

- Security-relevant: Kontakt POST bleibt oeffentlich, daher muessen CSRF und Rate Limiting unveraendert greifen.
- `cmd/server/main.go`: `/kontakt` GET/POST aus der passwortgeschuetzten Preview-Gruppe herausgezogen.
- `internal/handler/maintenance.go`: Public-Allowlist um `/kontakt` und `/agb` erweitert; Doku-Kommentar aktualisiert.
- `internal/handler/site_auth.go`: `/kontakt` als Public-Prelaunch-Exemption ergänzt.
- `web/templates/partials/nav.html` und `web/templates/partials/footer.html`: oeffentliche Links auf freigegebene Prelaunch-Seiten reduziert.
- Verification: `go test ./internal/handler -run TestRequireSitePassword` ist gruen.

## Abschluss

- [x] Wave abgeschlossen
