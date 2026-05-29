# Iteration 22: Public Prelaunch Gate, Legal Pages und Hansefit-Aktion

**Status:** Completed
**Rolle:** Acting as Product Owner. Planning Iteration 22 for padel-bz.
**Datum:** 2026-05-04

## Ziel

Die Website geht in einen oeffentlichen Prelaunch-Zustand: Besucher sehen zuerst eine Courts-Coming-Soon-Seite mit Logo, Eröffnung Herbst 2026, Hansefit-Registrierungs-CTA und Aktionshinweis "Ersten 100 Registrierten erhalten Baelle gratis!". Vorab erreichbar bleiben nur Impressum, Datenschutzerklaerung, AGB und die Hansefitregistrierung. Der restliche Website-Content bleibt fuer Besucher gesperrt, bis der Content final ist.

## Ausgangslage

- `/`, `/kontakt`, `/impressum`, `/datenschutz`, `/agb` existieren technisch bereits.
- `web/templates/pages/coming-soon.html` existiert als Standalone-Template.
- `MAINTENANCE_MODE` und `MaintenanceMiddleware` existieren bereits.
- `/kontakt` liegt aktuell in der site-password-geschuetzten Route-Gruppe und ist damit nicht oeffentlich erreichbar.
- `MaintenanceMiddleware` erlaubt aktuell nur `/impressum` und `/datenschutz`, aber nicht `/agb` und `/kontakt`.
- `web/templates/pages/agb.html` ist technisch vorbereitet, enthaelt aber noch Platzhaltertext.
- Final bereitgestellte Dokumente liegen im Code-Repo unter:
  - `docs/agb_courts.html`
  - `docs/datenschutzerklaerung_courts.html`
  - `docs/aktionsbedingungen-hansefit.html`
- Courts-Logo-Assets liegen bereits unter `web/static/img/`.

## Scope

- AGB und Datenschutzerklaerung aus den gelieferten `docs/*.html` in die bestehenden Go-Templates integrieren.
- Coming-Soon-Seite als oeffentliche Vorschaltseite gestalten.
- Hansefit-Aktion prominent, aber kompakt bewerben.
- Aktionsbedingungen als Popup/Dialog aus `docs/aktionsbedingungen-hansefit.html` bereitstellen.
- Oeffentliche Allowlist korrigieren:
  - `/kontakt` GET und POST
  - `/impressum`
  - `/datenschutz`
  - `/agb`
  - statische Assets
- Restlichen Content oeffentlich sperren.
- Interner Preview-/Staging-Zugriff bleibt fuer Betreiber moeglich, aber nicht oeffentlich verlinkt.
- Relevante Tests aktualisieren und ausfuehren.
- Deployment-Konfiguration fuer Prelaunch dokumentieren/pruefen.

## Nicht im Scope

- Inhaltliche juristische Bewertung oder Umschreiben der gelieferten Rechtstexte.
- Neues CMS-Feature.
- Datenbank-Migrationen.
- Neue Gewinnspiel-Tracking-Logik in der Datenbank.
- Automatische Pruefung, ob bereits 100 Registrierungen erreicht wurden.
- Relaunch der eigentlichen Landingpage-Inhalte.
- Entfernen bestehender Site-Preview-Funktionalitaet.

## Produktentscheidungen

- Die Hansefitregistrierung bleibt unter `/kontakt`, damit bestehende Formularlogik, CSRF, Rate Limiting, E-Mail-Versand und Admin-Auswertung weiterverwendet werden.
- Der Aktionshinweis ist Marketing-Text auf der Coming-Soon-Seite; rechtliche Details werden ueber einen barrierearmen Dialog erreichbar gemacht.
- Es wird kein neuer oeffentlicher Content-Pfad fuer die Aktionsbedingungen benoetigt, damit die oeffentliche Allowlist eng bleibt.
- Die Coming-Soon-Seite nutzt das bestehende Courts-Logo und die bestehende dunkle Markenwelt.
- `MAINTENANCE_MODE=true` bleibt der Deployment-Schalter fuer den Prelaunch-Zustand.

## Agenten-Zuordnung

| Agent | Skillset | Verantwortung |
|-------|----------|----------------|
| PO Lead | product-own | Scope, Priorisierung, Abnahme, Wave-Freigabe |
| Agent A | documentation-eng | Rechtstext-Integration fachlich strukturieren, Content-Quelle sichern |
| Agent B | handler-eng | Routing, Middleware, Public-Allowlist, CSRF-kompatible Kontaktfreigabe |
| Agent C | frontend-eng | Coming-Soon-Design, Dialog, Navigation/Footer fuer Prelaunch |
| Agent D | testing-eng | Handler-, Render-, Formular- und Regressionstests |
| Agent E | infrastructure-eng | Prelaunch-Deployment-Schalter und Betriebsnotizen |

## Waves

| Wave | Skill | Ziel | Status |
|------|-------|------|--------|
| Wave 1 | documentation-eng | Legal- und Aktionscontent integrierbar machen | Completed |
| Wave 2 | handler-eng | Public-Allowlist und Prelaunch-Gate korrigieren | Completed |
| Wave 3 | frontend-eng | Coming-Soon-Seite und Aktionsdialog umsetzen | Completed |
| Wave 4 | testing-eng | Tests und Regression verifizieren | Completed |
| Wave 5 | infrastructure-eng | Deployment-/Betriebscheck fuer Prelaunch | Completed |

## Critical Path

1. Legal-Content und Aktionsbedingungen in Template-taugliche Form bringen.
2. Public-Allowlist korrigieren, damit `/kontakt`, `/agb`, `/datenschutz`, `/impressum` wirklich oeffentlich sind.
3. Coming-Soon-Seite mit CTA und Aktionsdialog bauen.
4. Tests und Routenverhalten absichern.
5. Deployment-Schalter fuer Prelaunch pruefen und dokumentieren.

## Risiken

| Risiko | Impact | Umgang |
|--------|--------|--------|
| Rechtstexte aus `docs/*.html` enthalten komplettes Standalone-HTML mit eigenem CSS | Mittel | Nur den fachlichen Inhalt ins bestehende Template-System uebernehmen, Styling an vorhandene Legal-Styles anpassen |
| `/kontakt` POST oeffentlich freigeben veraendert Schutzmodell | Mittel | CSRF und Rate Limiting bleiben aktiv; Tests fuer GET/POST-Allowlist ergaenzen |
| Maintenance- und Site-Password-Gate koennen sich widersprechen | Mittel | Ein zentrales Allowlist-Verhalten definieren und in Tests abbilden |
| Aktionsbedingungen im Popup sind lang | Niedrig | Scrollbarer, fokussierbarer Dialog mit Schliessen-Button und Tastaturbedienung |
| Suchmaschinen indexieren gesperrten Content oder 503-Seite falsch | Niedrig | Coming Soon weiterhin als Maintenance/Prelaunch behandeln, keine Links auf gesperrten Content, optional noindex fuer Vorschaltseite pruefen |

## Definition of Done

- [x] `/` zeigt die Courts-Coming-Soon-Seite.
- [x] Coming-Soon-Seite zeigt Courts-Logo, Standort/Eröffnung, Hansefit-CTA und Aktionshinweis.
- [x] Aktionsbedingungen sind per Popup/Dialog erreichbar.
- [x] `/kontakt` GET und POST sind ohne Site-Passwort erreichbar.
- [x] `/impressum`, `/datenschutz`, `/agb` sind ohne Site-Passwort erreichbar.
- [x] Der restliche Website-Content ist oeffentlich nicht erreichbar.
- [x] AGB und Datenschutzerklaerung enthalten die gelieferten finalen Inhalte aus `docs/`.
- [x] `go test ./...` ist gruen.
- [x] `git diff --check` ist gruen.
- [x] Prelaunch-Betriebsnotiz ist dokumentiert.

## Abschlussnotes

- AGB, Datenschutzerklaerung und Aktionsbedingungen wurden aus den gelieferten `docs/*.html` in die Anwendung uebernommen.
- Public Prelaunch Allowlist ist umgesetzt: `/kontakt`, `/impressum`, `/datenschutz`, `/agb`, Assets und Admin bleiben erreichbar; restliche Inhalte laufen ueber Coming Soon.
- Coming-Soon-Seite wurde mit Courts-Logo, Hansefit-CTA, Aktionsbadge und Aktionsbedingungen-Dialog neu gestaltet.
- `.env.example` und `docs/Operations.md` dokumentieren `MAINTENANCE_MODE=true` fuer Prelaunch und Smoke-Checks.
- Verification: `go test ./...` und `git diff --check` sind gruen.
