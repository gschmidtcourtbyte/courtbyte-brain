# Iteration 23: Prelaunch Polish, Legal-Layout-Fix und Admin-Bypass

**Status:** Completed
**Rolle:** Acting as Product Owner. Planning Iteration 23 for padel-bz.
**Datum:** 2026-05-04

## Ziel

Iteration 22 hat den Prelaunch live gestellt, dabei sind drei Korrektur- und ein UX-Punkt offen geblieben:

1. Die Navbar wurde fuer den Prelaunch umgebaut (Datenschutz/AGB/Impressum statt Courts/Events/Kontakt + Instagram). Der vorherige Stand soll wiederhergestellt werden, weil hinter dem Site-Password weiter die alte Landingpage-Navigation gilt.
2. Auf `/agb` und `/datenschutz` ueberlappt das Inhaltsverzeichnis mit dem Flieusstext und der Sticky-Navbar.
3. Der Admin-Login `/admin/login` muss zuverlaessig per direkter URL erreichbar sein, ohne dass ein Button auf der oeffentlichen Seite erscheint.
4. Der Betreiber soll als eingeloggter Admin die uebrigen Endpoints (`/`, ggf. spaeter weitere) erreichen koennen, ohne jedes Mal das Site-Passwort einzugeben.

## Ausgangslage

- `web/templates/partials/nav.html` wurde in Iteration 22 von den alten Anker-Links (`/#courts`, `/#events`, `/#kontakt` + Instagram-Icon) auf Legal-Links (`/datenschutz`, `/agb`, `/impressum`) umgestellt. Dieser Umbau war nicht im Scope der Prelaunch-Aenderung und soll zurueckgenommen werden. Diff zeigt klar den vorherigen Stand (siehe Commit `f9a1575` vs. `ef9663e`).
- `web/templates/pages/agb.html` und `web/templates/pages/datenschutz.html` rendern unter `base.html` mit der Sticky-Navbar (`position:sticky; top:0; height:64px`). Das Inhaltsverzeichnis startet direkt unter `<main>` ohne hinreichenden Top-Abstand; beim Scrollen wandert die TOC-Box visuell hinter die Navbar. `.legal-section` hat `scroll-margin-top:110px`, aber die TOC selbst und die Headline der ersten Section bekommen keine vergleichbare Behandlung.
- `cmd/server/main.go` registriert `/admin/login` (GET/POST) ohne Site-Password-Schutz. `MaintenanceMiddleware` laesst `/admin` und `/admin/*` durch. `RequireSitePassword` exempted ebenfalls `/admin/`. Funktional ist der direkte URL-Aufruf damit moeglich, soll aber explizit verifiziert werden — und kein Link/Button im sichtbaren UI auf den Login zeigen.
- `RequireSitePassword` und `MaintenanceMiddleware` haben aktuell keine Kenntnis vom Admin-Cookie. Wer als Admin eingeloggt ist, muss heute trotzdem `/site-login` durchlaufen, um z.B. `/` zu sehen, sobald `MAINTENANCE_MODE=false` aber `SITE_PASSWORD` gesetzt ist. Im Prelaunch-Modus sieht der Admin sogar nur die Coming-Soon-Seite, weil die Maintenance-Allowlist `/` nicht enthaelt.

## Scope

### Frontend / Templates
- `web/templates/partials/nav.html` auf den vor-Iteration-22-Stand zuruecksetzen (Courts/Events/Kontakt-Anker, Instagram-Icon, Hansefit-CTA bleibt). Quelle: Diff `f9a1575..ef9663e`.
- Layout der Legal-Seiten so korrigieren, dass weder das Inhaltsverzeichnis noch die Section-Ueberschriften unter die Sticky-Navbar rutschen oder mit dem Flieusstext kollidieren.
  - `<main>` bzw. der Wrapper-`<section>` der Legal-Pages muss ausreichend Top-Padding gegen die Sticky-Navbar haben.
  - `.legal-toc` darf nicht direkt unter der Navbar kleben; `scroll-margin-top` muss konsistent zur Navbar-Hoehe sein.
  - Mobile-Verhalten verifizieren: Auf engen Viewports darf das TOC nicht ueber der `.sec-head` liegen.
- `/admin/login` darf nicht ueber sichtbare UI-Elemente (Footer, Navbar, Coming-Soon) verlinkt sein. Der Endpunkt bleibt als direkte URL erreichbar.

### Backend / Routing
- `RequireSitePassword` und `MaintenanceMiddleware` so erweitern, dass eine gueltige Admin-Session beide Gates automatisch passiert. Idee: gemeinsamer Bypass-Predicate `func(*http.Request) bool`, der vom `AdminAuthHandler` bereitgestellt wird (`IsAdmin`) und den Middlewares per Konstruktor uebergeben wird.
- Verifizieren, dass `/admin/login` (GET/POST) auch bei `MAINTENANCE_MODE=true` und gesetztem `SITE_PASSWORD` ohne Vorab-Authentifizierung erreichbar ist.
- Optional, aber empfohlen: Admin-Sessions sollen den Site-Password-Cookie implizit als gesetzt behandeln, damit der `/`-Aufruf nach dem Admin-Login nicht 302 auf `/site-login` macht.

### Tests
- Regressionstests fuer Maintenance- und Site-Auth-Allowlist erweitern um den Admin-Bypass-Pfad.
- Render-Smoke-Test fuer `/agb` und `/datenschutz` weiter gruen halten.
- Optional: Snapshot-/HTML-Test, dass `nav.html` keine Direktlinks auf `/admin/...` enthaelt.

## Nicht im Scope

- Kein Redesign der Coming-Soon-Seite.
- Keine inhaltliche Aenderung an AGB/Datenschutz/Impressum.
- Keine Aenderung am Admin-Login-Mechanismus selbst (bcrypt, Session-Cookie, Secrets).
- Keine neuen Admin-Features.
- Keine Aenderung an der `MAINTENANCE_MODE`-Allowlist fuer oeffentliche Pfade — die bleibt `/kontakt`, `/impressum`, `/datenschutz`, `/agb`, `/static`, `/admin`.
- Keine Migrationen.

## Produktentscheidungen

- Die Navbar-Aenderung aus Iteration 22 war ein Side-Effect im Prelaunch-Build. Sie wird zurueckgesetzt; das Inhaltsverzeichnis der Coming-Soon-Seite uebernimmt die Verlinkung der Legal-Seiten weiterhin.
- Admin-Login bleibt ueber direkte URL erreichbar (`/admin/login`). Es wird kein sichtbarer Button platziert. Begruendung: Public-Visibility soll fuer den Prelaunch-Auftritt clean bleiben.
- Admin-Bypass fuer Maintenance- und Site-Password-Gate ueber Cookie-Detection: einmal als Admin eingeloggt, sieht der Betreiber die Site exakt so, wie ein Besucher mit Site-Passwort sie sehen wuerde. Vorteil: Kein zweites Geheimnis im Kopf des Betreibers, keine separate Bypass-Token.
- Admin-Session-TTL bleibt unveraendert (7 Tage), Cookie `Secure` in Prod, `SameSite=Lax`. Der Bypass arbeitet ausschliesslich auf Basis dieses bestehenden Cookies.

## Idee fuer den Admin-Bypass (Detail)

`AdminAuthHandler.IsAdmin(r *http.Request) bool` wird als oeffentliche Methode exportiert (technisch identisch zum bestehenden `isAuthenticated`).
`MaintenanceMiddleware` und `SiteAuthHandler` bekommen je einen optionalen `adminCheck func(*http.Request) bool`. Wenn die Funktion vorhanden ist und `true` zurueckgibt, wird der Request unmittelbar an den naechsten Handler weitergereicht, bevor die Allowlist-Checks oder das Coming-Soon-Template greifen.
Vorteile:
- Keine neuen Cookies, keine neuen Secrets.
- Bestehende Tests bleiben aussagekraeftig: ohne Admin-Cookie sind alle bisherigen Pfade unveraendert.
- Loescht der Betreiber den Admin-Cookie, wirkt das Site-Password-Gate sofort wieder.
Wartungsanleitung:
- Admin meldet sich einmal unter `/admin/login` an.
- Solange das Cookie gueltig ist, sind alle internen Endpoints (`/`, ggf. zukuenftige Routen) ohne Site-Passwort erreichbar.
- Logout via `/admin/logout` (POST) entfernt den Bypass.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Abnahme, Wave-Freigabe |
| Agent A | frontend-eng | Navbar-Revert, Legal-Layout-Fix, sicherstellen dass kein UI-Link auf Admin-Login zeigt |
| Agent B | handler-eng | `AdminAuthHandler.IsAdmin` oeffentlich machen, Bypass in `MaintenanceMiddleware` und `RequireSitePassword`, `cmd/server/main.go` verdrahten |
| Agent C | testing-eng | Regressionstests Allowlist, Admin-Bypass, Render-Smoke fuer Legal-Seiten |

## Waves

| Wave | Skill | Ziel | Status |
|------|-------|------|--------|
| Wave 1 | frontend-eng | Navbar zuruecksetzen, Legal-TOC-/Layout-Fix, keine Admin-Links im UI | Completed |
| Wave 2 | handler-eng | Admin-Bypass in Maintenance- und Site-Auth-Middleware, `/admin/login` per URL verifiziert | Completed |
| Wave 3 | testing-eng | Tests aktualisieren / ergaenzen, `go test ./...` gruen | Completed |

Wave 1 und Wave 2 sind voneinander unabhaengig und koennen **parallel** laufen.
Wave 3 startet, sobald Wave 1 **und** Wave 2 abgeschlossen sind.

## Critical Path

1. Wave 1 + Wave 2 parallel.
2. Wave 3 nach Abschluss beider vorheriger Waves.
3. PO-Abnahme: Manueller Smoke-Check `/`, `/agb`, `/datenschutz`, `/admin/login`, Admin-Session -> `/`.

## Risiken

| Risiko | Impact | Umgang |
|--------|--------|--------|
| Navbar-Revert fuehrt dazu, dass die Legal-Seiten ihre primaere Verlinkung verlieren | Niedrig | Footer behaelt Legal-Links; Coming-Soon-Seite enthaelt sie ebenfalls; verifizieren in Wave 1 |
| Admin-Bypass umgeht versehentlich auch Maintenance-/Allowlist-Logik fuer NICHT-eingeloggte Anfragen | Hoch | Bypass ausschliesslich an `IsAdmin(r) == true` koppeln; explizite Tests fuer den Negativfall |
| TOC-Layout-Fix bricht Print-/Screenreader-Reihenfolge | Niedrig | Reine CSS-Aenderungen, semantisches Markup unveraendert |
| `/admin/login` ist nicht offensichtlich auffindbar | Mittel | Endpoint-URL in `docs/Operations.md` festhalten; kein UI-Link, aber Memo fuer Betreiber |

## Definition of Done

- [x] `web/templates/partials/nav.html` entspricht dem Stand vor Iteration 22 (Anker-Links, Instagram-Icon, Hansefit-CTA).
- [x] `/agb` und `/datenschutz` zeigen das Inhaltsverzeichnis ohne Ueberlappung mit Navbar oder Flieusstext (Mobile + Desktop).
- [x] Kein sichtbarer UI-Link/Button verweist auf `/admin/login` (Navbar, Footer, Coming-Soon, sonstige Templates).
- [x] `/admin/login` ist per direkter URL erreichbar — auch bei `MAINTENANCE_MODE=true` und gesetztem `SITE_PASSWORD`.
- [x] Eingeloggter Admin kann `/` und weitere geschuetzte Routen ohne `/site-login` und ohne Coming-Soon-503 erreichen.
- [x] Ohne Admin-Session bleibt das Verhalten der Allowlists exakt wie heute.
- [x] `go test ./...` ist gruen.
- [x] `git diff --check` ist gruen.
- [x] `docs/Operations.md` dokumentiert, wie der Betreiber via Admin-Login die Seite nutzt.

## Abschlussnotes

- Wave 1 (frontend-eng): `nav.html` per `git show f9a1575` zurueckgesetzt; `style.css` erhaelt `html { scroll-padding-top:84px }`, `.legal-content { padding:24px var(--pad) 0 }`, `.legal-toc { scroll-margin-top:110px }` plus Mobile-Puffer; keine Admin-Links in oeffentlichen Templates.
- Wave 2 (handler-eng): `AdminAuthHandler.IsAdmin` exportiert, `MaintenanceMiddleware` um `adminCheck`-Param erweitert, `SiteAuthHandler.SetAdminCheck` als Setter (minimaler API-Drift), Wiring in `cmd/server/main.go`, drei Zeilen `docs/Operations.md` zum Admin-Bypass.
- Wave 3 (testing-eng): Neue Tests `TestMaintenanceMiddlewareAdminBypass`, `TestRequireSitePassword_AdminBypass`, `TestRequireSitePassword_AdminBypassExemptStillExempt`, `TestNavPartialHasNoAdminLink`. Bestehende Tests unveraendert gruen.
- Verification: `go build ./...`, `go test ./...`, `go vet ./...`, `git diff --check` — alle gruen.
