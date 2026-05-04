# Wave 2: Public-Allowlist und Prelaunch-Gate

**Status:** Planned
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
- [ ] Bei `MAINTENANCE_MODE=true` wird `/kontakt` GET an den Kontakt-Handler durchgereicht.
- [ ] Bei `MAINTENANCE_MODE=true` wird `/kontakt` POST an den Kontakt-Handler durchgereicht.
- [ ] Bei `MAINTENANCE_MODE=true` wird `/agb` durchgereicht.
- [ ] `/impressum`, `/datenschutz`, `/static/*`, `/admin/*` bleiben durchgereicht.
- [ ] `/` und nicht freigegebene Marketing-/Content-Pfade zeigen die Coming-Soon-Seite.
- [ ] Kommentar/Code-Doku der Middleware nennt die aktuelle Allowlist korrekt.

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
- [ ] `/kontakt` GET ist ohne Site-Passwort erreichbar.
- [ ] `/kontakt` POST ist ohne Site-Passwort erreichbar.
- [ ] Nicht erlaubte Content-Routen bleiben ohne Site-Passwort gesperrt oder laufen in Maintenance auf Coming Soon.
- [ ] Admin-Routen bleiben nur ueber Admin-Auth geschuetzt.
- [ ] Bestehende Preview-/Staging-Funktionalitaet bleibt fuer Betreiber erhalten.

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
- [ ] Coming-Soon-Template verlinkt oeffentlich nur erlaubte Seiten.
- [ ] Keine oeffentliche Navigation zeigt Links auf die gesperrte Landingpage oder Baufortschritt.
- [ ] Falls Site-Preview erreichbar bleibt, wird er nicht auf der oeffentlichen Coming-Soon-Seite beworben.

**Kontext:**
- `web/templates/pages/coming-soon.html`
- `web/templates/partials/nav.html`
- `web/templates/partials/footer.html`

**Output:**
- Konsistentes Link-Verhalten fuer Prelaunch.

## Wave-2-Exit-Gate

- [ ] Oeffentliche Allowlist ist implementiert.
- [ ] `/kontakt`, `/impressum`, `/datenschutz`, `/agb` sind ohne Site-Passwort erreichbar.
- [ ] Restlicher Content bleibt gesperrt/vorgeschaltet.

## Notes

- Security-relevant: Kontakt POST bleibt oeffentlich, daher muessen CSRF und Rate Limiting unveraendert greifen.

## Abschluss

- [ ] Wave abgeschlossen
