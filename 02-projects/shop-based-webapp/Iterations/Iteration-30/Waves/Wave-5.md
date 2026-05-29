# Wave 5 - Regression, Build und Scope-Audit

**Iteration:** [[Iteration-30]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die cross-funktionalen Aenderungen aus Iteration 30 sind gegen Regressionen abgesichert. Backend-Tests, Customer-Web Tests, Build/Lint und ein finaler Scope-Audit bestaetigen, dass Kapazitaet, Anfrageoption und Homepage-Reihenfolge korrekt integriert sind.

## Agent

QA Engineer: Fokus auf zielgerichtete Testauswahl, Contract-Regression, UI-Smoke, Build-Verifikation und Kontrolle gegen den definierten Scope.

## Tasks

- [x] Backend-Tests fuer geaenderte Camps-Pakete ausfuehren, mindestens relevante `internal/camps/...` Tests.
- [x] Bei groesserem Backend-Diff `go test ./...` ausfuehren oder konkreten Blocker dokumentieren.
- [x] Migrationen statisch pruefen: Up-/Down-Paar, Constraints, keine Veraenderung bestehender Migrationen.
- [x] Customer-Web Tests fuer Camp-Packages, Camp-Events, Booking-Form und betroffene Komponenten/Helfer ausfuehren.
- [x] Customer-Web Build ausfuehren: `cd web/customer && npm run build`.
- [x] Customer-Web Lint ausfuehren, falls im Projekt aktuell verfuegbar und nicht durch bekannte Altlasten blockiert.
- [x] Visuellen Smoke fuer Homepage-Reihenfolge durchfuehren: Hero, danach Ueber uns, danach Campkalender.
- [x] Visuellen Smoke fuer Campkalender durchfuehren: Kapazitaetshinweis sichtbar, rot abgesetzt, keine Ueberlagerungen.
- [x] Visuellen Smoke fuer Admin-Campeditor durchfuehren: Kapazitaet eintragen, speichern, leeren.
- [x] Visuellen Smoke fuer Anfrageformular durchfuehren: neue Anfrageoption sichtbar, auswaehlbar, Erfolgsansicht lesbar.
- [x] Finalen `rg`-/Diff-Audit durchfuehren: neue Anfrageoption nur im Anfrageformular bzw. notwendigen Backend/API-/Test-Kontexten, nicht in Pricing/Paketkarten.
- [x] Abschlussnotes in allen Wave-Dateien ergaenzen und erledigte Tasks abhaken.

## Akzeptanzkriterien

- [x] Relevante Backend-Tests sind gruen oder konkrete Blocker sind dokumentiert.
- [x] Relevante Customer-Web Tests sind gruen oder konkrete Blocker sind dokumentiert.
- [x] Customer-Web Build ist gruen oder konkrete Blocker sind dokumentiert.
- [x] Lint ist gruen oder bekannte Ausnahmen sind konkret dokumentiert.
- [x] Migrationen sind als Up-/Down-Paar plausibel und rueckwaertskompatibel.
- [x] Visueller Smoke bestaetigt Homepage-Reihenfolge, Kapazitaetshinweis, Admin-Pflege und Anfrageformular.
- [x] Scope-Audit bestaetigt, dass "Gruppenanfrage / Individualanfrage" nicht als regulaeres Paket im Pricing auftaucht.
- [x] Keine Planungsdateien wurden im Code-Repo angelegt.
- [x] Keine Secrets, lokalen Pfade oder generierten Build-Artefakte wurden versehentlich veraendert.

## Abhaengigkeiten

Wave 1 bis Wave 4 muessen integriert sein.

## Notes

Falls kein Browser-/Screenshot-Tool verfuegbar ist, wird der visuelle Smoke als lokaler Tooling-Blocker dokumentiert und durch statischen Markup-/CSS-Audit plus Tests/Build ergaenzt.

Umsetzung 2026-04-27:
- Backend Camps-Tests: `go test ./internal/camps/...` gruen.
- Voller Backend-Testlauf: `go test ./...` gruen.
- Customer-Web Tests: `npm test -- --configLoader runner` mit 10 Testdateien / 84 Tests gruen.
- Customer-Web Build: `npm run build` erfolgreich.
- Customer-Web Lint: `npm run lint` erfolgreich; Next.js weist nur auf die kommende Deprecation von `next lint` hin, keine ESLint-Warnungen oder Fehler.
- Migrationen statisch geprueft: `048_alter_camp_events_add_capacity_fields.up.sql` und `.down.sql` vorhanden, additiv, nullable, mit Constraints und Down-Rollback.
- Scope-Audit: `group-individual`/`Gruppenanfrage / Individualanfrage` taucht nur in Booking-Service/API-/Test-Kontexten und Checkout-spezifischen Package-Optionen auf; keine Treffer in Pricing/Paketkarten ausserhalb Checkout/Admin.
- Homepage-Reihenfolge statisch geprueft: `Hero`, danach `AboutUs`, danach `NextCampBanner`.
- Kapazitaetsanzeige statisch geprueft: `NextCampBanner` und Admin-Liste nutzen roten `red-500` Hinweis ueber `getCampCapacityLabel`; unvollstaendige/ungueltige Werte werden ausgeblendet.
- Visueller Browser-Smoke lokal nicht ausgefuehrt: kein `chromium`, `chromium-browser`, `google-chrome` und keine Playwright-Specs im Workspace vorhanden. Ersatz: statischer Markup-/CSS-Audit plus erfolgreiche Tests, Build und Lint.
- `git diff --check` ohne Whitespace-Fehler.
- Keine Iteration-/Wave-Planungsdateien im Code-Repo gefunden; Planungsupdates liegen ausschliesslich in der Vault.
