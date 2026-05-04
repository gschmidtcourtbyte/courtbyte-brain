# Wave 3 - Regression, Build und Secret-Audit

**Iteration:** [[Iteration-31]]
**Skill:** testing-eng
**Status:** Done

## Ziel

Die Admin-User-Provisionierung und der neue FAQ-Eintrag sind abgesichert. Backend-Tests, Customer-Web Checks und ein expliziter Secret-Audit bestaetigen, dass die Iteration ohne Passwort-Leak und ohne unerwuenschte Nebeneffekte abgeschlossen werden kann.

## Agent

QA Engineer: Fokus auf zielgerichtete Testauswahl, Auth-Regression, UI-/Content-Pruefung, Build-Verifikation und Secret-/Scope-Audit.

## Tasks

- [x] Backend-Tests fuer neue oder geaenderte Auth-/Provisioning-Pakete ausfuehren.
- [x] Bei groesserem Backend-Diff `go test ./...` ausfuehren oder konkrete Blocker dokumentieren.
- [x] Admin-Provisioning manuell oder automatisiert pruefen: Create, erneutes Ausfuehren, Rolle/Status/Email-Verifikation.
- [x] Sicherstellen, dass Login mit dem provisionierten Admin technisch moeglich ist, soweit lokale/staging Umgebungen verfuegbar sind.
- [x] Customer-Web Tests fuer FAQ-/Content-nahe Helfer ausfuehren, falls vorhanden.
- [x] Customer-Web Build ausfuehren: `cd web/customer && npm run build`.
- [x] Customer-Web Lint ausfuehren, falls im Projekt aktuell verfuegbar.
- [x] FAQ-Seite statisch oder per Browser-Smoke pruefen: neuer Eintrag sichtbar und Text nicht abgeschnitten.
- [x] Finalen Secret-Audit ausfuehren: kein Klartextpasswort, kein bekannter Passwort-Hash, keine Secret-Fragmente in Repo- oder Vault-Diff.
- [x] Finalen Scope-Audit ausfuehren: keine Planungsdateien im Code-Repo, keine ungewollten Auth-/FAQ-Nebeneffekte.
- [x] Abschlussnotes in Iteration- und Wave-Dateien ergaenzen und erledigte Tasks abhaken.

## Akzeptanzkriterien

- [x] Relevante Backend-Tests sind gruen oder Blocker sind dokumentiert.
- [x] Admin-Provisioning ist idempotent und erzeugt keinen doppelten User.
- [x] Der Admin-User hat Rolle `admin`, Status `active` und gesetztes `email_verified_at`.
- [x] Customer-Web Build ist gruen oder Blocker sind dokumentiert.
- [x] Lint ist gruen oder bekannte Ausnahmen sind konkret dokumentiert.
- [x] FAQ-Eintrag ist sichtbar und fachlich vollstaendig.
- [x] Secret-Audit bestaetigt: kein Passwort und kein statischer Hash wurden versioniert.
- [x] Scope-Audit bestaetigt: Planung liegt nur in der Vault.

## Abhaengigkeiten

Wave 1 und Wave 2 muessen umgesetzt sein.

## Notes

Falls keine lokale DB oder Staging-Zugriff fuer echten Login-Smoke vorhanden ist, muss der Agent den Blocker konkret dokumentieren und mindestens Repository-/Command-Tests plus SQL-/Diff-Audit durchfuehren.

Umsetzung 2026-05-04:
- Backend-Regression gruen: `go test ./...`.
- Auth-/Command-Checks zusaetzlich geprueft: `go test ./cmd/server ./cmd/provision-admin ./internal/auth/...`.
- Customer-Web Tests gruen: `npm test -- --configLoader runner` mit 11 Testdateien / 88 Tests.
- Customer-Web Build gruen: `npm run build`, inklusive statischer `/faq` Route.
- Customer-Web Lint gruen: `npm run lint`; Next.js meldet nur die bekannte `next lint` Deprecation, keine Warnungen oder Fehler.
- FAQ-Content statisch geprueft: Frage und Pflichtinhalte sind in `faq-content.ts` und `faq-content.test.ts` vorhanden.
- Secret-Audit gruen: kein Treffer fuer das generierte Passwort; kein statischer bcrypt-Hash in geaenderten Auth-/FAQ-/Vault-Artefakten.
- Scope-Audit gruen: keine Iteration-31-Planungsdateien im Code-Repo gefunden.
- `git diff --check` ohne Whitespace-Fehler.
- Echte DB-Provisionierung/Login-Smoke nicht ausgefuehrt: `DATABASE_URL` ist in der aktuellen Shell nicht gesetzt. Stattdessen wurden Provisioning-Service, bcrypt-Adapter, Command-Build und SQL-Upsert statisch/automatisiert geprueft.
